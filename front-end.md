# Radiffect RPG Frontend System Architecture

## Architecture Overview

### Application Layers

**React Application Layer:**
- React 19 with TypeScript
- React Router for navigation
- React Hot Toast for notifications
- Tailwind CSS for styling

**Authentication & State Management:**
- AuthContext for global authentication state
- Suiet Wallet Kit for Web3/Sui wallet integration
- Local storage for token management

**Page Components:**
- LandingPage (home/marketing)
- Marketplace (item trading hub)
- Buy/Sell pages (marketplace transactions)
- GamePage (main game interface)
- UtilityPage (development tools)
- WebSocketDebug (development debugging)

**Game Engine Layer (Phaser.js):**
- Phaser 3.88.2 game engine
- Matter.js physics engine (built into Phaser)
- Phaser NavMesh plugin for pathfinding
- Rex UI plugin for game UI components

**Game Scenes:**
- BootScene (initial loading)
- MainMenuScene (game menu)
- CharacterScene (character management)
- GameScene (main gameplay)
- UIScene (game UI overlay)

**Game Systems (ECS Architecture):**
- Entity-Component-System pattern
- 25+ specialized systems for game logic
- Real-time multiplayer synchronization

**API Communication Layer:**
- HTTP Client (Axios) for REST API calls
- WebSocket Service for real-time communication
- Domain-specific API modules (auth, characters, marketplace, maps)

**Service Layer:**
- WebSocketService (real-time communication)
- InventoryService (item management)
- CraftingService (crafting system)
- SpawnerService (entity spawning)

**External Integrations:**
- Sui Blockchain via @mysten/sui
- Wallet integration via @suiet/wallet-kit
- 3D graphics via @splinetool/react-spline

## Application Flow Patterns

### 1. Application Initialization Flow
```
Browser → Main.tsx: Application bootstrap
Main.tsx → WalletProvider: Initialize wallet context
WalletProvider → AuthProvider: Setup authentication
AuthProvider → App.tsx: Initialize routing
App.tsx → React Router: Setup navigation
React Router → ErrorBoundary: Wrap with error handling
ErrorBoundary → Page Components: Render active route
```

### 2. Authentication Flow
```
User → LandingPage: Access application
LandingPage → Suiet Wallet: Connect wallet
Suiet Wallet → AuthContext: Update auth state
AuthContext → API/Auth: Validate with backend
API/Auth → HTTP Client: Send auth request
HTTP Client → Backend: POST /api/auth/login
Backend → HTTP Client: Return JWT tokens
HTTP Client → Local Storage: Store tokens
Local Storage → AuthContext: Update authentication state
AuthContext → Protected Routes: Enable access
```

### 3. Game Initialization Flow
```
User → GamePage: Navigate to game
GamePage → Game.tsx: Initialize game component
Game.tsx → Phaser Main: Start game engine
Phaser Main → BootScene: Load game assets
BootScene → MainMenuScene: Show game menu
MainMenuScene → CharacterScene: Character selection
CharacterScene → GameScene: Enter main game
GameScene → ECS Systems: Initialize game systems
ECS Systems → WebSocket Service: Connect real-time
WebSocket Service → Backend: Establish WebSocket connection
Backend → WebSocket Service: Confirm connection
WebSocket Service → Game Systems: Enable real-time features
```

### 4. Real-time Game Action Flow
```
User Input → Phaser Input System: Capture player action
Input System → Game Systems: Process action locally
Game Systems → WebSocket Service: Send action to server
WebSocket Service → Backend: Real-time action message
Backend → Game Logic: Validate and process action
Backend → Database: Update game state
Backend → WebSocket Service: Broadcast updates
WebSocket Service → Game Systems: Receive updates
Game Systems → Phaser Renderer: Update visual representation
Phaser Renderer → Browser: Display changes
```

### 5. Marketplace Transaction Flow
```
User → Marketplace Page: Browse items
Marketplace Page → API/Marketplace: Fetch listings
API/Marketplace → HTTP Client: GET /api/marketplace/listings
HTTP Client → Backend: Request marketplace data
Backend → HTTP Client: Return listings
HTTP Client → Marketplace Page: Display items
User → Buy/Sell Page: Initiate transaction
Buy/Sell Page → API/Marketplace: Submit transaction
API/Marketplace → HTTP Client: POST /api/marketplace/purchase
HTTP Client → Backend: Process transaction
Backend → HTTP Client: Return transaction result
HTTP Client → React Hot Toast: Show success/error
React Hot Toast → User: Display notification
```

## Component Architecture

### React Component Hierarchy
```
App.tsx (Root)
├── Router Configuration
├── Protected Routes
│   └── GamePage
│       └── Game.tsx (Phaser Integration)
├── Public Routes
│   ├── LandingPage
│   ├── Marketplace
│   │   ├── ListingCard
│   │   └── MonsterCarousel
│   ├── Buy Page
│   ├── Sell Page
│   └── WebSocketDebug (Dev only)
├── Shared Components
│   ├── LoadingSpinner
│   ├── ErrorBoundary
│   ├── ProtectedRoute
│   └── UsernameModal
└── Development Components
    ├── UtilityPage
    └── DebugNavigation
```

### Phaser Game Architecture
```
Game Engine (Phaser.js)
├── Scene Manager
│   ├── BootScene (Asset Loading)
│   ├── MainMenuScene (Game Menu)
│   ├── CharacterScene (Character Management)
│   ├── GameScene (Main Gameplay)
│   └── UIScene (Game UI Overlay)
├── ECS Systems
│   ├── Core Systems
│   │   ├── MovementSystem
│   │   ├── VelocitySystem
│   │   ├── PhysicsOptimizationSystem
│   │   └── InputPredictionSystem
│   ├── Combat Systems
│   │   ├── AttackSystem
│   │   ├── DetectionSystem
│   │   ├── TargetSystem
│   │   └── RetreatSystem
│   ├── AI Systems
│   │   ├── AIStateSystem
│   │   ├── PatrolSystem
│   │   ├── FollowSystem
│   │   └── NPCSpawnSystem
│   ├── Game Logic Systems
│   │   ├── SpawnerSystem
│   │   ├── ItemPickupSystem
│   │   ├── TriggerSystem
│   │   └── EntityInteractionSystem
│   ├── Visual Systems
│   │   ├── AnimationSystem
│   │   ├── VisualSystem
│   │   └── DirectionSystem
│   └── Utility Systems
│       ├── MapSystem
│       ├── DialogSystem
│       ├── StaminaSystem
│       ├── CharacterSystem
│       ├── CollisionHandlingSystem
│       ├── NavMeshManager
│       └── InterpolationSystem
├── Physics Engine (Matter.js)
│   ├── Collision Detection
│   ├── Physics Bodies
│   └── Constraint System
└── Plugins
    ├── NavMesh Plugin (Pathfinding)
    └── Rex UI Plugin (Game UI)
```

### Service Layer Architecture
```
Services Layer
├── Communication Services
│   ├── WebSocketService
│   │   ├── Connection Management
│   │   ├── Message Handling
│   │   ├── Reconnection Logic
│   │   └── Token Management
│   └── HTTP Client
│       ├── Request Interceptors
│       ├── Response Interceptors
│       ├── Authentication Headers
│       └── Error Handling
├── Game Services
│   ├── InventoryService
│   │   ├── Item Management
│   │   ├── Inventory Operations
│   │   └── Item Validation
│   ├── CraftingService
│   │   ├── Recipe Management
│   │   ├── Crafting Sessions
│   │   └── Material Validation
│   └── SpawnerService
│       ├── Entity Spawning
│       ├── Spawn Point Management
│       └── Entity Lifecycle
└── API Modules
    ├── Auth API
    ├── Characters API
    ├── Marketplace API
    └── Maps API
```

## Data Flow Patterns

### State Management Flow
```
User Action → React Component → Local State Update
Local State → useEffect Hook → Side Effect Trigger
Side Effect → API Service → HTTP/WebSocket Request
API Response → React Component → State Update
State Update → React Re-render → UI Update
```

### Game State Synchronization
```
Local Game Action → ECS System → Local State Update
Local State → WebSocket Service → Server Message
Server Validation → Server State Update → Database Persistence
Server State → WebSocket Broadcast → Client WebSocket
Client WebSocket → ECS System → Game State Sync
Game State → Phaser Renderer → Visual Update
```

### Authentication State Flow
```
Wallet Connection → AuthContext → Authentication Request
Auth Request → Backend Validation → JWT Token Generation
JWT Token → Local Storage → HTTP Client Headers
HTTP Headers → Protected Route Access → Component Rendering
Token Expiry → Auto-refresh → New Token Storage
```

## Technology Stack

### Frontend Framework
- **React 19** - UI framework with hooks and context
- **TypeScript** - Type safety and development experience
- **Vite** - Build tool and development server
- **React Router DOM** - Client-side routing
- **Tailwind CSS** - Utility-first CSS framework

### Game Engine
- **Phaser.js 3.88.2** - 2D game framework
- **Matter.js** - Physics engine (integrated with Phaser)
- **Phaser NavMesh** - Pathfinding plugin
- **Rex UI Plugin** - Game UI components

### State Management
- **React Context** - Global state management
- **Local Storage** - Persistent data storage
- **React Hooks** - Component-level state

### Communication
- **Axios** - HTTP client for REST API calls
- **WebSocket API** - Real-time bidirectional communication
- **React Hot Toast** - Notification system

### Web3 Integration
- **@mysten/sui** - Sui blockchain SDK
- **@suiet/wallet-kit** - Sui wallet integration

### Development Tools
- **ESLint** - Code linting
- **TypeScript Compiler** - Type checking
- **Vite Dev Server** - Hot module replacement

### Additional Libraries
- **GSAP** - Animation library
- **Lucide React** - Icon library
- **React Icons** - Additional icons
- **Swiper** - Touch slider component
- **React Scroll Parallax** - Parallax effects
- **Spline** - 3D graphics integration

## Request Flow Examples

### 1. Character Selection Process
1. User navigates to GamePage through protected route
2. ProtectedRoute validates authentication via AuthContext
3. GamePage renders Game.tsx component
4. Game.tsx initializes Phaser game engine
5. BootScene loads assets and transitions to CharacterScene
6. CharacterScene fetches characters via Characters API
7. HTTP Client adds authentication headers automatically
8. Backend returns character data
9. CharacterScene displays character selection UI
10. User selects character and enters GameScene

### 2. Real-time Inventory Pickup
1. Player clicks on item in GameScene
2. EntityInteractionSystem detects pickup action
3. ItemPickupSystem validates pickup possibility
4. WebSocketService sends pickup action to server
5. Backend validates action and updates database
6. Backend broadcasts inventory update to relevant players
7. WebSocketService receives inventory update
8. InventoryService processes the update
9. Game UI reflects new inventory state
10. Visual systems update item display

### 3. Marketplace Item Listing
1. User navigates to Sell page
2. Sell page component loads user's inventory
3. InventoryService fetches items via API
4. User selects item and sets price
5. Marketplace API creates listing via HTTP Client
6. Backend validates item ownership and creates listing
7. Success notification displayed via React Hot Toast
8. User redirected to marketplace to see listing
9. Marketplace page refreshes listings data
10. New listing appears in marketplace UI

## Security & Performance Features

### Security Measures
- JWT token-based authentication
- Automatic token refresh handling
- Protected route guards
- Input validation on all forms
- Secure WebSocket connection with token verification
- Error boundary for crash protection

### Performance Optimizations
- Phaser.js physics optimization system
- WebSocket connection pooling and reconnection
- HTTP request caching and interceptors
- Component-level state management
- Lazy loading of game assets
- Efficient ECS system updates
- Input prediction for responsive gameplay

### Development Features
- Hot module replacement via Vite
- TypeScript for type safety
- ESLint for code quality
- Development-only debug pages
- WebSocket debugging interface
- Utility page for testing
- Error logging and monitoring
