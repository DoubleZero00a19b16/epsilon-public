# Backend System Documentation

<a href="https://github.com/DoubleZero00a19b16">
  <img src="https://github.com/DoubleZero00a19b16.png" width="60px" style="border-radius:50%;" alt="DoubleZero00a19b16's Profile"/>
</a>

**Contributor:** [DoubleZero00a19b16](https://github.com/DoubleZero00a19b16)

- **Development Team**: Epsilon Backend Development
- **Role**: Backend Architecture, API Development, Database Design, AI Integration

---

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [Architecture](#architecture)
4. [Core Features](#core-features)
5. [Module Structure](#module-structure)
6. [Database](#database)
7. [Authentication & Security](#authentication--security)
8. [Deployment](#deployment)
9. [API Documentation](#api-documentation)
10. [Development Guide](#development-guide)

---

## Project Overview

**Bonus Card Integrated Product Rating System** is a comprehensive backend system designed to manage product ratings, customer interactions, and reward mechanisms for a bonus card ecosystem. The system integrates real-time product classification using machine learning with traditional REST API services.

**Key Objectives:**
- Provide robust API endpoints for product ratings and reviews
- Enable intelligent product categorization using AI/ML
- Manage customer loyalty and rewards programs
- Track market trends and product performance
- Support multiple user roles and permissions

---

## Technology Stack

### 🎯 Backend Framework & Runtime
- **Node.js** (v20 LTS) - JavaScript runtime for server execution
- **NestJS** (v10.0.0) - Progressive Node.js framework for building scalable server-side applications

### 📦 Core Dependencies

#### ORM & Database
- **TypeORM** (v0.3.17) - Object-Relational Mapper for type-safe database queries
- **MySQL2** (v3.6.5) - MySQL database driver for Node.js
- **MySQL** (v8.0+) - Primary database for persistent data storage

#### API & HTTP
- **@nestjs/axios** (v4.0.1) - HTTP client module for NestJS
- **@nestjs/platform-express** (v10.0.0) - Express adapter for NestJS
- **Swagger/OpenAPI** (@nestjs/swagger v7.1.16) - API documentation and interactive UI

#### Authentication & Authorization
- **@nestjs/jwt** (v11.0.2) - JWT token generation and validation
- **bcrypt** (v5.1.1) - Password hashing and encryption
- **nestjs-cls** (v6.1.0) - Continuation Local Storage for request context

#### Data Validation & Transformation
- **class-validator** (v0.14.0) - Declarative validation decorators
- **class-transformer** (v0.5.1) - Class transformation and serialization
- **Pydantic** - Data validation for Python services

#### Utilities
- **RxJS** (v7.8.1) - Reactive programming library
- **@nestjs/config** (v3.1.1) - Environment variable management
- **reflect-metadata** (v0.1.13) - Reflection and type metadata support

### 🤖 AI/ML Stack (Python Microservice)

#### FastAPI Framework
- **FastAPI** (v0.127.0) - Modern, fast web framework for building Python APIs
- **Uvicorn** (v0.40.0) - ASGI server for running FastAPI

#### Machine Learning Libraries
- **PyTorch** - Deep learning framework for neural networks
- **scikit-learn** (v1.5.0) - Traditional ML algorithms and utilities
- **sentence-transformers** (v3.0.0) - Semantic text embeddings using transformer models
- **HDBSCAN** (v0.8.33) - Hierarchical clustering algorithm for product categorization
- **NumPy** (v2.0.0) - Numerical computing library
- **Pandas** (v2.2.0) - Data manipulation and analysis

#### Data Validation
- **Pydantic** (v2.12.5) - Data validation and settings management

### 🛠️ Development Tools

#### Build & Compilation
- **TypeScript** (v5.1.3) - Typed JavaScript for type safety
- **ts-loader** (v9.4.3) - TypeScript loader for webpack
- **ts-node** (v10.9.1) - TypeScript execution for Node.js
- **@nestjs/cli** (v10.0.0) - NestJS command-line interface

---

## Architecture

### System Design Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     CLIENT APPLICATIONS                      │
│                  (Web, Mobile, Third-party)                 │
└──────────────────────────────┬──────────────────────────────┘
                               │
                ┌──────────────┴──────────────┐
                │                             │
    ┌───────────▼────────────┐   ┌──────────▼──────────┐
    │   NestJS API Server    │   │  FastAPI ML Service │
    │   (Port 3000/3005)     │   │   (Port 8000)       │
    │                        │   │                     │
    │ ┌────────────────────┐ │   │ ┌─────────────────┐ │
    │ │ Auth Module        │ │   │ │ ML Classification│ │
    │ │ Users Module       │ │   │ │ - Clustering    │ │
    │ │ Products Module    │ │   │ │ - Categorization│ │
    │ │ Ratings Module     │ │   │ │ - Embeddings    │ │
    │ │ Recommendations    │ │   │ └─────────────────┘ │
    │ │ Markets Module     │ │   │                     │
    │ │ Orders Module      │ │   │ Models:             │
    │ │ AI Classification  │ │   │ - Sentence-Trans   │
    │ └────────────────────┘ │   │ - HDBSCAN          │
    │                        │   │ - scikit-learn     │
    └────────────┬───────────┘   └──────────┬──────────┘
                 │                          │
                 └──────────────┬───────────┘
                                │
                    ┌───────────▼──────────┐
                    │   MySQL Database     │
                    │   (Port 3306)        │
                    │                      │
                    │ - Users              │
                    │ - Products           │
                    │ - Ratings            │
                    │ - Orders             │
                    │ - Markets            │
                    │ - Bonus/Rewards      │
                    └──────────────────────┘
```

### Deployment Architecture

**Docker Containerization:**
- Multi-stage Dockerfile for optimized production builds
- Node.js 20-slim base image for minimal footprint
- Docker Compose orchestration for local development
- Separate services for NestJS API and FastAPI ML

**Environment-based Configuration:**
- Development mode with hot-reloading
- Production mode with compiled code optimization
- Environment variables for database, JWT, and AI service configuration

---

## Core Features

### 1. **Authentication & Authorization**
- JWT-based authentication with secure token generation
- Password hashing using bcrypt
- Role-based access control (RBAC)
- Token expiration management (configurable)

### 2. **Product Rating & Review System**
- User-submitted product ratings and reviews
- Rating aggregation and statistics
- Product categorization by type (Private Label vs. Standard)
- Rating history tracking

### 3. **Intelligent Product Classification**
- AI-powered product categorization using sentence transformers
- Semantic clustering using HDBSCAN algorithm
- Automatic product grouping based on similarity
- Real-time classification requests

### 4. **Bonus & Reward System**
- Purchase cashback calculation (configurable percentage)
- Rating-based reward points
- Different point values for different product types
- Points-to-dollar conversion mechanism
- Reward pool management

### 5. **Order Management**
- Order creation and tracking
- Order status management
- Integration with bonus calculation
- Customer purchase history

### 6. **Market Analytics**
- Market trend tracking
- Product performance metrics
- Customer behavior analysis
- Reward distribution insights

### 7. **Recommendation Engine**
- Personalized product recommendations
- User preference-based suggestions
- Collaborative filtering potential
- Rating history integration

---

## Module Structure

### **Auth Module**
- Handles user authentication and authorization
- JWT token management
- Login/logout functionality
- Permission validation

### **Users Module**
- User profile management
- User information CRUD operations
- Account settings and preferences
- User role management

### **Products Module**
- Product catalog management
- Product details and attributes
- Product categorization
- Inventory tracking

### **Ratings Module**
- Rating creation and retrieval
- Review management
- Rating aggregation
- Quality metrics calculation

### **Recommendations Module**
- Recommendation algorithm implementation
- User preference analysis
- Personalized suggestions
- Ranking and sorting

### **Orders Module**
- Order processing
- Order history
- Order status tracking
- Payment integration support

### **Markets Module**
- Market information management
- Multi-market support
- Regional data tracking
- Market-specific configurations

### **AI Classification Module**
- Integration with FastAPI ML service
- Real-time product classification
- Classification result caching
- Error handling and fallbacks

---

## Database

### Database Technology
- **MySQL 8.0+** - Relational database management system
- **TypeORM** - Object-Relational Mapping for type-safe queries

### Key Tables/Entities

#### Core Entities
- **Users** - Customer and admin accounts
- **Products** - Product catalog
- **Ratings** - User ratings and reviews
- **Orders** - Customer orders
- **Markets** - Market/region information

#### Support Entities
- **Recommendations** - Personalized product recommendations
- **OrderItems** - Line items in orders
- **Bonuses** - Bonus/cashback records
- **RewardPoints** - User reward point balance
- **Classifications** - AI classification results

### Migrations
- Automated schema management using TypeORM migrations
- Version control for database changes
- Rollback capability for database modifications

### Seeding
- Data seeding for development/testing
- Sample data for demo purposes
- Database initialization scripts

---

## Authentication & Security

### Security Features

1. **Password Security**
   - Bcrypt hashing with salt rounds
   - Secure password storage
   - Password validation rules

2. **Token Security**
   - JWT (JSON Web Token) implementation
   - Token signing with secret key
   - Token expiration (default: 7 days)
   - Bearer token authentication

3. **Input Validation**
   - Class-validator for request validation
   - Whitelist/blacklist enforcement
   - Type transformation and sanitization
   - Custom validation decorators

4. **CORS Configuration**
   - Configurable allowed origins
   - HTTP methods filtering
   - Allowed headers management
   - Credential support

5. **Environment Security**
   - Environment variable management
   - Sensitive data separation from code
   - .env file for local development
   - Secret key rotation support

---

## Deployment

### Docker Deployment

**Build Process:**
```dockerfile
# Stage 1: Build
- Install dependencies
- Compile TypeScript to JavaScript
- Create dist directory

# Stage 2: Production
- Copy compiled code
- Install production dependencies only
- Optimize image size
- Expose port 3000
```

### Docker Compose

**Services:**
- NestJS API (port 3000/3005)
- FastAPI ML (port 8000)
- MySQL Database (port 3306)

**Quick Start:**
```bash
docker-compose up -d
```

### Environment Configuration

**Key Variables:**
- `PORT` - Application port (default: 3005)
- `NODE_ENV` - Execution environment (development/production)
- `DB_HOST` - Database hostname
- `DB_PORT` - Database port (default: 3306)
- `DB_USERNAME` - Database user
- `DB_PASSWORD` - Database password
- `DB_DATABASE` - Database name
- `JWT_SECRET` - JWT signing secret
- `JWT_EXPIRATION` - Token lifetime in seconds
- `FASTAPI_URL` - ML service URL
- `PURCHASE_CASHBACK_RATE` - Cashback percentage
- `PL_PRODUCT_REWARD_POINTS` - Private Label reward points
- `NORMAL_PRODUCT_REWARD_POINTS` - Standard product reward points
- `POINTS_TO_DOLLAR_RATE` - Points conversion rate

---

## API Documentation

### Swagger/OpenAPI Integration

**Access API Documentation:**
- URL: `http://localhost:3000/api/docs`
- Interactive API explorer
- Request/response examples
- JWT authentication testing
- Schema visualization

### Base API Path
- All endpoints prefixed with `/api/v1`

### API Endpoints Overview

#### Authentication
- `POST /api/v1/auth/login` - User login
- `POST /api/v1/auth/logout` - User logout
- `POST /api/v1/auth/register` - User registration

#### Users
- `GET /api/v1/users` - List all users
- `GET /api/v1/users/:id` - Get user details
- `PUT /api/v1/users/:id` - Update user profile
- `DELETE /api/v1/users/:id` - Delete user

#### Products
- `GET /api/v1/products` - List products
- `GET /api/v1/products/:id` - Get product details
- `POST /api/v1/products` - Create product
- `PUT /api/v1/products/:id` - Update product
- `DELETE /api/v1/products/:id` - Delete product

#### Ratings
- `GET /api/v1/ratings` - List ratings
- `POST /api/v1/ratings` - Create rating
- `GET /api/v1/ratings/:id` - Get rating details
- `PUT /api/v1/ratings/:id` - Update rating
- `DELETE /api/v1/ratings/:id` - Delete rating

#### Orders
- `GET /api/v1/orders` - List orders
- `POST /api/v1/orders` - Create order
- `GET /api/v1/orders/:id` - Get order details
- `PUT /api/v1/orders/:id` - Update order status

#### Recommendations
- `GET /api/v1/recommendations` - Get personalized recommendations
- `POST /api/v1/recommendations` - Generate recommendations

#### Markets
- `GET /api/v1/markets` - List markets
- `GET /api/v1/markets/:id` - Get market details

#### AI Classification
- `POST /api/v1/ai-classification/classify` - Classify product
- `POST /api/v1/ai-classification/batch-classify` - Batch classification
- `GET /api/v1/ai-classification/cache/:productId` - Get cached classification

---

## Development Guide

### Prerequisites
- **Node.js** v20+ and npm
- **MySQL** v8.0+
- **Python** 3.10+ (for ML service)
- **Git**

### Docker Development

**Build Image:**
```bash
docker build -t epsilon-api:dev -f Dockerfile .
```

**Run Container:**
```bash
docker run -p 3000:3000 --env-file .env epsilon-api:dev
```

**Using Docker Compose:**
```bash
docker-compose up -d
```

---

## Performance Optimization

### Backend Optimizations
- Connection pooling for database queries
- JWT caching for token validation
- Response compression for API payloads
- Async/await for non-blocking operations
- TypeORM query optimization

### Database Optimizations
- Indexed columns for frequently queried fields
- Proper foreign key relationships
- Pagination for large result sets
- Query result caching

### ML Service Optimizations
- Batch classification for efficiency
- Model caching in memory
- Async processing for long-running tasks
- GPU support ready (PyTorch)

---

## Scalability Considerations

### Horizontal Scaling
- Stateless API design for load balancing
- Database connection pooling
- Docker containerization for orchestration
- Session management via JWT (no server state)

### Vertical Scaling
- Async processing for heavy operations
- Connection pooling optimization
- Memory-efficient data structures
- Query optimization and indexing

### Future Enhancements
- Redis caching layer
- Message queue for async tasks
- Microservices architecture
- API rate limiting
- Advanced monitoring and logging

---

## Support & Contact

For questions, issues, or contributions, please contact the development team or refer to the project documentation.

---
