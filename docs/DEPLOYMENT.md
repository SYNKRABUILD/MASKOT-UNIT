# Deployment Guide

## Deployment Environments

### Development
- Local machine with Docker Compose
- Mock external services
- Development database

### Staging
- Cloud VPS (DigitalOcean/AWS)
- Real M-Pesa sandbox
- Staging database
- Performance testing environment

### Production
- Auto-scaling cluster (AWS/Google Cloud)
- Managed database service
- CDN integration
- Real M-Pesa integration
- 24/7 monitoring

## Pre-Deployment Checklist

- [ ] All tests passing (unit, integration, e2e)
- [ ] Code review completed
- [ ] Security audit passed
- [ ] Database migrations tested
- [ ] Environment variables configured
- [ ] Backups configured
- [ ] Monitoring setup complete
- [ ] Documentation updated
- [ ] Rollback plan prepared

## Docker Build & Push

### Build Images

```bash
# Backend
cd backend
docker build -t maskot-backend:latest .
docker tag maskot-backend:latest <registry>/maskot-backend:latest
docker push <registry>/maskot-backend:latest

# Frontend
cd frontend
docker build -t maskot-frontend:latest .
docker tag maskot-frontend:latest <registry>/maskot-frontend:latest
docker push <registry>/maskot-frontend:latest
```

### Build Arguments

```dockerfile
# Backend
ARG NODE_ENV=production
ARG PORT=3001

# Frontend
ARG REACT_APP_API_URL=https://api.maskotunit.com/api
ARG REACT_APP_ENV=production
```

## AWS Deployment

### 1. Setup ECR (Elastic Container Registry)

```bash
# Create ECR repositories
aws ecr create-repository --repository-name maskot-backend
aws ecr create-repository --repository-name maskot-frontend

# Get login token
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us-east-1.amazonaws.com

# Push images
docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/maskot-backend:latest
docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/maskot-frontend:latest
```

### 2. Setup RDS Database

```bash
# Create RDS instance
aws rds create-db-instance \
  --db-instance-identifier maskot-postgres \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password <secure_password> \
  --allocated-storage 100 \
  --backup-retention-period 30

# Get endpoint
aws rds describe-db-instances --db-instance-identifier maskot-postgres
```

### 3. Setup ECS (Elastic Container Service)

```bash
# Create ECS cluster
aws ecs create-cluster --cluster-name maskot-cluster

# Create task definitions
aws ecs register-task-definition --cli-input-json file://backend-task-def.json
aws ecs register-task-definition --cli-input-json file://frontend-task-def.json

# Create services
aws ecs create-service \
  --cluster maskot-cluster \
  --service-name maskot-backend \
  --task-definition maskot-backend:1 \
  --desired-count 2 \
  --launch-type FARGATE
```

### 4. Setup Load Balancer

```bash
# Create Application Load Balancer
aws elbv2 create-load-balancer \
  --name maskot-alb \
  --subnets subnet-xxx subnet-yyy \
  --security-groups sg-xxx

# Create target groups
aws elbv2 create-target-group \
  --name maskot-backend-targets \
  --protocol HTTP \
  --port 3001 \
  --vpc-id vpc-xxx

# Create listeners
aws elbv2 create-listener \
  --load-balancer-arn <alb_arn> \
  --protocol HTTP \
  --port 80 \
  --default-actions Type=forward,TargetGroupArn=<target_group_arn>
```

### 5. Setup CloudFront (CDN)

```bash
# Create CloudFront distribution
aws cloudfront create-distribution \
  --distribution-config file://cloudfront-config.json
```

## DigitalOcean Deployment

### 1. Create Droplet

```bash
doctl compute droplet create maskot-app \
  --region nyc3 \
  --image ubuntu-22-04-x64 \
  --size s-2vcpu-4gb \
  --enable-monitoring \
  --wait
```

### 2. Configure Server

```bash
# Connect to droplet
ssh root@<droplet_ip>

# Update system
apt update && apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### 3. Deploy with Docker Compose

```bash
# Clone repository
git clone https://github.com/SYNKRABUILD/MASKOT-UNIT.git
cd MASKOT-UNIT

# Create production .env
cp .env.example .env
# Edit .env with production values

# Start services
docker-compose -f docker-compose.prod.yml up -d

# View logs
docker-compose logs -f
```

### 4. Setup SSL with Let's Encrypt

```bash
# Install Certbot
apt-get install certbot python3-certbot-nginx -y

# Generate certificate
certbot certonly --standalone -d api.maskotunit.com

# Configure Nginx
# Update nginx config to use certificates
# Certificate path: /etc/letsencrypt/live/api.maskotunit.com/
```

## Database Migrations

### Before Deployment

```bash
# Test migrations in staging
cd backend
npm run migrate:test

# Run migrations
npm run migrate

# Verify migration
npm run migrate:status
```

### Rollback Procedure

```bash
# Rollback last migration
npm run migrate:rollback

# Rollback specific number
npm run migrate:rollback --steps=3
```

## Environment Configuration

### Production .env

```
# Server
NODE_ENV=production
PORT=3001
LOG_LEVEL=info

# Database
DATABASE_URL=postgresql://user:password@rds-endpoint:5432/maskot_unit_prod
REDIS_URL=redis://redis-endpoint:6379

# Security
JWT_SECRET=<very_long_random_string>
JWT_EXPIRE=24h
ENCRYPTION_KEY=<encryption_key>

# M-Pesa
MPESA_API_KEY=<production_api_key>
MPESA_SECRET=<production_secret>
MPESA_BUSINESS_CODE=<production_code>
MPESA_CALLBACK_URL=https://api.maskotunit.com/api/payments/callback

# Email
SENDGRID_API_KEY=<sendgrid_key>
SENDGRID_FROM=noreply@maskotunit.com

# Social Media
INSTAGRAM_APP_ID=<app_id>
INSTAGRAM_APP_SECRET=<app_secret>
TIKTOK_CLIENT_ID=<client_id>
TIKTOK_CLIENT_SECRET=<client_secret>
FACEBOOK_APP_ID=<app_id>
FACEBOOK_APP_SECRET=<app_secret>
YOUTUBE_API_KEY=<api_key>

# Cloud Storage
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=<access_key>
AWS_SECRET_ACCESS_KEY=<secret_key>
AWS_S3_BUCKET=maskot-unit-prod

# Monitoring
SENTRY_DSN=<sentry_dsn>
NEW_RELIC_LICENSE_KEY=<license_key>
```

## Monitoring & Health Checks

### Application Health

```bash
# Health check endpoint
GET /api/health

Response:
{
  "status": "ok",
  "timestamp": "2026-08-30T20:00:00Z",
  "uptime": 3600,
  "database": "connected",
  "redis": "connected"
}
```

### Logs & Metrics

```bash
# View application logs
docker-compose logs -f backend

# Monitor CPU/Memory
docker stats maskot-backend

# Database monitoring
# CloudWatch or DigitalOcean monitoring dashboard
```

## Backup Strategy

### Database Backups

```bash
# Automated backups (RDS: 7-day retention)
# Manual backup
aws rds create-db-snapshot \
  --db-instance-identifier maskot-postgres \
  --db-snapshot-identifier maskot-postgres-2026-08-30

# Backup to S3
pg_dump -h <rds_endpoint> -U admin maskot_unit_prod | \
  aws s3 cp - s3://maskot-backups/db-2026-08-30.sql.gz
```

### File Backups

```bash
# Backup S3 bucket
aws s3 sync s3://maskot-unit-prod s3://maskot-backups/s3-2026-08-30 --region us-east-1
```

## Scaling Strategy

### Horizontal Scaling

```bash
# Scale backend services
aws ecs update-service \
  --cluster maskot-cluster \
  --service maskot-backend \
  --desired-count 5

# Auto-scaling policy
aws autoscaling create-launch-configuration \
  --launch-configuration-name maskot-lc \
  --image-id <ami_id> \
  --instance-type t3.medium
```

### Database Scaling

```bash
# Enable RDS auto-scaling
aws application-autoscaling register-scalable-target \
  --service-namespace rds \
  --resource-id cluster:maskot-cluster \
  --scalable-dimension rds:cluster:DesiredDBInstanceCount \
  --min-capacity 2 \
  --max-capacity 10
```

## CI/CD Pipeline

### GitHub Actions Deployment

```yaml
name: Deploy to Production
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build and push Docker images
        run: |
          docker build -t maskot-backend:${{ github.sha }} ./backend
          docker push <registry>/maskot-backend:${{ github.sha }}
      
      - name: Deploy to AWS
        run: |
          aws ecs update-service \
            --cluster maskot-cluster \
            --service maskot-backend \
            --force-new-deployment
```

## Rollback Procedure

### Rollback Steps

1. Identify issue in production
2. Check last working deployment version
3. Update ECS service to previous image version
4. Verify health checks pass
5. Monitor error rates
6. Post-incident review

```bash
# Rollback to previous image
aws ecs update-service \
  --cluster maskot-cluster \
  --service maskot-backend \
  --force-new-deployment \
  --force-new-image maskot-backend:previous_version

# Verify
aws ecs describe-services \
  --cluster maskot-cluster \
  --services maskot-backend
```

## Post-Deployment Checklist

- [ ] Verify all services are running
- [ ] Check application health endpoint
- [ ] Test critical user journeys
- [ ] Verify database connectivity
- [ ] Test payment integration
- [ ] Check CDN cache
- [ ] Verify SSL certificates
- [ ] Monitor error rates
- [ ] Check performance metrics
- [ ] Send deployment notification

## Troubleshooting

### Service Won't Start

```bash
# Check logs
docker-compose logs backend

# Check configuration
docker inspect maskot-backend

# Restart service
docker-compose restart backend
```

### Database Connection Issues

```bash
# Test connection
psql -h <rds_endpoint> -U admin -d maskot_unit_prod

# Check security groups
aws ec2 describe-security-groups --group-ids sg-xxx

# Verify credentials
env | grep DATABASE
```

### High Memory Usage

```bash
# Check container memory limits
docker stats maskot-backend

# Increase limits in docker-compose.yml
# Restart service
docker-compose up -d --force-recreate backend
```

---

**Last Updated:** August 30, 2026
