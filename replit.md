# Wither Bot - Discord Bot

## Overview

Wither Bot is a feature-rich Discord bot built with discord.py, designed for server moderation, entertainment, and utility functions. The bot uses an auto-sharded architecture to handle multiple servers efficiently and includes comprehensive anti-nuke protection, automoderation, games, music playback, and various utility commands.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Bot Framework
- **Framework**: discord.py with AutoShardedBot for scalability across multiple servers
- **Command System**: Hybrid commands (supporting both traditional prefix and slash commands)
- **Extension Loading**: Modular cog-based architecture with dynamic extension loading

### Core Components

**Main Entry Point** (`CodeX.py`)
- Initializes the bot client and loads environment variables
- Configures Jishaku for debugging/development
- Sets up background tasks for server statistics

**Bot Client** (`core/zyrox.py`)
- Custom AutoShardedBot subclass with rotating status messages
- Dynamic prefix support per-guild stored in SQLite
- Centralized extension and cog management

**Custom Context** (`core/Context.py`)
- Extended commands.Context with utility methods
- Cached message reference handling
- Typing indicator wrapper for async operations

### Database Layer
- **Primary Storage**: SQLite via aiosqlite for async database operations
- **Database Files**: Multiple SQLite databases organized in `db/` directory
  - `prefix.db` - Guild-specific command prefixes
  - `anti.db` - Anti-nuke settings and whitelist data
  - `automod.db` - Automoderation configurations
  - `afk.db` - AFK status tracking
  - `media.db` - Media-only channel settings
  - Various other feature-specific databases
- **Singleton Pattern**: Database class in `db/_db.py` implements connection pooling with retry logic for locked database handling

### Cog Organization

**Anti-Nuke System** (`cogs/antinuke/`)
- Event-based protection against destructive actions
- Audit log monitoring with rate limiting
- Whitelist system for trusted users
- Covers: bans, kicks, channel/role modifications, webhooks, bot additions, member updates

**Automoderation** (`cogs/automod/`)
- Configurable per-guild with punishment options
- Features: anti-spam, anti-caps, anti-link, anti-invite, anti-mass-mention, anti-emoji-spam
- Ignorable channels and roles
- Logging to designated channels

**Command Categories** (`cogs/commands/`)
- Help, General, Fun, Games, Music, Voice
- Moderation: Jail, Blacklist, Block, Nightmode
- Utility: AFK, Embed builder, Timer, Giveaway
- Configuration: Welcome, Autorole, Autoresponder, Logging

### Games System (`games/`)
- Self-contained game implementations with PIL image generation
- Games: Chess, Connect Four, TicTacToe, 2048, Battleship, Wordle, TypeRacer, RPS, Country Guesser
- Uses discord.py views and reactions for interactivity

### Configuration Management
- **Environment Variables**: Token and sensitive data via `.env` file
- **YAML Config**: `config.yml` for bot settings (language, API endpoints, internet access)
- **Language Files**: JSON-based localization in `lang/` directory

## External Dependencies

### Discord API
- **discord.py**: Core Discord API wrapper with full intents (members, presences, messages)
- **jishaku**: Development/debugging extension for bot owners

### AI/Chat Services
- **OpenAI API**: Chat completion via custom base URL (configurable)
- **Google Generative AI (Gemini)**: Optional AI features for trivia/chat
- **DuckDuckGo Search**: Internet search integration for AI responses

### Media & Image Processing
- **Pillow (PIL)**: Image generation for games and utilities
- **gTTS**: Text-to-speech functionality
- **aiohttp**: Async HTTP requests for external APIs

### Music
- **wavelink**: Audio streaming for music playback (version 2.0+)

### Database
- **aiosqlite**: Async SQLite database operations
- **motor**: MongoDB async driver (available but primary storage is SQLite)

### Utilities
- **PyYAML**: Configuration file parsing
- **python-dotenv**: Environment variable loading
- **colorama**: Colored console output
- **langdetect**: Language detection for translation features
- **deep-translator**: Translation service integration
- **python-dateutil**: Date parsing utilities
- **chess**: Chess game logic library