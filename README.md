# UnityOuroborosLine

Project Overview

A solo-developed Unity survival stealth game set in a post-apocalyptic wasteland, where all survivors are trapped on a non-stop, class-segregated train with no escape. 

Technical Architecture & Key Decisions

1. ECS Architecture (Component Pattern)
Rationale: It enhances scalability—adding new carriages or NPC types only requires extending component systems, no core logic rewrite.
2. FSM State Machine (State Pattern) (Future: Behavior Tree)
Decision: Implemented a hierarchical state machine for NPC AI, paired with a node-based dialogue system.
Rationale: NPCs must respond dynamically to player actions, time passage, and social changes. The hierarchical state machine provides clear, maintainable control over NPC behaviors (idle, work, protest, trade).
3. Separate Carriage Loading (including the carriage model and the items and npcs data inside the carriage)
Decision: Each carriage is loaded asynchronously as an independent additive model, with the main game scene managing global systems (time, player state, cross-carriage events).
Rationale: 9 carriages form a large segmented world. Modular carriage loading reduces initial load time, supports targeted updates (e.g., reloading only one carriage after player actions; only load the carriage that the player is currently inside and the carriagess that are next to the player's current carriage), and simplifies level design collaboration. Unloading unoccupied carriages optimizes memory efficiency.
4. Inventory System (Tree Structure)
Tree-structured inventory with nested backpacks, each divided into n*m grids. Items occupy x*y grids (no placement if insufficient space). Both players and NPCs have inventories—players can steal or purchase items from NPC backpacks.
5. Excel Configuration System
Designed 6 core Excel configuration tables (Map, Achievement, Item, Multilingual, Dialogue, NPC) with Python scripts to automate conversion to JSON files and C# classes. This allows designers to adjust game content (e.g., item properties, NPC behaviors, dialogue) without relying on programmers.
- Map Table: Carriage models, music, item/NPC IDs and positions.
- Item Table: Item names, grid size, energy boost, favorability impact, unlock requirements, prices, model paths.
- Achievement Table: Names, descriptions, rewards, and completion conditions.
- Multilingual Table: English-Chinese translations for in-game text.
- Dialogue Table: NPC dialogue and conversation flow (next dialogue jumps).
- NPC Table: Avatar/model paths, movement speeds, alert values, initial energy, random states, and interaction functions.
6. YooAsset Resource Management (similar to Addressables)
Managed all resources (JSON configs, character/item models, music, UI images) via YooAsset, packaged into the StreamingAssets folder. The game’s start screen only loads a minimal UI, with subsequent resources loaded dynamically. Supports future hot updates by separating core code from resources.
7. Persistent Save System
Implements world state persistence, recording: NPC inventory items, NPC stats (energy, favorability, charm, alert), conversation history, player inventory, item positions in carriages, NPC equipment, player’s held item, NPC positions, carriage object states (lockers, doors), riot data per carriage, and current player carriage ID. Auto-saves on exit and auto-loads on re-entry. The data are stored in json format.
8. Design Patterns
- Singleton Pattern: Map Manager, Event Manager, Music Manager.
- Object Pool Pattern: Manages UI prefabs (e.g., inventory items, grid cells) for efficient instantiation/hiding.
- Facade Pattern: Map data management class with public interfaces (initialize, get/save data) and private implementation details hidden.
- Event Bus (Event Center): Singleton class for unified event listening and dispatching.
- Strategy Pattern: Abstracted AnimController interface with three implementations (2D frame animation, 2D Spine animation, 3D animation). FSM calls interface methods (no dependency on 2D/3D) for seamless runtime switching.
9. Additional Technical Features
- Scriptable Objects: Manages player/NPC state enums and corresponding FSM state class names for smooth state transitions.
- Technical Documentation: Created project documentation guiding designers on Excel config modification, import processes, and implemented features.
- Git Version Control: Used Git/GitHub Desktop for code management and collaboration.
- Dialogue Recording & Rewards: Records all player-NPC conversations (via dialogue IDs from JSON) and supports reward collection.
- Multilingual System: English-Chinese toggle for in-game UI text.
- Testing Panel: In-game panel for developers/designers to test (add items to carriages/NPC inventories, spawn NPCs, use items).
- String Management: Centralized management of scattered strings (tags, resource paths, event names, layers) via manager classes.

Project Status

This project completes core framework and technical architecture (all systems listed above are fully implemented).
- Note: Due to contractual restrictions, the full project files cannot be shared—this README serves as a detailed overview of the project’s design, and technical implementation.
Development Environment
- Game Engine: Unity
- Programming Language: C#
- Tools/Libraries: ECS (Unity Entities), YooAsset, Python (Excel to JSON/C# conversion), Git
- Supported Features: 2D/3D Animation, Multilingual (English/Chinese), Persistent Saving, Dynamic Events
