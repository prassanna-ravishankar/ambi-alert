# Torale Microservices Architecture with Supabase

## Overview

This document outlines the microservices architecture for Torale, integrating Supabase as the unified authentication and database solution. The architecture breaks down the monolithic backend into logical, scalable services while maintaining data consistency and security.

## Architecture Diagram

```
┌─────────────────┐     ┌─────────────────────────────────┐
│   Next.js App   │────▶│        Main Backend             │
│  (Supabase SDK) │     │        (FastAPI)                │
└─────────────────┘     │  ┌─────────────────────────────┐│
                        │  │ • User Queries              ││
                        │  │ • Content Processing        ││
                        │  │ • Change Detection          ││
              ┌─────────┼──┤ • Notifications (Integrated)││
              │         │  │ • Monitoring & Alerts       ││
              │         │  └─────────────────────────────┘│
              │         └─────────────┬───────────────────┘
              │                       │
        ┌─────▼─────┐                 │
        │ Discovery │                 │
        │ Service   │                 │
        │ :8001     │                 │
        └─────┬─────┘                 │
              │                       │
              └───────────┬───────────┘
                          │
                   ┌──────▼──────┐
                   │  Supabase   │
                   │  - Auth     │
                   │  - Database │
                   │  - Realtime │
                   │  - Functions│
                   │  - RLS      │
                   └─────────────┘
```

## Service Breakdown

### 1. Main Backend Service (FastAPI)
**Repository**: `backend/` 
**Responsibilities**:
- Authentication & authorization via Supabase JWT
- User query management
- Content processing and ingestion
- Change detection and alerts
- **Integrated notification system** with email delivery
- Background task processing
- Monitored source CRUD operations

**Key Changes**:
- ✅ Supabase JWT verification middleware
- ✅ User-scoped data access with RLS
- ✅ UUID-based models
- ✅ Async database operations

**Database Tables Owned**:
- `user_queries`
- `monitored_sources` 
- `change_alerts`
- `scraped_contents`
- `content_embeddings`
- `notification_preferences`
- `notification_logs`

### 2. Discovery Service (Microservice) ✅
**Repository**: `discovery-service/` 
**Status**: **Deployed and operational**
**Responsibilities**:
- Natural language query refinement
- URL identification from queries
- AI provider abstraction (Perplexity/OpenAI)
- Stateless computation service

**Key Features**:
- ✅ No database dependencies
- ✅ Independent Docker container (`:8001`)
- ✅ AI provider abstraction (Perplexity primary, OpenAI fallback)
- ✅ Cache-friendly responses
- ✅ Horizontal scaling ready

**API Interface**:
```
POST /discover-sources/
- Input: raw query + user context
- Output: list of monitorable URLs
```

### 3. Integrated Notification System 📧
**Repository**: `backend/app/services/notification_*`
**Status**: **Integrated in main backend** (not a separate microservice)
**Approach**: Database-first with Python processing

**Components**:
- **NotificationService**: Email delivery via SendGrid
- **NotificationProcessor**: Background task processing 
- **Database Functions**: Queue management and statistics
- **API Endpoints**: User preferences and logs

**Key Features**:
- ✅ Email notifications with HTML templates
- ✅ User preference management
- ✅ Background processing with retry logic
- ✅ Comprehensive delivery logging
- ✅ Real-time queue monitoring
- ✅ Supabase database integration

**Why Integrated Instead of Microservice**:
- Better performance (no network calls)
- Simpler deployment and maintenance  
- Leverages Supabase database capabilities
- Easier debugging and error handling

## Database Architecture

### Supabase PostgreSQL Schema

```sql
-- Core tables with RLS enabled
user_queries (user_id, raw_query, status, config_hints_json)
monitored_sources (user_id, user_query_id, url, status, config)
scraped_contents (monitored_source_id, content, embeddings)
content_embeddings (scraped_content_id, vector, model_name)
change_alerts (user_id, monitored_source_id, summary, details, notification_sent)

-- Notification tables
notification_preferences (user_id, email_enabled, email_frequency, browser_enabled)
notification_logs (alert_id, user_email, notification_type, status, sent_at)
```

### Row Level Security (RLS)
- All tables enforce user-scoped access
- Service role can bypass RLS for background tasks
- Automatic user context injection via JWT

### Vector Search with pgvector
- Efficient similarity queries for change detection
- Indexed vector operations
- Support for multiple embedding models

## Authentication & Authorization

### Supabase Integration
1. **Frontend**: Supabase SDK handles auth flows
2. **Backend**: JWT verification with Supabase public key
3. **Database**: RLS policies enforce data isolation
4. **Services**: Service role for background operations

### Security Features
- JWT token validation on all endpoints
- User-scoped database access
- Secure API key management
- CORS configuration

## Current Architecture Status

### ✅ Completed Milestones

#### Milestone 1: Discovery Service Extraction ✅
- [x] **Discovery Service** extracted to separate microservice
- [x] Independent Docker container at `:8001`
- [x] AI provider abstraction (Perplexity/OpenAI)
- [x] Stateless API interface
- [x] Docker Compose orchestration

#### Database Integration ✅
- [x] Supabase PostgreSQL schema
- [x] Row Level Security (RLS) policies
- [x] JWT authentication system
- [x] Vector search with pgvector

#### Notification System ✅
- [x] **Integrated notification system** (not microservice)
- [x] Email delivery via SendGrid
- [x] Background processing with retries
- [x] User preference management
- [x] Database functions for queue management

### 🎯 Current Architecture Decision

**Hybrid Approach**: 
- **Discovery Service**: True microservice (successful decomposition)
- **Notification System**: Integrated backend service (better for this use case)
- **Main Backend**: Handles all other services as monolith

### Phase 3: Background Processing
1. **Replace FastAPI BackgroundTasks**
   - Implement Celery + Redis
   - Create periodic task scheduler
   - Add job monitoring

2. **Extract Content Services**
   - Separate scraping and embedding services
   - Implement event-driven architecture
   - Add horizontal scaling

### Phase 4: Advanced Features
1. **Real-time Updates**
   - Supabase Realtime for live alerts
   - WebSocket connections
   - Push notifications

2. **Analytics & Monitoring**
   - Service mesh (Istio/Linkerd)
   - Distributed tracing
   - Performance monitoring

## Development Workflow

### Local Development
```bash
# Start Supabase (local)
supabase start

# Start API Gateway
cd backend && uv run uvicorn app.main:app --reload

# Start individual services (future)
docker-compose up discovery-service
docker-compose up content-processor
```

### Testing Strategy
- **Unit Tests**: Service-specific logic
- **Integration Tests**: Service-to-service communication
- **E2E Tests**: Full user workflows
- **Load Tests**: Service scalability

### Deployment
- **API Gateway**: Traditional deployment (Railway/Fly.io)
- **Services**: Containerized (Docker + Kubernetes)
- **Database**: Supabase hosted
- **Message Queue**: Redis Cloud/AWS SQS

## Configuration Management

### Environment Variables
```env
# Supabase
SUPABASE_URL=https://project.supabase.co
SUPABASE_SERVICE_KEY=service_key
SUPABASE_JWT_SECRET=jwt_secret

# Service URLs (for microservices)
DISCOVERY_SERVICE_URL=http://discovery:8001
CONTENT_PROCESSOR_URL=http://processor:8002
NOTIFICATION_SERVICE_URL=http://notifications:8003

# AI Providers
OPENAI_API_KEY=sk-...
PERPLEXITY_API_KEY=pplx-...
```

### Service Discovery
- Environment-based configuration initially
- Kubernetes service discovery for production
- Health checks and circuit breakers

## Benefits of This Architecture

### Scalability
- Scale services independently based on load
- Discovery service can handle burst traffic
- Content processing can run on high-memory instances

### Reliability
- Service isolation prevents cascading failures
- Database-level security with RLS
- Graceful degradation capabilities

### Development Velocity
- Teams can work on services independently
- Clear service boundaries and APIs
- Easier testing and deployment

### Cost Optimization
- Pay-as-you-scale for individual services
- Supabase handles database management
- Efficient resource allocation

## Monitoring & Observability

### Metrics
- Service-specific metrics (response time, error rate)
- Database query performance
- AI provider API usage
- User engagement analytics

### Logging
- Structured logging across all services
- Centralized log aggregation
- User-scoped log filtering

### Alerting
- Service health monitoring
- Error rate thresholds
- Performance degradation alerts
- AI provider quota monitoring

## Next Steps

1. **Immediate**: Test current Supabase integration
2. **Week 1**: Extract Discovery Service
3. **Week 2**: Implement proper job queue
4. **Week 3**: Extract Notification Service
5. **Month 1**: Complete microservices extraction
6. **Month 2**: Production deployment and monitoring