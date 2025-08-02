# 🚀 DevOps & Deployment - Getting Code to Production

## 🎯 What Is DevOps?

DevOps = Development + Operations

Think of it like a restaurant:
- **Development** = Chefs creating recipes
- **Operations** = Running the restaurant
- **DevOps** = Making sure recipes work in the real restaurant

DevOps makes sure code works everywhere, not just on developer's laptop.

## 🔄 CI/CD Pipeline

### What Is CI/CD?

**Continuous Integration (CI)**:
- Automatically test code when it's pushed
- Catch problems early
- Merge code frequently

**Continuous Deployment (CD)**:
- Automatically deploy working code
- Fast, reliable releases
- Reduce manual errors

### CI/CD Flow

```
Developer → Git Push → CI Tests → CD Deploy → Production
```

## 📋 GitHub Actions - CI/CD for GitHub

### What Are GitHub Actions?

GitHub Actions = Automated workflows for your repository

When you push code, GitHub can:
- Run tests
- Build the application
- Deploy to production
- Send notifications

### Basic Workflow

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Run linting
      run: npm run lint
    
    - name: Build application
      run: npm run build

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to production
      run: |
        echo "Deploying to production..."
        # Your deployment commands here
```

### Real-World Example: Deploy to Netlify

```yaml
name: Deploy to Netlify

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm install
    
    - name: Run tests
      run: npm test
    
    - name: Build project
      run: npm run build
    
    - name: Deploy to Netlify
      uses: nwtgck/actions-netlify@v2.0
      with:
        publish-dir: './build'
        production-branch: main
        github-token: ${{ secrets.GITHUB_TOKEN }}
        deploy-message: "Deploy from GitHub Actions"
      env:
        NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
        NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
```

## 🌐 Deployment Platforms

### 1. Netlify - Static Sites

```bash
# Manual deployment
npm run build
npm install -g netlify-cli
netlify deploy --prod --dir=build

# Automatic deployment
# 1. Connect GitHub repo to Netlify
# 2. Set build command: npm run build
# 3. Set publish directory: build
# 4. Every push auto-deploys
```

### 2. Vercel - Next.js & React

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Or connect GitHub repository
# Automatic deployment on every push
```

#### Vercel Configuration

```json
// vercel.json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "env": {
    "NODE_ENV": "production"
  },
  "build": {
    "env": {
      "NEXT_PUBLIC_API_URL": "https://api.myapp.com"
    }
  }
}
```

### 3. Railway - Full-Stack Apps

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login and deploy
railway login
railway deploy

# Or connect GitHub
# Automatic deployment with database support
```

#### Railway Configuration

```json
// railway.json
{
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "npm start",
    "healthcheckPath": "/health",
    "healthcheckTimeout": 100,
    "restartPolicyType": "ON_FAILURE"
  }
}
```

### 4. Heroku - Traditional PaaS

```bash
# Install Heroku CLI
# Create Procfile
echo "web: node server.js" > Procfile

# Deploy
git add .
git commit -m "Deploy to Heroku"
heroku create my-app-name
git push heroku main
```

## 🐳 Docker - Containerization

### What Is Docker?

Docker = Shipping containers for code

Like how shipping containers:
- Work on any ship
- Contain everything needed
- Same everywhere

Docker containers:
- Work on any server
- Contain app + dependencies
- Same in dev and production

### Dockerfile Example

```dockerfile
# Dockerfile
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Build application
RUN npm run build

# Expose port
EXPOSE 3000

# Define the command to run the app
CMD ["npm", "start"]
```

### Docker Commands

```bash
# Build image
docker build -t my-app .

# Run container
docker run -p 3000:3000 my-app

# List running containers
docker ps

# Stop container
docker stop <container-id>

# Remove container
docker rm <container-id>
```

### Docker Compose - Multiple Services

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:password@db:5432/myapp
    depends_on:
      - db
      - redis

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

```bash
# Start all services
docker-compose up

# Start in background
docker-compose up -d

# Stop all services
docker-compose down
```

## ☁️ Cloud Deployment

### AWS Deployment Options

#### 1. Elastic Beanstalk (Easy)

```bash
# Install EB CLI
pip install awsebcli

# Initialize
eb init

# Create environment
eb create production

# Deploy
eb deploy
```

#### 2. ECS (Containers)

```yaml
# ecs-task-definition.json
{
  "family": "my-app",
  "taskRoleArn": "arn:aws:iam::123456789:role/ecsTaskRole",
  "executionRoleArn": "arn:aws:iam::123456789:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [
    {
      "name": "my-app",
      "image": "my-app:latest",
      "portMappings": [
        {
          "containerPort": 3000,
          "protocol": "tcp"
        }
      ],
      "essential": true,
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/my-app",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ]
}
```

## 🏢 **DEPLOYMENT ENVIRONMENTS - Critical for BA Understanding**

### 🎯 **What Are Deployment Environments?**

Deployment environments are **separate copies of your application** used for different purposes in the development and release process. Think of them as **different stages in a theater production**.

**Theater Analogy:**
- **Rehearsal Studio** (Development) - Actors practice scenes, make mistakes freely
- **Dress Rehearsal** (Staging) - Full production run with costumes, lights, etc.
- **Opening Night** (Production) - Real audience, everything must work perfectly

**Software Environments:**
- **Development** - Developers write and test code
- **Staging/Pre-Production** - Final testing before release
- **Production** - Real users, real data, real business impact

### 🚀 **Complete Environment Pipeline**

```markdown
Developer Laptop → Development → Staging → Pre-Production → Production
     ↓                ↓           ↓            ↓              ↓
   Local testing    Integration  User Testing  Final Check   Live Users
```

### 📋 **Environment Details for Business Analysts**

#### 1. **Development Environment**
```markdown
**Purpose:** Where developers write and test code
**Who Uses:** Development team
**Data:** Fake/sample data, test accounts
**Uptime:** Not critical, can be down during development
**Updates:** Multiple times per day
**Business Impact:** None (internal only)

**What BAs Need to Know:**
- Used for initial feature development
- Bugs here are expected and normal
- Not suitable for stakeholder demos
- Fastest environment to get changes
```

#### 2. **Staging Environment** 
```markdown
**Purpose:** Test complete features before production
**Who Uses:** Developers, QA, Product Managers, BAs
**Data:** Production-like data (anonymized)
**Uptime:** High availability during business hours
**Updates:** Daily or after major features
**Business Impact:** Affects testing schedule

**What BAs Need to Know:**
- Best environment for stakeholder demos
- Use for user acceptance testing (UAT)
- Data resets periodically (don't expect persistence)
- Should mirror production as closely as possible
- Great for training and documentation
```

#### 3. **Pre-Production Environment**
```markdown
**Purpose:** Final validation before production release
**Who Uses:** QA, DevOps, Senior developers
**Data:** Recent production data copy (sanitized)
**Uptime:** Very high (99%+)
**Updates:** Only for production-ready code
**Business Impact:** Delays affect release schedules

**What BAs Need to Know:**
- Final checkpoint before going live
- Used for performance and security testing
- Exact replica of production infrastructure
- No experimental features allowed
- Critical for compliance and audit requirements
```

#### 4. **Production Environment**
```markdown
**Purpose:** Serve real customers and business operations
**Who Uses:** Real users, customers, business operations
**Data:** Live business data, real transactions
**Uptime:** Maximum (99.9%+ required)
**Updates:** Scheduled releases only
**Business Impact:** Direct impact on revenue and customers

**What BAs Need to Know:**
- Any issues here affect real customers
- Changes require extensive approval process
- Monitoring and alerts are critical
- Backup and disaster recovery essential
- Compliance and security are paramount
```

### 🔄 **Environment Promotion Process**

```markdown
**Code Promotion Flow:**
1. Developer writes code → Development Environment
2. Code reviewed and tested → Staging Environment
3. Stakeholder approval → Pre-Production Environment
4. Final validation → Production Environment

**Data Flow:**
Production Data → Sanitized Copy → Pre-Production
Production Data → Anonymized Copy → Staging
Fake/Test Data → Development
```

### 🎯 **Environment Configuration Examples**

#### Development Environment
```javascript
// config/development.js
module.exports = {
  database: {
    host: 'localhost',
    port: 5432,
    name: 'myapp_dev',
    username: 'dev_user',
    password: 'dev_password'
  },
  api: {
    baseUrl: 'http://localhost:3000',
    timeout: 5000
  },
  features: {
    enableLogging: true,
    enableDebugMode: true,
    enableExperimentalFeatures: true
  },
  payments: {
    provider: 'stripe',
    mode: 'test', // Using test keys
    webhookSecret: 'test_webhook_secret'
  }
};
```

#### Staging Environment
```javascript
// config/staging.js
module.exports = {
  database: {
    host: 'staging-db.company.com',
    port: 5432,
    name: 'myapp_staging',
    username: 'staging_user',
    password: process.env.STAGING_DB_PASSWORD
  },
  api: {
    baseUrl: 'https://staging-api.company.com',
    timeout: 10000
  },
  features: {
    enableLogging: true,
    enableDebugMode: false,
    enableExperimentalFeatures: true
  },
  payments: {
    provider: 'stripe',
    mode: 'test', // Still test mode
    webhookSecret: process.env.STAGING_WEBHOOK_SECRET
  }
};
```

#### Production Environment
```javascript
// config/production.js
module.exports = {
  database: {
    host: 'prod-db.company.com',
    port: 5432,
    name: 'myapp_production',
    username: 'prod_user',
    password: process.env.PROD_DB_PASSWORD
  },
  api: {
    baseUrl: 'https://api.company.com',
    timeout: 15000
  },
  features: {
    enableLogging: false, // Only errors
    enableDebugMode: false,
    enableExperimentalFeatures: false
  },
  payments: {
    provider: 'stripe',
    mode: 'live', // Real payments!
    webhookSecret: process.env.PROD_WEBHOOK_SECRET
  }
};
```

### 📊 **Environment Comparison for BAs**

| Aspect | Development | Staging | Pre-Production | Production |
|--------|-------------|---------|----------------|------------|
| **Purpose** | Code development | Feature testing | Final validation | Live operations |
| **Data** | Fake/Test | Production-like | Production copy | Real business |
| **Updates** | Hourly | Daily | Weekly | Scheduled |
| **Bugs** | Expected | Some expected | Should be none | Critical issue |
| **Downtime** | Acceptable | Limited | Minimal | Never acceptable |
| **Access** | Developers | Team + stakeholders | Limited team | Customers |
| **Monitoring** | Basic | Moderate | Extensive | Maximum |

### 🚨 **Environment Responsibilities for Different Roles**

#### **Business Analysts Should:**
```markdown
✅ Use Staging for stakeholder demos
✅ Test user stories in Staging environment
✅ Understand data refresh schedules
✅ Know promotion timelines and requirements
✅ Report environment-specific issues clearly
✅ Plan UAT in appropriate environment

❌ Don't use Development for stakeholder demos
❌ Don't expect data persistence in test environments
❌ Don't request Production access for testing
❌ Don't skip environment validation steps
```

#### **Developers Should:**
```markdown
✅ Test thoroughly in Development before promotion
✅ Ensure staging deployment works before production
✅ Monitor application performance in all environments
✅ Fix environment-specific configuration issues
```

#### **QA Should:**
```markdown
✅ Test in Staging environment primarily
✅ Validate in Pre-Production before release approval
✅ Report environment-specific bugs clearly
✅ Maintain test data and test scenarios
```

### 🔄 **Environment Deployment Workflow**

#### **Typical Release Process:**
```markdown
Week 1: Development
- Features developed and unit tested
- Code merged to development branch
- Deployed to Development environment

Week 2: Staging
- Code promoted to staging branch
- Deployed to Staging environment
- QA testing and stakeholder review
- User acceptance testing (UAT)

Week 3: Pre-Production
- Code promoted to pre-production
- Performance and security testing
- Final business validation
- Production deployment rehearsal

Week 4: Production
- Scheduled production deployment
- Monitoring and validation
- User communication and support
- Post-deployment review
```

### 🎯 **Environment Issues and Solutions**

#### **Common Environment Problems:**
```markdown
Problem: "It works on my machine but not in staging"
Solution: Environment configuration differences
Action: Standardize configurations across environments

Problem: "Data is missing in staging"
Solution: Data refresh process needed
Action: Schedule regular data updates from production

Problem: "Feature works in staging but fails in production"
Solution: Infrastructure or scaling differences
Action: Make pre-production identical to production

Problem: "Can't reproduce bug from production in staging"
Solution: Data or traffic volume differences
Action: Improve staging data and load testing
```

### 📈 **Environment Monitoring and Metrics**

#### **Development Environment:**
```markdown
Metrics to Track:
- Build success rate
- Test coverage
- Deployment frequency
- Developer productivity

Monitoring:
- Basic uptime monitoring
- Build pipeline status
- Simple error logging
```

#### **Staging Environment:**
```markdown
Metrics to Track:
- Feature completion rate
- Bug detection rate
- Stakeholder approval rate
- UAT success rate

Monitoring:
- Application performance
- Database performance
- Integration test results
- User behavior analytics
```

#### **Production Environment:**
```markdown
Metrics to Track:
- System uptime (99.9%+ target)
- Response times (<200ms target)
- Error rates (<0.1% target)
- Business KPIs (conversion, revenue)

Monitoring:
- 24/7 system monitoring
- Real-time alerts
- Business impact tracking
- Security monitoring
```

### 🔧 Environment Management

### Environment Variables

```bash
# .env (development)
NODE_ENV=development
DATABASE_URL=postgresql://localhost:5432/myapp_dev
API_KEY=dev_key_123
JWT_SECRET=dev_secret

# .env.production (production)
NODE_ENV=production
DATABASE_URL=postgresql://prod-server:5432/myapp_prod
API_KEY=prod_key_456
JWT_SECRET=super_secure_secret
```

### Loading Environment Variables

```javascript
// config/env.js
require('dotenv').config();

const config = {
  development: {
    port: process.env.PORT || 3000,
    database: {
      url: process.env.DATABASE_URL || 'postgresql://localhost:5432/myapp_dev'
    },
    jwt: {
      secret: process.env.JWT_SECRET || 'dev_secret'
    }
  },
  
  production: {
    port: process.env.PORT || 80,
    database: {
      url: process.env.DATABASE_URL
    },
    jwt: {
      secret: process.env.JWT_SECRET
    }
  }
};

module.exports = config[process.env.NODE_ENV || 'development'];
```

## 📊 Monitoring & Logging

### Basic Health Check

```javascript
// health.js
const express = require('express');
const app = express();

// Health check endpoint
app.get('/health', (req, res) => {
  res.status(200).json({
    status: 'healthy',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    memory: process.memoryUsage()
  });
});

// Readiness check
app.get('/ready', async (req, res) => {
  try {
    // Check database connection
    await database.ping();
    
    // Check external services
    await redis.ping();
    
    res.status(200).json({ status: 'ready' });
  } catch (error) {
    res.status(503).json({ 
      status: 'not ready',
      error: error.message 
    });
  }
});
```

### Structured Logging

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  defaultMeta: { service: 'my-app' },
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' }),
    new winston.transports.Console({
      format: winston.format.simple()
    })
  ]
});

// Usage
logger.info('User logged in', { userId: 123, email: 'user@example.com' });
logger.error('Database connection failed', { error: error.message });

// In Express middleware
app.use((req, res, next) => {
  logger.info('Request received', {
    method: req.method,
    url: req.url,
    userAgent: req.get('User-Agent')
  });
  next();
});
```

## 🔐 Security Best Practices

### 1. Secrets Management

```bash
# Don't commit secrets!
# Use environment variables or secret management services

# AWS Secrets Manager
aws secretsmanager get-secret-value --secret-id prod/myapp/database

# HashiCorp Vault
vault kv get secret/myapp/database
```

### 2. HTTPS & SSL

```javascript
// Redirect HTTP to HTTPS
app.use((req, res, next) => {
  if (req.header('x-forwarded-proto') !== 'https') {
    res.redirect(`https://${req.header('host')}${req.url}`);
  } else {
    next();
  }
});

// Security headers
const helmet = require('helmet');
app.use(helmet());
```

### 3. Rate Limiting

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP'
});

app.use('/api/', limiter);
```

## 🚦 Deployment Strategies

### 1. Blue-Green Deployment

```
Blue Environment (Current)    Green Environment (New)
├── Load Balancer ──────────► ├── Application v1.0
│                             ├── Database
│                             └── Services
│
└── Switch traffic ─────────► ├── Application v2.0
                              ├── Database
                              └── Services
```

### 2. Rolling Deployment

```
Server 1: v1.0 → v2.0 ✓
Server 2: v1.0 → v2.0 ✓
Server 3: v1.0 → v2.0 ✓
```

### 3. Canary Deployment

```
95% Traffic → Old Version
 5% Traffic → New Version

Monitor metrics → If good, increase to 100%
```

## 📈 Performance Monitoring

### Application Performance Monitoring (APM)

```javascript
// Using New Relic
require('newrelic');

// Using Datadog
const tracer = require('dd-trace').init();

// Custom metrics
const StatsD = require('node-statsd');
const client = new StatsD();

// Track API response times
app.use((req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = Date.now() - start;
    client.timing('api.response_time', duration, [`endpoint:${req.path}`]);
  });
  
  next();
});
```

### Database Monitoring

```javascript
// Monitor slow queries
const mongoose = require('mongoose');

mongoose.set('debug', (collection, method, query, doc) => {
  logger.debug('Mongoose query', {
    collection,
    method,
    query,
    executionTime: Date.now() - query._startTime
  });
});
```

## 🔄 Rollback Strategies

### Quick Rollback

```bash
# Git-based rollback
git revert HEAD
git push origin main

# Container rollback
docker tag my-app:previous my-app:latest
docker push my-app:latest

# Database migration rollback
npm run migrate:rollback
```

### Feature Flags

```javascript
// Feature flag system
const featureFlags = {
  newCheckoutFlow: process.env.FEATURE_NEW_CHECKOUT === 'true',
  betaFeatures: process.env.FEATURE_BETA === 'true'
};

// Use in code
if (featureFlags.newCheckoutFlow) {
  return newCheckoutProcess();
} else {
  return oldCheckoutProcess();
}

// External service (LaunchDarkly, Split.io)
const client = LaunchDarkly.init('your-sdk-key');

app.get('/api/feature', async (req, res) => {
  const showFeature = await client.variation('new-feature', user, false);
  res.json({ enabled: showFeature });
});
```

## 🏗️ Infrastructure as Code

### Terraform Example

```hcl
# main.tf
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
  
  tags = {
    Name = "MyApp-Production"
    Environment = "production"
  }
}

resource "aws_security_group" "web" {
  name = "web-sg"
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

```bash
# Deploy infrastructure
terraform init
terraform plan
terraform apply
```

## 🎯 Complete Deployment Checklist

### Pre-Deployment

- [ ] Code reviewed and approved
- [ ] Tests passing (unit, integration, e2e)
- [ ] Security scan completed
- [ ] Performance benchmarks met
- [ ] Documentation updated
- [ ] Environment variables configured
- [ ] Database migrations ready
- [ ] Rollback plan prepared

### Deployment

- [ ] Backup current version
- [ ] Deploy to staging first
- [ ] Run smoke tests
- [ ] Deploy to production
- [ ] Verify health checks
- [ ] Monitor error rates
- [ ] Check performance metrics

### Post-Deployment

- [ ] Verify all features working
- [ ] Check logs for errors
- [ ] Monitor user feedback
- [ ] Performance within acceptable limits
- [ ] Update monitoring dashboards
- [ ] Document any issues

## 🎯 What This Means for BAs

Understanding DevOps helps you:

1. **Release Planning** - Know deployment complexity and timelines
2. **Environment Strategy** - Understand dev/staging/prod workflows
3. **Risk Assessment** - Appreciate rollback and monitoring needs
4. **Quality Assurance** - See how automated testing fits in
5. **Performance Expectations** - Understand monitoring and scaling

## ✅ Key Takeaways

- DevOps automates the path from code to production
- CI/CD pipelines catch problems early
- Multiple deployment platforms serve different needs
- Docker provides consistent environments
- Monitoring and logging are essential
- Security must be built into the process
- Have a rollback plan before deploying
- Infrastructure as Code makes deployments repeatable

---

## 🎉 Course Complete!

Congratulations! You've completed the Software Development Lifecycle course. You now understand:

- How developers manage code (Git)
- The tools they use daily (VS Code, ESLint)
- How websites are built (HTML, CSS, JavaScript)
- Modern frameworks (React, Next.js)
- Backend development (Node.js, APIs)
- Databases (SQL, NoSQL)
- Cloud services (AWS)
- AI integration
- UI libraries and styling
- Mobile development
- How code gets to production (DevOps)

You're now equipped to communicate effectively with development teams and understand the technical side of software projects!