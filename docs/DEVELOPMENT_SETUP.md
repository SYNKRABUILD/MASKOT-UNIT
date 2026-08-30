# Development Setup Guide

## Prerequisites

- Node.js 18+ or Python 3.10+
- npm or yarn
- Git
- Database (PostgreSQL or MongoDB)
- Redis (optional, for caching)

## Backend Setup

### Node.js Backend

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Configure your .env file
# Edit .env with your database credentials, API keys, etc.

# Run database migrations
npm run migrate

# Seed database with initial data (optional)
npm run seed

# Start development server
npm run dev
```

### Python Backend

```bash
# Navigate to backend directory
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create environment file
cp .env.example .env

# Configure your .env file

# Run migrations
python manage.py migrate

# Start development server
python manage.py runserver
```

## Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Configure API endpoint in .env
# REACT_APP_API_URL=http://localhost:3000/api

# Start development server
npm run dev

# Open browser to http://localhost:3000
```

## Database Setup

### PostgreSQL

```bash
# Create database
createdb maskot_unit_dev

# Connect to database
psql maskot_unit_dev

# Or use Docker
docker run -d \
  --name postgres \
  -e POSTGRES_PASSWORD=yourpassword \
  -e POSTGRES_DB=maskot_unit_dev \
  -p 5432:5432 \
  postgres:14
```

### MongoDB

```bash
# Using Docker
docker run -d \
  --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  -p 27017:27017 \
  mongo:6
```

## Redis Setup (Optional)

```bash
# Using Docker
docker run -d \
  --name redis \
  -p 6379:6379 \
  redis:7-alpine
```

## Docker Compose (Recommended)

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

## Environment Variables

Create `.env` files in both frontend and backend directories:

### Backend (.env)

```
# Server
NODE_ENV=development
PORT=3001

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/maskot_unit_dev
# OR
MONGO_URI=mongodb://admin:password@localhost:27017/maskot_unit_dev

# JWT
JWT_SECRET=your_secret_key_here
JWT_EXPIRE=24h

# M-Pesa
MPESA_API_KEY=your_mpesa_key
MPESA_SECRET=your_mpesa_secret
MPESA_BUSINESS_CODE=your_business_code

# Email
SENDGRID_API_KEY=your_sendgrid_key

# Social Media APIs
INSTAGRAM_ACCESS_TOKEN=your_token
TIKTOK_ACCESS_TOKEN=your_token
FACEBOOK_ACCESS_TOKEN=your_token
YOUTUBE_API_KEY=your_key

# Redis
REDIS_URL=redis://localhost:6379
```

### Frontend (.env)

```
REACT_APP_API_URL=http://localhost:3001/api
REACT_APP_ENV=development
```

## Running the Application

### Terminal 1: Backend

```bash
cd backend
npm run dev
```

### Terminal 2: Frontend

```bash
cd frontend
npm run dev
```

### Terminal 3: Database & Services (if needed)

```bash
docker-compose up
```

## Verification

- Backend API: http://localhost:3001/api
- Frontend App: http://localhost:3000
- Database: localhost:5432 (PostgreSQL) or localhost:27017 (MongoDB)
- Redis: localhost:6379 (if enabled)

## Testing

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test

# End-to-end tests
npm run test:e2e
```

## Debugging

### Backend

```bash
# Enable debug logging
DEBUG=maskot:* npm run dev

# Use debugger
node --inspect=9229 index.js
# Then open chrome://inspect
```

### Frontend

```bash
# Use React DevTools browser extension
# Use Redux DevTools extension (if using Redux)
# Use browser console for debugging
```

## Troubleshooting

### Port Already in Use

```bash
# Linux/Mac
lsof -i :3001
kill -9 <PID>

# Windows
netstat -ano | findstr :3001
taskkill /PID <PID> /F
```

### Database Connection Error

- Verify database is running
- Check connection string in .env
- Verify credentials
- Check firewall rules

### Module Not Found

```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

## Next Steps

1. Review ARCHITECTURE.md for system design
2. Check API.md for endpoint documentation
3. Read CONTRIBUTING.md for development guidelines
4. Create your first feature branch and start coding!

---

**Last Updated:** August 30, 2026
