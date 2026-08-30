# Maskot Unit - Project Structure

## Directory Organization

```
MASKOT-UNIT/
│
├── backend/                    # Backend services
│   ├── src/
│   │   ├── api/               # API endpoints
│   │   ├── services/          # Business logic
│   │   ├── models/            # Data models
│   │   ├── middleware/        # Express/server middleware
│   │   ├── utils/             # Helper functions
│   │   ├── config/            # Configuration
│   │   └── index.js           # Entry point
│   ├── tests/                 # Test files
│   ├── .env.example           # Environment variables template
│   ├── package.json           # Dependencies
│   └── README.md              # Backend setup guide
│
├── frontend/                  # Frontend application
│   ├── public/                # Static files
│   ├── src/
│   │   ├── components/        # Reusable React components
│   │   ├── pages/             # Page components
│   │   ├── services/          # API client services
│   │   ├── hooks/             # Custom React hooks
│   │   ├── context/           # React context providers
│   │   ├── styles/            # Global styles
│   │   ├── utils/             # Helper functions
│   │   └── App.js             # Main app component
│   ├── tests/                 # Test files
│   ├── .env.example           # Environment variables template
│   ├── package.json           # Dependencies
│   └── README.md              # Frontend setup guide
│
├── mobile/                    # Mobile application (future)
│   ├── ios/                   # iOS specific code
│   ├── android/               # Android specific code
│   └── shared/                # Shared code
│
├── docs/                      # Documentation
│   ├── PROJECT_STRUCTURE.md   # This file
│   ├── API.md                 # API documentation
│   ├── DATABASE.md            # Database schema
│   ├── DEPLOYMENT.md          # Deployment guide
│   ├── DEVELOPMENT_SETUP.md   # Development setup
│   └── ARCHITECTURE.md        # System architecture
│
├── config/                    # Configuration files
│   ├── database.config.js     # Database configuration
│   ├── api.config.js          # API configuration
│   └── app.config.js          # App configuration
│
├── scripts/                   # Utility scripts
│   ├── setup.sh               # Initial setup script
│   ├── migrate.js             # Database migrations
│   └── seed.js                # Database seeding
│
├── .github/                   # GitHub specific files
│   ├── workflows/             # CI/CD workflows
│   │   ├── test.yml           # Test workflow
│   │   ├── deploy.yml         # Deployment workflow
│   │   └── lint.yml           # Code quality workflow
│   └── ISSUE_TEMPLATE/        # Issue templates
│
├── .gitignore                 # Git ignore rules
├── LICENSE                    # Project license
├── README.md                  # Project overview
├── CONTRIBUTING.md            # Contribution guidelines
├── package.json               # Root dependencies (if monorepo)
└── docker-compose.yml         # Docker composition
```

## Module Descriptions

### Backend (`/backend`)
Handles all server-side logic including:
- User authentication & authorization
- Creator profile management
- Social media integrations
- Payment processing (M-Pesa)
- Music promotion workflows
- Admin operations
- Analytics & reporting

**Tech Stack (To Define):**
- Runtime: Node.js / Python / Other
- Framework: Express / FastAPI / Other
- Database: PostgreSQL / MongoDB / Other
- Cache: Redis (optional)

### Frontend (`/frontend`)
Client-facing web application with:
- Creator dashboard
- Portfolio management
- Social media analytics
- Payment management
- Profile customization
- Admin interface

**Tech Stack (To Define):**
- Framework: React / Vue / Next.js / Other
- State Management: Redux / Context / Zustand / Other
- Styling: Tailwind / Material-UI / Other
- Build Tool: Webpack / Vite / Other

### Mobile (`/mobile`)
Native mobile applications (future):
- iOS app
- Android app
- Shared business logic

### Documentation (`/docs`)
- **API.md**: Complete API endpoint documentation
- **DATABASE.md**: Data models and schema
- **DEVELOPMENT_SETUP.md**: How to set up development environment
- **DEPLOYMENT.md**: Production deployment procedures
- **ARCHITECTURE.md**: System design and architecture

### Configuration (`/config`)
Centralized configuration management:
- Database connections
- API endpoints
- Environment-specific settings
- Third-party service credentials

### Scripts (`/scripts`)
Automation utilities:
- Database migrations
- Data seeding
- Setup automation
- Deployment helpers

## Development Workflow

1. **Clone Repository**
   ```bash
   git clone https://github.com/SYNKRABUILD/MASKOT-UNIT.git
   cd MASKOT-UNIT
   ```

2. **Choose Your Focus**
   - Backend: `cd backend && npm install`
   - Frontend: `cd frontend && npm install`

3. **Set Up Environment**
   - Copy `.env.example` to `.env`
   - Configure with your settings

4. **Start Development**
   - Backend: `npm run dev`
   - Frontend: `npm run dev`

5. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature
   ```

6. **Commit & Push**
   ```bash
   git add .
   git commit -m "feat: add your feature"
   git push origin feature/your-feature
   ```

7. **Create Pull Request**
   - Link related issues
   - Describe changes
   - Request reviews

## Key Directories at a Glance

| Directory | Purpose | Owner |
|-----------|---------|-------|
| `backend` | Server logic & APIs | Backend Team |
| `frontend` | Web UI & Dashboard | Frontend Team |
| `mobile` | Mobile apps | Mobile Team |
| `docs` | Project documentation | All |
| `config` | Configuration management | DevOps/Tech Lead |
| `scripts` | Automation tools | DevOps |

## Adding New Features

When adding new features, follow this structure:

**Backend Feature:**
```
backend/src/
├── models/MyModel.js
├── services/myService.js
├── api/myRoutes.js
└── tests/myService.test.js
```

**Frontend Feature:**
```
frontend/src/
├── components/MyComponent.js
├── pages/MyPage.js
├── services/myService.js
└── tests/MyComponent.test.js
```

## Naming Conventions

- **Folders:** lowercase, hyphen-separated (e.g., `my-folder`)
- **Files:** camelCase or PascalCase based on content
- **Variables:** camelCase
- **Constants:** UPPER_SNAKE_CASE
- **Components:** PascalCase
- **Functions:** camelCase

## Dependencies

Keep dependencies organized:
- **Core:** Framework, build tools
- **API:** HTTP clients, validators
- **Database:** ORMs, query builders
- **Auth:** JWT, OAuth libraries
- **Payments:** M-Pesa SDK
- **Testing:** Jest, Mocha, etc.
- **Linting:** ESLint, Prettier

## Environment Files

Never commit `.env` files. Use `.env.example`:
```
# .env.example
DATABASE_URL=your_database_url
API_KEY=your_api_key
MPESA_API_KEY=your_mpesa_key
```

---

**Last Updated:** August 30, 2026
