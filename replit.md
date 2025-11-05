# Research Sentiment Analysis Platform

## Overview

This is a full-stack financial research sentiment analysis platform that ingests recurring publications from banks and economists, analyzes sentiment using AI, and provides interactive dashboards and RAG-based chat functionality. The system processes research documents, extracts sentiment scores (0-100 scale), and aggregates insights by region and asset class.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- React 18 with TypeScript
- Vite as build tool and dev server
- Wouter for client-side routing
- TanStack Query (React Query) for server state management
- shadcn/ui component library built on Radix UI primitives
- Tailwind CSS for styling with Material Design 3 principles
- Recharts for data visualization

**Design System:**
- Material Design 3 approach emphasizing data clarity and professional trust
- Typography: Inter for UI/body text, JetBrains Mono for numerical data
- Custom Tailwind configuration with HSL-based color tokens
- "New York" shadcn variant with rounded borders and clean aesthetics
- Consistent spacing primitives (2, 4, 6, 8, 12, 16)

**Key Pages:**
- Dashboard: Sentiment pendulums for Global/Europe/USA regions, 90-day trend charts, recent articles
- Articles: Searchable/filterable table of research documents
- Chat: RAG-powered conversational interface with contextual filters

**Component Structure:**
- Reusable presentational components (Pendulum, SentimentBadge, SentimentChart, ChatMessage, ArticlesTable)
- Smart filter components (ArticleFilters) with controlled state
- Consistent loading states and empty states across all views

### Backend Architecture

**Technology Stack:**
- Node.js with Express.js
- TypeScript throughout
- Drizzle ORM for database interactions
- Neon serverless PostgreSQL driver
- OpenAI API for chat/RAG functionality
- Session management with connect-pg-simple

**API Design:**
- RESTful endpoints under `/api` prefix
- Health check endpoint
- Article retrieval with flexible filtering (query, source, region, asset class, date range)
- Sentiment aggregation endpoints (current snapshot and time series)
- Chat query endpoint for RAG interactions
- Benchmark data caching endpoint

**Request/Response Flow:**
1. Express middleware logs requests and captures JSON responses
2. Routes defined in `server/routes.ts` handle business logic
3. Storage layer (`server/storage.ts`) abstracts database operations
4. Vite middleware serves frontend in development
5. Static file serving in production

### Data Layer

**Database: PostgreSQL (Neon Serverless)**

**Schema Design:**

1. **sources** table:
   - Tracks research publishers (banks, economists)
   - Fields: name, homepage, RSS URL, active status
   - Serial primary key

2. **documents** table:
   - Stores research articles with metadata
   - UUID primary key for scalability
   - Foreign key to sources
   - Content hash for deduplication
   - Embedding field (stored as JSON string for 384-dim vectors)
   - Indexed fields: publishedAt, region, assetClass, contentHash
   - Supports categorization by region (global/europe/usa) and asset class (equity/macro/rates/fx)

3. **sentiment** table:
   - Links to documents with scores and classifications
   - Score range: 0-100 (bearish/neutral/bullish)
   - Method field tracks analysis approach
   - Confidence score for quality assessment

4. **benchmarkCache** table:
   - Caches normalized market benchmark data
   - Tracks ticker, date, and normalized price
   - Used for comparing sentiment against market performance

**ORM Approach:**
- Drizzle ORM with type-safe schema definitions
- Schema exported from `shared/schema.ts` for frontend/backend sharing
- Drizzle-Zod integration for runtime validation
- Migration support via drizzle-kit

**Storage Interface:**
- Abstraction layer in `server/storage.ts` implementing IStorage interface
- Methods for CRUD operations on all entities
- Filtered queries with flexible parameters
- Aggregation methods for sentiment analysis
- Vector similarity search support (via pgvector extension, though not fully implemented in provided code)

### External Dependencies

**Database:**
- Neon Serverless PostgreSQL (DATABASE_URL environment variable)
- pgvector extension for embedding storage and similarity search
- WebSocket support for Neon's serverless architecture

**AI/ML Services:**
- OpenAI API (OPENAI_API_KEY environment variable)
- Model: gpt-4o-mini (configurable via OPENAI_MODEL)
- Used for RAG chat functionality and potentially sentiment analysis
- Sentence transformers mentioned for embeddings (paraphrase-multilingual-MiniLM-L12-v2)

**Content Extraction:**
- Planned: trafilatura for web scraping and text extraction
- Planned: pypdf and pdfminer.six for PDF processing
- RSS feed parsing for content discovery

**Financial Data:**
- Planned: yfinance for market benchmark data
- BENCHMARK_TICKER configuration (URTH, EUNL.DE, IWDA.AS)

**Development Tools:**
- Replit-specific plugins for dev experience (cartographer, dev-banner, runtime-error-modal)
- Vite with HMR for fast development

**Authentication:**
- Session-based (connect-pg-simple for PostgreSQL session store)
- No OAuth/external auth providers visible in current implementation

**Deployment:**
- Environment variables for configuration (.env file)
- CORS_ORIGINS for cross-origin control
- APP_BASE_URL for absolute URL generation
- Build process: Vite for frontend, esbuild for backend bundling