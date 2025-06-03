# Radiffect RPG Backend System Architecture

## Architecture Overview

### Architecture Layers

**Client Layer:**
- Client Applications (Frontend/Game UI)
- Connects via HTTP REST API and WebSocket connections

**API Gateway Layer:**
- HTTP REST API with middleware (Rate Limiting, Authentication, CORS)
- WebSocket Manager for connection management

**Routing Layer:**
- API Routes: auth, currency, survivors, characters, maps, spawners, marketplace

**Controller Layer:**
- Auth Controller
- Currency Controller  
- Survivor Controller
- Character Controller
- Marketplace Controller
- Sui Auth Controller

**WebSocket Processing:**
- WebSocket Manager → Game Action Handler (Real-time Game Actions)

**Service Layer:**
- JWT Service, User Service (from Auth Controller)
- Currency Service (from Currency Controller)
- Player Service (from Survivor Controller)
- Character Service (from Character Controller)
- Marketplace Service (from Marketplace Controller)

**Game Services (from Game Action Handler):**
- Game Manager
- Inventory Service
- Spawner Service
- Crafting Service
- Loot Service
- Stat Service
- Action Validation Service

**State Management (from Game Manager):**
- Player State Manager
- Player Position Service
- Player Connection Service
- Player Stats Service
- Player Currency Service

**Cache Layer:**
- Redis Cache for session management
- Connected to: JWT Service, Player State Manager, Player Connection Service
- Data Cache Service (from Marketplace Service) → Redis

**Database Layer:**
- PostgreSQL Database storing game data
- Connected to: User Service, Player Service, Character Service, Currency Service, Marketplace Service, Inventory Service, Spawner Service, Crafting Service

**Repository Layer:**
- Character Repository
- Crafting Repository
- User Repository
- Marketplace Repository

**External Services:**
- Sui Blockchain API (from Sui Controller)

**Broadcasting:**
- WebSocket Broadcasting for real-time updates
- Connected to: WebSocket Manager, Game Manager, Spawner Service

## Data Flow Patterns

### 1. Authentication Flow
```
Client → Auth Controller: POST /api/auth/login
Auth Controller → Database: Validate credentials
Database → Auth Controller: User data
Auth Controller → JWT Service: Generate tokens
JWT Service → Redis: Store refresh token
JWT Service → Auth Controller: Access & refresh tokens
Auth Controller → Client: Authentication response
```

### 2. Real-time Game Action Flow
```
Client → WebSocket Manager: WebSocket message (game action)
WebSocket Manager → WebSocket Manager: Rate limiting & validation
WebSocket Manager → Game Action Handler: Process action
Game Action Handler → Game Action Handler: Check ownership & rate limits
Game Action Handler → Game Manager: Route to appropriate handler
Game Manager → Game Services: Execute game logic
Game Services → Database: Update game state
Database → Game Services: Confirm update
Game Services → Game Manager: Action result
Game Manager → Game Action Handler: Response data
Game Action Handler → WebSocket Manager: Success/error response
WebSocket Manager → Broadcast: Broadcast to relevant players
WebSocket Manager → Client: Action response
```

### 3. Marketplace Transaction Flow
```
Client → Marketplace Controller: POST /api/marketplace/purchase
Marketplace Controller → Marketplace Service: Process purchase
Marketplace Service → Currency Service: Check currency balance
Currency Service → Database: Validate funds
Database → Currency Service: Balance confirmed
Marketplace Service → Database: Begin transaction
Marketplace Service → Currency Service: Deduct currency
Marketplace Service → Inventory Service: Transfer item
Marketplace Service → Database: Update listing status
Database → Marketplace Service: Transaction complete
Marketplace Service → Marketplace Controller: Purchase result
Marketplace Controller → Client: Response
```

## Component Interaction Patterns

### Core Service Dependencies

**Core Services:**
- Redis Service
- Database Service
- JWT Service
- WebSocket Manager
- Cache Manager

**Player Services:**
- Player State Manager → Redis Service
- Player Position Service
- Player Connection Service → Redis Service
- Player Stats Service
- Player Currency Service

**Game Services:**
- Game Manager → Player State Manager, Player Position Service, Player Connection Service
- Inventory Service → Database Service
- Spawner Service → Database Service
- Crafting Service → Database Service
- Loot Service
- Stat Service
- Currency Service

**Business Services:**
- Marketplace Service → Currency Service, Inventory Service, Cache Manager
- User Service → Database Service
- Character Service → Database Service

**Additional Dependencies:**
- JWT Service → Redis Service
- Cache Manager → Redis Service

## API Endpoint Structure

### REST API Routes
```
/api/
├── auth/
│   ├── POST /login
│   ├── POST /register
│   ├── POST /refresh
│   └── POST /logout
├── currencies/
│   ├── GET /
│   ├── GET /:id
│   └── GET /player/:playerId
├── survivors/
│   ├── GET /
│   ├── POST /
│   ├── GET /:id
│   └── PUT /:id
├── characters/
│   ├── GET /
│   ├── POST /
│   ├── GET /:id
│   ├── PUT /:id
│   └── DELETE /:id
├── maps/
│   └── GET /
├── spawners/
│   ├── GET /
│   ├── POST /
│   ├── GET /:id
│   ├── PUT /:id
│   └── DELETE /:id
└── marketplace/
    ├── GET /listings
    ├── POST /list
    ├── POST /purchase
    ├── DELETE /listing/:id
    └── GET /user/:userId
```

### WebSocket Actions
```
Real-time Actions:
├── player:position
├── player:attack
├── player:interact
├── player:chat
├── map:change
├── inventory:pickup
├── inventory:drop
├── inventory:transfer
├── inventory:use
├── spawner:pickup
├── spawner:monster_kill
├── spawner:get_entities
├── currency:get_balances
└── crafting:*
    ├── get_recipes
    ├── get_player_recipes
    ├── learn_recipe
    ├── validate
    ├── start
    ├── cancel
    ├── get_sessions
    └── check_completion
```

## Database Schema Overview

### Core Tables
- **users** - User accounts and authentication
- **characters** - Player characters and their attributes
- **survivors** - Survivor entities in the game
- **currencies** - Available currency types
- **playerCurrencies** - Player currency balances
- **items** - Game items and their properties
- **inventories** - Player inventories
- **maps** - Game world maps
- **spawners** - Entity spawn points
- **spawnedEntities** - Currently spawned entities
- **monsters** - Monster definitions
- **capturedMonsters** - Player-captured monsters
- **marketplaceListings** - Marketplace item listings
- **recipes** - Crafting recipes
- **sessions** - User session management
- **worldState** - Global game state

## Security & Performance Features

### Security Measures
- JWT-based authentication with refresh tokens
- Rate limiting on API endpoints and WebSocket actions
- Input validation and sanitization
- Character ownership validation
- Token verification for WebSocket connections
- User restriction/banning system

### Performance Optimizations
- Redis caching for frequently accessed data
- Connection pooling for database operations
- WebSocket connection management with heartbeat
- Rate limiting to prevent abuse
- Periodic cleanup of expired data
- Efficient database queries with Drizzle ORM

## Technology Stack

### Backend Framework
- **Node.js** with **TypeScript**
- **Express.js** for REST API
- **WebSocket (ws)** for real-time communication
- **Drizzle ORM** for database operations
- **PostgreSQL** as primary database
- **Redis** for caching and session management

### External Integrations
- **Sui Blockchain** for blockchain-related operations
- **Swagger/OpenAPI** for API documentation

### Development Tools
- **ts-node-dev** for development
- **Winston** for logging
- **Helmet** for security headers
- **CORS** for cross-origin requests
- **Zod** for runtime validation

## Request Flow Examples

### 1. User Login Process
1. Client sends POST request to `/api/auth/login` with credentials
2. Auth Controller validates credentials against PostgreSQL database
3. JWT Service generates access and refresh tokens
4. Refresh token stored in Redis cache
5. Tokens returned to client for future authentication

### 2. Real-time Game Action (e.g., Player Movement)
1. Client sends WebSocket message with player position data
2. WebSocket Manager validates connection and applies rate limiting
3. Game Action Handler verifies character ownership and action validity
4. Game Manager processes the position update
5. Player Position Service updates the player's location
6. Database updated with new position
7. Position broadcast to other players in the same area
8. Success response sent back to client

### 3. Marketplace Item Purchase
1. Client sends POST request to `/api/marketplace/purchase`
2. Marketplace Controller forwards to Marketplace Service
3. Currency Service checks if player has sufficient funds
4. Database transaction begins
5. Currency deducted from buyer's account
6. Item transferred from seller's to buyer's inventory
7. Marketplace listing updated as sold
8. Transaction committed and success response returned
