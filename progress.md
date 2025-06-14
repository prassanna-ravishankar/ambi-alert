# Progress Log

## 2025-05-12: Backend Iteration 1 Completed & Tested ✅

### Changes Made & Outcomes

*   **Completed Backend Iteration 1**: Successfully implemented and tested all core features defined for Iteration 1 in `backend/next-gen-plan.md`.
    *   ✅ Completed `UserQuery` model, schemas, and API endpoint (`POST /user-queries/`).
    *   ✅ Integrated the new `UserQuery` flow with `SourceDiscoveryService` via background tasks.
    *   ✅ Reviewed and tested AI client implementations (`PerplexityClient`, `OpenAIClient`).
    *   ✅ Refined and tested dependency injection for AI models.
    *   ✅ Updated `backend/next-gen-plan.md` to reflect task completion.
*   **Comprehensive Testing**: Addressed numerous issues identified during extensive testing:
    *   Resolved test environment setup problems (DB creation, settings loading).
    *   Fixed async transaction handling in endpoints and test fixtures.
    *   Corrected mocking strategies for async database operations.
    *   Refined service logic to handle awaitables correctly.
    *   Adjusted brittle timestamp tests for reliability.
*   **Successful Test Execution**: All 48 backend tests are now passing.

### Benefits

*   ✅ **Functional User Query Flow**: Users can now submit queries, have them processed to discover sources, and have corresponding `MonitoredSource` records created.
*   ✅ **Stable and Tested Foundation**: Iteration 1 components are verified, providing a solid base for subsequent features.
*   ✅ **Robust Async Architecture**: Further validation of the async nature of the backend.

### Next Steps (Backend - Iteration 2 from `next-gen-plan.md`)

1.  **Background Task Management**: Implement a robust system (e.g., Celery) for periodic tasks (triggering discovery, content ingestion, change detection).
2.  **Scalability & Error Handling**: Design for scalability and implement advanced error handling/retries.
3.  **Monitoring & Observability**: Integrate application monitoring.
4.  **Storage Evolution**: Evaluate dedicated vector databases.
5.  Continue development based on `backend/next-gen-plan.md` Iteration 2 goals.

## 2025-05-09: Frontend Iteration 1 Plan Adopted 📄

### Plan Overview

- **Reviewed and Adopted:** The "Next-Gen Frontend Plan (Iteration 1 Refocused - API Aligned)" as detailed in `frontend/next-gen-plan.md` is now the guiding document for frontend development.
- **Key Focus Areas:**
    - User Authentication & Profile (Next.js Middleware, Supabase, Axios JWT interceptor).
    - Source Discovery UI (`/discover`, `RawQueryInput`, `POST /api/v1/discover-sources/`).
    - Monitored Source Management (CRUD for `MonitoredSource` on `/sources` using TanStack Query, react-hook-form, Zod).
    - Change Alert Display (List/detail for `ChangeAlertSchema` on `/alerts` using TanStack Query).
    - Robust API client (`src/lib/axiosInstance.ts`) and state management (TanStack Query, react-hot-toast).
- **Implementation Order:**
    1. Foundational: Auth, Axios instance, TanStack Query provider.
    2. Features: Discovery -> Source Management -> Alert Display.
    3. Ongoing: Specific API client functions and TanStack Query hooks.

### Next Steps (Frontend - Iteration 1)

1.  **Foundational Setup:**
    *   ✅ **Task 1 (User Authentication & Profile Context)** from `frontend/next-gen-plan.md` - **COMPLETED**.
        *   ✅ Next.js Middleware (`frontend/src/middleware.ts`) updated to protect `/discover` and `/sources` using `@supabase/ssr`.
        *   ✅ Profile page (`/profile` via `frontend/src/components/auth/UserProfile.tsx`) now displays user info and includes a logout mechanism.
    *   ✅ Set up the dedicated Axios instance (`src/lib/axiosInstance.ts`) with Supabase JWT interceptor (**Task 5, initial part - COMPLETED**).
    *   ✅ Integrate TanStack Query (`QueryClientProvider` in `frontend/src/app/layout.tsx` via `frontend/src/components/providers/QueryProvider.tsx`) (**Task 5, initial part - COMPLETED**).
2.  **Begin Feature Development** as per the plan, starting with Source Discovery (Task 2).
    *   ✅ **Task 2 (Source Discovery UI & Logic)** - **COMPLETED**
        *   ✅ 2.1. Created `/discover` page (`frontend/src/app/discover/page.tsx`).
        *   ✅ 2.2. Developed a form with textarea and submit button.
        *   ✅ 2.3. Implemented API call to `POST /api/v1/discover-sources/`.
        *   ✅ 2.4. Display of `monitorable_urls` implemented.
        *   ✅ 2.5. "Add to Monitor" button now navigates to `/sources/new` with the URL pre-filled.
    *   ✅ **Task 3 (Monitored Source Management UI & Logic)** - **COMPLETED**
        *   ✅ 3.1. List View (`/sources/page.tsx`) implemented with TanStack Query `useQuery` to display sources.
        *   ✅ 3.2. Create Form (`/sources/new/page.tsx`) implemented with `react-hook-form`, `zod`, TanStack Query `useMutation`, and pre-fills from `/discover`.
        *   ✅ 3.3. Edit Form (`/sources/[id]/edit/page.tsx`) implemented, fetches data, allows updates for URL, interval, and status using `useMutation`.
        *   ✅ 3.4. Delete Functionality implemented in list view with confirmation and `useMutation`.
    *   ✅ **Task 4 (Change Alert Display UI & Logic)** - **COMPLETED**
        *   ✅ 4.1. List View (`/alerts/page.tsx`) implemented:
            *   Fetches and displays `ChangeAlertSchema` records using `useQuery`.
            *   Includes basic filtering for `is_acknowledged`.
            *   Displays key alert info (source, summary, timestamp, details, screenshot).
            *   Placeholders for advanced filtering/sorting.
        *   ✅ 4.2. Detail View (`/alerts/[id]/page.tsx`) implemented: Fetches and displays comprehensive alert details.
        *   ✅ 4.3. Acknowledge Functionality: Implemented on both list and detail views with `useMutation`.

## 2025-05-07: Iteration 1 Backend Stabilization & Testing Success 🟢

### Changes Made & Outcomes

*   **Successful Test Execution**: All previously implemented tests for the core monitoring API (`test_monitoring_api.py`), Pydantic schemas (`test_monitoring_schemas.py`), and the Perplexity AI client (`test_perplexity_client.py`) are now **passing**.
*   **Async & Database Corrections**:
    *   Ensured asynchronous operations are correctly handled throughout the FastAPI application lifecycle (startup events in `app/main.py`) and in database interactions (`app/core/db.py` and API endpoints in `app/api/endpoints/monitoring.py`).
    *   Resolved SQLAlchemy compatibility issues, including `MissingGreenlet` errors and type mismatches (e.g., `HttpUrl` to `str` for database storage).
*   **Pydantic V2 Compatibility**: Updated schemas and relevant tests to align with Pydantic V2 (e.g., `from_attributes` instead of `orm_mode`).
*   **Refined AI Client Mocking**: Improved the mocking strategy for `aiohttp.ClientSession` in `test_perplexity_client.py` for more reliable and accurate testing of the AI client.
*   **Dependency Verification**: Ensured necessary dependencies like `greenlet` are correctly installed and utilized.

### Benefits

*   ✅ **Stable Core Functionality**: The foundational CRUD operations for monitored sources and the Perplexity AI client integration are now stable and verified by a full suite of passing tests.
*   ✅ **Robust Asynchronous Architecture**: The backend's asynchronous architecture for database and API operations has been validated.
*   ✅ **Foundation for Further Development**: With these core components stabilized, development can proceed on subsequent Iteration 1 tasks (like `OpenAIClient` implementation, further service refinements, and expanded test coverage) with greater confidence.

### Next Steps (Continuing Iteration 1 from `next-gen-plan.md`)

1.  **Full AI Client Implementation**: Finalize `OpenAIClient`.
2.  **Refine Dependency Injection for AI Models** in API endpoints (ensure all services use DI for AI models).
3.  **Robust Scraping Enhancements** in `ContentIngestionService`.
4.  **Refine Preprocessing Logic** in `ContentIngestionService`.
5.  **Comprehensive Testing (Phase 2)**: Expand unit and integration tests, especially for `OpenAIClient` and the core services.
6.  **Logging Implementation**: Continue replacing `print()` with a proper logging framework across all new components.

## 2024-05-07: CI Pipeline Refinements and Backend Test Expansion 🛠️

### Changes Made

- **Backend Test Expansion:**
  - Added tests for `AlertCreate` and `AlertUpdate` Pydantic schemas (`tests/test_schemas.py`).
  - Added tests for `EmbeddingService` (`tests/test_embedding_service.py`), including mocking the `SentenceTransformer` model using `unittest.mock.patch`.
  - Refined mocking strategy in `test_embedding_service.py` to correctly handle fixture scope vs. patch scope.
  - Fixed Ruff linting errors (E501, N803, E402, E712) and silenced PLR2004 warnings in test files.
- **CI Pipeline Fixes:**
  - Configured Codecov action (`backend.yml`) to use `CODECOV_TOKEN` secret, resolving tokenless upload errors.
  - Confirmed frontend build step (`frontend.yml`) includes necessary Supabase environment variables via GitHub secrets/environments.
  - Ensured backend `pytest` runs successfully after test additions and fixes.

### Benefits

- ✅ Increased backend test coverage (schemas, embedding service).
- ✅ Resolved Codecov upload failures in CI.
- ✅ More stable and reliable CI pipeline for both frontend and backend.
- ✅ Addressed various linting issues in backend tests.

### Next Steps

- Continue expanding test coverage (e.g., other services, API endpoints).
- Address frontend linting warnings (e.g., `img` tags).
- Proceed with core feature development.

## 2024-05-06: Testing Framework Setup and Initial Tests 🧪

### Changes Made

- **Frontend Testing (Vitest):**
  - Integrated Vitest, React Testing Library, and related dependencies.
  - Configured Vitest (`vitest.config.ts`) including path alias resolution and `.env.local` loading via `dotenv`.
  - Added test setup file (`src/test/setup.ts`) for global configurations (e.g., jest-dom matchers).
  - Added `test` and `coverage` scripts to `package.json` (`vitest run`).
  - Resolved issues preventing `npm test` execution (corrupted `package.json` parsing).
  - Implemented initial component tests for `Navigation.tsx` (`components/Navigation.test.tsx`).
  - Included mocking for `next/navigation` (`usePathname`), custom hooks (`useAuth`), and external dependencies (`supabaseClient`).
  - Resolved TypeScript (`tsc`) errors related to Vitest globals and mock types.
  - Updated frontend CI (`frontend.yml`) to inject Supabase environment variables during `next build` using GitHub secrets and environments.

- **Backend Testing (Pytest):**
  - Set up `tests` directory structure.
  - Resolved initial Mypy configuration issues ("Source file found twice"); subsequently removed Mypy from CI workflow (`backend.yml`) due to type errors in dependencies.
  - Implemented initial tests for Pydantic schemas (`tests/test_schemas.py`) covering validation and defaults.
  - Implemented initial tests for `EmbeddingService` (`tests/test_embedding_service.py`), including mocking `SentenceTransformer` using `unittest.mock.patch`.
  - Resolved test failures related to mock scope/timing.
  - Fixed various Ruff linting errors in test files (E501, N803, E402, E712) and silenced PLR2004 (magic numbers) warnings with `noqa` comments.

### Benefits

- ✅ Basic testing infrastructure established for both frontend and backend.
- ✅ CI workflows updated to handle build requirements and run basic checks.
- ✅ Examples of component testing, mocking, and schema validation.
- ✅ Improved code quality through linting fixes in tests.

### Next Steps

1. Expand test coverage for both frontend and backend components/services.
2. Re-evaluate adding Mypy back to backend CI once underlying type issues are addressed.
3. Implement remaining TODOs in existing test files (e.g., `Navigation.test.tsx`).
4. Continue feature development (profile management, RLS, etc.).

## 2024-04-14: Authentication Migration & Debugging 🔒

### Changes Made

- **Implemented User Authentication:**
  - Set up Supabase Auth for the frontend.
  - Created Sign In, Sign Up, and user profile components.
  - Added email/password and magic link authentication methods.
- **Migrated Auth Helpers:**
  - Encountered persistent cookie issues with `@supabase/auth-helpers-nextjs` in the Next.js App Router context (Middleware/Route Handlers).
  - Migrated from `@supabase/auth-helpers-nextjs` to the recommended `@supabase/ssr` library.
  - Updated client creation (`createBrowserClient`, `createServerClient`) and middleware/route handler logic according to `@supabase/ssr` patterns.
- **Extensive Debugging:**
  - Iteratively debugged cookie setting/reading issues between Route Handlers and Middleware.
  - Added and removed detailed logging across multiple components.
  - Explored various cookie setting strategies (automatic, manual, header append).
- **Dependency Management:**
  - Resolved installation issues with `@supabase/ssr` via clean installs (`npm install`).
  - Generated Supabase types using the CLI (`supabase gen types`).
- **Cleanup:**
  - Removed excessive debugging logs.
  - Uninstalled unused dependencies (`@types/cookie`).
  - Added documentation comments to relevant auth files.

### Benefits

- ✅ Functional user authentication flow (Sign Up, Sign In, Sign Out).
- ✅ Robust session management using modern `@supabase/ssr` library.
- ✅ Correct cookie handling across Next.js server/client contexts.
- ✅ Cleaner codebase after debugging and cleanup.

### Next Steps

1. Implement user profile management (displaying/editing profile data).
2. Configure Supabase Row Level Security (RLS) policies.
3. Secure backend API endpoints based on user authentication.

## 2024-03-29: UV Integration 🚀

### Changes Made

1. **Backend Build System**

   - Switched to `uv` for dependency management
   - Added `.python-version` file
   - Updated `pyproject.toml` for `uv` compatibility
   - Configured `uv` settings
   - Implemented virtualenv-based workflow with `uv run`

2. **GitHub Actions**
   - Updated backend workflow to use `uv` with virtualenvs
   - Added `setup-uv` action
   - Improved dependency caching
   - Streamlined Python setup
   - Removed pip-based installation
   - Using `uv run` for all commands

### Benefits

- ✅ Faster dependency resolution
- ✅ Reproducible builds
- ✅ Better caching in CI/CD
- ✅ Modern Python tooling
- ✅ Improved development experience
- ✅ Cleaner virtual environment management
- ✅ Simplified command execution with `uv run`

### Next Steps

1. Add `uv` setup instructions to README
2. Configure pre-commit hooks with `uv`
3. Add Docker support with `uv`
4. Update development documentation
5. Add virtualenv cleanup instructions
6. Add virtualenv activation scripts
7. Add `uv run` examples to documentation

## 2024-03-29: Frontend Implementation - Phase 1 🎨

### Changes Made

1. **Project Setup**

   - Created Next.js project with TypeScript
   - Added Tailwind CSS for styling
   - Configured ESLint and TypeScript
   - Added necessary dependencies

2. **Core Components**

   - Navigation bar with responsive design
   - Dashboard for alert management
   - New alert form with validation
   - Settings page for user preferences

3. **Features**

   - Form validation with Zod
   - Toast notifications
   - Responsive design
   - Type-safe API client
   - Clean component structure

4. **Dependencies**
   - Next.js for framework
   - Tailwind CSS for styling
   - React Hook Form for forms
   - Zod for validation
   - Axios for API calls
   - Headless UI for components
   - Heroicons for icons

### Benefits

- ✅ Modern, responsive UI
- ✅ Type-safe development
- ✅ Form validation
- ✅ Clean component structure
- ✅ Reusable API client
- ✅ User-friendly interface

### Next Steps

1. Implement user authentication
2. Add real-time updates
3. Add alert history view
4. Add alert statistics
5. Add email verification
6. Add mobile responsiveness improvements
7. Add loading states
8. Add error boundaries

## 2024-03-29: Backend Implementation - Phase 1 🏗️

### Changes Made

1. **Project Structure**

   - Created new backend directory structure
   - Set up FastAPI application
   - Configured SQLAlchemy with async support
   - Added environment configuration

2. **Core Components**

   - Implemented Alert model with SQLAlchemy
   - Created database configuration
   - Added environment settings management
   - Set up CORS middleware

3. **Services**

   - AlertService: Core alert management
   - EmbeddingService: Semantic comparison
   - MonitorService: Content fetching
   - NotificationService: Email alerts

4. **API Routes**

   - POST /api/v1/alerts: Create alerts
   - GET /api/v1/alerts: List alerts
   - DELETE /api/v1/alerts/{id}: Remove alerts
   - POST /api/v1/alerts/check: Trigger checks

5. **Dependencies**
   - FastAPI for API framework
   - SQLAlchemy for database
   - sentence-transformers for embeddings
   - SendGrid for email
   - aiohttp for async HTTP
   - BeautifulSoup for web scraping

### Benefits

- ✅ Clean, modular architecture
- ✅ Async operations for better performance
- ✅ Type-safe code with mypy
- ✅ Comprehensive API documentation
- ✅ Easy configuration management
- ✅ Proper error handling

### Next Steps

1. Implement frontend with Next.js/v0.dev
2. Add YouTube API integration
3. Add RSS feed support
4. Add user authentication
5. Add rate limiting
6. Add monitoring dashboard
7. Add tests for all components
8. Add deployment configuration

## 2024-03-29: Project Refactoring Plan 🔄

### Overview

Based on the new project goals, we're undertaking a major refactoring to:

1. Remove smolagents dependency
2. Implement a cleaner, more focused architecture
3. Better align with the new MVP goals

### Planned Changes

1. **Core Architecture Updates**

   - Remove smolagents and related code
   - Implement direct LLM integration for query parsing
   - Add embedding-based change detection
   - Implement proper FastAPI backend structure

2. **Component Refactoring**

   - Frontend: Create new Next.js/v0.dev UI
   - Backend: Implement FastAPI endpoints
   - Monitoring: Add support for multiple content sources
   - Storage: Implement proper database schema
   - Alerting: Enhance notification system

3. **Dependencies Update**

   - Remove: smolagents, duckduckgo_search
   - Add: FastAPI, Next.js, OpenAI, sentence-transformers
   - Update: Database client, web scraping tools

4. **Code Organization**
   - Split into frontend and backend packages
   - Implement proper API documentation
   - Add comprehensive testing
   - Improve error handling

### Next Steps

1. Set up new project structure
2. Implement core backend components
3. Create frontend application
4. Add monitoring and alerting systems
5. Implement proper testing
6. Add documentation

### Benefits

- ✅ Cleaner architecture
- ✅ Better separation of concerns
- ✅ More maintainable codebase
- ✅ Better aligned with project goals
- ✅ Improved scalability
- ✅ Better user experience

## 2024-03-20: Rate Limiter Removal 🗑️

### Changes Made

- 🗑️ Removed rate limiter module and all its references:
  - Deleted rate_limiter.py
  - Deleted test_rate_limiter.py
  - Removed rate limiter from WebsiteMonitor
  - Removed rate limiter from EmailAlertBackend
  - Cleaned up related tests and documentation
- 🔄 Simplified request handling in monitor and alerting modules
- 📝 Updated documentation to reflect changes

### Benefits

- ✅ Simpler codebase
- ✅ Less complexity in request handling
- ✅ More straightforward monitoring and alerting logic
- ✅ Reduced maintenance overhead

### Next Steps

1. Add retry mechanisms for failed operations
2. Add more alert backends (Slack, Discord)
3. Add async tests
4. Add alert queuing system
5. Add alert delivery confirmation

## 2024-03-20: Async Implementation - Phase 3 📨

### Changes Made

- 📧 Updated alert backends with async support:
  - Converted EmailAlertBackend to use aiosmtplib
  - Added proper SMTP connection management
  - Added async context manager support
  - Added connection pooling for SMTP
  - Updated MockAlertBackend for async
- 🔄 Enhanced AlertManager:
  - Converted to async operations
  - Added proper error handling
  - Added success/failure reporting
- 🧹 Improved resource cleanup:
  - Added proper SMTP connection cleanup
  - Added cleanup in Torale's run_monitor
  - Better error handling for cleanup
- 📝 Updated type hints and documentation

### Benefits

- ✅ Non-blocking alert delivery
- ✅ Better SMTP connection management
- ✅ More reliable email delivery
- ✅ Proper resource cleanup
- ✅ Better error handling and reporting
- ✅ Type-safe async code

### Next Steps

1. Add retry mechanisms for failed alerts
2. Add more alert backends (Slack, Discord)
3. Add connection pooling for HTTP requests
4. Add async tests for alert system
5. Add alert queuing system
6. Add alert delivery confirmation

## 2024-03-20: Async Implementation - Phase 2 🔄

### Changes Made

- 🗃️ Updated DatabaseManager with async support:
  - Converted to aiosqlite for async database operations
  - Added proper connection management
  - Added async context manager support
  - Converted all database operations to async
- 🔄 Enhanced Torale class:
  - Added async context manager support
  - Added proper resource cleanup
  - Updated all database calls to async
- 🎯 Updated example.py to use async context managers
- 📝 Added type hints for all async methods
- 🧹 Improved error handling and resource cleanup

### Benefits

- ✅ Non-blocking database operations
- ✅ Better connection management
- ✅ Proper resource cleanup
- ✅ More efficient database access
- ✅ Better error handling
- ✅ Type-safe async code

### Next Steps

1. Implement async alert backends
2. Add proper connection pooling
3. Add retry mechanisms for failed operations
4. Add async tests
5. Update documentation with async examples
6. Consider adding async caching

## 2024-03-20: Async Implementation - Phase 1 🚀

### Changes Made

- 📦 Added async dependencies:
  - aiohttp for async HTTP requests
  - aiosqlite for async database operations
  - asyncio for async/await support
- 🔄 Updated WebsiteMonitor with async support:
  - Async session management
  - Async content fetching
  - Async content hash generation
  - Async relevance checking
  - Proper resource cleanup
- 🎯 Added type hints for async functions
- 📝 Added documentation for async methods
- 🔧 Maintained backward compatibility where possible

### Benefits

- ✅ Better resource utilization
- ✅ Improved scalability for multiple URLs
- ✅ More efficient content fetching
- ✅ Proper connection management
- ✅ Better error handling

### Next Steps

1. Update database operations to use aiosqlite
2. Implement async alert backends
3. Update main Torale class for async operation
4. Update CLI for async support
5. Add async tests
6. Update documentation with async examples

## 2024-03-20: Task Updates and Monitoring Control 🎮

### Changes Made

- 🔄 Added option to disable monitoring in example.py
- 🤔 Analyzed query expander placement:
  - Decision: Query expander should remain in initial setup (not monitor/alerter) because:
    1. Expansion is needed when setting up monitoring targets
    2. Monitor should focus on checking existing URLs
    3. Alerter should focus on notification delivery
- 📋 Updated project goals with new tasks
- 🎯 Next focus: Implementing async functionality

### Benefits

- ✅ Better control over monitoring behavior
- ✅ Clearer separation of concerns
- ✅ More flexible usage patterns
- ✅ Better user control over resource usage

### Next Steps

1. Implement asynchronous code in:
   - Website monitoring
   - Content fetching
   - Database operations
2. Add proper error handling for async code
3. Add async support to alert backends
4. Update documentation for async features

## 2024-03-20: Project Structure and Documentation Updates 📚

### Changes Made

- 📦 Added proper project packaging with pyproject.toml
- 🔧 Configured development tools:
  - ruff for linting and formatting
  - mypy for type checking
  - pytest for testing
  - mkdocs for documentation
- 📝 Added comprehensive README with:
  - Installation instructions
  - Quick start guide
  - Python API examples
  - Development setup guide
- 🎯 Added example.py for demonstration
- 🛠️ Added dependency groups for development tools
- 📋 Added project metadata and classifiers
- 🔄 Updated version constraints for Python and dependencies

### Benefits

- ✅ Better project organization
- ✅ Proper packaging for PyPI distribution
- ✅ Clear documentation for users and contributors
- ✅ Modern development toolchain
- ✅ Example code for quick start

### Next Steps

1. Add more alert backends (e.g., Slack, Discord)
2. Implement diff-based change detection
3. Add support for robots.txt
4. Add tests
5. Add more documentation
6. Add support for custom content extractors
7. Add support for authentication on monitored websites
8. Consider implementing other components as proper Agents

## 2024-03-19: Agent Interface Improvements 🧠

### Changes Made

- 🏗️ Refactored QueryExpander to properly implement smolagents' Agent interface
- 🧠 Added comprehensive system prompt for query expansion agent
- 🎯 Improved query expansion logic with:
  - Better prompt engineering
  - Fallback handling
  - Bullet/numbering cleanup
  - More detailed query generation guidelines
- 🔄 Updated main module to use the new QueryExpanderAgent
- 📚 Added more detailed documentation and type hints

### Benefits

- ✅ Better integration with smolagents ecosystem
- ✅ More robust query expansion
- ✅ Clearer agent responsibilities
- ✅ Improved error handling
- ✅ More maintainable code structure

### Next Steps

1. Add more alert backends (e.g., Slack, Discord)
2. Implement diff-based change detection
3. Add support for robots.txt
4. Add tests
5. Add more documentation
6. Add support for custom content extractors
7. Add support for authentication on monitored websites
8. Consider implementing other components as proper Agents

## 2024-03-19: Smolagents Integration Enhancement 🤖

### Changes Made

- ✨ Refactored to better utilize smolagents' multi-agent capabilities
- 🔄 Replaced standalone QueryExpander with dedicated query agent
- 🎯 Created specialized agents for different tasks:
  - `search_agent`: Web search and content retrieval
  - `query_agent`: Query expansion and refinement
  - `relevance_agent`: Content analysis and summarization
  - `manager_agent`: Multi-agent coordination
- 📝 Improved prompts for better agent interactions
- 🔍 Enhanced content relevance checking with dedicated agent
- 🎨 Cleaner code organization and better separation of concerns

## 2024-03-19: Initial Implementation ✨

### Completed Features

- ✅ Basic project structure using uv
- ✅ Query expansion using LLMs
- ✅ Web search functionality using DuckDuckGo
- ✅ Website monitoring with content hashing
- ✅ SQLite database for URL tracking
- ✅ AI-powered relevance checking
- ✅ Mock and Email alerting systems
- ✅ Command-line interface
- ✅ Python API
- ✅ Documentation and README

### Implementation Details

1. **Core Modules Created**:

   - `query_expander.py`: Smart query expansion using LLMs
   - `database.py`: SQLite-based URL tracking
   - `monitor.py`: Website monitoring and change detection
   - `alerting.py`: Flexible alerting system
   - `main.py`: Main Torale coordination
   - `cli.py`: Command-line interface

2. **Key Features Implemented**:

   - Query expansion that generates 3-5 related search queries
   - Efficient content monitoring using hash-based change detection
   - AI-powered relevance checking of content changes
   - Modular alerting system with email and mock backends
   - Persistent storage with SQLite
   - Error handling and automatic retries

3. **Dependencies**:
   - Core functionality:
     - smolagents (>=0.0.10): Intelligent search and LLM integration
     - duckduckgo_search (>=4.4.0): Web search functionality
   - Web scraping and processing:
     - beautifulsoup4 (>=4.12.0): HTML parsing and content extraction
     - requests (>=2.31.0): HTTP operations
     - lxml (>=5.1.0): Fast HTML parsing backend
     - html5lib (>=1.1): Robust HTML parsing for malformed pages
     - markdownify (>=0.11.6): Text processing
   - Built-in modules:
     - sqlite3: Database storage
     - email: Email notifications
     - argparse: CLI interface