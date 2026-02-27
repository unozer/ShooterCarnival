# Gemini Context: ShooterCarnival

This document provides a high-level overview of the ShooterCarnival project, its architecture, and development conventions to be used as a context for AI-assisted development.

## Project Overview

ShooterCarnival is a 2D vertical shoot-'em-up game being developed with the **Godot Engine (v4.5)**. The project is open-source and community-driven, with a strong emphasis on being beginner-friendly, particularly for developers familiar with Python, due to its use of **GDScript**.

The game is inspired by classic arcade shooters like *Gradius*, *R-Type*, and *TwinBee*. The project's goal is to create a feature-rich, fun-to-play game while providing a learning platform for new game developers.

## Building and Running

This is a standard Godot project. There are no command-line build or run scripts.

*   **Running the Game:** The project can be opened and run directly from the Godot Engine (version 4.5 or later). The main scene is `src/scenes/ui/main_menu.tscn`.
*   **Building for Release:** Game builds for different platforms (e.g., Web, Windows, Linux) are created using the `Project > Export...` menu in the Godot editor. The `src/export_presets.cfg` file stores the configuration for these exports.

## Architecture and Development Conventions

The project follows a modular and decoupled architecture.

### Scene Structure

*   **Main Entry:** The application starts with the main menu (`src/scenes/ui/main_menu.tscn`).
*   **Game Container:** The main game scene (`src/scenes/main.tscn`) acts as a container. Its primary roles are to manage the parallax scrolling background and to load the current gameplay stage.
*   **Stages:** Individual levels are implemented as separate "stage" scenes (e.g., `src/scenes/stages/stage_01.tscn`). Each stage is responsible for its own logic, including spawning the player, enemies, and the UI/HUD for that level.

### Core Architectural Patterns

*   **Component-Based Design:** The project makes use of reusable components to encapsulate specific functionalities. A key example is the `src/components/health/health_component.gd`, which can be attached to any entity (player, enemies) to give it health and damage-taking capabilities.
*   **Message Bus (Observer Pattern):** A central message bus is used for decoupled communication between game objects.
    *   **Implementation:** The `MessageBus` class is defined in `src/components/messages/message_bus.gd`.
    *   **Global Instance:** A global instance named `CombatBus` is preloaded in `src/scripts/global_utils.gd` (from `src/resources/combat_bus.tres`) and is available throughout the project as `GlobalUtils.CombatBus`.
    *   **Usage:** This bus is used to publish game-wide events, such as `PLAYER_DAMAGED` or `ENEMY_DIED`. Other nodes (like the HUD for updating the score) can subscribe to these events without being directly coupled to the sender.

### File and Folder Structure

The `src` directory is organized logically:

*   `assets/`: Contains all art and audio assets.
*   `components/`: For reusable nodes/scripts that provide specific functionalities (e.g., health).
*   `entities/`: Contains scenes and scripts for all game objects, like the player ship, enemies, and bullets.
*   `scenes/`: Contains the main game scenes, stages, and UI screens.
*   `scripts/`: For globally accessible scripts. `global_utils.gd` is an autoloaded script, making its functions and variables globally available.
*   `resources/`: For saved Resource files (`.tres`), like the shared `combat_bus.tres`.
