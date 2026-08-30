# Database Schema & Design

## Overview

This document outlines the database structure for Maskot Unit. The schema supports:
- User management (Creators, Admins, Brand Ambassadors)
- Profile management
- Social media integrations
- Payment processing
- Music promotion
- Analytics tracking

## Core Tables

### Users

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  phone VARCHAR(20),
  role ENUM('creator', 'admin', 'brand_ambassador') DEFAULT 'creator',
  status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
  email_verified BOOLEAN DEFAULT FALSE,
  email_verified_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  deleted_at TIMESTAMP
);

INDEX: email, role, status, created_at
```

### Profiles

```sql
CREATE TABLE profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  display_name VARCHAR(100) NOT NULL,
  bio TEXT,
  profile_picture_url VARCHAR(500),
  cover_image_url VARCHAR(500),
  location VARCHAR(100),
  website VARCHAR(255),
  verified BOOLEAN DEFAULT FALSE,
  rating DECIMAL(3,2) DEFAULT 0,
  total_reviews INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INDEX: user_id, display_name, verified, rating
```

### SocialMediaAccounts

```sql
CREATE TABLE social_media_accounts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  platform ENUM('instagram', 'tiktok', 'facebook', 'youtube') NOT NULL,
  account_id VARCHAR(255) NOT NULL,
  username VARCHAR(255) NOT NULL,
  access_token VARCHAR(1000),
  refresh_token VARCHAR(1000),
  followers_count INT DEFAULT 0,
  connected_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_synced_at TIMESTAMP,
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INDEX: user_id, platform, account_id
UNIQUE: user_id, platform
```

### Music

```sql
CREATE TABLE music (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  creator_id UUID NOT NULL REFERENCES users(id),
  title VARCHAR(255) NOT NULL,
  description TEXT,
  artist_name VARCHAR(255) NOT NULL,
  genre VARCHAR(100),
  duration_seconds INT,
  release_date DATE,
  file_url VARCHAR(500),
  cover_image_url VARCHAR(500),
  status ENUM('draft', 'submitted', 'approved', 'published') DEFAULT 'draft',
  plays_count INT DEFAULT 0,
  downloads_count INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INDEX: creator_id, status, release_date, genre
```

### Campaigns

```sql
CREATE TABLE campaigns (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  creator_id UUID NOT NULL REFERENCES users(id),
  name VARCHAR(255) NOT NULL,
  description TEXT,
  budget_amount DECIMAL(10,2),
  currency VARCHAR(3) DEFAULT 'KES',
  start_date DATE NOT NULL,
  end_date DATE NOT NULL,
  status ENUM('draft', 'active', 'paused', 'completed') DEFAULT 'active',
  objective VARCHAR(100),
  target_audience TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INDEX: creator_id, status, start_date, end_date
```

### Payments

```sql
CREATE TABLE payments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  amount DECIMAL(10,2) NOT NULL,
  currency VARCHAR(3) DEFAULT 'KES',
  payment_method ENUM('mpesa', 'card', 'bank_transfer') NOT NULL,
  transaction_id VARCHAR(255) UNIQUE,
  status ENUM('pending', 'completed', 'failed', 'refunded') DEFAULT 'pending',
  description VARCHAR(500),
  mpesa_response JSONB,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INDEX: user_id, status, created_at, transaction_id
```

### Analytics

```sql
CREATE TABLE analytics (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  campaign_id UUID REFERENCES campaigns(id),
  music_id UUID REFERENCES music(id),
  metric_type VARCHAR(100) NOT NULL,
  metric_value INT,
  platform VARCHAR(100),
  date DATE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INDEX: user_id, campaign_id, music_id, date, metric_type
```

## Migrations

Database migrations should be version-controlled and run in order:

```
migrations/
├── 001_create_users.sql
├── 002_create_profiles.sql
├── 003_create_social_media_accounts.sql
├── 004_create_music.sql
├── 005_create_campaigns.sql
├── 006_create_payments.sql
└── 007_create_analytics.sql
```

## Backup & Recovery

```bash
# PostgreSQL Backup
pg_dump -U username database_name > backup.sql

# MongoDB Backup
mongodump --uri=mongodb://user:pass@localhost:27017/maskot_unit

# Restore
psql -U username database_name < backup.sql
mongorestore --uri=mongodb://user:pass@localhost:27017 dump/
```

## Performance Considerations

- Indexes on frequently queried columns
- Partitioning large tables (analytics) by date
- Archiving old data periodically
- Connection pooling for database access
- Query optimization and EXPLAIN ANALYZE

---

**Last Updated:** August 30, 2026
