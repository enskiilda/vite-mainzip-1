# Open WebUI

## Overview

Open WebUI is a self-hosted AI chat platform designed to provide a user-friendly interface for interacting with large language models (LLMs). The application is built as a full-stack web application with a SvelteKit frontend and Python backend, offering features like chat management, knowledge bases, model management, user authentication, and administrative controls.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: SvelteKit 2.x with Svelte 5
- Static site generation (SSG) with client-side rendering
- Uses `@sveltejs/adapter-static` for building static HTML/CSS/JS bundles
- Progressive Web App (PWA) capabilities for mobile offline access
- TypeScript for type safety throughout the frontend

**Build System**: Vite 5.x
- Hot module replacement (HMR) for development
- ES module format for web workers
- Source maps enabled for debugging
- Static asset copying for WASM files (ONNX runtime)

**UI Framework**: Tailwind CSS 4.x
- Custom design tokens for theming (color scales, typography)
- Dark mode support via CSS class toggling
- Custom UI scale via CSS variables for accessibility
- Typography plugin for rich text rendering

**State Management**:
- Svelte stores for global state (user, config, theme, chat state)
- LocalStorage for persistence of user preferences
- WebSocket connections via Socket.io for real-time features

**Rich Text Editing**: TipTap 3.x
- Extensions for mentions, code blocks, tables, YouTube embeds, images
- Syntax highlighting via Highlight.js
- Markdown and LaTeX rendering support
- Drag-and-drop file handling

**Key Frontend Libraries**:
- Chart.js for analytics visualization
- CodeMirror 6 for code editing (Python, JavaScript, and other languages)
- Day.js for date/time formatting with i18n support
- Marked.js for Markdown rendering with custom extensions
- @huggingface/transformers for ML capabilities (embedding, TTS)

### Backend Architecture

**Framework**: Python with FastAPI/Uvicorn (implied by start.sh and uvicorn references)
- RESTful API design following `/api/v1/` convention
- Reverse proxy pattern for Ollama API integration
- Token-based authentication (Bearer tokens)

**API Structure**:
- `/api/v1/auths/` - Authentication and authorization
- `/api/v1/chats/` - Chat management
- `/api/v1/files/` - File upload and processing
- `/api/v1/knowledge/` - Knowledge base management
- `/api/v1/functions/` - Custom function/tool management
- `/api/v1/groups/` - User group management
- `/api/v1/channels/` - Channel-based collaboration
- `/api/v1/images/` - Image generation
- `/api/v1/audio/` - Text-to-speech/speech-to-text
- `/api/v1/retrieval/` - RAG (Retrieval Augmented Generation)
- `/ollama` - Proxied Ollama API
- `/openai` - OpenAI-compatible API endpoint

**Authentication & Authorization**:
- JWT-based token authentication
- Role-based access control (RBAC) with admin/user/pending roles
- Granular permissions system for sharing and collaboration
- Support for SSO/OIDC integration (via MSAL browser library)

**Design Patterns**:
- Backend reverse proxy prevents CORS issues and secures API access
- Stateless API design with token validation per request
- Async/await patterns for I/O operations
- WebSocket support for real-time user presence and updates

### Data Storage

**Database**: PostgreSQL (inferred from DATABASE_URL environment variable and Drizzle ORM references)
- Connection pooling via environment configuration
- Schema migrations handled by backend

**File Storage**:
- Backend manages file uploads with metadata
- Supports various file types (PDF, DOCX, audio, video, code files)
- Streaming file processing with progress updates
- Content embedding for RAG functionality

**Client-Side Storage**:
- LocalStorage for user preferences and session data
- IndexedDB potential use via browser APIs

### Machine Learning Integration

**Embedding Models**: 
- @huggingface/transformers for client-side embeddings
- ONNX runtime for WebAssembly/WebGPU acceleration
- Support for multiple embedding providers

**Text-to-Speech**:
- Kokoro TTS via Web Workers
- OpenAI TTS API integration
- Multiple provider support (Deepgram, local Whisper, etc.)
- Configurable audio playback with queue management

**Speech-to-Text**:
- Multiple STT providers configurable
- Audio file processing support

**RAG System**:
- 9 vector database options supported
- Hybrid search capabilities
- Knowledge base indexing with async embedding processing
- Document parsing (PDF, DOCX, HTML, etc.)

### Internationalization (i18n)

**Framework**: i18next
- Browser language detection with fallback to en-US
- Dynamic locale loading from JSON files
- Support for 50+ languages
- i18next-parser for extracting translation keys

## External Dependencies

### Third-Party Services

**AI Model Providers**:
- Ollama (local LLM runtime) - Primary integration
- OpenAI API - Compatible endpoint support
- LMStudio, GroqCloud, Mistral, OpenRouter - Via OpenAI-compatible API

**Image Generation**:
- OpenAI DALL-E
- Configurable via backend API

**Audio Services**:
- OpenAI TTS/Whisper
- Deepgram (STT)
- Local Whisper models

**Search Providers** (15+ for RAG web search):
- SearXNG, Google PSE, Brave Search, Kagi, Mojeek
- Tavily, Perplexity, Serpstack, Serper, Serply
- DuckDuckGo, and others

**Cloud Storage Integrations**:
- Google Drive (OAuth2 with Picker API)
- Microsoft OneDrive (Personal and Business via MSAL)
- SharePoint integration

**Authentication**:
- Azure MSAL for Microsoft SSO
- OIDC support for custom identity providers

### Development & Testing

**Testing Framework**:
- Cypress for E2E testing
- Vitest for unit tests
- Test coverage for chat, settings, authentication flows

**Code Quality**:
- ESLint with TypeScript support
- Prettier for code formatting
- TypeScript strict mode enabled
- Svelte-check for component validation

**CI/CD**:
- GitHub Actions workflows (implied by .github structure)
- Pull request templates with changelog requirements
- Version management via git commit hashes

### Deployment

**Containerization**: Docker
- Multi-stage builds for frontend and backend
- Support for CUDA-tagged images for GPU acceleration
- Network configuration for Ollama communication
- Environment variable configuration

**Infrastructure Options**:
- Docker Compose for local development
- Kubernetes deployment (kubectl, kustomize, helm)
- Static hosting for frontend build output
- Reverse proxy setup (Apache, Nginx) for HTTPS

**Environment Configuration**:
- `.env` file support via Vite
- Runtime environment variable injection
- Public/private environment variable separation
- Feature flags via environment variables