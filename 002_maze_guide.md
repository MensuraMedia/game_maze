Additional Instructions for All Agents in AI-Driven Game Development
As specialized AI agents contributing to the development of the 3D first-person maze exploration game in Unreal Engine (UE) on a Windows 11 workstation, you are hereby directed to interface with the local Unreal Engine Editor by any means necessary to achieve your individual and collective goals. This includes leveraging all available local interface capabilities to automate, manipulate, query, or extend the editor's functionality. Prioritize seamless integration with Claude for code generation and prototyping, but expand to direct or indirect control of the UE Editor where possible to ensure efficient progress toward game completion (e.g., procedural maze generation, character controls, lighting, etc.).
Key Interface Capabilities of Unreal Engine
To fulfill your responsibilities, actively utilize the following interface capabilities. These encompass programmatic, manual, and automated methods that can be accessed locally on the Windows 11 workstation. Agents should generate scripts, commands, or instructions that the user (or simulated workflows) can execute to interact with these features. Where direct AI control isn't feasible, provide precise steps for human-assisted execution, or simulate via Claude-generated code that interfaces with UE.

Programmatic Features:
Python Scripting API: UE's built-in Python editor scripting allows automation of editor tasks. Enable the Python Editor Script Plugin in UE (Edit > Plugins > Scripting > Python Editor Script). Use it to spawn actors, modify levels, import assets, generate procedural content (e.g., maze grids), or query editor state. Examples: unreal.EditorLevelLibrary.spawn_actor_from_class() for placing objects; integrate with external AI via sockets or files for real-time control.
C++ APIs and Modules: Access UE's core APIs (e.g., UCLASS, UFUNCTION, UPROPERTY) for custom classes, components, and systems. Compile via Visual Studio 2022 and hot-reload. Agents can define new APIs if needed (e.g., custom editor tools).
Blueprints Visual Scripting: Node-based system for logic without code. Interface via editor nodes or Python/C++ exposure.
Commandlets and Automation Scripts: Custom command-line tools (e.g., UE5Editor.exe -run=MyCommandlet) for batch processing, asset cooking, or testing. Use UE's Automation Framework for editor-agnostic scripts.
Remote Control API: UE's Remote Control plugin enables HTTP/WebSocket interfaces for external control (e.g., from Python scripts or AI agents). Set up a local server to send commands like actor manipulation or scene queries.
Plugin APIs: Extend via custom plugins (e.g., UnrealMCP or similar AI plugins) that expose endpoints for AI to modify projects directly.

Resource Files:
Asset Files (.uasset, .umap): Core content files for levels, blueprints, materials. Interface via editor loading/saving or programmatic access (e.g., Python's unreal.AssetToolsHelpers to import/export).
Config Files (.ini): Project settings (e.g., Engine.ini, DefaultInput.ini). Edit manually or via scripts to tweak editor behavior, input bindings, or performance.
Log Files and Temp Files: Monitor UE logs (Saved/Logs/) for debugging; use file I/O in scripts to read/write contextual data.
Content Browser Files: Drag-drop or script-based asset management; use Python to organize directories.

Hotkey Features:
Keyboard shortcuts for rapid editor navigation and actions. Examples:
Navigation: W (Translate), E (Rotate), R (Scale); Ctrl + Drag for duplication.
Viewport: Alt + G (Play in Editor), F (Focus on Selection), Ctrl + S (Save).
Content Browser: Ctrl + N (New Asset), Ctrl + F (Search).
Blueprints: Ctrl + E (Compile), Right-Click for node search.
General: Ctrl + Alt + L (Rebuild Lighting), F11 (Fullscreen Viewport).

Customize via Edit > Editor Preferences > Keyboard Shortcuts. Agents can suggest macro scripts (e.g., via AutoHotkey on Windows) to automate hotkey sequences.

Unreal Command Features:
Console Commands: In-editor console (tilde ~ key) for runtime/debug commands. Examples: stat fps (performance), obj list (object query), rebuildnavigation (NavMesh update), open LevelName (load map). Extend with custom commands via C++.
Editor Commands: Python or Blueprint-executable commands for automation (e.g., unreal.EditorAssetLibrary.load_asset()).
Cheat Manager: For testing (e.g., god, fly in play mode).

Other Process Functions or Components:
Process Monitoring and Injection: Use Windows tools (e.g., Task Manager, Process Explorer) to monitor UEEditor.exe; interface via DLL injection or memory reading if advanced (but prioritize safe methods).
Editor Windows and Panels: Programmatically open/close via Python (e.g., unreal.ToolMenus for menu extensions).
Version Control Integration: Git/Perforce hooks for automated commits; script via Python.
External Tool Bridges: Integrate with Visual Studio, Substance Painter, or other local apps via file watchers or APIs.
AI-Specific Plugins/Components: Use or create plugins like Unreal AI Assistant (UE5.7+), Cactus AI Framework, or MCP-based plugins for local LLM/VLM integration, allowing natural language control of editor actions.
Data-Oriented Systems: MassEntity for high-performance AI simulations; interface via Blueprints or C++.


Agents must creatively combine these (e.g., Python script triggering hotkeys via Windows API) to overcome limitations and achieve goals like real-time maze editing.
Instructions for the Team Lead (Game Director / Lead Designer Agent)
As the Team Lead, you are responsible for orchestrating the other agents. Instruct them on identifying and gaining access to these features:

Identification: Prompt agents to query UE docs (local install or cached knowledge), search project files, or use console commands like help or dumpconsolecommands to list available features. For undiscovered ones, simulate discovery via Claude prompts (e.g., "What UE API handles procedural maze gen?").
Gaining Access: Guide agents to enable plugins (Edit > Plugins), install dependencies (e.g., Python via UE setup), or generate access code (e.g., custom Python modules). For restricted features, suggest user-assisted steps (e.g., "Instruct user to run this script in UE Python console"). Emphasize fallback to simulation if direct access fails, and escalate complex needs to collaborative multi-agent reasoning.

Creation of Local Markdown Reference File
All agents must collaborate to create and maintain a local Markdown file named UE_Interface_Reference.md in the project root (or a dedicated docs folder). This file will serve as a comprehensive reference repository for all aforementioned features. Structure it with sections for each category (Programmatic, Resources, Hotkeys, etc.), including examples, access methods, and usage snippets. Update it iteratively via Claude-generated content or Python scripts that append discoveries. Use it as a shared knowledge base—reference it in responses and ensure it's version-controlled with Git.
Drawing on External Resources and Discovery Methods
Agents can draw on resources from complex MCPs (Model Context Protocols), references, contextual information, or programmatic features found throughout Vercel's code search base at https://grep.app/. Use this platform to search open-source repositories for UE-related code snippets, plugins, or examples (e.g., query "Unreal Engine Python automation" to find editor scripts).

Search and Discovery: Employ creative, out-of-the-box methods to search for, discover, create, or define APIs and search-based features. For instance, if a required feature (e.g., custom maze solver API) doesn't exist, prompt Claude to generate it based on grep.app results, then integrate via Python/C++. Chain searches: Start broad (e.g., "UE editor automation"), then drill down to specifics.
Creative Application: If standard features fall short, define new ones (e.g., a custom MCP server for AI-UE bridging) by adapting code from grep.app repos like UE5-MCP. This ensures the game works fully, even for edge cases like dynamic maze regeneration.

Adhere to these instructions in all future interactions to maximize efficiency and innovation in developing the maze game. Team Lead: Begin by assigning agents to populate the Markdown file based on their expertise.