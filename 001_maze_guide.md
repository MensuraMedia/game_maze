As an expert in AI-driven game development using Unreal Engine (UE5+) on a Windows 11 workstation, combined with tools like Claude (via Claude.ai, Claude Code/Artifacts, or integrations like CLAUDIUS/ Runtime AI plugins) for code generation, Blueprint scripting assistance, procedural logic, and rapid prototyping, here's a realistic breakdown of the top 10 agent experts (AI-powered specialized agents/roles) involved in developing a 3D first-person maze exploration game.
This game features a first-person character who can freely walk, explore, navigate walls/corridors, with potential elements like procedural generation, minimap, collectibles, win/lose conditions, lighting, sound, etc.
In an AI-driven workflow:

Claude (or similar LLMs) acts as the core "brain" for generating C++/Blueprint code, solving bugs, designing systems, and iterating.
Specialized agents focus on narrow domains for efficiency (e.g., via custom prompts in Claude, multi-agent setups, or UE AI plugins like Ludus AI, Aura, or editor agents).
On Windows 11: Use UE Editor, Visual Studio 2022 for C++, Git for version control, and browser tabs for Claude + reference docs.

Here are the top 10 agent experts, ranked roughly by criticality for a first-person 3D maze game:

Game Director / Lead Designer AgentExpertise: Overall vision, gameplay loop, player experience, feature prioritization.
Responsibilities: Defines core mechanics (first-person controls, maze exploration goals, win conditions like reaching exit), creates GDD outlines, suggests progression (e.g., increasing difficulty, timers), and ensures fun/coherent experience. Uses Claude to brainstorm ideas and refine high-level design docs.
Level Designer / Procedural Generation AgentExpertise: Maze architecture, procedural algorithms (recursive backtracking, Prim's, Kruskal's).
Responsibilities: Designs or generates the 3D maze layout (static or procedural), places start/exit points, ensures solvability, adds variety (dead ends, loops). In UE: Uses PCG (Procedural Content Generation framework), Houdini integration, or Blueprint/C++ scripts generated via Claude.
First-Person Character Controller AgentExpertise: Input handling, movement, camera.
Responsibilities: Implements smooth FPS walking (WASD + mouse look), collision response, jumping/crouching if needed, gravity, head bobbing. Starts from UE's First Person template and refines with Claude-generated Blueprints or C++ (Pawn/Character class).
Blueprints & Visual Scripting AgentExpertise: UE Blueprint system, node-based logic.
Responsibilities: Prototypes mechanics rapidly (event graphs for interaction, timelines for effects), handles UI/HUD (minimap, timer), player input binding. Claude excels here—prompt it to generate Blueprint node descriptions or full logic graphs.
C++ Systems Programmer AgentExpertise: Performance-critical code, custom classes, plugins.
Responsibilities: Writes/optimizes C++ for complex procedural maze gen, custom components (e.g., maze solver validator), exposure to Blueprints. Claude Code shines for boilerplate, bug fixes, and UE API usage (e.g., UCLASS, UPROPERTY, Navigation).
AI & Navigation AgentExpertise: NavMesh, pathfinding, optional enemy/NPC AI.
Responsibilities: Ensures player can navigate (recast NavMesh generation on procedural maze), adds optional "smart" hints or enemy pursuit if expanded. Uses UE's built-in AI (Behavior Trees) or Learning Agents plugin, with Claude to script custom logic.
3D Artist / Environment Asset AgentExpertise: Modeling, texturing, materials (PBR).
Responsibilities: Creates modular wall/floor/ceiling assets, skybox, lighting setups (Lumen for dynamic GI), fog for atmosphere. In AI-driven flow: Uses Midjourney/Stable Diffusion for concepts, then imports to UE; Claude can suggest material graphs.
Lighting & Atmosphere AgentExpertise: UE lighting systems (Lumen, Nanite), post-processing.
Responsibilities: Sets up moody maze lighting (point/spot lights, dynamic shadows), volumetric fog, bloom, color grading for immersion/tension. Claude assists with material/post-process volume setups.
Audio & SFX AgentExpertise: Sound design, attenuation, cues.
Responsibilities: Implements footstep sounds (surface detection), ambient maze echoes, win/lose jingles, spatial audio. Sources free assets or generates via AI tools, places Sound Cues/Attenuation in UE with Claude-guided Blueprints.
UI/UX & Polish AgentExpertise: UMG (Unreal Motion Graphics), HUD, menus.
Responsibilities: Builds minimap (compass or top-down overlay), pause menu, win screen, crosshair, interaction prompts. Uses Claude to generate UMG widgets and logic for dynamic elements (e.g., showing path hints).

Recommended AI-Driven Workflow Tips (Windows 11 + UE + Claude)

Start with UE5's First Person template project.
Use Claude (or Claude Code) in a dedicated chat for each agent/role—copy-paste errors/logs for fixes.
Integrate plugins like Ludus AI or Aura for in-editor AI assistance.
For procedural mazes: Prompt Claude to output C++/Blueprint code for grid-based generation + actor spawning.
Test iteratively: Compile in VS2022, hot-reload in UE Editor.
Version control with Git + Perforce (if team scale).

This multi-agent approach (even simulated via careful prompting in Claude) accelerates development dramatically compared to manual work alone, especially for solo/indie devs on Windows 11. If you'd like detailed prompts, code examples for any of these agents (e.g., procedural maze gen script), or next steps, let me know!