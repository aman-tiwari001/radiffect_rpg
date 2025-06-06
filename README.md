<p align="left">
  <img src="https://github.com/user-attachments/assets/45a16bcf-a280-48e1-a595-24ea156031f0" alt="Radiffect Logo" width="115"/>
  <img src="https://github.com/user-attachments/assets/174e060d-d5ff-4f57-9e46-f793404ed450" alt="Radiffect Logo" width="300"/>
</p>

<h1 align="left">Radiffect: A Post-Apocalyptic Retro-Style RPG on 💧SUI Network</h1>
<p align="left">
  <a href="https://radiffect.com/" target="_blank">
    🌐 <strong>radiffect.com</strong>
  </a> 
|
  <a href="https://youtu.be/JjeQP2_9Pw8?si=-aynU3suhGr382wc" target="_blank">
     <strong>Demo Video</strong>
  </a> 
|
  <a href="https://youtu.be/AjZCxK22XMw" target="_blank">
     <strong>Code Walk-through Video</strong>
  </a> 
|
  <a href="https://www.canva.com/design/DAGpSvCmEjc/kVbofkSNqvKzEc3GAHvseQ/edit?utm_content=DAGpSvCmEjc&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton" target="_blank">
     <strong>Pitch Deck</strong>
  </a>
</p>
  
**Note**: *We’ve kept the main project repository private due to security reasons. However, for judging purposes, we have included a detailed system design architecture in readme files. Additionally, a brief code walk-through video is provided for reference.*

**Radiffect** is a retro-style RPG where adventure, strategy, and survival collide in a post-apocalyptic world transformed by a sudden surge of inner radiation from Mother Earth. The catastrophe wiped out most of humanity, and the radiation mutated every living being on the planet. As one of the few survivors, your mission is to become the ultimate hunter by capturing, taming, and mastering these mutated creatures. Radiffect combines classic RPG with strategic item collection and a dynamic in-game marketplace for trading collectibles. 

---

## 🌍 Game Synopsis

> *From the Earth’s very heart… it awoke. A surge of inner radiation mutates life as we know it.*
> *Humans vanished. Nature thrived, twisted. Welcome to the world of Radiants.*

In the aftermath of a catastrophic radiation outbreak, the world is overrun with mutated creatures called **Radiants**. As a survivor, you must explore, adapt, and conquer this new realm. Craft items, capture and tame Radiants, complete quests, and earn digital assets while navigating an immersive and dangerous world.

---

## 🚀 Key Features

* **Choose Your Hunter**: Play as Jack, Jasmine, or James — each with unique abilities.
* **Combat**: Real-time battles with mutated monsters.
* **Exploration**: Discover handcrafted post-apocalyptic maps.
* **Quests & NPCs**: Engage in rich dialogue and story arcs with survivors.
* **Crafting**: Gather resources and craft tools, weapons, and survival items.
* **Radiant Capture System**: Capture, tame, and fight alongside Radiants.
* **Marketplace**: Buy, sell, and trade collectibles and NFTs within the game.

---

## 🧠 Tech Stack

*Note: for more detailed system design and architecture please refer to [front-end.md](https://github.com/aman-tiwari001/radiffect_rpg/blob/main/front-end.md) and [back-end.md](https://github.com/aman-tiwari001/radiffect_rpg/blob/main/back-end.md)*

### 🖥 Frontend

* **React 19** – UI rendering and SPA logic.
* **Tailwind CSS** – Modern, utility-first styling.
* **Axios** – REST API calls.
* **GSAP** – Smooth animations and transitions.

### 🎮 Game Engine

* **Phaser.js** – Core 2D game engine.
* **Matter.js** – Physics simulation and collision detection.
* **Phaser NavMesh** – Intelligent NPC and player pathfinding.
* **RexUI** – Phaser-based UI component suite.

### 🛠️ Backend

* **Node.js + TypeScript** – Server logic and real-time orchestration.
* **Express.js** – RESTful APIs.
* **WebSocket (ws)** – Real-time game events and multiplayer features.
* **Drizzle ORM** – Type-safe database operations.
* **PostgreSQL** – Persistent data storage.
* **Redis** – Caching and session management.

### 💧 SUI Blockchain Integration

#### 🔧 SDKs & Libraries
* **`@suiet/wallet-kit`** – Enables seamless integration with SUI wallets like Suiet, Slush, and others for user authentication.
* **`@mysten/sui`** – Official TypeScript SDK to interact with the SUI blockchain for executing and verifying transactions.
 
#### ⚙️ SUI Primitive Features
* **Wallet Authentication** – Secure login using supported SUI wallets (Slush, Suiet)
* **On-Chain Transactions** – All in-game transactions (purchases, marketplace, fee, trading, etc) are executed SUI Testnet.
* **Transaction Verification** – Real-time verification using SUI RPC endpoints.
* **Server-Side Execution** – Backend handles transactions via the SUI SDK for secure and automated processing.

### 🛡 Dev Tooling

* **ts-node-dev** – Live TypeScript development.
* **Helmet.js** – Secure HTTP headers.
* **Zod** – Input validation.
* **Swagger/OpenAPI** – Auto-generated API documentation.

---

## 🪙 Monetization Strategy

* **NFT Sales**: Hunter Licenses and special in-game items.
* **In-Game Battle Pass**: Unlock exclusive Radiants, rewards, and maps.
* **Royalties**: Revenue from secondary NFT sales.
* **Limited Editions**: Packs, Radiants, and cosmetic drops.
* **Ad Boards**: In-game real estate for promotional banners.

---

## 📬 Contact

* Team **100x**Engineers
* Email: [radiffect@gmail.com](mailto:radiffect@gmail.com)
* Discord: [https://discord.gg/bQmXEHu5HZ](https://discord.gg/bQmXEHu5HZ)
* Twitter: [@radiffect](https://x.com/radiffect)

*Note: We’ve kept the project repository private due to security reasons. However, for judging purposes, we have included a detailed system design architecture in readme files. Additionally, a brief code walkthrough video highlighting key components is provided for reference.*
