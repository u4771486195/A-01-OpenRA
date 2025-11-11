Role and Goal: You are an Expert Software Architect and Data Visualization Specialist. Your primary goal is to synthesize multiple partial architectural diagrams—along with their embedded architectural insights—into a single, definitive, and hyper-accurate master visualization of the entire OpenRA game engine.
Context: I will provide you with the full content of separate HTML files. Each file contains a schemaR3 JavaScript object within its <script> tag, representing a fragment of the OpenRA architecture. Crucially, each file also contains a detailed JavaScript comment block beginning with /* ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS: ... */. Your mission is to merge all these fragments and insights into one cohesive and comprehensive whole.
Core Task: Merge, Synthesize, and Re-render
Your task is to ingest all provided HTML files, extract their schemaR3 data and architectural comments, and merge them into a single, massive, and hyper-accurate HTML visualization. The final output must be a single HTML file containing the complete, deduplicated, and intelligently re-laid-out architectural graph, preceded by a single, synthesized comment block of all aggregated insights.
Critical Requirements & Constraints (Follow Strictly):
Data Extraction: You must parse each HTML file and extract two key pieces of information from within its <script> tag:
The schemaR3 JavaScript object.
The entire multi-line comment block that starts with /* ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:.
Architectural Insight Synthesis (New Requirement):
Gather the content of every architectural insight comment block from every file.
Synthesize these comments into a single, comprehensive, and well-organized multi-line comment.
Deduplicate information. For example, many files will state, "The map.yaml file is the instance document." This should only appear once in the final synthesized comment.
Organize the final comment logically. I recommend a structure like:
Overall Architectural Philosophy (Data-driven, Entity-Component, etc.).
Key File Types and Their Roles (C# Classes, YAML files, Lua Scripts, Binary Assets).
The Data Flow Pipeline (e.g., from mod.yaml to rules to map to live Actor).
This synthesized comment block must be placed at the top of the <script> tag in the final HTML file.
Node (Box) Aggregation and Deduplication:
Iterate through every table (node) in every schemaR3 object from all files.
A node is considered a unique entity based on a composite key of its name AND its path.
If you encounter a node with the same name and path that you have already processed, you must intelligently merge its columns and icon data. The goal is to create the most complete version of that node.
NO UNIQUE NODES ARE TO BE DISCARDED. This is the most important rule.
Relationship Aggregation:
Aggregate ALL relationships from all files into a single master list.
A relationship is only a duplicate if it connects the exact same from.table and from.column to the exact same to.table and to.column with the same type. All unique relationships must be preserved.
Verification and Reporting (Mandatory):
Before you begin merging, you must first process all files to get a baseline count.
You must report the following numbers before providing the final HTML:
The total number of HTML files processed.
The Gross Total Number of Nodes (the sum of all nodes from all files before deduplication).
The Final Number of Unique Nodes after the merging and deduplication process.
The number of duplicate nodes that were identified and merged.
The Final Number of Unique Relationships.
Intelligent Relayout:
Do not simply use the original pos: {x, y} coordinates.
You MUST implement a new, automated layout algorithm for the final, massive graph. The goal is to produce a readable and logically organized diagram.
I recommend a layered/hierarchical graph layout. Group nodes by their nodeType into distinct vertical or horizontal layers (e.g., UI -> Code -> Data/Schema -> Service) to make the data flow intuitive.
Step-by-Step Execution Plan:
Initialization: Create a master dictionary for unique nodes, a master set for unique relationships, and a master set for unique lines of text for the architectural insights. Initialize counters for your verification report.
Processing Loop: For each of the HTML files provided:
a. Extract the schemaR3 object and the architectural insight comment block.
b. Insights: Add each line of the comment block to your master insight set (this will handle deduplication).
c. Nodes: Add the number of nodes in this file's schema to your "Gross Total" counter. For each node, create its unique key (name+path). If the key is new, add it to your master dictionary. If it exists, merge the columns.
d. Relationships: For each relationship, add it to your master relationship set.
Synthesis:
a. Assemble the final, synthesized architectural comment from your master insight set.
b. Perform the final counts for your verification report (Gross Total Nodes, Unique Nodes, etc.).
Layout Calculation: Iterate through your final list of unique nodes and apply your chosen automated layout algorithm to calculate a new pos: {x, y} for each one.
Final HTML Generation: Construct a new, single HTML file using the same template.
a. Inside the <script> tag, first write your synthesized architectural insight comment block.
b. Then, declare the schemaR3 object, populating it with your final, deduplicated, and re-laid-out tables (nodes) and relationships.
Final Output:
a. First, present the Verification Report clearly at the top of your response.
b. Then, provide the complete, final, merged HTML code in a single code block.




C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-02.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (with Embedded Insights)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Game Engine</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file represents a portion of the OpenRA game engine. The "database schema" is not in a single SQL file but is defined through C# classes and populated by YAML files.

    Key Files Defining the Schema and Data Structure:
    - Ruleset.cs: This is the master container for all game rules. It holds dictionaries of ActorInfo, WeaponInfo, MusicInfo, etc. Think of it as the entire database schema for a given mod (like Red Alert or Tiberian Dawn).
    - ActorInfo.cs: This class is the blueprint for every unit, building, and projectile in the game. It is the equivalent of a CREATE TABLE statement for an "actors" table. It defines which components (Traits) an actor will have.
    - TraitInfo.cs (and all its derivatives): Every file that ends in ...Info.cs and inherits from TraitInfo acts like a "column definition" for the ActorInfo "table". For example, HealthInfo defines that an actor has health points, and ArmamentInfo defines that an actor has weapons.
    - YAML Files (*.yaml): These are the actual data files that populate the schema defined by the Info.cs classes. For example, rules/infantry.yaml defines the specific stats for all infantry units, which are then loaded into ActorInfo objects at runtime.
    - Core State Classes (Actor.cs, Player.cs, World.cs): These classes represent the live, in-game instances of the data. Actor.cs is like a row in the "actors" table, holding the current state (position, health, owner) of a single unit.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  UI & LAUNCHER ZONE
        // ======================================================================
        { id: 1, name: "OpenRA.Launcher", path:"OpenRA.Launcher/Program.cs", nodeType: "ui", pos: { x: 50, y: 50 }, icon: "🚀", columns: [{ name: "Main(string[] args)" }, { name: "Game.InitializeAndRun()" }] },
        { id: 2, name: "Widget System", path:"OpenRA.Game/Widgets/Widget.cs", nodeType: "ui", pos: { x: 50, y: 300 }, icon: "🖼️", columns: [{ name: "Ui.Tick()" }, { name: "Ui.Draw()" }, { name: "HandleMouseInput()" }, { name: "HandleKeyPress()" }] },
        { id: 3, name: "WidgetLoader.cs", path:"OpenRA.Game/Widgets/", nodeType: "ui", pos: { x: 50, y: 550 }, icon: "🧩", columns: [{ name: "LoadWidget()" }] },

        // ======================================================================
        //  CORE ENGINE ZONE
        // ======================================================================
        { id: 10, name: "Game.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 50 }, icon: "🎮", columns: [{ name: "InitializeAndRun()" }, { name: "Loop()" }, { name: "LogicTick()" }, { name: "RenderTick()" }] },
        { id: 11, name: "World.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }, { name: "SyncHash()" }] },
        { id: 12, name: "Actor.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 650 }, icon: "🤖", columns: [{ name: "Tick()" }, { name: "ResolveOrder()" }, { name: "TraitsImplementing<T>()" }] },
        { id: 13, name: "Player.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 950 }, icon: "🧑", columns: [{ name: "PlayerName" }, { name: "Faction" }, { name: "RelationshipWith()" }] },
        
        // ======================================================================
        //  SYSTEMS & MANAGERS ZONE
        // ======================================================================
        { id: 20, name: "OrderManager.cs", path:"OpenRA.Game/Network/", nodeType: "code", pos: { x: 750, y: 50 }, icon: "📦", columns: [{ name: "IssueOrder()" }, { name: "ReceiveOrders()" }, { name: "TryTick()" }] },
        { id: 21, name: "WorldRenderer.cs", path:"OpenRA.Game/Graphics/", nodeType: "code", pos: { x: 750, y: 300 }, icon: "🎨", columns: [{ name: "PrepareRenderables()" }, { name: "Draw()" }, { name: "ScreenPxPosition()" }] },
        { id: 22, name: "TraitDictionary.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 750, y: 550 }, icon: "📚", columns: [{ name: "AddTrait()" }, { name: "Get<T>()" }, { name: "ActorsWithTrait<T>()" }] },
        { id: 23, name: "Activity.cs", path:"OpenRA.Game/Activities/", nodeType: "code", pos: { x: 750, y: 750 }, icon: "🏃", columns: [{ name: "TickOuter()" }, { name: "OnFirstRun()" }, { name: "Cancel()" }] },
        { id: 24, name: "Connection.cs", path:"OpenRA.Game/Network/", nodeType: "code", pos: { x: 750, y: 950 }, icon: "🔌", columns: [{ name: "Send()" }, { name: "Receive()" }] },
        { id: 25, name: "Sound.cs", path:"OpenRA.Game/Sound/", nodeType: "code", pos: { x: 750, y: 1150 }, icon: "🔊", columns: [{ name: "Play()" }, { name: "PlayMusic()" }] },

        // ======================================================================
        //  DATA/SCHEMA DEFINITION ZONE
        // ======================================================================
        { id: 100, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 1150, y: 50 }, icon: "📜", columns: [{ name: "Actors (Dictionary)" }, { name: "Weapons (Dictionary)" }, { name: "LoadDefaults()" }] },
        { id: 101, name: "ActorInfo.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 1150, y: 300 }, icon: "📝", columns: [{ name: "Name" }, { name: "TraitsInConstructOrder()" }, { name: "TraitInfo<T>()" }] },
        { id: 102, name: "TraitInfo.cs", path:"OpenRA.Game/Traits/", nodeType: "db", pos: { x: 1150, y: 550 }, icon: "🔧", columns: [{ name: "Create(ActorInitializer)" }, { name: "Requires<T>" }, { name: "NotBefore<T>" }] },
        { id: 103, name: "Map.cs", path:"OpenRA.Game/Map/", nodeType: "db", pos: { x: 1150, y: 750 }, icon: "🗺️", columns: [{ name: "MapSize" }, { name: "Tiles (CellLayer)" }, { name: "Height (CellLayer)" }, { name: "Actors (Definitions)" }] },
        { id: 104, name: "MiniYaml.cs", path:"OpenRA.Game/", nodeType: "db", pos: { x: 1500, y: 50 }, icon: "📄", columns: [{ name: "FromFile()" }, { name: "FromStream()" }, { name: "Merge()" }] },
        { id: 105, name: "FieldLoader.cs", path:"OpenRA.Game/", nodeType: "db", pos: { x: 1500, y: 250 }, icon: "📥", columns: [{ name: "Load(object, MiniYaml)" }, { name: "GetValue<T>()" }] },
        { id: 106, name: "WeaponInfo.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 1150, y: 1000 }, icon: "💥", columns: [{ name: "Range" }, { name: "Projectile" }, { name: "Warheads" }] },
        { id: 107, name: "Order.cs", path:"OpenRA.Game/Network/", nodeType: "db", pos: { x: 1150, y: 1200 }, icon: "➡️", columns: [{ name: "OrderString" }, { name: "Subject" }, { name: "Target" }, { name: "Serialize()" }] },

        // ======================================================================
        //  MOD-SPECIFIC EXAMPLE ZONE
        // ======================================================================
        { id: 200, name: "Manifest.cs (Mod Definition)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 1500, y: 750 }, icon: "📦", columns: [{ name: "Rules" }, { name: "Sequences" }, { name: "Weapons" }, { name: "Chrome" }] },
        { id: 201, name: "Sandworm.cs (Mod Trait)", path:"OpenRA.Mods.D2k/Traits/", nodeType: "code", pos: { x: 1500, y: 1000 }, icon: "🐛", columns: [{ name: "Tick()" }, { name: "RescanForTargets()" }] },
        { id: 202, name: "AttractsWorms.cs (Mod Trait)", path:"OpenRA.Mods.D2k/Traits/", nodeType: "code", pos: { x: 1850, y: 1000 }, icon: "🔊", columns: [{ name: "Intensity" }, { name: "AttractionAtPosition()" }] },

        // ======================================================================
        //  EXTERNAL SERVICES ZONE
        // ======================================================================
        { id: 300, name: "GeoIP Database", path:"OpenRA.Game/Network/GeoIP.cs", nodeType: "service", pos: { x: 1850, y: 50 }, icon: "🌐", columns: [{ name: "LookupCountry()" }, { name: "fetches IP2LOCATION DB" }] }
    ],
    relationships: [
        // Launcher & UI Flow
        { from: { table: "OpenRA.Launcher", column: "Main(string[] args)" }, to: { table: "Game.cs", column: "InitializeAndRun()" }, type: "flow" },
        { from: { table: "WidgetLoader.cs", column: "LoadWidget()" }, to: { table: "MiniYaml.cs", column: "FromFile()" }, type: "read" },
        { from: { table: "Game.cs", column: "LogicTick()" }, to: { table: "Widget System", column: "Ui.Tick()" }, type: "flow" },
        { from: { table: "Game.cs", column: "RenderTick()" }, to: { table: "Widget System", column: "Ui.Draw()" }, type: "flow" },

        // Core Game Loop
        { from: { table: "Game.cs", column: "LogicTick()" }, to: { table: "OrderManager.cs", column: "TryTick()" }, type: "flow" },
        { from: { table: "OrderManager.cs", column: "TryTick()" }, to: { table: "World.cs", column: "Tick()" }, type: "flow" },
        { from: { table: "World.cs", column: "Tick()" }, to: { table: "Actor.cs", column: "Tick()" }, type: "flow" },
        { from: { table: "Actor.cs", column: "Tick()" }, to: { table: "Activity.cs", column: "TickOuter()" }, type: "flow" },
        
        // Data Loading and Schema
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "Manifest.cs (Mod Definition)", column: "Rules" }, type: "read" },
        { from: { table: "Manifest.cs (Mod Definition)", column: "Rules" }, to: { table: "MiniYaml.cs", column: "FromStream()" }, type: "read" },
        { from: { table: "MiniYaml.cs", column: "Merge()" }, to: { table: "Ruleset.cs", column: "LoadDefaults()" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "ActorInfo.cs", column: "Name" }, type: "write" },
        { from: { table: "ActorInfo.cs", column: "TraitsInConstructOrder()" }, to: { table: "TraitInfo.cs", column: "Create(ActorInitializer)" }, type: "read" },
        { from: { table: "World.cs", column: "CreateActor()" }, to: { table: "ActorInfo.cs", column: "TraitsInConstructOrder()" }, type: "read" },
        { from: { table: "World.cs", column: "CreateActor()" }, to: { table: "Actor.cs", column: "Tick()" }, type: "write" },
        
        // Actor Composition
        { from: { table: "Actor.cs", column: "TraitsImplementing<T>()" }, to: { table: "TraitDictionary.cs", column: "WithInterface<T>()" }, type: "read" },
        { from: { table: "TraitDictionary.cs", column: "AddTrait()" }, to: { table: "TraitInfo.cs", column: "Create(ActorInitializer)" }, type: "flow" },
        
        // Networking
        { from: { table: "OrderManager.cs", column: "ReceiveOrders()" }, to: { table: "Connection.cs", column: "Receive()" }, type: "flow" },
        { from: { table: "Connection.cs", column: "Send()" }, to: { table: "Order.cs", column: "Serialize()" }, type: "flow" },
        { from: { table: "OrderManager.cs", column: "IssueOrder()" }, to: { table: "Order.cs", column: "OrderString" }, type: "write" },
        { from: { table: "Actor.cs", column: "ResolveOrder()" }, to: { table: "Order.cs", column: "Subject" }, type: "read" },
        
        // Rendering
        { from: { table: "Game.cs", column: "RenderTick()" }, to: { table: "WorldRenderer.cs", column: "Draw()" }, type: "flow" },
        { from: { table: "WorldRenderer.cs", column: "PrepareRenderables()" }, to: { table: "World.cs", column: "Actors" }, type: "read" },
        { from: { table: "WorldRenderer.cs", column: "Draw()" }, to: { table: "Map.cs", column: "Tiles (CellLayer)" }, type: "read" },

        // Mod-specific Example
        { from: { table: "Sandworm.cs (Mod Trait)", column: "RescanForTargets()" }, to: { table: "AttractsWorms.cs (Mod Trait)", column: "AttractionAtPosition()" }, type: "read" },
        { from: { table: "Sandworm.cs (Mod Trait)", column: "RescanForTargets()" }, to: { table: "Actor.cs", column: "Tick()" }, type: "flow" },
        { from: { table: "World.cs", column: "Tick()" }, to: { table: "Sandworm.cs (Mod Trait)", column: "Tick()" }, type: "flow" },
        
        // External Service Example
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "GeoIP Database", column: "fetches IP2LOCATION DB" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC (STABLE - WORKING DRAG & DROP)
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`; // Use unique ID
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => `<li id="${canvasId}-${table.id}-${col.name}"><span class="col-name">${col.name}</span>${col.type ? `<span class="col-type">${col.type}</span>` : ''}</li>`).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            const fromElId = `${canvasId}-${fromId}-${rel.from.column}`;
            const toElId = `${canvasId}-${toId}-${rel.to.column}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-03-part1.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (with Embedded Insights)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Game Engine</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file represents a portion of the OpenRA game engine. The "database schema" is not in a single SQL file but is defined through C# classes and populated by YAML files.

    Key Files Defining the Schema and Data Structure:
    - Ruleset.cs: This is the master container for all game rules. It holds dictionaries of ActorInfo, WeaponInfo, MusicInfo, etc. Think of it as the entire database schema for a given mod (like Red Alert or Tiberian Dawn).
    - ActorInfo.cs: This class is the blueprint for every unit, building, and projectile in the game. It is the equivalent of a CREATE TABLE statement for an "actors" table. It defines which components (Traits) an actor will have.
    - TraitInfo.cs (and all its derivatives): Every file that ends in ...Info.cs and inherits from TraitInfo acts like a "column definition" for the ActorInfo "table". For example, HealthInfo defines that an actor has health points, and ArmamentInfo defines that an actor has weapons.
    - YAML Files (*.yaml): These are the actual data files that populate the schema defined by the Info.cs classes. For example, rules/infantry.yaml defines the specific stats for all infantry units, which are then loaded into ActorInfo objects at runtime. Tileset YAMLs define terrain types, tile templates, and brushes for map creation.
    - Core State Classes (Actor.cs, Player.cs, World.cs): These classes represent the live, in-game instances of the data. Actor.cs is like a row in the "actors" table, holding the current state (position, health, owner) of a single unit.
    - Lua Script Files (*.lua): These files contain game logic, mission scripts, and AI behaviors. They interact with the core C# engine to create dynamic gameplay, acting like business logic that reads from and writes to the live game state.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  UI & LAUNCHER ZONE
        // ======================================================================
        { id: 1, name: "OpenRA.Launcher", path:"OpenRA.Launcher/Program.cs", nodeType: "ui", pos: { x: 50, y: 50 }, icon: "🚀", columns: [{ name: "Main(string[] args)" }, { name: "Game.InitializeAndRun()" }] },
        
        // ======================================================================
        //  CORE ENGINE ZONE
        // ======================================================================
        { id: 10, name: "Game.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 50 }, icon: "🎮", columns: [{ name: "InitializeAndRun()" }, { name: "Loop()" }, { name: "LogicTick()" }, { name: "RenderTick()" }] },
        { id: 11, name: "World.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }, { name: "SyncHash()" }] },
        { id: 12, name: "Actor.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 650 }, icon: "🤖", columns: [{ name: "Tick()" }, { name: "ResolveOrder()" }, { name: "LoadPassenger()" }, { name: "Destroy()" }] },
        
        // ======================================================================
        //  SYSTEMS & SCRIPTING ZONE
        // ======================================================================
        { id: 20, name: "OrderManager.cs", path:"OpenRA.Game/Network/", nodeType: "code", pos: { x: 750, y: 50 }, icon: "📦", columns: [{ name: "IssueOrder()" }] },
        { id: 23, name: "Activity.cs", path:"OpenRA.Game/Activities/", nodeType: "code", pos: { x: 750, y: 250 }, icon: "🏃", columns: [{ name: "TickOuter()" }, { name: "ScriptedMove()" }, { name: "Wait()" }] },
        { id: 26, name: "campaign.lua", path:"mods/cnc/scripts/", nodeType: "code", pos: { x: 750, y: 500 }, icon: "📜", columns: [{ name: "InitObjectives()" }, { name: "ReinforceWithLandingCraft()" }, { name: "ProduceUnits()" }, { name: "CheckForBase()" }] },
        
        // ======================================================================
        //  DATA/SCHEMA DEFINITION ZONE
        // ======================================================================
        { id: 100, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 1150, y: 50 }, icon: "📜", columns: [{ name: "Actors (Dictionary)" }, { name: "Weapons (Dictionary)" }, { name: "LoadDefaults()" }] },
        { id: 101, name: "ActorInfo.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 1150, y: 300 }, icon: "📝", columns: [{ name: "Name" }, { name: "TraitsInConstructOrder()" }] },
        { id: 103, name: "Map.cs", path:"OpenRA.Game/Map/", nodeType: "db", pos: { x: 1150, y: 500 }, icon: "🗺️", columns: [{ name: "MapSize" }, { name: "Tiles (CellLayer)" }, { name: "Actors (Definitions)" }] },
        { id: 104, name: "MiniYaml.cs", path:"OpenRA.Game/", nodeType: "db", pos: { x: 1150, y: 750 }, icon: "📄", columns: [{ name: "FromFile()" }, { name: "FromStream()" }, { name: "Merge()" }] },
        { id: 107, name: "Order.cs", path:"OpenRA.Game/Network/", nodeType: "db", pos: { x: 1150, y: 950 }, icon: "➡️", columns: [{ name: "OrderString" }, { name: "Subject" }, { name: "Target" }] },
        { id: 108, name: "temperat.yaml", path:"mods/cnc/tilesets/", nodeType: "db", pos: { x: 1500, y: 400 }, icon: "🏞️", columns: [{ name: "General (Tileset Def)" }, { name: "Terrain (Types)" }, { name: "Templates (Tile Rules)" }, { name: "MultiBrushCollections" }] },
        { id: 109, name: "ai.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1500, y: 700 }, icon: "🧠", columns: [{ name: "ModularBot Definitions" }, { name: "SupportPowerBotModule" }, { name: "BaseBuilderBotModule" }, { name: "UnitBuilderBotModule" }] },
        { id: 110, name: "aircraft.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1500, y: 950 }, icon: "✈️", columns: [{ name: "TRAN (Transport)" }, { name: "HELI (Apache)" }, { name: "ORCA (Orca)" }, { name: "C17 (Cargo Plane)" }] },

        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES ZONE
        // ======================================================================
        { id: 300, name: "afld.shp", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 50 }, icon: "🖼️", columns: [{ name: "Sprite/Animation Data" }] },
        { id: 301, name: "cliffsl1.tem", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 250 }, icon: "🎨", columns: [{ name: "Tileset Image Data" }] },
        { id: 302, name: "civcapt1.aud", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 450 }, icon: "🔊", columns: [{ name: "Audio Data" }] },
        { id: 303, name: "snow.mix", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 650 }, icon: "📦", columns: [{ name: "Package Archive" }] },
        { id: 304, name: "snow.pal", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 850 }, icon: "🎨", columns: [{ name: "Color Palette" }] }
    ],
    relationships: [
        // Launcher & Game Loop
        { from: { table: "OpenRA.Launcher", column: "Main(string[] args)" }, to: { table: "Game.cs", column: "InitializeAndRun()" }, type: "flow" },
        { from: { table: "Game.cs", column: "LogicTick()" }, to: { table: "World.cs", column: "Tick()" }, type: "flow" },
        { from: { table: "World.cs", column: "Tick()" }, to: { table: "Actor.cs", column: "Tick()" }, type: "flow" },
        
        // Data Loading Flow
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "MiniYaml.cs", column: "FromFile()" }, type: "flow" },
        { from: { table: "MiniYaml.cs", column: "FromFile()" }, to: { table: "temperat.yaml", column: "General (Tileset Def)" }, type: "read" },
        { from: { table: "MiniYaml.cs", column: "FromFile()" }, to: { table: "ai.yaml", column: "ModularBot Definitions" }, type: "read" },
        { from: { table: "MiniYaml.cs", column: "FromFile()" }, to: { table: "aircraft.yaml", column: "TRAN (Transport)" }, type: "read" },
        { from: { table: "MiniYaml.cs", column: "Merge()" }, to: { table: "Ruleset.cs", column: "LoadDefaults()" }, type: "write" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "ActorInfo.cs", column: "Name" }, type: "write" },
        
        // Tileset and Asset Loading
        { from: { table: "temperat.yaml", column: "Templates (Tile Rules)" }, to: { table: "cliffsl1.tem", column: "Tileset Image Data" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "afld.shp", column: "Sprite/Animation Data" }, type: "read" },
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "snow.pal", column: "Color Palette" }, type: "read" },
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "snow.mix", column: "Package Archive" }, type: "read" },
        
        // Scripting and Game Logic
        { from: { table: "campaign.lua", column: "ReinforceWithLandingCraft()" }, to: { table: "Actor.cs", column: "CreateActor()" }, type: "write" },
        { from: { table: "campaign.lua", column: "ReinforceWithLandingCraft()" }, to: { table: "Actor.cs", column: "LoadPassenger()" }, type: "write" },
        { from: { table: "campaign.lua", column: "ReinforceWithLandingCraft()" }, to: { table: "Activity.cs", column: "ScriptedMove()" }, type: "flow" },
        { from: { table: "campaign.lua", column: "ProduceUnits()" }, to: { table: "Actor.cs", column: "Build()" }, type: "write" },
        { from: { table: "campaign.lua", column: "CheckForBase()" }, to: { table: "World.cs", column: "GetActorsByType()" }, type: "read" },
        { from: { table: "OrderManager.cs", column: "IssueOrder()" }, to: { table: "Order.cs", column: "OrderString" }, type: "write" },
        { from: { table: "Actor.cs", column: "ResolveOrder()" }, to: { table: "Order.cs", column: "Target" }, type: "read" },
        
        // World Creation
        { from: { table: "World.cs", column: "CreateActor()" }, to: { table: "ActorInfo.cs", column: "TraitsInConstructOrder()" }, type: "read" },
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "Map.cs", column: "Actors (Definitions)" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC (STABLE - WORKING DRAG & DROP)
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`; // Use unique ID
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => `<li id="${canvasId}-${table.id}-${col.name}"><span class="col-name">${col.name}</span>${col.type ? `<span class="col-type">${col.type}</span>` : ''}</li>`).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            // Attempt to find the specific column element, but fall back to the table node if the column doesn't exist
            let fromElId = `${canvasId}-${fromId}-${rel.from.column}`;
            let fromEl = document.getElementById(fromElId);
            if (!fromEl) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${rel.to.column}`;
            let toEl = document.getElementById(toElId);
            if (!toEl) toElId = `${canvasId}-${toId}`;

            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-04-part1.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Full Data Flow)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Game Engine</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file represents a portion of the OpenRA game engine. The "database schema" is not in a single SQL file but is defined through C# classes and populated by YAML files.

    Key Files Defining the Schema and Data Structure:
    - Ruleset.cs: This is the master container for all game rules. It holds dictionaries of ActorInfo, WeaponInfo, MusicInfo, etc. Think of it as the entire database schema for a given mod (like Red Alert or Tiberian Dawn).
    - ActorInfo.cs: This class is the blueprint for every unit, building, and projectile in the game. It is the equivalent of a CREATE TABLE statement for an "actors" table. It defines which components (Traits) an actor will have.
    - TraitInfo.cs (and all its derivatives): Every file that ends in ...Info.cs and inherits from TraitInfo acts like a "column definition" for the ActorInfo "table". For example, HealthInfo defines that an actor has health points, and ArmamentInfo defines that an actor has weapons.
    - YAML Files (*.yaml): These are the actual data files that populate the schema defined by the Info.cs classes. For example, rules/infantry.yaml defines the specific stats for all infantry units, which are then loaded into ActorInfo objects at runtime. Tileset YAMLs define terrain types, tile templates, and brushes for map creation.
    - Core State Classes (Actor.cs, Player.cs, World.cs): These classes represent the live, in-game instances of the data. Actor.cs is like a row in the "actors" table, holding the current state (position, health, owner) of a single unit.
    - Lua Script Files (*.lua): These files contain game logic, mission scripts, and AI behaviors. They interact with the core C# engine to create dynamic gameplay, acting like business logic that reads from and writes to the live game state.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  UI / CHROME ZONE
        // ======================================================================
        { id: 1, name: "mainmenu.yaml", path:"mods/cnc/chrome/", nodeType: "ui", pos: { x: 50, y: 50 }, icon: "🖥️", columns: [{ name: "SINGLEPLAYER_BUTTON" }, { name: "MULTIPLAYER_BUTTON" }] },
        { id: 2, name: "lobby.yaml", path:"mods/cnc/chrome/", nodeType: "ui", pos: { x: 50, y: 250 }, icon: "🛋️", columns: [{ name: "START_GAME_BUTTON" }, { name: "MAP_PREVIEW_ROOT" }] },
        { id: 3, name: "settings.yaml", path:"mods/cnc/chrome/", nodeType: "ui", pos: { x: 50, y: 450 }, icon: "⚙️", columns: [{ name: "DISPLAY_PANEL" }, { name: "AUDIO_PANEL" }, { name: "HOTKEYS_PANEL" }] },
        { id: 4, name: "WidgetLoader.cs", path:"OpenRA.Game/Widgets/", nodeType: "ui", pos: { x: 50, y: 650 }, icon: "🧩", columns: [{ name: "LoadWidget()" }] },

        // ======================================================================
        //  CORE ENGINE ZONE
        // ======================================================================
        { id: 10, name: "Game.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 50 }, icon: "🎮", columns: [{ name: "InitializeAndRun()" }, { name: "LogicTick()" }] },
        { id: 11, name: "World.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 300 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs", path:"OpenRA.Game/", nodeType: "code", pos: { x: 400, y: 550 }, icon: "🤖", columns: [{ name: "Tick()" }, { name: "ResolveOrder()" }] },

        // ======================================================================
        //  MAP & MISSION SCRIPTING ZONE
        // ======================================================================
        { id: 20, name: "nod09/map.yaml", path:"mods/cnc/maps/", nodeType: "db", pos: { x: 750, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Reinforce Egypt" }, { name: "Tileset: DESERT" }, { name: "Players: GDI, Nod" }, { name: "Actors: ..." }, { name: "Rules: nod09.lua" }] },
        { id: 21, name: "nod09.lua", path:"mods/cnc/maps/nod09/", nodeType: "code", pos: { x: 750, y: 350 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "SendGDIAirstrike()" }, { name: "CheckForSams()" }] },
        { id: 22, name: "map-generators.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 750, y: 600 }, icon: "🎲", columns: [{ name: "Tilesets: DESERT, SNOW..." }, { name: "Water: 0" }, { name: "Mountains: 100" }] },
        
        // ======================================================================
        //  GAME RULES (SCHEMA) ZONE
        // ======================================================================
        { id: 100, name: "world.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1150, y: 50 }, icon: "📜", columns: [{ name: "Locomotor@FOOT" }, { name: "Locomotor@WHEELED" }] },
        { id: 101, name: "player.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1150, y: 250 }, icon: "🧑‍⚖️", columns: [{ name: "^BasePlayer" }, { name: "SupportPowerManager" }] },
        { id: 102, name: "infantry.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1150, y: 450 }, icon: "🚶", columns: [{ name: "E1: ^Soldier" }, { name: "Cost: 100" }, { name: "Weapon: M16" }] },
        { id: 103, name: "vehicles.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1150, y: 650 }, icon: "🚚", columns: [{ name: "MCV: ^Vehicle" }, { name: "Transforms: IntoActor: fact" }] },
        { id: 104, name: "structures.yaml", path:"mods/cnc/rules/", nodeType: "db", pos: { x: 1150, y: 850 }, icon: "🏗️", columns: [{ name: "FACT: ^BaseBuilding" }, { name: "Production:" }, { name: "Power:" }] },
        { id: 105, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 1150, y: 1050 }, icon: "📚", columns: [{ name: "Actors (Dictionary)" }, { name: "LoadDefaults()" }] },

        // ======================================================================
        //  ANIMATION SEQUENCES ZONE
        // ======================================================================
        { id: 200, name: "infantry.yaml (Sequences)", path:"mods/cnc/sequences/", nodeType: "db", pos: { x: 1500, y: 450 }, icon: "🎬", columns: [{ name: "e1:" }, { name: "stand: Facings: 8" }, { name: "run: Length: 6" }] },
        { id: 201, name: "vehicles.yaml (Sequences)", path:"mods/cnc/sequences/", nodeType: "db", pos: { x: 1500, y: 650 }, icon: "🎬", columns: [{ name: "mcv:" }, { name: "idle: Facings: 32" }] },

        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES ZONE
        // ======================================================================
        { id: 300, name: "e1.shp", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 450 }, icon: "🖼️", columns: [{ name: "Infantry Sprite Data" }] },
        { id: 301, name: "mcv.shp", path:"mods/cnc/bits/", nodeType: "service", pos: { x: 1850, y: 650 }, icon: "🖼️", columns: [{ name: "MCV Sprite Data" }] },
        { id: 302, name: "desert.pal", path:"mods/cnc/palettes/", nodeType: "service", pos: { x: 1850, y: 850 }, icon: "🎨", columns: [{ name: "Color Palette" }] }
    ],
    relationships: [
        // UI Flow
        { from: { table: "mainmenu.yaml", column: "SINGLEPLAYER_BUTTON" }, to: { table: "lobby.yaml", column: "MAP_PREVIEW_ROOT" }, type: "flow" },
        { from: { table: "lobby.yaml", column: "START_GAME_BUTTON" }, to: { table: "Game.cs", column: "InitializeAndRun()" }, type: "flow" },
        { from: { table: "mainmenu.yaml", column: "SINGLEPLAYER_BUTTON" }, to: { table: "WidgetLoader.cs", column: "LoadWidget()" }, type: "read" },
        
        // Game Start & Map Loading
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "nod09/map.yaml", column: "Title: Reinforce Egypt" }, type: "read" },
        { from: { table: "nod09/map.yaml", column: "Rules: nod09.lua" }, to: { table: "nod09.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "nod09.lua", column: "SendGDIAirstrike()" }, to: { table: "World.cs", column: "CreateActor()" }, type: "write" },

        // Rule & Data Loading Pipeline
        { from: { table: "Game.cs", column: "InitializeAndRun()" }, to: { table: "Ruleset.cs", column: "LoadDefaults()" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "world.yaml", column: "Locomotor@FOOT" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "player.yaml", column: "^BasePlayer" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "infantry.yaml", column: "E1: ^Soldier" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "vehicles.yaml", column: "MCV: ^Vehicle" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "structures.yaml", column: "FACT: ^BaseBuilding" }, type: "read" },
        
        // Actor Creation (Instance from Schema)
        { from: { table: "World.cs", column: "CreateActor()" }, to: { table: "Ruleset.cs", column: "Actors (Dictionary)" }, type: "read" },
        { from: { table: "nod09/map.yaml", column: "Actors: ..." }, to: { table: "infantry.yaml", column: "E1: ^Soldier" }, type: "read" },
        
        // Linking Rules to Sequences
        { from: { table: "infantry.yaml", column: "E1: ^Soldier" }, to: { table: "infantry.yaml (Sequences)", column: "e1:" }, type: "read" },
        { from: { table: "vehicles.yaml", column: "MCV: ^Vehicle" }, to: { table: "vehicles.yaml (Sequences)", column: "mcv:" }, type: "read" },

        // Linking Sequences to Raw Assets
        { from: { table: "infantry.yaml (Sequences)", column: "e1:" }, to: { table: "e1.shp", column: "Infantry Sprite Data" }, type: "read" },
        { from: { table: "vehicles.yaml (Sequences)", column: "mcv:" }, to: { table: "mcv.shp", column: "MCV Sprite Data" }, type: "read" },
        { from: { table: "nod09/map.yaml", column: "Tileset: DESERT" }, to: { table: "desert.pal", column: "Color Palette" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`; // Use unique ID
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null; // Fallback for connections to the node itself
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`; // fallback to node

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`; // fallback to node
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-04-part2.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Full Mission Flow)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: nod07a</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file represents a portion of the OpenRA game engine. The "database schema" is not in a single SQL file but is defined through C# classes and populated by YAML files.

    Key Files Defining the Schema and Data Structure:
    - Ruleset.cs: This is the master container for all game rules. It holds dictionaries of ActorInfo, WeaponInfo, MusicInfo, etc. Think of it as the entire database schema for a given mod (like Red Alert or Tiberian Dawn).
    - ActorInfo.cs: This class is the blueprint for every unit, building, and projectile in the game. It is the equivalent of a CREATE TABLE statement for an "actors" table. It defines which components (Traits) an actor will have.
    - TraitInfo.cs (and all its derivatives): Every file that ends in ...Info.cs and inherits from TraitInfo acts like a "column definition" for the ActorInfo "table". For example, HealthInfo defines that an actor has health points, and ArmamentInfo defines that an actor has weapons.
    - YAML Files (*.yaml): These are the actual data files that populate the schema defined by the Info.cs classes. For example, rules/infantry.yaml defines the specific stats for all infantry units, which are then loaded into ActorInfo objects at runtime. Tileset YAMLs define terrain types, tile templates, and brushes for map creation.
    - Core State Classes (Actor.cs, Player.cs, World.cs): These classes represent the live, in-game instances of the data. Actor.cs is like a row in the "actors" table, holding the current state (position, health, owner) of a single unit.
    - Lua Script Files (*.lua): These files contain game logic, mission scripts, and AI behaviors. They interact with the core C# engine to create dynamic gameplay, acting like business logic that reads from and writes to the live game state.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 300 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 550 }, icon: "🤖", columns: [{ name: "IsDead" }, { name: "Owner" }, { name: "Location" }, { name: "Move()" }, { name: "AttackMove()" }, { name: "Build()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION
        // ======================================================================
        { id: 20, name: "map.yaml", path:"nod07a/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Sick and Dying" }, { name: "Tileset: DESERT" }, { name: "Actors:" }, { name: "  GDIBuilding1: gtwr" }, { name: "  NodBuilding1: fact" }, { name: "  AttackPath1: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"nod07a/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: nod07a.lua" }, { name: "           nod07a-AI.lua" }, { name: "MusicPlaylist:" }, { name: "MissionData:" }] },
        { id: 22, name: "weapons.yaml", path:"nod07a/", nodeType: "db", pos: { x: 400, y: 750 }, icon: "💥", columns: [{ name: "HighV.IN" }, { name: "Inherits: HighV" }, { name: "Range: 4c0" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER
        // ======================================================================
        { id: 30, name: "nod07a.lua", path:"nod07a/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "Tick()" }, { name: "CheckForSams()" }, { name: "Trigger.OnKilled(GDIProc,..)" }, { name: "Trigger.OnEnteredFootprint(..)" }, { name: "StartAI()" }] },
        { id: 31, name: "nod07a-AI.lua", path:"nod07a/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "StartAI()" }, { name: "ProduceInfantry(building)" }, { name: "ProduceVehicle(building)" }, { name: "BuildBuilding(building,..)" }] },
        { id: 32, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 750 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }, { name: "ReinforceWithTransport(..)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL DATA
        // ======================================================================
        { id: 40, name: "map.bin", path:"nod07a/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "map.png", path:"nod07a/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🖼️", columns: [{ name: "Map Preview Image" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.yaml", column: "Title: Sick and Dying" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "weapons.yaml", column: "HighV.IN" }, type: "read" },
        
        // Script Loading
        { from: { table: "rules.yaml", column: "  Scripts: nod07a.lua" }, to: { table: "nod07a.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "nod07a.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },
        { from: { table: "nod07a.lua", column: "StartAI()" }, to: { table: "nod07a-AI.lua", column: "StartAI()" }, type: "flow" },

        // Mission Logic -> Engine Interaction
        { from: { table: "nod07a.lua", column: "WorldLoaded()" }, to: { table: "map.yaml", column: "  GDIBuilding1: gtwr" }, type: "read", label: "References Actor" },
        { from: { table: "nod07a.lua", column: "Trigger.OnKilled(GDIProc,..)" }, to: { table: "Actor.cs (Object)", column: "IsDead" }, type: "read", label: "Listens for event" },
        { from: { table: "nod07a.lua", column: "CheckForSams()" }, to: { table: "World.cs (State)", column: "GetActorsByType()" }, type: "read", label: "Queries world state" },
        
        // AI Logic -> Engine Interaction
        { from: { table: "nod07a-AI.lua", column: "ProduceInfantry(building)" }, to: { table: "Actor.cs (Object)", column: "Build()" }, type: "write", label: "Issues command" },
        { from: { table: "nod07a-AI.lua", column: "BuildBuilding(building,..)" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write", label: "Creates new actor" },
        
        // UI (Conceptual)
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.png", column: "Map Preview Image" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`; // Use unique ID
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null; // Fallback for connections to the node itself
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`; // fallback to node

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`; // fallback to node
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-05.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (GDI Mission 06)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: gdi06</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (GDI Mission 06) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map, which in this case is `gdi06`.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, actor (unit/building), and waypoint with a unique name (e.g., `SAM01`, `Airfield`, `waypoint10`). This acts as the database for the mission.
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine to load and execute `gdi06.lua`.
    5.  The `WorldLoaded()` function in `gdi06.lua` is called. It sets up objectives and registers triggers with the C# engine (e.g., "if all actors in `IslandSamSites` are killed, create a flare").
    6.  The mission progresses as the Lua script reacts to game events (like units entering an area) and issues commands back to the C# engine (e.g., `Actor.Create`, `unit.Patrol`). This shows a clear separation of concerns between data (YAML) and logic (Lua).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap('gdi06')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "IsDead" }, { name: "Owner" }, { name: "Location" }, { name: "Move()" }, { name: "AttackMove()" }, { name: "Patrol()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"gdi06/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Infiltrate Nod Base" }, { name: "Players: GDI, Nod" }, { name: "SAM01: sam" }, { name: "Airfield: afld" }, { name: "waypoint10: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"gdi06/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: gdi06.lua" }, { name: "MusicPlaylist: rain-ambient" }, { name: "BriefingVideo: gdi6.vqa" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 30, name: "gdi06.lua", path:"gdi06/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "IslandSamSites = {SAM01,..}" }, { name: "Trigger.OnAllKilled(IslandSamSites,..)" }, { name: "Trigger.OnKilled(Airfield,..)" }, { name: "Trigger.OnEnteredFootprint(...)" }, { name: "unit.Patrol(FootPatrol1Route,...)" }] },
        { id: 31, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 450 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }, { name: "ReinforceWithLandingCraft(...)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"gdi06/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "gdi6.vqa", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] },
        { id: 42, name: "rain-ambient.aud", path:"gdi06/", nodeType: "service", pos: { x: 1200, y: 450 }, icon: "🔊", columns: [{ name: "Ambient Audio Data" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('gdi06')" }, to: { table: "map.yaml", column: "Title: Infiltrate Nod Base" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('gdi06')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script & Asset Loading
        { from: { table: "rules.yaml", column: "  Scripts: gdi06.lua" }, to: { table: "gdi06.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: gdi6.vqa" }, to: { table: "gdi6.vqa", column: "Briefing Video Asset" }, type: "read" },
        { from: { table: "rules.yaml", column: "MusicPlaylist: rain-ambient" }, to: { table: "rain-ambient.aud", column: "Ambient Audio Data" }, type: "read" },
        { from: { table: "gdi06.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "gdi06.lua", column: "IslandSamSites = {SAM01,..}" }, to: { table: "map.yaml", column: "SAM01: sam" }, type: "read" },
        { from: { table: "gdi06.lua", column: "unit.Patrol(FootPatrol1Route,...)" }, to: { table: "map.yaml", column: "waypoint10: waypoint" }, type: "read" },
        
        // Scripts -> Issue Commands to Engine (The Event Loop)
        { from: { table: "gdi06.lua", column: "Trigger.OnAllKilled(IslandSamSites,..)" }, to: { table: "Actor.cs (Object)", column: "IsDead" }, type: "read" },
        { from: { table: "Actor.cs (Object)", column: "IsDead" }, to: { table: "gdi06.lua", column: "Trigger.OnAllKilled(IslandSamSites,..)" }, type: "flow" },
        { from: { table: "gdi06.lua", column: "Trigger.OnAllKilled(IslandSamSites,..)" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-06-part1-608.517tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Atreides Mission 05)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: atreides-05</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Atreides Mission 05) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, actor (unit/building), and waypoint with a unique name (e.g., `HarkonnenBarracks`, `Starport`). This acts as the database for the mission.
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine which Lua scripts to load and execute.
    5.  The engine loads `atreides05.lua` (for mission events) and `atreides05-AI.lua` (for opponent AI).
    6.  The `WorldLoaded()` function in the main Lua script is called. It sets up objectives and registers triggers with the C# engine (e.g., "if the `Starport` actor is captured, call a function").
    7.  The AI script begins its own loops, using timed delays (`Trigger.AfterDelay`) to produce units and launch attack waves.
    8.  The mission progresses as the Lua scripts react to game events (like units being killed or entering an area) and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.AttackMove`).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap('atreides05')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "IsDead" }, { name: "Owner" }, { name: "Location" }, { name: "AttackMove()" }, { name: "Capture()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"atreides-05/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Atreides 05" }, { name: "Players: Atreides, Harkonnen,.." }, { name: "HarkonnenBarracks: barracks" }, { name: "Starport: starport" }, { name: "HarkonnenRally1: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"atreides-05/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: atreides05.lua" }, { name: "           atreides05-AI.lua" }, { name: "BriefingVideo: A_BR05_E.VQA" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 30, name: "atreides05.lua", path:"atreides-05/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "SendHarkonnen()" }, { name: "SendMercenaries()" }, { name: "Trigger.OnCapture(Starport,...)" }, { name: "Trigger.OnKilled(HarkonnenBarracks,...)" }] },
        { id: 31, name: "atreides05-AI.lua", path:"atreides-05/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ProduceInfantry()" }, { name: "SendAttack()" }] },
        { id: 32, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 750 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }, { name: "ReinforceWithTransport(...)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"atreides-05/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "A_BR05_E.VQA", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('atreides05')" }, to: { table: "map.yaml", column: "Title: Atreides 05" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('atreides05')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script & Asset Loading
        { from: { table: "rules.yaml", column: "  Scripts: atreides05.lua" }, to: { table: "atreides05.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: A_BR05_E.VQA" }, to: { table: "A_BR05_E.VQA", column: "Briefing Video Asset" }, type: "read" },
        { from: { table: "atreides05.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },
        { from: { table: "atreides05.lua", column: "WorldLoaded()" }, to: { table: "atreides05-AI.lua", column: "ActivateAI()" }, type: "flow" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "atreides05.lua", column: "Trigger.OnCapture(Starport,...)" }, to: { table: "map.yaml", column: "Starport: starport" }, type: "read" },
        { from: { table: "atreides05.lua", column: "SendHarkonnen()" }, to: { table: "map.yaml", column: "HarkonnenRally1: waypoint" }, type: "read" },
        
        // AI Logic -> Engine Interaction
        { from: { table: "atreides05-AI.lua", column: "ProduceInfantry()" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" },
        { from: { table: "atreides05-AI.lua", column: "SendAttack()" }, to: { table: "Actor.cs (Object)", column: "AttackMove()" }, type: "write" },

        // Script -> Engine Interaction (Event Loop)
        { from: { table: "atreides05.lua", column: "Trigger.OnCapture(Starport,...)" }, to: { table: "Actor.cs (Object)", column: "Capture()" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-06-part2-624.766tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Harkonnen Mission 09a)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: harkonnen-09a</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a complex mission (Harkonnen 09a) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, their alliances, and every actor (unit/building) with a unique name (e.g., `HMCV`, `APalace`, `CStarport`). This acts as the database for the mission.
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine which Lua scripts to load and execute for this specific mission.
    5.  The engine loads `harkonnen09a.lua` (for mission events) and `harkonnen09a-AI.lua` (for opponent AI).
    6.  The `WorldLoaded()` function in the main Lua script is called. It sets up objectives and registers triggers with the C# engine (e.g., "when all buildings in the `AtreidesMainBase` group are destroyed, call a function").
    7.  The AI script begins its own loops, using timed delays (`Trigger.AfterDelay`) to produce units from multiple enemy factions and launch attack waves defined by waypoints from `map.yaml`.
    8.  The mission progresses as the Lua scripts react to game events and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.AttackMove`, `AHiTechFactory.TargetAirstrike`).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap('harkonnen09a')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "AttackMove()" }, { name: "Produce()" }, { name: "TargetAirstrike()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"harkonnen-09a/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Harkonnen 09a" }, { name: "Players: Harkonnen, Atreides, Corrino" }, { name: "HMCV: mcv" }, { name: "APalace: palace" }, { name: "CStarport: starport" }, { name: "HarkonnenRally: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"harkonnen-09a/", nodeType: "db", pos: { x: 400, y: 500 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: harkonnen09a.lua" }, { name: "           harkonnen09a-AI.lua" }, { name: "BriefingVideo: H_BR09_E.VQA" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 30, name: "harkonnen09a.lua", path:"harkonnen-09a/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "Trigger.AfterDelay(...)" }, { name: "SendAirStrike()" }, { name: "BuildFremen()" }, { name: "SendHarkonnenReinforcements()" }, { name: "ActivateAI()" }] },
        { id: 31, name: "harkonnen09a-AI.lua", path:"harkonnen-09a/", nodeType: "code", pos: { x: 800, y: 500 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "DefendAndRepairBase(...)" }, { name: "ProduceUnits(AtreidesMain, ...)" }, { name: "ProduceUnits(CorrinoMain, ...)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"harkonnen-09a/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "H_BR09_E.VQA", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('harkonnen09a')" }, to: { table: "map.yaml", column: "Title: Harkonnen 09a" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('harkonnen09a')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script & Asset Loading
        { from: { table: "rules.yaml", column: "  Scripts: harkonnen09a.lua" }, to: { table: "harkonnen09a.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "harkonnen09a.lua", column: "ActivateAI()" }, to: { table: "harkonnen09a-AI.lua", column: "ActivateAI()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: H_BR09_E.VQA" }, to: { table: "H_BR09_E.VQA", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data & Issues Commands
        { from: { table: "harkonnen09a.lua", column: "BuildFremen()" }, to: { table: "map.yaml", column: "APalace: palace" }, type: "read" },
        { from: { table: "harkonnen09a.lua", column: "BuildFremen()" }, to: { table: "Actor.cs (Object)", column: "Produce()" }, type: "write" },
        { from: { table: "harkonnen09a.lua", column: "SendAirStrike()" }, to: { table: "Actor.cs (Object)", column: "TargetAirstrike()" }, type: "write" },
        
        // AI Logic -> Reads from Map Data & Issues Commands
        { from: { table: "harkonnen09a-AI.lua", column: "ProduceUnits(AtreidesMain, ...)" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" },
        { from: { table: "harkonnen09a.lua", column: "SendCarryallReinforcements(...)" }, to: { table: "map.yaml", column: "AtreidesRally1: waypoint" }, type: "read" },
        { from: { table: "harkonnen09a.lua", column: "SendCarryallReinforcements(...)" }, to: { table: "Actor.cs (Object)", column: "AttackMove()" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-07-621.929tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Ordos Mission 05)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: ordos-05</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Ordos Mission 05) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, their alliances, and every actor (unit/building) with a unique name (e.g., `AConyard`, `AStarport`). This acts as the database for the mission.
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine which Lua scripts to load and execute for this specific mission.
    5.  The engine loads `ordos05.lua` (for mission events) and `ordos05-AI.lua` (for opponent AI).
    6.  The `WorldLoaded()` function in the main Lua script is called. It sets up objectives and registers triggers with the C# engine (e.g., "if the `AStarport` actor is captured, call a function").
    7.  The AI script begins its own loops, using timed delays (`Trigger.AfterDelay`) to produce units and launch attack waves defined by waypoints from `map.yaml`.
    8.  The mission progresses as the Lua scripts react to game events and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.AttackMove`).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap('ordos05')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "IsDead" }, { name: "Owner" }, { name: "AttackMove()" }, { name: "Capture()" }, { name: "Sell()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"ordos-05/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Ordos 05" }, { name: "Players: Ordos, AtreidesMainBase..." }, { name: "AConyard: construction_yard" }, { name: "AStarport: starport" }, { name: "AtreidesRally1: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"ordos-05/", nodeType: "db", pos: { x: 400, y: 500 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: ordos05.lua" }, { name: "           ordos05-AI.lua" }, { name: "BriefingVideo: O_BR05_E.VQA" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 30, name: "ordos05.lua", path:"ordos-05/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "Tick()" }, { name: "Trigger.OnCapture(AStarport,...)" }, { name: "Trigger.OnKilled(AStarport,...)" }, { name: "ActivateAI()" }] },
        { id: 31, name: "ordos05-AI.lua", path:"ordos-05/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ActivateAIProduction()" }, { name: "ProduceUnits(AtreidesMain,...)" }, { name: "DefendAndRepairBase(...)" }] },
        { id: 32, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 800 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }, { name: "ReinforceWithTransport(...)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"ordos-05/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "O_BR05_E.VQA", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('ordos05')" }, to: { table: "map.yaml", column: "Title: Ordos 05" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('ordos05')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script & Asset Loading
        { from: { table: "rules.yaml", column: "  Scripts: ordos05.lua" }, to: { table: "ordos05.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "ordos05.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },
        { from: { table: "ordos05.lua", column: "ActivateAI()" }, to: { table: "ordos05-AI.lua", column: "ActivateAI()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: O_BR05_E.VQA" }, to: { table: "O_BR05_E.VQA", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "ordos05.lua", column: "Trigger.OnCapture(AStarport,...)" }, to: { table: "map.yaml", column: "AStarport: starport" }, type: "read" },
        
        // AI Logic -> Reads from Map Data
        { from: { table: "ordos05-AI.lua", column: "ProduceUnits(AtreidesMain,...)" }, to: { table: "map.yaml", column: "ABarracks1: barracks" }, type: "read" },
        { from: { table: "ordos05.lua", column: "SendCarryallReinforcements(...)" }, to: { table: "map.yaml", column: "AtreidesRally1: waypoint" }, type: "read" },
        
        // Scripts -> Issue Commands to Engine (The Event Loop)
        { from: { table: "ordos05.lua", column: "Trigger.OnCapture(AStarport,...)" }, to: { table: "Actor.cs (Object)", column: "Capture()" }, type: "read" },
        { from: { table: "Actor.cs (Object)", column: "Capture()" }, to: { table: "ordos05.lua", column: "Trigger.OnCapture(AStarport,...)" }, type: "flow" },
        { from: { table: "ordos05.lua", column: "Trigger.OnCapture(AStarport,...)" }, to: { table: "campaign.lua", column: "ReinforceWithTransport(...)" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-08-part1-346.611tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Tiberian Sun Mod)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mod: Tiberian Sun</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of the Tiberian Sun mod in OpenRA. The engine uses a data-driven design where YAML files define game entities and rules. This visualization traces the full data pipeline for a GDI Jumpjet Infantry unit.

    Key Architectural Flow (Example: Creating a Jumpjet):
    1.  The C# Game Engine (`Game.cs`) starts and loads the mod's rules via `Ruleset.cs`.
    2.  `Ruleset.cs` parses `defaults.yaml` to understand abstract base actor types like `^Infantry` and `^Soldier`, which define shared traits.
    3.  It then parses specific actor definitions from `gdi-infantry.yaml`. The `JUMPJET` actor `Inherits` its properties from `^Soldier` and adds its own specific data like `Cost`, `HP`, `Weapon`, and the special `jumpjet` locomotor.
    4.  The `world.yaml` file defines the behavior of the `jumpjet` locomotor, such as its speed on different terrain types.
    5.  When a Jumpjet is created in the game, the C# `Actor.cs` instance refers to `sequences/infantry.yaml` to find the correct animation sequences (e.g., `stand`, `run`, `flying`).
    6.  This sequence file points to the raw sprite asset, `jumpjet.shp`, which is then loaded from the `bits/` directory and rendered by the engine.
    This demonstrates a clear separation of concerns: C# for behavior, YAML for data/schema, and binary files for assets.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMod('ts')" }] },
        { id: 11, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 300 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "Build()" }, { name: "Render()" }] },

        // ======================================================================
        //  RULES & DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "📚", columns: [{ name: "LoadDefaults()" }, { name: "Actors (Dictionary)" }] },
        { id: 21, name: "defaults.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 400, y: 300 }, icon: "📜", columns: [{ name: "^Infantry" }, { name: "^Soldier" }, { name: "^GainsExperience" }, { name: "^Vehicle" }, { name: "^Building" }] },
        { id: 22, name: "world.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 400, y: 600 }, icon: "🌍", columns: [{ name: "Locomotor@FOOT" }, { name: "JumpjetLocomotor@JUMPJET" }, { name: "TerrainSpeeds" }] },
        { id: 23, name: "palettes.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 400, y: 850 }, icon: "🎨", columns: [{ name: "PaletteFromFile@player" }, { name: "PaletteFromFile@bright" }] },

        // ======================================================================
        //  SPECIFIC ACTOR DEFINITION (DATA/SCHEMA)
        // ======================================================================
        { id: 30, name: "gdi-infantry.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 800, y: 150 }, icon: "🚶", columns: [{ name: "JUMPJET" }, { name: "  Inherits: ^Soldier" }, { name: "  Inherits: ^GainsExperience" }, { name: "  Cost: 600" }, { name: "  Weapon: JumpCannon" }] },
        { id: 31, name: "gdi-structures.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 800, y: 450 }, icon: "🏗️", columns: [{ name: "GAPOWR" }, { name: "  Inherits: ^Building" }, { name: "  ProvidesPrerequisite: anypower" }] },
        { id: 32, name: "gdi-vehicles.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 800, y: 700 }, icon: "🚚", columns: [{ name: "APC" }, { name: "  Inherits: ^Tank" }, { name: "  Cargo: MaxWeight: 5" }] },
        
        // ======================================================================
        //  ASSET PIPELINE (DATA/SCHEMA -> SERVICE)
        // ======================================================================
        { id: 40, name: "sequences/infantry.yaml", path:"mods/ts/sequences/", nodeType: "db", pos: { x: 1200, y: 150 }, icon: "🎬", columns: [{ name: "jumpjet:" }, { name: "  Filename: jumpjet.shp" }, { name: "  stand:" }, { name: "  flying:" }] },
        { id: 41, name: "jumpjet.shp", path:"(inferred asset)", nodeType: "service", pos: { x: 1600, y: 150 }, icon: "🖼️", columns: [{ name: "Jumpjet Sprite Data" }] },
        { id: 42, name: "husks.yaml", path:"mods/ts/rules/", nodeType: "db", pos: { x: 1200, y: 400 }, icon: "💀", columns: [{ name: "JUMPJET.Husk" }, { name: "  Inherits: ^AircraftHusk" }, { name: "  RenderSprites: Image: jumpjet" }] },
        { id: 43, name: "unittem.pal", path:"(inferred asset)", nodeType: "service", pos: { x: 1600, y: 400 }, icon: "🎨", columns: [{ name: "Unit Color Palette" }] }
    ],
    relationships: [
        // Engine Initialization
        { from: { table: "Game.cs (Engine)", column: "LoadMod('ts')" }, to: { table: "Ruleset.cs", column: "LoadDefaults()" }, type: "flow" },

        // Ruleset loading all definition files
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "defaults.yaml", column: "^Infantry" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "gdi-infantry.yaml", column: "JUMPJET" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "world.yaml", column: "JumpjetLocomotor@JUMPJET" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "palettes.yaml", column: "PaletteFromFile@player" }, type: "read" },

        // Jumpjet Inheritance and Definition
        { from: { table: "gdi-infantry.yaml", column: "  Inherits: ^Soldier" }, to: { table: "defaults.yaml", column: "^Soldier" }, type: "read" },
        { from: { table: "gdi-infantry.yaml", column: "  Inherits: ^GainsExperience" }, to: { table: "defaults.yaml", column: "^GainsExperience" }, type: "read" },
        { from: { table: "gdi-infantry.yaml", column: "  Locomotor: jumpjet" }, to: { table: "world.yaml", column: "JumpjetLocomotor@JUMPJET" }, type: "read" },
        
        // Actor Creation and Rendering Pipeline
        { from: { table: "Actor.cs (Object)", column: "Render()" }, to: { table: "gdi-infantry.yaml", column: "JUMPJET" }, type: "read" },
        { from: { table: "Actor.cs (Object)", column: "Render()" }, to: { table: "sequences/infantry.yaml", column: "jumpjet:" }, type: "read" },
        { from: { table: "sequences/infantry.yaml", column: "  Filename: jumpjet.shp" }, to: { table: "jumpjet.shp", column: "Jumpjet Sprite Data" }, type: "read" },
        
        // Death and Husk Creation
        { from: { table: "gdi-infantry.yaml", column: "SpawnActorOnDeath@airborne" }, to: { table: "husks.yaml", column: "JUMPJET.Husk" }, type: "read" },
        
        // Palette Usage
        { from: { table: "palettes.yaml", column: "PaletteFromFile@player" }, to: { table: "unittem.pal", column: "Unit Color Palette" }, type: "read" },
        { from: { table: "sequences/infantry.yaml", column: "Palette: player-nomuzzle" }, to: { table: "palettes.yaml", column: "PaletteFromFile@player" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-08-part2-A-585.587tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Map Instantiation)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Map Loading Process</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of how OpenRA loads and instantiates a game map. The system is highly data-driven, separating the core C# engine logic from the specific map and mod data defined in YAML and other asset files.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) is instructed to load a map (e.g., "karasjok").
    2.  It finds the corresponding `map.yaml` file, which acts as the master "instance document" for the level.
    3.  The engine parses the `map.yaml` for high-level information like `Title`, `MapSize`, `Players`, and the `Tileset` to use.
    4.  Crucially, it reads the long `Actors:` list. For each entry (e.g., `Actor13: aban11`), it looks up the definition of that actor type (`aban11`) in the mod's global `Ruleset` (which was pre-loaded from `rules/*.yaml` files).
    5.  Simultaneously, the engine reads the `map.bin` file. This binary file contains the raw grid data for the terrain (hills, cliffs, water).
    6.  The `World.cs` state manager then begins to populate the game world. It uses the `map.bin` data and the specified `Tileset` graphics to render the terrain.
    7.  It then iterates through the `Actors:` list from `map.yaml` and creates an `Actor.cs` object in memory for each one, placing it at the specified `Location` and assigning it to the specified `Owner`.
    8.  The result is a fully instantiated, playable game world where all objects and terrain are derived from these structured data files.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "LoadMap('karasjok')" }, { name: "StartGame()" }] },
        { id: 11, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "📚", columns: [{ name: "LoadFromMod()" }, { name: "ActorInfoFor(string name)" }] },
        { id: 12, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🌍", columns: [{ name: "CreateActor()" }, { name: "LoadTerrain()" }] },
        
        // ======================================================================
        //  MAP INSTANCE & DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"maps/karasjok/", nodeType: "db", pos: { x: 450, y: 50 }, icon: "🗺️", columns: [{ name: "RequiresMod: ts" }, { name: "Tileset: SNOW" }, { name: "Players: Multi0, Multi1..." }, { name: "Actors: (1347 items)" }, { name: "  Actor13: aban11" }] },
        { id: 21, name: "Mod Rules (YAML)", path:"mods/ts/rules/", nodeType: "db", pos: { x: 450, y: 400 }, icon: "📝", columns: [{ name: "aban11: ..." }, { name: "tracks04: ..." }, { name: "ingrnlmp: ..." }, { name: "tree05: ..." }] },
        
        // ======================================================================
        //  RAW ASSETS / SERVICES
        // ======================================================================
        { id: 30, name: "map.bin", path:"maps/karasjok/", nodeType: "service", pos: { x: 850, y: 50 }, icon: "▦", columns: [{ name: "Raw Tile Grid Data" }, { name: "Heightmap Info" }] },
        { id: 31, name: "SNOW Tileset Graphics", path:"mods/ts/tilesets/", nodeType: "service", pos: { x: 850, y: 250 }, icon: "🎨", columns: [{ name: "snow.png" }, { name: "snow.json" }] },
        { id: 32, name: "Actor Sprites", path:"mods/ts/bits/", nodeType: "service", pos: { x: 850, y: 450 }, icon: "🖼️", columns: [{ name: "aban11.shp" }, { name: "tree05.shp" }] },

        // ======================================================================
        //  RUNTIME OBJECTS (CONCEPTUAL)
        // ======================================================================
        { id: 50, name: "Live Game World", path:"(In Memory)", nodeType: "code", pos: { x: 450, y: 750 }, icon: "💡", columns: [{ name: "Rendered Terrain" }, { name: "Actor Instances" }, { name: "Player States" }] }
    ],
    relationships: [
        // 1. Engine starts loading the map
        { from: { table: "Game.cs (Engine)", column: "LoadMap('karasjok')" }, to: { table: "map.yaml", column: "Title: Town of Karasjok" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('karasjok')" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" },

        // 2. Ruleset is populated with mod data
        { from: { table: "Game.cs (Engine)", column: "LoadMap('karasjok')" }, to: { table: "Ruleset.cs", column: "LoadFromMod()" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod()" }, to: { table: "Mod Rules (YAML)", column: "aban11: ..." }, type: "read" },
        
        // 3. World loads terrain from binary data, using tileset specified in map.yaml
        { from: { table: "World.cs (State)", column: "LoadTerrain()" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Tileset: SNOW" }, to: { table: "SNOW Tileset Graphics", column: "snow.png" }, type: "read" },
        { from: { table: "World.cs (State)", column: "LoadTerrain()" }, to: { table: "Live Game World", column: "Rendered Terrain" }, type: "write" },

        // 4. World creates actors based on the map's actor list
        { from: { table: "World.cs (State)", column: "CreateActor()" }, to: { table: "map.yaml", column: "Actors: (1347 items)" }, type: "read" },
        { from: { table: "map.yaml", column: "  Actor13: aban11" }, to: { table: "Ruleset.cs", column: "ActorInfoFor(string name)" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "ActorInfoFor(string name)" }, to: { table: "Mod Rules (YAML)", column: "aban11: ..." }, type: "read" },
        { from: { table: "Mod Rules (YAML)", column: "aban11: ..." }, to: { table: "Actor Sprites", column: "aban11.shp" }, type: "read" },
        { from: { table: "World.cs (State)", column: "CreateActor()" }, to: { table: "Live Game World", column: "Actor Instances" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-08-part2-B-636.732tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Shellmap Example)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Shellmap: Fields of Green</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of the "Fields of Green" shellmap in the OpenRA engine. A shellmap is a dynamic, non-interactive map used as a background for menus. It's an excellent example of a purely data- and script-driven process.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) loads the shellmap.
    2.  It reads `map.yaml`, which defines the players (`Nod`, `GDI`) and instantiates key actors with unique names like `nodhand1` (Hand of Nod), `gdibar1` (Barracks), and waypoints (`NodW`, `GDIW`, `North`, `South`). This `map.yaml` acts as a data source of named objects for the script.
    3.  The `map.yaml` points to `rules.yaml`, which the engine loads for map-specific rules.
    4.  The `rules.yaml` does two important things: it defines a custom `unkillable` condition and uses the `LuaScript:` key to tell the engine to load and execute `fields-of-green.lua`.
    5.  The `WorldLoaded()` function in the Lua script is the entry point. It immediately starts several infinite loops:
        - It tells the named factories (`nodhand1`, `gdibar1`) to start producing units in a loop (`ProduceUnits`).
        - It starts timed loops using `Trigger.AfterDelay` to spawn waves of reinforcements from the named waypoints (`SendNodInfantry`, `SendGDIVehicles`).
    6.  Every unit created by these loops is given a simple command via `BindActorTriggers`: `AttackMove` to the location of the enemy base.
    7.  This creates a continuous, self-running battle diorama, all orchestrated by the Lua script acting on the data and actor instances defined in the YAML files.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap('fields-of-green')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Produce(unit)" }, { name: "AttackMove(loc)" }, { name: "GrantCondition('unkillable')" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"fields-of-green/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Players: Nod, GDI" }, { name: "nodhand1: nahand" }, { name: "gdibar1: gapile" }, { name: "NodW: waypoint" }, { name: "GDIW: waypoint" }, { name: "North: waypoint" }, { name: "South: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"fields-of-green/", nodeType: "db", pos: { x: 400, y: 550 }, icon: "🔧", columns: [{ name: "LuaScript: Scripts: fields-of-green.lua" }, { name: "DamageMultiplier@UNKILLABLE" }] },
        
        // ======================================================================
        //  SHELLMAP LOGIC (CODE)
        // ======================================================================
        { id: 30, name: "fields-of-green.lua", path:"fields-of-green/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "SetupFactories()" }, { name: "SetupInvulnerability()" }, { name: "ProduceUnits(factory, units)" }, { name: "SendNodInfantry()" }, { name: "SendGDIVehicles()" }, { name: "BindActorTriggers(actor, loc)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"fields-of-green/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('fields-of-green')" }, to: { table: "map.yaml", column: "Title: Fields of Green" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('fields-of-green')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript: Scripts: fields-of-green.lua" }, type: "read" },
        
        // Script Loading & Initialization
        { from: { table: "rules.yaml", column: "LuaScript: Scripts: fields-of-green.lua" }, to: { table: "fields-of-green.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "fields-of-green.lua", column: "SetupInvulnerability()" }, to: { table: "rules.yaml", column: "DamageMultiplier@UNKILLABLE" }, type: "read" },
        { from: { table: "fields-of-green.lua", column: "SetupInvulnerability()" }, to: { table: "Actor.cs (Object)", column: "GrantCondition('unkillable')" }, type: "write" },
        
        // Logic -> Reads from Map Data
        { from: { table: "fields-of-green.lua", column: "ProduceUnits(factory, units)" }, to: { table: "map.yaml", column: "nodhand1: nahand" }, type: "read" },
        { from: { table: "fields-of-green.lua", column: "SendNodInfantry()" }, to: { table: "map.yaml", column: "NodW: waypoint" }, type: "read" },
        { from: { table: "fields-of-green.lua", column: "SendGDIVehicles()" }, to: { table: "map.yaml", column: "North: waypoint" }, type: "read" },

        // Script -> Issues Commands to Engine
        { from: { table: "fields-of-green.lua", column: "ProduceUnits(factory, units)" }, to: { table: "Actor.cs (Object)", column: "Produce(unit)" }, type: "write" },
        { from: { table: "fields-of-green.lua", column: "SendNodInfantry()" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" },
        { from: { table: "fields-of-green.lua", column: "BindActorTriggers(actor, loc)" }, to: { table: "Actor.cs (Object)", column: "AttackMove(loc)" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-08-part3-A-610.971tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Full Data Pipeline)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Map Loading Process</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of how OpenRA loads and instantiates a game map. The system is highly data-driven, separating the core C# engine logic from the specific map and mod data defined in YAML and other asset files.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) is instructed to load a map (e.g., "uganda").
    2.  It finds the corresponding `map.yaml` file, which acts as the master "instance document" for the level.
    3.  The engine parses the `map.yaml` for high-level information like `Title`, `MapSize`, `Players`, and the `Tileset` to use.
    4.  Crucially, it reads the long `Actors:` list. For each entry (e.g., `Actor9: tree22`), it looks up the definition of that actor type (`tree22`) in the mod's global `Ruleset` (which was pre-loaded from `rules/*.yaml` files).
    5.  Simultaneously, the engine reads the `map.bin` file. This binary file contains the raw grid data for the terrain (hills, cliffs, water).
    6.  The `World.cs` state manager then begins to populate the game world. It uses the `map.bin` data and the specified `Tileset` graphics to render the terrain.
    7.  It then iterates through the `Actors:` list from `map.yaml` and creates an `Actor.cs` object in memory for each one, placing it at the specified `Location` and assigning it to the specified `Owner`.
    8.  The result is a fully instantiated, playable game world where all objects and terrain are derived from these structured data files.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "LoadMap(mapName)" }, { name: "StartGame()" }] },
        { id: 11, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "📚", columns: [{ name: "LoadFromMod(modId)" }, { name: "ActorInfoFor(actorName)" }] },
        { id: 12, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🌍", columns: [{ name: "CreateActor(actorInfo)" }, { name: "LoadTerrain(mapData)" }] },
        
        // ======================================================================
        //  MAP INSTANCE & MOD DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml (Instance Data)", path:"maps/uganda/", nodeType: "db", pos: { x: 450, y: 50 }, icon: "🗺️", columns: [{ name: "RequiresMod: ts" }, { name: "Tileset: TEMPERATE" }, { name: "Actors: (Hundreds of entries)" }, { name: "Actor397: tree20" }, { name: "Actor398: tree22" }] },
        { id: 21, name: "Mod Rules (*.yaml)", path:"mods/ts/rules/", nodeType: "db", pos: { x: 450, y: 450 }, icon: "📝", columns: [{ name: "Defines: tree20" }, { name: "Defines: tree22" }, { name: "Defines: ^Infantry" }, { name: "..." }] },
        { id: 22, name: "Tileset Definitions", path:"mods/ts/tilesets/", nodeType: "db", pos: { x: 450, y: 700 }, icon: "🏞️", columns: [{ name: "temperate.yaml" }, { name: "snow.yaml" }] },
        
        // ======================================================================
        //  RAW ASSETS / SERVICES
        // ======================================================================
        { id: 30, name: "map.bin", path:"maps/uganda/", nodeType: "service", pos: { x: 850, y: 50 }, icon: "▦", columns: [{ name: "Raw Tile Grid Data" }, { name: "Heightmap Info" }] },
        { id: 31, name: "Tileset Graphics (*.png)", path:"mods/ts/tilesets/", nodeType: "service", pos: { x: 850, y: 250 }, icon: "🎨", columns: [{ name: "temperat.png" }, { name: "snow.png" }] },
        { id: 32, name: "Actor Sprites (*.shp)", path:"mods/ts/bits/", nodeType: "service", pos: { x: 850, y: 450 }, icon: "🖼️", columns: [{ name: "tree20.shp" }, { name: "tree22.shp" }] },

        // ======================================================================
        //  RUNTIME OBJECTS (CONCEPTUAL)
        // ======================================================================
        { id: 50, name: "Live Game World", path:"(In Memory)", nodeType: "code", pos: { x: 450, y: 950 }, icon: "💡", columns: [{ name: "Rendered Terrain" }, { name: "Actor Instances" }, { name: "Player States" }] }
    ],
    relationships: [
        // 1. Engine starts loading the map
        { from: { table: "Game.cs (Engine)", column: "LoadMap(mapName)" }, to: { table: "map.yaml (Instance Data)", column: "Title: The Way to Uganda" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap(mapName)" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" },

        // 2. Ruleset is populated with mod data based on RequiresMod
        { from: { table: "map.yaml (Instance Data)", column: "RequiresMod: ts" }, to: { table: "Ruleset.cs", column: "LoadFromMod(modId)" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod(modId)" }, to: { table: "Mod Rules (*.yaml)", column: "Defines: tree20" }, type: "read" },
        
        // 3. World loads terrain from binary data, using tileset specified in map.yaml
        { from: { table: "World.cs (State)", column: "LoadTerrain(mapData)" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" },
        { from: { table: "map.yaml (Instance Data)", column: "Tileset: TEMPERATE" }, to: { table: "Tileset Definitions", column: "temperate.yaml" }, type: "read" },
        { from: { table: "Tileset Definitions", column: "temperate.yaml" }, to: { table: "Tileset Graphics (*.png)", column: "temperat.png" }, type: "read" },
        { from: { table: "World.cs (State)", column: "LoadTerrain(mapData)" }, to: { table: "Live Game World", column: "Rendered Terrain" }, type: "write" },

        // 4. World creates actors based on the map's actor list
        { from: { table: "World.cs (State)", column: "CreateActor(actorInfo)" }, to: { table: "map.yaml (Instance Data)", column: "Actors: (Hundreds of entries)" }, type: "read" },
        { from: { table: "map.yaml (Instance Data)", column: "Actor397: tree20" }, to: { table: "Ruleset.cs", column: "ActorInfoFor(actorName)" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "ActorInfoFor(actorName)" }, to: { table: "Mod Rules (*.yaml)", column: "Defines: tree20" }, type: "read" },
        { from: { table: "Mod Rules (*.yaml)", column: "Defines: tree20" }, to: { table: "Actor Sprites (*.shp)", column: "tree20.shp" }, type: "read" },
        { from: { table: "World.cs (State)", column: "CreateActor(actorInfo)" }, to: { table: "Live Game World", column: "Actor Instances" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-08-part3-B-627.622tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Map Instantiation)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Map Instantiation</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of how OpenRA loads and instantiates a game map. The system is highly data-driven, separating the core C# engine logic from the specific map and mod data defined in YAML and other asset files.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) is instructed to load a map (e.g., "1ice6").
    2.  It finds the corresponding `map.yaml` file, which acts as the master "instance document" for the level.
    3.  The engine parses the `map.yaml` for high-level information like `Title`, `MapSize`, `Players`, and the `Tileset` to use.
    4.  Crucially, it reads the long `Actors:` list. For each entry (e.g., `Actor0: aban03`), it looks up the definition of that actor type (`aban03`) in the mod's global `Ruleset` (which was pre-loaded from `rules/*.yaml` files).
    5.  Simultaneously, the engine reads the `map.bin` file. This binary file contains the raw grid data for the terrain (hills, cliffs, water).
    6.  The `World.cs` state manager then begins to populate the game world. It uses the `map.bin` data and the specified `Tileset` graphics to render the terrain.
    7.  It then iterates through the `Actors:` list from `map.yaml` and creates an `Actor.cs` object in memory for each one, placing it at the specified `Location` and assigning it to the specified `Owner`.
    8.  The result is a fully instantiated, playable game world where all objects and terrain are derived from these structured data files.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "LoadMap(mapName)" }, { name: "StartGame()" }] },
        { id: 11, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "📚", columns: [{ name: "LoadFromMod(modId)" }, { name: "ActorInfoFor(actorName)" }] },
        { id: 12, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🌍", columns: [{ name: "CreateActor(actorInfo)" }, { name: "LoadTerrain(mapData)" }] },
        
        // ======================================================================
        //  MAP INSTANCE & MOD DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml (Instance Data)", path:"maps/1ice6/", nodeType: "db", pos: { x: 450, y: 50 }, icon: "🗺️", columns: [{ name: "RequiresMod: ts" }, { name: "Tileset: SNOW" }, { name: "Players: Multi0, Multi1..." }, { name: "Actors: (400+ items)" }, { name: "Actor0: aban03" }, { name: "Actor53: mutant" }] },
        { id: 21, name: "Mod Rules (*.yaml)", path:"mods/ts/rules/", nodeType: "db", pos: { x: 450, y: 450 }, icon: "📝", columns: [{ name: "Defines: aban03" }, { name: "Defines: mutant" }, { name: "Defines: ^Infantry" }, { name: "..." }] },
        { id: 22, name: "Tileset Definitions", path:"mods/ts/tilesets/", nodeType: "db", pos: { x: 450, y: 700 }, icon: "🏞️", columns: [{ name: "temperate.yaml" }, { name: "snow.yaml" }] },
        
        // ======================================================================
        //  RAW ASSETS / SERVICES
        // ======================================================================
        { id: 30, name: "map.bin", path:"maps/1ice6/", nodeType: "service", pos: { x: 850, y: 50 }, icon: "▦", columns: [{ name: "Raw Tile Grid Data" }, { name: "Heightmap Info" }] },
        { id: 31, name: "Tileset Graphics (*.png)", path:"mods/ts/tilesets/", nodeType: "service", pos: { x: 850, y: 250 }, icon: "🎨", columns: [{ name: "temperat.png" }, { name: "snow.png" }] },
        { id: 32, name: "Actor Sprites (*.shp)", path:"mods/ts/bits/", nodeType: "service", pos: { x: 850, y: 450 }, icon: "🖼️", columns: [{ name: "aban03.shp" }, { name: "mutant.shp" }] },

        // ======================================================================
        //  RUNTIME OBJECTS (CONCEPTUAL)
        // ======================================================================
        { id: 50, name: "Live Game World", path:"(In Memory)", nodeType: "code", pos: { x: 450, y: 950 }, icon: "💡", columns: [{ name: "Rendered Terrain" }, { name: "Actor Instances" }, { name: "Player States" }] }
    ],
    relationships: [
        // 1. Engine starts loading the map
        { from: { table: "Game.cs (Engine)", column: "LoadMap(mapName)" }, to: { table: "map.yaml (Instance Data)", column: "Title: No where to run" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap(mapName)" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" },

        // 2. Ruleset is populated with mod data based on RequiresMod
        { from: { table: "map.yaml (Instance Data)", column: "RequiresMod: ts" }, to: { table: "Ruleset.cs", column: "LoadFromMod(modId)" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod(modId)" }, to: { table: "Mod Rules (*.yaml)", column: "Defines: aban03" }, type: "read" },
        
        // 3. World loads terrain from binary data, using tileset specified in map.yaml
        { from: { table: "World.cs (State)", column: "LoadTerrain(mapData)" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" },
        { from: { table: "map.yaml (Instance Data)", column: "Tileset: SNOW" }, to: { table: "Tileset Definitions", column: "snow.yaml" }, type: "read" },
        { from: { table: "Tileset Definitions", column: "snow.yaml" }, to: { table: "Tileset Graphics (*.png)", column: "snow.png" }, type: "read" },
        { from: { table: "World.cs (State)", column: "LoadTerrain(mapData)" }, to: { table: "Live Game World", column: "Rendered Terrain" }, type: "write" },

        // 4. World creates actors based on the map's actor list
        { from: { table: "World.cs (State)", column: "CreateActor(actorInfo)" }, to: { table: "map.yaml (Instance Data)", column: "Actors: (400+ items)" }, type: "read" },
        { from: { table: "map.yaml (Instance Data)", column: "Actor0: aban03" }, to: { table: "Ruleset.cs", column: "ActorInfoFor(actorName)" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "ActorInfoFor(actorName)" }, to: { table: "Mod Rules (*.yaml)", column: "Defines: aban03" }, type: "read" },
        { from: { table: "Mod Rules (*.yaml)", column: "Defines: aban03" }, to: { table: "Actor Sprites (*.shp)", column: "aban03.shp" }, type: "read" },
        { from: { table: "World.cs (State)", column: "CreateActor(actorInfo)" }, to: { table: "Live Game World", column: "Actor Instances" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-09-part1-581.429tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Full Architecture Overview</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Engine & Mod Architecture</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the comprehensive architecture of the OpenRA engine and its modding system, using the Tiberian Sun (`ts`) mod as an example. The engine follows a highly data-driven design where C# code provides the core functionality, but almost all game content—units, rules, missions, and assets—is defined in external data files, primarily YAML.

    Key Architectural Flow:
    1.  **Engine Start:** The `Game.cs` executable launches. It knows about the different mods available (e.g., `ra`, `cnc`, `ts`).
    2.  **Mod Loading:** When a mod like `ts` is selected, the engine reads its `mod.yaml` file. This file acts as a manifest, providing a complete list of all other data files to load.
    3.  **Rule Compilation:** `Ruleset.cs` is responsible for parsing all the YAML files listed under the `Rules:` section of `mod.yaml`. It starts with `defaults.yaml` to create abstract "base classes" for actors (like `^Vehicle` or `^Infantry`) and then layers on the specific definitions from files like `gdi-vehicles.yaml`. This creates a complete, in-memory database of all possible actors and their properties.
    4.  **Map Instantiation:** When a map is chosen, the engine reads its `map.yaml` file. This is an "instance" document.
        - The `RequiresMod: ts` line confirms the context.
        - The `Tileset:` line specifies which graphics to use for rendering the terrain.
        - The `Actors:` list contains every object to be placed on the map. For each actor name (e.g., `tree01`), the engine queries the `Ruleset` for its full definition.
    5.  **World Creation:** The `World.cs` class uses the data from `map.yaml` and `map.bin` (for terrain geometry) to build the game world. It creates an `Actor.cs` object for every entry in the map's actor list, placing it at the specified location.
    6.  **Asset Linking:** When a specific actor (like a `1TNK`) needs to be rendered, the engine looks up its name in the appropriate `sequences/*.yaml` file. This file provides the mapping between animation states (`idle`, `move`) and the actual binary sprite file (`1tnk.shp`) that contains the pixel data.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine Entry)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Main()" }, { name: "LoadMod(modId)" }, { name: "LoadMap(mapName)" }] },
        { id: 11, name: "Ruleset.cs (Data Loader)", path:"OpenRA.Game/GameRules/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "📚", columns: [{ name: "LoadFromMod()" }, { name: "ActorInfoFor(actorName)" }] },
        { id: 12, name: "World.cs (Game State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🌍", columns: [{ name: "CreateActor(actorInfo)" }, { name: "Tick()" }] },
        { id: 13, name: "Actor.cs (Game Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 900 }, icon: "🤖", columns: [{ name: "Info (ActorInfo)" }, { name: "Owner" }, { name: "Location" }] },

        // ======================================================================
        //  MOD & MAP DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 100, name: "mod.yaml (Mod Manifest)", path:"mods/ts/mod.yaml", nodeType: "db", pos: { x: 400, y: 50 }, icon: "📦", columns: [{ name: "Title: Tiberian Sun" }, { name: "Rules: [...]" }, { name: "Sequences: [...]" }, { name: "Weapons: [...]" }, { name: "Missions: missions.yaml" }] },
        { id: 101, name: "missions.yaml (Mission List)", path:"mods/ra/missions.yaml", nodeType: "db", pos: { x: 400, y: 350 }, icon: "📄", columns: [{ name: "Allied Campaign:" }, { name: "  allies-01" }, { name: "Soviet Campaign:" }, { name: "  soviet-01" }] },
        { id: 102, name: "map.yaml (Level Instance)", path:"maps/sunstroke/map.yaml", nodeType: "db", pos: { x: 400, y: 650 }, icon: "🗺️", columns: [{ name: "RequiresMod: ts" }, { name: "Tileset: TEMPERATE" }, { name: "Actors:" }, { name: "  Actor615: mpspawn" }, { name: "  Actor627: trock05" }] },
        
        // ======================================================================
        //  RULES PIPELINE (DATA/SCHEMA)
        // ======================================================================
        { id: 200, name: "defaults.yaml (Base Types)", path:"mods/ts/rules/", nodeType: "db", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "^Infantry" }, { name: "^Vehicle" }, { name: "^Building" }] },
        { id: 201, name: "vehicles.yaml (Concrete Types)", path:"mods/ts/rules/", nodeType: "db", pos: { x: 800, y: 350 }, icon: "🚚", columns: [{ name: "4TNK (Mammoth Tank)" }, { name: "Inherits: ^Vehicle" }, { name: "Armament: Weapon: 120mm" }] },
        { id: 202, name: "weapons.yaml (Component Data)", path:"mods/ts/weapons/", nodeType: "db", pos: { x: 800, y: 650 }, icon: "💥", columns: [{ name: "120mm:" }, { name: "ReloadDelay: 80" }, { name: "Range: 6c768" }] },

        // ======================================================================
        //  ASSET & UI PIPELINE
        // ======================================================================
        { id: 300, name: "sequences/vehicles.yaml", path:"mods/ts/sequences/", nodeType: "db", pos: { x: 1200, y: 350 }, icon: "🎬", columns: [{ name: "4tnk:" }, { name: "  idle: ..." }, { name: "  Filename: 4tnk.shp" }] },
        { id: 301, name: "4tnk.shp (Sprite Asset)", path:"mods/ts/bits/", nodeType: "service", pos: { x: 1600, y: 350 }, icon: "🖼️", columns: [{ name: "Binary Sprite Data" }] },
        { id: 302, name: "chrome.yaml (UI Skin)", path:"mods/ra/chrome.yaml", nodeType: "ui", pos: { x: 1200, y: 50 }, icon: "🎨", columns: [{ name: "^Sidebar" }, { name: "sidebar-allies" }, { name: "sidebar-soviet" }] },
        { id: 303, name: "map.bin (Terrain Data)", path:"maps/sunstroke/", nodeType: "service", pos: { x: 1200, y: 650 }, icon: "▦", columns: [{ name: "Raw Tile Grid" }, { name: "Heightmap Info" }] }
    ],
    relationships: [
        // 1. Engine loads the Mod
        { from: { table: "Game.cs (Engine)", column: "LoadMod(modId)" }, to: { table: "mod.yaml (Mod Manifest)", column: "Title: Tiberian Sun" }, type: "read" },
        
        // 2. Mod Manifest points to all rule files
        { from: { table: "mod.yaml (Mod Manifest)", column: "Rules: [...]" }, to: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, type: "flow" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, to: { table: "defaults.yaml (Base Types)", column: "^Vehicle" }, type: "read" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, to: { table: "vehicles.yaml (Concrete Types)", column: "4TNK (Mammoth Tank)" }, type: "read" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, to: { table: "weapons.yaml (Component Data)", column: "120mm:" }, type: "read" },

        // 3. Inheritance within rules
        { from: { table: "vehicles.yaml (Concrete Types)", column: "Inherits: ^Vehicle" }, to: { table: "defaults.yaml (Base Types)", column: "^Vehicle" }, type: "read" },
        { from: { table: "vehicles.yaml (Concrete Types)", column: "Armament: Weapon: 120mm" }, to: { table: "weapons.yaml (Component Data)", column: "120mm:" }, type: "read" },

        // 4. Engine loads a specific map
        { from: { table: "Game.cs (Engine)", column: "LoadMap(mapName)" }, to: { table: "map.yaml (Instance Data)", column: "RequiresMod: ts" }, type: "read" },
        
        // 5. World creates actors from map instance data
        { from: { table: "World.cs (State)", column: "CreateActor(actorInfo)" }, to: { table: "map.yaml (Instance Data)", column: "Actors: (Hundreds of entries)" }, type: "read" },
        { from: { table: "map.yaml (Instance Data)", column: "Actor397: tree20" }, to: { table: "Ruleset.cs (Data Loader)", column: "ActorInfoFor(actorName)" }, type: "read" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "ActorInfoFor(actorName)" }, to: { table: "Mod Rules (*.yaml)", column: "Defines: tree20" }, type: "read" },
        { from: { table: "World.cs (State)", column: "CreateActor(actorInfo)" }, to: { table: "Actor.cs (Game Object)", column: "Info (ActorInfo)" }, type: "write" },
        
        // 6. Rendering pipeline: Actor -> Sequence -> Sprite
        { from: { table: "Actor.cs (Game Object)", column: "Render()" }, to: { table: "sequences/vehicles.yaml", column: "4tnk:" }, type: "read" },
        { from: { table: "sequences/vehicles.yaml", column: "  Filename: 4tnk.shp" }, to: { table: "Actor Sprites (*.shp)", column: "tree20.shp" }, type: "read" },
        
        // 7. Terrain Loading
        { from: { table: "World.cs (State)", column: "LoadTerrain(mapData)" }, to: { table: "map.bin", column: "Raw Tile Grid Data" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-09-part2.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Full Data Pipeline)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mod: Red Alert</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of the Red Alert mod in OpenRA. The engine uses a data-driven design where YAML files define game entities and rules, which are then interpreted by the core C# engine.

    Key Architectural Flow (Example: AI building a Light Tank):
    1.  The C# Game Engine (`Game.cs`) starts and loads the mod's rules via `Ruleset.cs`.
    2.  `Ruleset.cs` parses `defaults.yaml` to understand base actor types like `^Vehicle`.
    3.  It then parses specific actor definitions like `vehicles.yaml`, where `1tnk` (Light Tank) `Inherits` its properties from `^Vehicle` and sets its own specific `Cost`, `HP`, and `Weapon`.
    4.  The AI logic, defined in `ai.yaml`, decides to build a `1tnk` based on its strategy.
    5.  When the tank is created, the C# `Actor.cs` instance looks up its animation data in `sequences/vehicles.yaml`.
    6.  This sequence file points to the raw sprite asset, `1tnk.shp`, which is then loaded from the `bits/` directory and rendered.
    This demonstrates a clear separation of concerns: C# for behavior, YAML for data/schema, and binary files for assets.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "Build()" }] },

        // ======================================================================
        //  AI & GAME LOGIC (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "ai.yaml", path:"mods/ra/rules/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🧠", columns: [{ name: "UnitBuilderBotModule" }, { name: "UnitsToBuild:" }, { name: "  1tnk: 50" }, { name: "BaseBuilderBotModule" }] },
        { id: 21, name: "Ruleset.cs", path:"OpenRA.Game/GameRules/", nodeType: "db", pos: { x: 400, y: 350 }, icon: "📚", columns: [{ name: "Actors (Dictionary)" }, { name: "LoadDefaults()" }] },
        
        // ======================================================================
        //  ACTOR DEFINITION PIPELINE (DATA/SCHEMA)
        // ======================================================================
        { id: 30, name: "defaults.yaml", path:"mods/ra/rules/", nodeType: "db", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "^Vehicle" }, { name: "  Health:" }, { name: "  Armor:" }, { name: "  Selectable:" }] },
        { id: 31, name: "vehicles.yaml", path:"mods/ra/rules/", nodeType: "db", pos: { x: 800, y: 350 }, icon: "🚚", columns: [{ name: "1TNK (Light Tank)" }, { name: "  Inherits: ^Vehicle" }, { name: "  Valued: Cost: 600" }, { name: "  Armament: Weapon: 75mm" }] },
        { id: 32, name: "weapons.yaml", path:"mods/ra/rules/", nodeType: "db", pos: { x: 800, y: 650 }, icon: "💥", columns: [{ name: "75mm:" }, { name: "  Range: 4c0" }] },
        
        // ======================================================================
        //  ASSET PIPELINE (DATA/SCHEMA -> SERVICE)
        // ======================================================================
        { id: 40, name: "sequences/vehicles.yaml", path:"mods/ra/sequences/", nodeType: "db", pos: { x: 1200, y: 350 }, icon: "🎬", columns: [{ name: "1tnk:" }, { name: "  idle: ..." }, { name: "  move: ..." }, { name: "  turret: ..." }] },
        { id: 41, name: "1tnk.shp", path:"mods/ra/bits/", nodeType: "service", pos: { x: 1600, y: 350 }, icon: "🖼️", columns: [{ name: "Sprite/Animation Data" }] },
        { id: 42, "name": "e1.shp", "path": "mods/ra/bits/", "nodeType": "service", "pos": { "x": 1600, "y": 50 }, "icon": "🚶", "columns": [{ "name": "Infantry Sprite Data" }] },
        { id: 43, "name": "fact.shp", "path": "mods/ra/bits/", "nodeType": "service", "pos": { "x": 1600, "y": 650 }, "icon": "🏗️", "columns": [{ "name": "Building Sprite Data" }] }
    ],
    relationships: [
        // AI decides to build a unit
        { from: { table: "Game.cs (Engine)", column: "Run()" }, to: { table: "ai.yaml", column: "UnitBuilderBotModule" }, type: "flow" },
        { from: { table: "ai.yaml", column: "  1tnk: 50" }, to: { table: "Actor.cs (Object)", column: "Build()" }, type: "write" },

        // Engine loads all rules
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "Ruleset.cs", column: "LoadDefaults()" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "defaults.yaml", column: "^Vehicle" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadDefaults()" }, to: { table: "vehicles.yaml", column: "1TNK (Light Tank)" }, type: "read" },

        // Unit definition inheritance
        { from: { table: "vehicles.yaml", column: "  Inherits: ^Vehicle" }, to: { table: "defaults.yaml", column: "^Vehicle" }, type: "read" },
        { from: { table: "vehicles.yaml", column: "  Armament: Weapon: 75mm" }, to: { table: "weapons.yaml", column: "75mm:" }, type: "read" },

        // Unit creation and asset loading
        { from: { table: "World.cs (State)", column: "CreateActor()" }, to: { table: "vehicles.yaml", column: "1TNK (Light Tank)" }, type: "read" },
        { from: { table: "Actor.cs (Object)", column: "Owner" }, to: { table: "sequences/vehicles.yaml", column: "1tnk:" }, type: "read" },
        { from: { table: "sequences/vehicles.yaml", column: "1tnk:" }, to: { table: "1tnk.shp", column: "Sprite/Animation Data" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-10-part1-557.867tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Full Architecture Overview</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mod Architecture</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the comprehensive architecture of the OpenRA engine and its modding system, using the Red Alert (`ra`) mod as an example. The engine follows a highly data-driven design where C# code provides the core functionality, but almost all game content—units, rules, missions, and assets—is defined in external data files, primarily YAML.

    Key Architectural Flow:
    1.  **Engine Start:** The `Game.cs` executable launches. It knows about the different mods available.
    2.  **Mod Loading:** When a mod like `ra` is selected, the engine reads its `mod.yaml` file. This file acts as a manifest, providing a complete list of all other data files to load (rules, sequences, chrome, etc.).
    3.  **Rule Compilation:** `Ruleset.cs` is responsible for parsing all the YAML files listed under `Rules:`. It starts with `world.yaml` or `defaults.yaml` to create abstract "base classes" for actors (like `^Vehicle` or `^TrackedVehicle`) and then layers on the specific definitions from files like `vehicles.yaml`. This creates a complete, in-memory database of all possible actors.
    4.  **Map Instantiation:** When a map is chosen (e.g., `a-path-beyond.oramap`), the engine reads its `map.yaml` file. This is an "instance" document.
        - The `RequiresMod: ra` line confirms the context.
        - The `Actors:` list contains every object to be placed on the map. For each actor name (e.g., `1TNK`), the engine queries the `Ruleset` for its full definition.
    5.  **World Creation:** The `World.cs` class uses the data from `map.yaml` and `map.bin` (for terrain geometry) to build the game world. It creates an `Actor.cs` object for every entry in the map's actor list.
    6.  **Asset Linking:** When a specific actor (like a `1TNK`) needs to be rendered, the engine looks up its name in the appropriate `sequences/*.yaml` file. This file provides the mapping between animation states (`idle`, `move`) and the actual binary sprite file (`1tnk.shp`) that contains the pixel data.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "LoadMod('ra')" }, { name: "LoadMap(mapName)" }] },
        { id: 11, name: "Ruleset.cs (Data Loader)", path:"OpenRA.Game/GameRules/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "📚", columns: [{ name: "LoadFromMod()" }, { name: "ActorInfoFor(actorName)" }] },
        { id: 12, name: "World.cs (Game State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🌍", columns: [{ name: "CreateActor(actorInfo)" }] },
        
        // ======================================================================
        //  MOD & MAP DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 100, name: "mod.yaml (Mod Manifest)", path:"mods/ra/mod.yaml", nodeType: "db", pos: { x: 400, y: 50 }, icon: "📦", columns: [{ name: "Rules: [...]" }, { name: "Sequences: [...]" }, { name: "Missions: missions.yaml" }] },
        { id: 101, name: "missions.yaml", path:"mods/ra/missions.yaml", nodeType: "db", pos: { x: 400, y: 300 }, icon: "📄", columns: [{ name: "allies-01" }, { name: "soviet-01" }] },
        { id: 102, name: "map.yaml (Level Instance)", path:"maps/a-path-beyond/", nodeType: "db", pos: { x: 400, y: 550 }, icon: "🗺️", columns: [{ name: "RequiresMod: ra" }, { name: "Actors: 1TNK, E1, ..." }] },
        
        // ======================================================================
        //  RULES PIPELINE (DATA/SCHEMA)
        // ======================================================================
        { id: 200, name: "world.yaml (Base Types)", path:"mods/ra/rules/", nodeType: "db", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "^Vehicle" }, { name: "Locomotor@TRACKED" }] },
        { id: 201, name: "vehicles.yaml", path:"mods/ra/rules/", nodeType: "db", pos: { x: 800, y: 300 }, icon: "🚚", columns: [{ name: "1TNK (Light Tank)" }, { name: "Inherits: ^TrackedVehicle" }, { name: "Armament: Weapon: 25mm" }] },
        { id: 202, name: "weapons.yaml", path:"mods/ra/weapons/", nodeType: "db", pos: { x: 800, y: 550 }, icon: "💥", columns: [{ name: "25mm:" }, { name: "Range: 4c0" }] },

        // ======================================================================
        //  UI & ASSET PIPELINE
        // ======================================================================
        { id: 300, name: "ingame-player.yaml", path:"mods/ra/chrome/", nodeType: "ui", pos: { x: 1200, y: 50 }, icon: "🖥️", columns: [{ name: "@SIDEBAR_PRODUCTION" }, { name: "@COMMAND_BAR" }] },
        { id: 301, name: "sequences/vehicles.yaml", path:"mods/ra/sequences/", nodeType: "db", pos: { x: 1200, y: 300 }, icon: "🎬", columns: [{ name: "1tnk:" }, { name: "Filename: 1tnk.shp" }] },
        { id: 302, name: "1tnk.shp", path:"mods/ra/bits/", nodeType: "service", pos: { x: 1600, y: 300 }, icon: "🖼️", columns: [{ name: "Sprite Data" }] },
        { id: 303, name: "map.bin", path:"maps/a-path-beyond/", nodeType: "service", pos: { x: 1200, y: 550 }, icon: "▦", columns: [{ name: "Terrain Grid Data" }] },
        { id: 304, "name": "e1.shp", "path": "mods/ra/bits/", "nodeType": "service", "pos": { "x": 1600, "y": 500 }, "icon": "🚶", "columns": [{ "name": "Infantry Sprite" }] },
        { id: 305, "name": "gunfire2.aud", "path": "mods/ra/audio/", "nodeType": "service", "pos": { "x": 1600, "y": 700 }, "icon": "🔊", "columns": [{ "name": "Sound Effect" }] }
    ],
    relationships: [
        // 1. Engine loads the Mod
        { from: { table: "Game.cs (Engine)", column: "LoadMod('ra')" }, to: { table: "mod.yaml (Mod Manifest)", column: "Title: Red Alert" }, type: "read" },
        
        // 2. Mod Manifest points to rule files, which are loaded by the Ruleset
        { from: { table: "mod.yaml (Mod Manifest)", column: "Rules: [...]" }, to: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, type: "flow" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, to: { table: "world.yaml (Base Types)", column: "^Vehicle" }, type: "read" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "LoadFromMod()" }, to: { table: "vehicles.yaml", column: "1TNK (Light Tank)" }, type: "read" },

        // 3. Inheritance and Composition within Rules
        { from: { table: "vehicles.yaml", column: "Inherits: ^TrackedVehicle" }, to: { table: "world.yaml (Base Types)", column: "^Vehicle" }, type: "read" },
        { from: { table: "vehicles.yaml", column: "Armament: Weapon: 25mm" }, to: { table: "weapons.yaml", column: "25mm:" }, type: "read" },

        // 4. Engine loads a map
        { from: { table: "Game.cs (Engine)", column: "LoadMap(mapName)" }, to: { table: "map.yaml (Level Instance)", column: "RequiresMod: ra" }, type: "read" },
        
        // 5. World creates actors from the map instance, using the loaded ruleset
        { from: { table: "World.cs (Game State)", column: "CreateActor(actorInfo)" }, to: { table: "map.yaml (Level Instance)", column: "Actors: 1TNK, E1, ..." }, type: "read" },
        { from: { table: "map.yaml (Level Instance)", column: "Actors: 1TNK, E1, ..." }, to: { table: "Ruleset.cs (Data Loader)", column: "ActorInfoFor(actorName)" }, type: "read" },
        { from: { table: "Ruleset.cs (Data Loader)", column: "ActorInfoFor(actorName)" }, to: { table: "vehicles.yaml", column: "1TNK (Light Tank)" }, type: "read" },
        
        // 6. Asset & UI Pipeline
        { from: { table: "vehicles.yaml", column: "1TNK (Light Tank)" }, to: { table: "sequences/vehicles.yaml", column: "1tnk:" }, type: "read" },
        { from: { table: "sequences/vehicles.yaml", column: "Filename: 1tnk.shp" }, to: { table: "1tnk.shp", column: "Sprite Data" }, type: "read" },
        { from: { table: "World.cs (Game State)", column: "LoadTerrain()" }, to: { table: "map.bin", column: "Terrain Grid Data" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMod('ra')" }, to: { table: "ingame-player.yaml", column: "@SIDEBAR_PRODUCTION" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-10-part2.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Minigame Example)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Minigame: Bomber John</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file represents a portion of the OpenRA game engine. The "database schema" is not in a single SQL file but is defined through C# classes and populated by YAML files.

    Key Files Defining the Schema and Data Structure:
    - Ruleset.cs: This is the master container for all game rules. It holds dictionaries of ActorInfo, WeaponInfo, MusicInfo, etc. Think of it as the entire database schema for a given mod (like Red Alert or Tiberian Dawn).
    - ActorInfo.cs: This class is the blueprint for every unit, building, and projectile in the game. It is the equivalent of a CREATE TABLE statement for an "actors" table. It defines which components (Traits) an actor will have.
    - TraitInfo.cs (and all its derivatives): Every file that ends in ...Info.cs and inherits from TraitInfo acts like a "column definition" for the ActorInfo "table". For example, HealthInfo defines that an actor has health points, and ArmamentInfo defines that an actor has weapons.
    - YAML Files (*.yaml): These are the actual data files that populate the schema defined by the Info.cs classes. `map.yaml` defines the initial state of a level. `rules.yaml` defines the game logic, custom units, and behaviors for that level. `sequences.yaml` links actors to their animation assets.
    - Core State Classes (Actor.cs, Player.cs, World.cs): These classes represent the live, in-game instances of the data. Actor.cs is like a row in the "actors" table, holding the current state (position, health, owner) of a single unit.
    - Lua Script Files (*.lua): These files contain game logic, mission scripts, and AI behaviors. They interact with the core C# engine to create dynamic gameplay, acting like business logic that reads from and writes to the live game state.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 300 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 550 }, icon: "🤖", columns: [{ name: "Health" }, { name: "TransformsInto" }, { name: "FireWarheadsOnDeath" }] },

        // ======================================================================
        //  MAP INSTANCE & UI
        // ======================================================================
        { id: 20, name: "map.yaml", path:"bomber-john/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Bomber John" }, { name: "Players: Multi0-7" }, { name: "Actors:" }, { name: "mnlyr (Bomber)" }, { name: "ftur (Turret)" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "map.ftl", path:"bomber-john/", nodeType: "ui", pos: { x: 400, y: 400 }, icon: "💬", columns: [{ name: "actor-mnlyr-name = Bomber" }, { name: "actor-minvv.name = Bomb" }] },
        
        // ======================================================================
        //  GAME RULES & CUSTOM UNITS
        // ======================================================================
        { id: 30, name: "rules.yaml", path:"bomber-john/", nodeType: "db", pos: { x: 800, y: 50 }, icon: "🔧", columns: [{ name: "MNLYR (Bomber)" }, { name: "  Inherits: ^TrackedVehicle" }, { name: "  Transforms: IntoActor: ftur" }, { name: "MINVV (Bomb)" }, { name: "  Inherits: ^SpriteActor" }, { name: "  ChangesHealth: Step: -100" }, { name: "  FireWarheadsOnDeath" }] },
        { id: 31, name: "weapons.yaml", path:"bomber-john/", nodeType: "db", pos: { x: 800, y: 450 }, icon: "💥", columns: [{ name: "CrateNuke" }, { name: "SubMissile" }, { name: "8Inch" }] },
        { id: 32, name: "sequences.yaml", path:"bomber-john/", nodeType: "db", pos: { x: 800, y: 700 }, icon: "🎬", columns: [{ name: "miner:" }, { name: "  Filename: miner.shp" }] },

        // ======================================================================
        //  RAW ASSETS / EXTERNAL DATA
        // ======================================================================
        { id: 40, name: "miner.shp", path:"bomber-john/bits/", nodeType: "service", pos: { x: 1200, y: 700 }, icon: "🖼️", columns: [{ name: "Bomber Sprite Data" }] },
        { id: 41, name: "map.bin", path:"bomber-john/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 42, name: "music.yaml", path:"bomber-john/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎵", columns: [{ name: "rain: Rain (ambient)" }] },
        { id: 43, name: "rain.aud", path:"bomber-john/", nodeType: "service", pos: { x: 1200, y: 450 }, icon: "🔊", columns: [{ name: "Rain Sound Data" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.yaml", column: "Title: Bomber John" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "MNLYR (Bomber)" }, type: "read" },

        // Custom Unit Definition Flow
        { from: { table: "rules.yaml", column: "MNLYR (Bomber)" }, to: { table: "Actor.cs (Object)", column: "TransformsInto" }, type: "write" },
        { from: { table: "rules.yaml", column: "MINVV (Bomb)" }, to: { table: "Actor.cs (Object)", column: "Health" }, type: "write" },
        { from: { table: "rules.yaml", column: "MINVV (Bomb)" }, to: { table: "weapons.yaml", column: "CrateNuke" }, type: "read" },

        // Asset Loading Flow
        { from: { table: "rules.yaml", column: "MNLYR (Bomber)" }, to: { table: "sequences.yaml", column: "miner:" }, type: "read" },
        { from: { table: "sequences.yaml", column: "miner:" }, to: { table: "miner.shp", column: "Bomber Sprite Data" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "music.yaml", column: "rain: Rain (ambient)" }, type: "read" },
        { from: { table: "music.yaml", column: "rain: Rain (ambient)" }, to: { table: "rain.aud", column: "Rain Sound Data" }, type: "read" },

        // UI Text
        { from: { table: "Game.cs (Engine)", column: "Run()" }, to: { table: "map.ftl", column: "actor-mnlyr-name = Bomber" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`; // Use unique ID
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null; // Fallback for connections to the node itself
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`; // fallback to node

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`; // fallback to node
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-11-part1.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Fort Lonestar Mission)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: Fort Lonestar</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of the "Fort Lonestar" mission in the OpenRA engine. It showcases a data-driven design where YAML files define game entities and rules, which are then orchestrated by Lua scripts and executed by the core C# engine.

    Key Architectural Flow (Example: Defining the Sniper unit):
    1.  The C# Game Engine (`Game.cs`) loads the mission, which points to `rules.yaml`.
    2.  `rules.yaml` is the central schema. It defines the `SNIPER` actor, inheriting from `^Soldier`, and specifies its traits like `Health`, `Cost`, `Buildable`, and `Armament`. It also tells the engine to load `fort-lonestar.lua`.
    3.  The `Armament` trait references a weapon called `Sniper`. The definition for this weapon (Range, Damage, etc.) is located in a separate `weapons.yaml` file, demonstrating separation of concerns.
    4.  To determine the Sniper's appearance, the engine consults `sequences.yaml`. This file maps the `sniper` actor name to its animation sequences (`stand`, `run`, `shoot`) and points to the binary asset `sniper.shp`.
    5.  The engine then loads the raw pixel data from `sniper.shp` and the UI icon from `snipericon.shp`.
    6.  The `fort-lonestar.lua` script contains the mission-specific logic, issuing commands to the `SNIPER` actor instance in the game world.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CODE)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "Move()" }, { name: "Attack()" }] },

        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 20, name: "fort-lonestar.lua", path:"maps/fort-lonestar/", nodeType: "code", pos: { x: 400, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "Tick()" }, { name: "SpawnSovietInfantry()" }] },
        { id: 21, name: "fort-lonestar-AI.lua", path:"maps/fort-lonestar/", nodeType: "code", pos: { x: 400, y: 350 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ProduceAircraft()" }] },
        
        // ======================================================================
        //  GAME RULES & DEFINITIONS (DATA/SCHEMA)
        // ======================================================================
        { id: 30, name: "rules.yaml", path:"maps/fort-lonestar/", nodeType: "db", pos: { x: 800, y: 50 }, icon: "🔧", columns: [{ name: "LuaScript: Scripts:" }, { name: "MusicPlaylist: rain" }, { name: "SNIPER:" }, { name: "  Inherits: ^Soldier" }, { name: "  Armament@PRIMARY: Sniper" }, { name: "  WithProductionIconOverlay" }] },
        { id: 31, name: "weapons.yaml", path:"maps/fort-lonestar/", nodeType: "db", pos: { x: 800, y: 450 }, icon: "💥", columns: [{ name: "Sniper:" }, { name: "  Inherits: ^SnipeWeapon" }, { name: "  ReloadDelay: 70" }, { name: "  Range: 10c0" }] },
        { id: 32, name: "sequences.yaml", path:"maps/fort-lonestar/", nodeType: "db", pos: { x: 800, y: 750 }, icon: "🎬", columns: [{ name: "sniper:" }, { name: "  Defaults: Filename: sniper.shp" }, { name: "  stand: ..." }, { name: "  run: ..." }, { name: "  shoot: ..." }] },
        
        // ======================================================================
        //  RAW ASSETS (SERVICE)
        // ======================================================================
        { id: 40, name: "sniper.shp", path:"maps/fort-lonestar/", nodeType: "service", pos: { x: 1200, y: 750 }, icon: "🖼️", columns: [{ name: "Sniper Sprite Data" }] },
        { id: 41, name: "snipericon.shp", path:"maps/fort-lonestar/", nodeType: "service", pos: { x: 1200, y: 550 }, icon: "🖼️", columns: [{ name: "Sniper UI Icon" }] },
        { id: 42, name: "thunder-ambient.aud", path:"maps/fort-lonestar/", nodeType: "service", pos: { x: 1200, y: 350 }, icon: "🔊", columns: [{ name: "Ambient Audio Data" }] }
    ],
    relationships: [
        // Game Startup and Script Loading
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "rules.yaml", column: "LuaScript: Scripts:" }, type: "read" },
        { from: { table: "rules.yaml", column: "LuaScript: Scripts:" }, to: { table: "fort-lonestar.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "LuaScript: Scripts:" }, to: { table: "fort-lonestar-AI.lua", column: "ActivateAI()" }, type: "flow" },
        
        // Script issuing commands to the Engine
        { from: { table: "fort-lonestar.lua", column: "SpawnSovietInfantry()" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" },
        { from: { table: "fort-lonestar-AI.lua", column: "ProduceAircraft()" }, to: { table: "Actor.cs (Object)", column: "Build()" }, type: "write" },

        // Sniper Definition Data Flow
        { from: { table: "World.cs (State)", column: "CreateActor()" }, to: { table: "rules.yaml", column: "SNIPER:" }, type: "read" },
        { from: { table: "rules.yaml", column: "  Armament@PRIMARY: Sniper" }, to: { table: "weapons.yaml", column: "Sniper:" }, type: "read" },
        { from: { table: "rules.yaml", column: "SNIPER:" }, to: { table: "sequences.yaml", column: "sniper:" }, type: "read" },
        { from: { table: "rules.yaml", column: "  WithProductionIconOverlay" }, to: { table: "snipericon.shp", column: "Sniper UI Icon" }, type: "read" },
        { from: { table: "sequences.yaml", column: "  Defaults: Filename: sniper.shp" }, to: { table: "sniper.shp", column: "Sniper Sprite Data" }, type: "read" },

        // Audio Asset Loading
        { from: { table: "rules.yaml", column: "MusicPlaylist: rain" }, to: { table: "thunder-ambient.aud", column: "Ambient Audio Data" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-11-part2.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Allied Mission 08b)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: allies-08b</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Allied Mission 08b) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, actor (unit/building), and waypoint with a unique name (e.g., `Chronosphere`, `USSRWarFactory`).
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine which Lua scripts to load and execute.
    5.  The engine loads `allies08b.lua` (for mission events) and `allies08b-AI.lua` (for opponent AI).
    6.  The `WorldLoaded()` function in the main Lua script is called. It sets up objectives and registers triggers with the C# engine (e.g., "if this building is destroyed, fail the mission").
    7.  The AI script begins its own loops, using timed delays (`Trigger.AfterDelay`) to produce units and launch attack waves defined by waypoints from `map.yaml`.
    8.  The mission progresses as the Lua scripts react to game events (like units entering an area) and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.AttackMove`).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "GetActorsByTypes()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "Move()" }, { name: "AttackMove()" }, { name: "Chronoshift()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION
        // ======================================================================
        { id: 20, name: "map.yaml", path:"allies-08b/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Protect the Chronosphere" }, { name: "Players: Greece, USSR, England" }, { name: "Chronosphere: pdox" }, { name: "USSRWarFactory: weap" }, { name: "AttackChrono: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"allies-08b/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: allies08b.lua" }, { name: "           allies08b-AI.lua" }, { name: "MissionData: BriefingVideo: ally8.vqa" }, { name: "MCV: Buildable: ~disabled" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER
        // ======================================================================
        { id: 30, name: "allies08b.lua", path:"allies-08b/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "Tick() (Timer)" }, { name: "CreateScientists()" }, { name: "DefendChronosphereCompleted()" }, { name: "Trigger.OnAnyKilled(...)" }, { name: "Trigger.OnEnteredFootprint(...)" }] },
        { id: 31, name: "allies08b-AI.lua", path:"allies-08b/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ProduceInfantry()" }, { name: "ProduceVehicles()" }, { name: "GroundWaves()" }, { name: "WTransWaves()" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL DATA
        // ======================================================================
        { id: 40, name: "map.bin", path:"allies-08b/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "ally8.vqa", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.yaml", column: "Title: Protect the Chronosphere" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script Loading
        { from: { table: "rules.yaml", column: "  Scripts: allies08b.lua" }, to: { table: "allies08b.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "  Scripts: allies08b-AI.lua" }, to: { table: "allies08b-AI.lua", column: "ActivateAI()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "MissionData: BriefingVideo: ally8.vqa" }, to: { table: "ally8.vqa", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "allies08b.lua", column: "WorldLoaded()" }, to: { table: "map.yaml", column: "Chronosphere: pdox" }, type: "read" },
        { from: { table: "allies08b.lua", column: "CreateScientists()" }, to: { table: "map.yaml", column: "ScientistsExit: waypoint" }, type: "read" },

        // AI Logic -> Reads from Map Data
        { from: { table: "allies08b-AI.lua", column: "ProduceVehicles()" }, to: { table: "map.yaml", column: "USSRWarFactory: weap" }, type: "read" },
        { from: { table: "allies08b-AI.lua", column: "GroundWaves()" }, to: { table: "map.yaml", column: "AttackChrono: waypoint" }, type: "read" },
        
        // Scripts -> Issue Commands to Engine
        { from: { table: "allies08b-AI.lua", column: "GroundWaves()" }, to: { table: "Actor.cs (Object)", column: "AttackMove()" }, type: "write" },
        { from: { table: "allies08b.lua", column: "DefendChronosphereCompleted()" }, to: { table: "Actor.cs (Object)", column: "Chronoshift()" }, type: "write" },
        { from: { table: "allies08b.lua", column: "Trigger.OnAnyKilled(...)" }, to: { table: "World.cs (State)", column: "Tick()" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-12-part1.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Allied Mission 10a)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: allies-10a</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Allied Mission 10a) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, actor (unit/building), and waypoint with a unique name (e.g., `CommandCenter`, `MissileSilo1`). This acts as the database for the mission.
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine which Lua scripts to load and execute.
    5.  The engine loads `allies10a.lua` (for mission events) and `allies10a-AI.lua` (for opponent AI).
    6.  The `WorldLoaded()` function in the main Lua script is called. It sets up objectives and registers triggers with the C# engine (e.g., "if a unit enters this area, call my `LaunchMissiles` function").
    7.  The AI script begins its own loops, using timed delays (`Trigger.AfterDelay`) to produce units and launch attack waves defined by waypoints from `map.yaml`.
    8.  The mission progresses as the Lua scripts react to game events and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.AttackMove`).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "Move()" }, { name: "AttackMove()" }, { name: "ActivateNukePower()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION
        // ======================================================================
        { id: 20, name: "map.yaml", path:"allies-10a/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Suspicion" }, { name: "Players: Greece, USSR, BadGuy" }, { name: "CommandCenter: fcom" }, { name: "MissileSilo1: mslo" }, { name: "FCom: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"allies-10a/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: allies10a.lua" }, { name: "           allies10a-AI.lua" }, { name: "BriefingVideo: ally10.vqa" }, { name: "MCV: Buildable: ~disabled" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER
        // ======================================================================
        { id: 30, name: "allies10a.lua", path:"allies-10a/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "MissionStart()" }, { name: "MissionTriggers()" }, { name: "Trigger.OnEnteredProximityTrigger(...)" }, { name: "LaunchMissiles()" }, { name: "ActivateAI()" }] },
        { id: 31, name: "allies10a-AI.lua", path:"allies-10a/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ProduceInfantry()" }, { name: "ProduceVehicles()" }, { name: "SendAttackGroup()" }, { name: "Paradrop()" }] },
        { id: 32, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 800 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL DATA
        // ======================================================================
        { id: 40, name: "map.bin", path:"allies-10a/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "ally10.vqa", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.yaml", column: "Title: Suspicion" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script Loading
        { from: { table: "rules.yaml", column: "  Scripts: allies10a.lua" }, to: { table: "allies10a.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "allies10a.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },
        { from: { table: "allies10a.lua", column: "ActivateAI()" }, to: { table: "allies10a-AI.lua", column: "ActivateAI()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: ally10.vqa" }, to: { table: "ally10.vqa", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data & Issues Commands
        { from: { table: "allies10a.lua", column: "Trigger.OnEnteredProximityTrigger(...)" }, to: { table: "map.yaml", column: "FCom: waypoint" }, type: "read" },
        { from: { table: "allies10a.lua", column: "Trigger.OnEnteredProximityTrigger(...)" }, to: { table: "allies10a.lua", column: "LaunchMissiles()" }, type: "flow" },
        { from: { table: "allies10a.lua", column: "LaunchMissiles()" }, to: { table: "map.yaml", column: "MissileSilo1: mslo" }, type: "read" },
        { from: { table: "allies10a.lua", column: "LaunchMissiles()" }, to: { table: "Actor.cs (Object)", column: "ActivateNukePower()" }, type: "write" },
        
        // AI Logic -> Reads from Map Data & Issues Commands
        { from: { table: "allies10a-AI.lua", column: "ProduceVehicles()" }, to: { table: "map.yaml", column: "USSRWarFactory: weap" }, type: "read" },
        { from: { table: "allies10a-AI.lua", column: "ProduceVehicles()" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" },
        { from: { table: "allies10a-AI.lua", column: "SendAttackGroup()" }, to: { table: "map.yaml", column: "AttackWaypoint1: waypoint" }, type: "read" },
        { from: { table: "allies10a-AI.lua", column: "SendAttackGroup()" }, to: { table: "Actor.cs (Object)", column: "AttackMove()" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-12-part2-A-443.054tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Allied Mission 06b)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: allies-06b</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Allied Mission 06b) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  **Engine Start:** The `Game.cs` executable loads the mod and is instructed to start the `allies-06b` map.
    2.  **Map Data Loading:** The engine reads `map.yaml`. This file is the "database instance" of the world, defining every player and every single actor with a unique, script-accessible name (e.g., `TechLab1`, `WarFactory`, `AlliedEntry1`, `SovietBaseAttack`).
    3.  **Configuration Loading:** `map.yaml` points to `rules.yaml`. The engine loads this to get mission-specific configurations.
    4.  **Script Orchestration:** Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine to load and execute `allies06b.lua` (for mission events) and `allies06b-AI.lua` (for opponent AI).
    5.  **Mission Logic Initialization:** The `WorldLoaded()` function in `allies06b.lua` is called by the engine. It sets up objectives and registers `Trigger`s (event listeners) for events like a building being captured (`OnCapture(RadarDome, ...)`).
    6.  **AI Activation:** The main mission script calls `ActivateAI()`, which starts the logic in the separate `allies06b-AI.lua` file. This AI script then runs its own production loops (`ProduceInfantry`, `ProduceVehicles`) and schedules attack waves (`WTransWaves`) using timed delays.
    7.  **Event Loop:** The mission progresses as the Lua scripts react to game events (like units being killed or a spy infiltrating a building) and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.Patrol`). This showcases a clear separation of concerns between core engine code, data/configuration (YAML), and logic (Lua).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "LoadMap('allies-06b')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "AttackMove()" }, { name: "Infiltrate()" }, { name: "Capture()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"allies-06b/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Cripple Iron Curtain" }, { name: "TechLab1: stek" }, { name: "WarFactory: weap" }, { name: "SovietBaseAttack: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"allies-06b/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: allies06b.lua" }, { name: "           allies06b-AI.lua" }, { name: "BriefingVideo: ally6.vqa" }, { name: "MCV: Buildable: ~disabled" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 30, name: "allies06b.lua", path:"allies-06b/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "InfiltrateTechCenter()" }, { name: "Trigger.OnInfiltrated(a, ...)" }, { name: "Trigger.OnCapture(RadarDome, ...)" }, { name: "ActivateAI()" }] },
        { id: 31, name: "allies06b-AI.lua", path:"allies-06b/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ProduceInfantry()" }, { name: "ProduceVehicles()" }, { name: "WTransWaves()" }, { name: "SendAttack(units, path)" }] },
        { id: 32, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 800 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }, { name: "ReinforceWithTransport(...)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"allies-06b/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "ally6.vqa", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('allies-06b')" }, to: { table: "map.yaml", column: "Title: Cripple Iron Curtain" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('allies-06b')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script Loading
        { from: { table: "rules.yaml", column: "  Scripts: allies06b.lua" }, to: { table: "allies06b.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "allies06b.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },
        { from: { table: "allies06b.lua", column: "ActivateAI()" }, to: { table: "allies06b-AI.lua", column: "ActivateAI()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: ally6.vqa" }, to: { table: "ally6.vqa", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "allies06b.lua", column: "InfiltrateTechCenter()" }, to: { table: "map.yaml", column: "TechLab1: stek" }, type: "read" },
        
        // AI Logic -> Reads from Map Data and Issues Commands
        { from: { table: "allies06b-AI.lua", column: "ProduceVehicles()" }, to: { table: "map.yaml", column: "WarFactory: weap" }, type: "read" },
        { from: { table: "allies06b-AI.lua", column: "SendAttack(units, path)" }, to: { table: "map.yaml", column: "SovietBaseAttack: waypoint" }, type: "read" },
        { from: { table: "allies06b-AI.lua", column: "SendAttack(units, path)" }, to: { table: "Actor.cs (Object)", column: "AttackMove()" }, type: "write" },

        // Script -> Engine Interaction (Event Loop)
        { from: { table: "allies06b.lua", column: "Trigger.OnInfiltrated(a, ...)" }, to: { table: "Actor.cs (Object)", column: "Infiltrate()" }, type: "read" },
        { from: { table: "Actor.cs (Object)", column: "Infiltrate()" }, to: { table: "allies06b.lua", column: "Trigger.OnInfiltrated(a, ...)" }, type: "flow" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-12-part2-B-594.297tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Soviet Mission 04a)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: soviet-04a</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Soviet Mission 04a) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  **Engine Start:** The `Game.cs` executable loads the mod and is instructed to start the `soviet-04a` map.
    2.  **Map Data Loading:** The engine reads `map.yaml`. This file is the "database instance" of the world, defining every player and every single actor with a unique, script-accessible name (e.g., `RadarDome`, `CYard`, `VillagePoint`, `SovietBasePoint`).
    3.  **Configuration Loading:** `map.yaml` points to `rules.yaml`. The engine loads this to get mission-specific configurations.
    4.  **Script Orchestration:** Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine to load and execute a list of Lua scripts, including `soviet04a.lua` (for mission events) and `soviet04a-AI.lua` (for opponent AI).
    5.  **Mission Logic Initialization:** The `WorldLoaded()` function in `soviet04a.lua` is called by the engine. It sets up objectives and registers `Trigger`s (event listeners) for events like the `RadarDome` being destroyed.
    6.  **AI Activation:** The main mission script calls functions like `BuildBase()` and `ProduceInfantry()`, which start the logic loops in the separate `soviet04a-AI.lua` file. This AI script then runs its own production and base-building logic.
    7.  **Event Loop:** The mission progresses as the Lua scripts react to game events (like units being killed or a timer expiring) and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.Hunt`). This showcases a clear separation of concerns between core engine code, data/configuration (YAML), and logic (Lua).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "LoadMap('soviet-04a')" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "CreateActor()" }, { name: "Tick()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "AttackMove()" }, { name: "Build()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION (DATA/SCHEMA)
        // ======================================================================
        { id: 20, name: "map.yaml", path:"soviet-04a/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Behind the Lines" }, { name: "RadarDome: dome" }, { name: "CYard: fact" }, { name: "village1: v11" }, { name: "StartPoint: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"soviet-04a/", nodeType: "db", pos: { x: 400, y: 500 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: soviet04a.lua" }, { name: "           soviet04a-AI.lua" }, { name: "BriefingVideo: soviet4.vqa" }, { name: "MCV: Buildable: ~disabled" }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER (CODE)
        // ======================================================================
        { id: 30, name: "soviet04a.lua", path:"soviet-04a/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "RunInitialActivities()" }, { name: "Trigger.OnKilled(RadarDome, ...)" }, { name: "Trigger.OnAllKilled(Village, ...)" }] },
        { id: 31, name: "soviet04a-AI.lua", path:"soviet-04a/", nodeType: "code", pos: { x: 800, y: 400 }, icon: "🧠", columns: [{ name: "BuildBase()" }, { name: "ProduceInfantry()" }, { name: "ProduceArmor()" }, { name: "SendUnits(units, waypoints)" }] },
        { id: 32, name: "soviet04a-reinforcements.lua", path:"soviet-04a/", nodeType: "code", pos: { x: 800, y: 700 }, icon: "✈️", columns: [{ name: "ReinfInf()" }, { name: "ReinfArmor()" }, { name: "BringPatrol1()" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL SERVICES
        // ======================================================================
        { id: 40, name: "map.bin", path:"soviet-04a/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "soviet4.vqa", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap('soviet-04a')" }, to: { table: "map.yaml", column: "Title: Behind the Lines" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap('soviet-04a')" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script Loading
        { from: { table: "rules.yaml", column: "  Scripts: soviet04a.lua" }, to: { table: "soviet04a.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "soviet04a.lua", column: "RunInitialActivities()" }, to: { table: "soviet04a-AI.lua", column: "BuildBase()" }, type: "flow" },
        { from: { table: "soviet04a.lua", column: "RunInitialActivities()" }, to: { table: "soviet04a-reinforcements.lua", column: "BringPatrol1()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: soviet4.vqa" }, to: { table: "soviet4.vqa", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "soviet04a.lua", column: "Trigger.OnKilled(RadarDome, ...)" }, to: { table: "map.yaml", column: "RadarDome: dome" }, type: "read" },
        
        // AI Logic -> Reads from Map Data & Issues Commands
        { from: { table: "soviet04a-AI.lua", column: "BuildBuilding(building)" }, to: { table: "map.yaml", column: "GreeceCYard: waypoint" }, type: "read" },
        { from: { table: "soviet04a-AI.lua", column: "SendUnits(units, waypoints)" }, to: { table: "Actor.cs (Object)", column: "AttackMove()" }, type: "write" },

        // Reinforcement Logic -> Checks Game State
        { from: { table: "soviet04a-reinforcements.lua", column: "ReinfArmor()" }, to: { table: "soviet04a.lua", column: "Trigger.OnKilled(RadarDome, ...)" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-13.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Architecture (Allied Mission 05c)</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Mission: allies-05c</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the architecture of a single mission (Allied Mission 05c) in the OpenRA engine. It demonstrates a data-driven design where YAML files define the world state and configuration, while Lua scripts handle the dynamic mission logic and AI behavior.

    Key Architectural Flow:
    1.  The C# Game Engine (`Game.cs`) starts and loads a specific map.
    2.  It reads the map's instance data from `map.yaml`, which defines every player, actor (unit/building), and waypoint with a unique name (e.g., `Prison`, `Warfactory`). This acts as the database for the mission.
    3.  The `map.yaml` points to a `rules.yaml` file. The engine loads this to get mission-specific configurations and rules.
    4.  Crucially, `rules.yaml` contains a `LuaScript:` key, which tells the engine which Lua scripts to load and execute.
    5.  The engine loads `allies05c.lua` (for mission events) and `allies05c-AI.lua` (for opponent AI).
    6.  The `WorldLoaded()` function in the main Lua script is called. It sets up objectives and registers triggers with the C# engine (e.g., "if the `Warfactory` actor is infiltrated, call this function").
    7.  The AI script begins its own loops, using timed delays (`Trigger.AfterDelay`) to produce units and launch attack waves.
    8.  The mission progresses as the Lua scripts react to game events (like units being killed or entering an area) and issue commands back to the C# engine (e.g., `Actor.Create`, `unit.AttackMove`).
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  CORE ENGINE (CONCEPTUAL)
        // ======================================================================
        { id: 10, name: "Game.cs (Engine)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 50 }, icon: "🎮", columns: [{ name: "Run()" }, { name: "LoadMap()" }] },
        { id: 11, name: "World.cs (State)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 350 }, icon: "🌍", columns: [{ name: "Tick()" }, { name: "CreateActor()" }] },
        { id: 12, name: "Actor.cs (Object)", path:"OpenRA.Game/", nodeType: "code", pos: { x: 50, y: 650 }, icon: "🤖", columns: [{ name: "Owner" }, { name: "Location" }, { name: "Move()" }, { name: "Demolish()" }, { name: "Infiltrate()" }] },

        // ======================================================================
        //  MAP INSTANCE & CONFIGURATION
        // ======================================================================
        { id: 20, name: "map.yaml", path:"allies-05c/", nodeType: "db", pos: { x: 400, y: 50 }, icon: "🗺️", columns: [{ name: "Title: Tanya's Tale" }, { name: "Players: Greece, USSR" }, { name: "Prison: miss" }, { name: "Warfactory: weap.infiltratable" }, { name: "Truk: truk.mission" }, { name: "TrukWaypoint1: waypoint" }, { name: "Rules: rules.yaml" }] },
        { id: 21, name: "rules.yaml", path:"allies-05c/", nodeType: "db", pos: { x: 400, y: 450 }, icon: "🔧", columns: [{ name: "LuaScript:" }, { name: "  Scripts: allies05c.lua" }, { name: "           allies05c-AI.lua" }, { name: "BriefingVideo: ally5.vqa" }, { name: "WEAP.infiltratable: ..." }] },
        
        // ======================================================================
        //  MISSION SCRIPTING LAYER
        // ======================================================================
        { id: 30, name: "allies05c.lua", path:"allies-05c/", nodeType: "code", pos: { x: 800, y: 50 }, icon: "📜", columns: [{ name: "WorldLoaded()" }, { name: "InitTriggers()" }, { name: "Trigger.OnInfiltrated(Warfactory,..)" }, { name: "Trigger.OnInfiltrated(Prison,..)" }, { name: "WarfactoryInfiltrated()" }, { name: "FreeTanya()" }, { name: "SendReinforcements()" }] },
        { id: 31, name: "allies05c-AI.lua", path:"allies-05c/", nodeType: "code", pos: { x: 800, y: 450 }, icon: "🧠", columns: [{ name: "ActivateAI()" }, { name: "ProduceUSSRInfantry()" }, { name: "ProduceUSSRVehicles()" }, { name: "SendAttackGroup()" }] },
        { id: 32, name: "campaign.lua", path:"(shared)", nodeType: "code", pos: { x: 800, y: 750 }, icon: "📚", columns: [{ name: "InitObjectives(player)" }, { name: "ReinforceWithTransport(..)" }] },
        
        // ======================================================================
        //  RAW ASSETS / EXTERNAL DATA
        // ======================================================================
        { id: 40, name: "map.bin", path:"allies-05c/", nodeType: "service", pos: { x: 1200, y: 50 }, icon: "▦", columns: [{ name: "Binary Terrain Data" }] },
        { id: 41, name: "ally5.vqa", path:"(videos)/", nodeType: "service", pos: { x: 1200, y: 250 }, icon: "🎬", columns: [{ name: "Briefing Video Asset" }] }
    ],
    relationships: [
        // Game Startup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.yaml", column: "Title: Tanya's Tale" }, type: "read" },
        { from: { table: "Game.cs (Engine)", column: "LoadMap()" }, to: { table: "map.bin", column: "Binary Terrain Data" }, type: "read" },
        { from: { table: "map.yaml", column: "Rules: rules.yaml" }, to: { table: "rules.yaml", column: "LuaScript:" }, type: "read" },
        
        // Script Loading
        { from: { table: "rules.yaml", column: "  Scripts: allies05c.lua" }, to: { table: "allies05c.lua", column: "WorldLoaded()" }, type: "flow" },
        { from: { table: "allies05c.lua", column: "WorldLoaded()" }, to: { table: "campaign.lua", column: "InitObjectives(player)" }, type: "flow" },
        { from: { table: "allies05c.lua", column: "SendReinforcements()" }, to: { table: "allies05c-AI.lua", column: "ActivateAI()" }, type: "flow" },
        { from: { table: "rules.yaml", column: "BriefingVideo: ally5.vqa" }, to: { table: "ally5.vqa", column: "Briefing Video Asset" }, type: "read" },

        // Mission Logic -> Reads from Map Data
        { from: { table: "allies05c.lua", column: "Trigger.OnInfiltrated(Warfactory,..)" }, to: { table: "map.yaml", column: "Warfactory: weap.infiltratable" }, type: "read" },
        { from: { table: "allies05c.lua", column: "WarfactoryInfiltrated()" }, to: { table: "map.yaml", column: "Truk: truk.mission" }, type: "read" },
        
        // AI Logic -> Engine Interaction
        { from: { table: "allies05c-AI.lua", column: "ProduceUSSRInfantry()" }, to: { table: "World.cs (State)", column: "CreateActor()" }, type: "write" },
        { from: { table: "allies05c-AI.lua", column: "SendAttackGroup()" }, to: { table: "Actor.cs (Object)", column: "Move()" }, type: "write" },

        // Script -> Engine Interaction (Event Loop)
        { from: { table: "allies05c.lua", column: "Trigger.OnInfiltrated(Warfactory,..)" }, to: { table: "Actor.cs (Object)", column: "Infiltrate()" }, type: "read" },
        { from: { table: "Actor.cs (Object)", column: "Infiltrate()" }, to: { table: "allies05c.lua", column: "WarfactoryInfiltrated()" }, type: "flow" },
        { from: { table: "allies05c.lua", column: "WarfactoryInfiltrated()" }, to: { table: "Actor.cs (Object)", column: "Move()" }, type: "write" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-16-252.044tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Full Architecture Overview</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Command Flow</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the comprehensive architecture of the OpenRA engine, focusing on the core gameplay loop of issuing a unit command. The engine follows a highly data-driven, entity-component (Actor-Trait) design.

    Key Architectural Flow (Player issues a Move/Attack order):
    1.  **UI Layer (`WorldInteractionControllerWidget.cs`):** Captures the player's mouse click in the game world.
    2.  **Order Generation (`UnitOrderGenerator.cs`):** The widget passes the input to an `OrderGenerator`. This class interprets the context (what was clicked, what is selected) and creates a formal `Order` object (e.g., an "Attack" order with a target).
    3.  **Order Resolution (`Actor.cs`):** The `Order` is issued to the selected `Actor`(s). Each actor has a collection of `Trait`s. Traits that implement `IResolveOrder` (e.g., `Mobile.cs`, `AttackFollow.cs`) will check if they can handle the order.
    4.  **Activity Queuing:** A trait that handles an order (like `Mobile`) will then queue a high-level `Activity` on the actor (e.g., a `Move` activity). This allows actors to perform a sequence of actions.
    5.  **Trait Execution:** The `Activity` (e.g., `Move.cs`) uses other traits on the actor to perform its function.
        - A `Move` activity uses the `Mobile` trait, which in turn calls the global `HierarchicalPathFinder.cs` to calculate a path.
        - An `Attack` activity uses an `AttackBase` trait (like `AttackFollow.cs`). This trait finds a valid `Armament` to fire.
    6.  **Data-Driven Behavior:** The `Armament`'s behavior is not hardcoded. It looks up its properties from a `WeaponInfo` object, which was loaded at startup from a `weapons.yaml` file. The `WeaponInfo` itself references a `Warhead` (e.g., `SpreadDamageWarhead.cs`) which defines the actual damage and effects.
    7.  **Rendering:** As the actor's state (position, health, current animation) changes, its `Render*` traits (`WithSpriteBody.cs`, `RenderSprites.cs`) are used by the `WorldRenderer` to draw the actor on screen.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  UI & INPUT LAYER
        // ======================================================================
        { id: 1, name: "WorldInteractionControllerWidget.cs", path:"Widgets/", nodeType: "ui", pos: { x: 50, y: 300 }, icon: "🖱️", columns: [{ name: "HandleMouseInput()" }] },
        { id: 2, name: "UnitOrderGenerator.cs", path:"Orders/", nodeType: "ui", pos: { x: 50, y: 600 }, icon: "📜", columns: [{ name: "Order()" }, { name: "GetCursor()" }] },

        // ======================================================================
        //  ORDER & ACTIVITY LAYER
        // ======================================================================
        { id: 10, name: "Order.cs", path:"(Core Engine)", nodeType: "code", pos: { x: 400, y: 50 }, icon: "📦", columns: [{ name: "OrderString" }, { name: "Subject (Actor)" }, { name: "Target" }] },
        { id: 11, name: "Actor.cs (Core)", path:"(Core Engine)", nodeType: "code", pos: { x: 400, y: 450 }, icon: "🤖", columns: [{ name: "QueueActivity()" }, { name: "Tick() -> Tick activities" }, { name: "TraitsImplementing<T>()" }] },
        { id: 12, name: "Move.cs (Activity)", path:"Activities/Move/", nodeType: "code", pos: { x: 400, y: 800 }, icon: "🏃", columns: [{ name: "Tick()" }, { name: "Uses -> IMove" }] },
        { id: 13, name: "Attack.cs (Activity)", path:"Activities/", nodeType: "code", pos: { x: 400, y: 1050 }, icon: "💥", columns: [{ name: "Tick()" }, { name: "Uses -> AttackBase" }] },

        // ======================================================================
        //  ACTOR TRAIT (BEHAVIOR) LAYER
        // ======================================================================
        { id: 20, name: "Mobile.cs (IResolveOrder)", path:"Traits/", nodeType: "code", pos: { x: 750, y: 800 }, icon: "➡️", columns: [{ name: "ResolveOrder('Move')" }, { name: "PathFinder.FindPath()" }] },
        { id: 21, name: "AttackFollow.cs (IResolveOrder)", path:"Traits/Attack/", nodeType: "code", pos: { x: 750, y: 1050 }, icon: "🎯", columns: [{ name: "ResolveOrder('Attack')" }, { name: "DoAttack()" }] },
        { id: 22, name: "Armament.cs", path:"Traits/", nodeType: "code", pos: { x: 1100, y: 1050 }, icon: "🔫", columns: [{ name: "CheckFire()" }, { name: "WeaponInfo" }] },
        { id: 23, name: "Health.cs", path:"Traits/", nodeType: "code", pos: { x: 1100, y: 1300 }, icon: "❤️", columns: [{ name: "InflictDamage()" }] },
        
        // ======================================================================
        //  CORE SYSTEMS
        // ======================================================================
        { id: 30, name: "HierarchicalPathFinder.cs", path:"Pathfinder/", nodeType: "code", pos: { x: 1100, y: 800 }, icon: "🗺️", columns: [{ name: "FindPath()" }] },
        { id: 31, name: "Ruleset.cs", path:"(Core Engine)", nodeType: "db", pos: { x: 1450, y: 50 }, icon: "📚", columns: [{ name: "LoadFromMod()" }, { name: "Actors", type:"Dictionary" }, { name: "Weapons", type:"Dictionary" }] },

        // ======================================================================
        //  DATA / SCHEMA LAYER
        // ======================================================================
        { id: 100, name: "vehicles.yaml (Rules)", path:"mods/ra/rules/", nodeType: "db", pos: { x: 1800, y: 50 }, icon: "📄", columns: [{ name: "1TNK: (Light Tank)" }, { name: "  Inherits: ^Vehicle" }, { name: "  Armament: Weapon: 25mm" }] },
        { id: 101, name: "weapons.yaml", path:"mods/ra/weapons/", nodeType: "db", pos: { x: 1450, y: 450 }, icon: "📄", columns: [{ name: "25mm:" }, { name: "  Projectile: ap" }, { name: "  Warhead: SpreadDamage" }] },
        { id: 102, name: "SpreadDamageWarhead.cs", path:"Warheads/", nodeType: "db", pos: { x: 1450, y: 750 }, icon: "💣", columns: [{ name: "DoImpact()" }, { name: "InflictDamage()" }] },
        
        // ======================================================================
        //  RENDERING LAYER
        // ======================================================================
        { id: 200, name: "RenderSprites.cs", path:"Traits/Render/", nodeType: "ui", pos: { x: 750, y: 450 }, icon: "🖼️", columns: [{ name: "Render()" }] },
        { id: 201, name: "WithSpriteBody.cs", path:"Traits/Render/", nodeType: "ui", pos: { x: 1100, y: 450 }, icon: "🧍", columns: [{ name: "DefaultAnimation" }] },
        { id: 202, name: "sequences/vehicles.yaml", path:"mods/ra/sequences/", nodeType: "db", pos: { x: 1450, y: 450 }, icon: "🎬", columns: [{ name: "1tnk: Filename: 1tnk.shp" }] },
        { id: 203, name: "1tnk.shp", path:"(asset)", nodeType: "service", pos: { x: 1800, y: 450 }, icon: "🎨", columns: [{ name: "Raw Sprite Data" }] }
    ],
    relationships: [
        // Input Flow
        { from: { table: "WorldInteractionControllerWidget.cs", column: "HandleMouseInput()" }, to: { table: "UnitOrderGenerator.cs", column: "Order()" }, type: "flow" },
        { from: { table: "UnitOrderGenerator.cs", column: "Order()" }, to: { table: "Order.cs", column: "OrderString" }, type: "write" },
        { from: { table: "Order.cs", column: "Subject (Actor)" }, to: { table: "Actor.cs (Core)", column: "TraitsImplementing<T>()" }, type: "flow" },
        
        // Order Resolution to Activity
        { from: { table: "Actor.cs (Core)", column: "TraitsImplementing<T>()" }, to: { table: "Mobile.cs (IResolveOrder)", column: "ResolveOrder('Move')" }, type: "read" },
        { from: { table: "Actor.cs (Core)", column: "TraitsImplementing<T>()" }, to: { table: "AttackFollow.cs (IResolveOrder)", column: "ResolveOrder('Attack')" }, type: "read" },
        { from: { table: "Mobile.cs (IResolveOrder)", column: "ResolveOrder('Move')" }, to: { table: "Actor.cs (Core)", column: "QueueActivity()" }, type: "flow" },
        { from: { table: "Actor.cs (Core)", column: "QueueActivity()" }, to: { table: "Move.cs (Activity)", column: "Tick()" }, type: "flow" },
        { from: { table: "AttackFollow.cs (IResolveOrder)", column: "ResolveOrder('Attack')" }, to: { table: "Actor.cs (Core)", column: "QueueActivity()" }, type: "flow" },
        { from: { table: "Actor.cs (Core)", column: "QueueActivity()" }, to: { table: "Attack.cs (Activity)", column: "Tick()" }, type: "flow" },

        // Activity Execution
        { from: { table: "Move.cs (Activity)", column: "Uses -> IMove" }, to: { table: "Mobile.cs (IResolveOrder)", column: "PathFinder.FindPath()" }, type: "read" },
        { from: { table: "Mobile.cs (IResolveOrder)", column: "PathFinder.FindPath()" }, to: { table: "HierarchicalPathFinder.cs", column: "FindPath()" }, type: "read" },
        { from: { table: "Attack.cs (Activity)", column: "Uses -> AttackBase" }, to: { table: "AttackFollow.cs (IResolveOrder)", column: "DoAttack()" }, type: "read" },
        { from: { table: "AttackFollow.cs (IResolveOrder)", column: "DoAttack()" }, to: { table: "Armament.cs", column: "CheckFire()" }, type: "read" },
        
        // Data Lookup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMod('ra')" }, to: { table: "Ruleset.cs", column: "LoadFromMod()" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod()" }, to: { table: "vehicles.yaml (Rules)", column: "1TNK: (Light Tank)" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod()" }, to: { table: "weapons.yaml", column: "25mm:" }, type: "read" },
        { from: { table: "Armament.cs", column: "WeaponInfo" }, to: { table: "Ruleset.cs", column: "Weapons" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "Weapons" }, to: { table: "weapons.yaml", column: "25mm:" }, type: "read" },
        { from: { table: "weapons.yaml", column: "  Warhead: SpreadDamage" }, to: { table: "SpreadDamageWarhead.cs", column: "DoImpact()" }, type: "read" },
        { from: { table: "SpreadDamageWarhead.cs", column: "InflictDamage()" }, to: { table: "Health.cs", column: "InflictDamage()" }, type: "write" },

        // Rendering Flow
        { from: { table: "Actor.cs (Core)", column: "Tick() -> Tick activities" }, to: { table: "RenderSprites.cs", column: "Render()" }, type: "flow" },
        { from: { table: "RenderSprites.cs", column: "Render()" }, to: { table: "WithSpriteBody.cs", column: "DefaultAnimation" }, type: "read" },
        { from: { table: "WithSpriteBody.cs", column: "DefaultAnimation" }, to: { table: "sequences/vehicles.yaml", column: "1tnk: Filename: 1tnk.shp" }, type: "read" },
        { from: { table: "sequences/vehicles.yaml", column: "1tnk: Filename: 1tnk.shp" }, to: { table: "1tnk.shp", column: "Raw Sprite Data" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                return `<li id="${canvasId}-${table.id}-${colName}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;

            let fromElId = `${canvasId}-${fromId}-${fromColName}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>
C:\Users\EmulatorPC\Desktop\biancas\db-designer-01-OpenRA-18-277.678tokens.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Raptor 3: OpenRA Full Architecture Overview</title>
<style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #111827; color: #d1d5db; margin: 0; display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
    .header { background-color: #1f2937; padding: 1rem 2rem; border-bottom: 1px solid #374151; flex-shrink: 0; display: flex; justify-content: space-between; align-items: center; }
    h1 { color: white; margin: 0; }
    .controls { display: flex; align-items: center; gap: 20px; }
    .legend { display: flex; gap: 20px; font-size: 12px; align-items: center; }
    .legend-item { display: flex; align-items: center; gap: 5px; }
    .main-container { display: flex; flex-grow: 1; }
    .panel { min-width: 150px; height: 100%; display: flex; flex-direction: column; position: relative; }
    .panel:nth-child(1) { flex: 0 1 15%; }
    .panel:nth-child(3) { flex: 1 1 70%; }
    .panel:nth-child(5) { flex: 0 1 15%; }

    .splitter { width: 6px; background-color: #374151; cursor: col-resize; flex-shrink: 0; z-index: 100; }
    .splitter:hover { background-color: #4b5563; }
    .panel-header { padding: 1rem; text-align: center; border-bottom: 1px solid #374151; background-color: #1f2937; }
    .panel-header h2 { margin: 0; font-size: 1.5rem; }
    .header-r2 { color: #fca5a5; }
    .header-r3 { color: #86efac; }
    .header-r4 { color: #c4b5fd; }
    .canvas { position: relative; width: 100%; flex-grow: 1; overflow: hidden; cursor: grab; }
    .canvas:active { cursor: grabbing; }
    .connections { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 1; }
    .transform-container { position: absolute; top: 0; left: 0; transform-origin: 0 0; z-index: 2; }
    
    .node { position: absolute; border: 3px solid #4b5563; border-radius: 8px; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); font-family: monospace; width: 280px; cursor: move; z-index: 10; user-select: none; padding-top: 30px; }
    .node h3 { padding: 8px 12px 8px 12px; margin: 0; border-bottom: 1px solid rgba(0,0,0,0.2); border-radius: 8px 8px 0 0; font-size: 14px; color: white; word-break: break-all; }
    .node h3 .path { font-size: 0.8em; color: #9ca3af; display: block; font-weight: normal; }
    .node ul { list-style: none; padding: 0; margin: 0; }
    .node li { display: flex; align-items: center; padding: 8px 12px; border-bottom: 1px solid rgba(255,255,255,0.05); font-size: 12px; position: relative; }
    .node li:last-child { border-bottom: none; }
    .node .col-name { font-weight: 500; }
    .node .col-type { margin-left: auto; color: #9ca3af; }

    .db-node { background-color: rgba(196, 181, 253, 0.15); border-color: #a78bfa; }
    .db-node h3 { background-color: #8b5cf6; }
    
    .ui-node { background-color: rgba(191, 219, 254, 0.15); border-color: #60a5fa; }
    .ui-node h3 { background-color: #3b82f6; }

    .code-node { background-color: rgba(134, 239, 172, 0.1); border-color: #4ade80; }
    .code-node h3 { background-color: #22c55e; }

    .service-node { background-color: rgba(252, 165, 165, 0.1); border-color: #f87171; }
    .service-node h3 { background-color: #ef4444; }
    
    .icon-display { position: absolute; top: -32px; left: 50%; transform: translateX(-50%); font-size: 64px; z-index: 12; pointer-events: none; opacity: 0.8; }
</style>
</head>
<body>

<div class="header">
    <h1>Full Stack Designer - OpenRA Architecture</h1>
    <div class="controls">
        <div class="legend">
             <div class="legend-item"><svg width="10" height="10" style="background:#3b82f6; border-radius:3px;"></svg> UI</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#22c55e; border-radius:3px;"></svg> Code</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#8b5cf6; border-radius:3px;"></svg> Data/Schema</div>
            <div class="legend-item"><svg width="10" height="10" style="background:#ef4444; border-radius:3px;"></svg> Service</div>
        </div>
    </div>
</div>

<div class="main-container">
    <div class="panel">
        <div class="panel-header"><h2 class="header-r2">Raptor 2: Legacy</h2></div>
        <div id="canvas-r2" class="canvas"><svg id="connections-r2" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r3">OpenRA Command Flow</h2></div>
        <div id="canvas-r3" class="canvas"><svg id="connections-r3" class="connections"></svg><div class="transform-container"></div></div>
    </div>
    <div class="splitter"></div>
    <div class="panel">
        <div class="panel-header"><h2 class="header-r4">Raptor 4: Future</h2></div>
        <div id="canvas-r4" class="canvas"><svg id="connections-r4" class="connections"></svg><div class="transform-container"></div></div>
    </div>
</div>

<script>
// ===================================================================================
//
//  DATA SOURCES
//
// ===================================================================================
/*
    ARCHITECTURAL INSIGHTS FOR FUTURE AI ANALYSIS:
    This file visualizes the comprehensive architecture of the OpenRA engine, focusing on the core gameplay loop of issuing a unit command. The engine follows a highly data-driven, entity-component (Actor-Trait) design.

    Key Architectural Flow (Player issues a Move/Attack order):
    1.  **UI Layer (`WorldInteractionControllerWidget.cs`):** Captures the player's mouse click in the game world.
    2.  **Order Generation (`UnitOrderGenerator.cs`):** The widget passes the input to an `OrderGenerator`. This class interprets the context (what was clicked, what is selected) and creates a formal `Order` object (e.g., an "Attack" order with a target).
    3.  **Order Resolution (`Actor.cs`):** The `Order` is issued to the selected `Actor`(s). Each actor has a collection of `Trait`s. Traits that implement `IResolveOrder` (e.g., `Mobile.cs`, `AttackFollow.cs`) will check if they can handle the order.
    4.  **Activity Queuing:** A trait that handles an order (like `Mobile`) will then queue a high-level `Activity` on the actor (e.g., a `Move` activity). This allows actors to perform a sequence of actions.
    5.  **Trait Execution:** The `Activity` (e.g., `Move.cs`) uses other traits on the actor to perform its function.
        - A `Move` activity uses the `Mobile` trait, which in turn calls the global `HierarchicalPathFinder.cs` to calculate a path.
        - An `Attack` activity uses an `AttackBase` trait (like `AttackFollow.cs`). This trait finds a valid `Armament` to fire.
    6.  **Data-Driven Behavior:** The `Armament`'s behavior is not hardcoded. It looks up its properties from a `WeaponInfo` object, which was loaded at startup from a `weapons.yaml` file. The `WeaponInfo` itself references a `Warhead` (e.g., `SpreadDamageWarhead.cs`) which defines the actual damage and effects.
    7.  **Rendering:** As the actor's state (position, health, current animation) changes, its `Render*` traits (`WithSpriteBody.cs`, `RenderSprites.cs`) are used by the `WorldRenderer` to draw the actor on screen.
*/
const schemaR2 = { tables: [], relationships: [] };
const schemaR4 = { tables: [], relationships: [] };

const schemaR3 = {
    tables: [
        // ======================================================================
        //  UI & INPUT LAYER
        // ======================================================================
        { id: 1, name: "WorldInteractionControllerWidget.cs", path:"Widgets/", nodeType: "ui", pos: { x: 50, y: 300 }, icon: "🖱️", columns: [{ name: "HandleMouseInput()" }] },
        { id: 2, name: "UnitOrderGenerator.cs", path:"Orders/", nodeType: "ui", pos: { x: 50, y: 600 }, icon: "📜", columns: [{ name: "Order()" }, { name: "GetCursor()" }] },

        // ======================================================================
        //  ORDER & ACTIVITY LAYER
        // ======================================================================
        { id: 10, name: "Order.cs", path:"(Core Engine)", nodeType: "code", pos: { x: 400, y: 50 }, icon: "📦", columns: [{ name: "OrderString" }, { name: "Subject (Actor)" }, { name: "Target" }] },
        { id: 11, name: "Actor.cs (Core)", path:"(Core Engine)", nodeType: "code", pos: { x: 400, y: 450 }, icon: "🤖", columns: [{ name: "QueueActivity()" }, { name: "Tick() -> Tick activities" }, { name: "TraitsImplementing<T>()" }] },
        { id: 12, name: "Move.cs (Activity)", path:"Activities/Move/", nodeType: "code", pos: { x: 400, y: 800 }, icon: "🏃", columns: [{ name: "Tick()" }, { name: "Uses -> IMove" }] },
        { id: 13, name: "Attack.cs (Activity)", path:"Activities/", nodeType: "code", pos: { x: 400, y: 1050 }, icon: "💥", columns: [{ name: "Tick()" }, { name: "Uses -> AttackBase" }] },

        // ======================================================================
        //  ACTOR TRAIT (BEHAVIOR) LAYER
        // ======================================================================
        { id: 20, name: "Mobile.cs (IResolveOrder)", path:"Traits/", nodeType: "code", pos: { x: 750, y: 800 }, icon: "➡️", columns: [{ name: "ResolveOrder('Move')" }, { name: "PathFinder.FindPath()" }] },
        { id: 21, name: "AttackFollow.cs (IResolveOrder)", path:"Traits/Attack/", nodeType: "code", pos: { x: 750, y: 1050 }, icon: "🎯", columns: [{ name: "ResolveOrder('Attack')" }, { name: "DoAttack()" }] },
        { id: 22, name: "Armament.cs", path:"Traits/", nodeType: "code", pos: { x: 1100, y: 1050 }, icon: "🔫", columns: [{ name: "CheckFire()" }, { name: "WeaponInfo" }] },
        { id: 23, name: "Health.cs", path:"Traits/", nodeType: "code", pos: { x: 1100, y: 1300 }, icon: "❤️", columns: [{ name: "InflictDamage()" }] },
        
        // ======================================================================
        //  CORE SYSTEMS
        // ======================================================================
        { id: 30, name: "HierarchicalPathFinder.cs", path:"Pathfinder/", nodeType: "code", pos: { x: 1100, y: 800 }, icon: "🗺️", columns: [{ name: "FindPath()" }] },
        { id: 31, name: "Ruleset.cs", path:"(Core Engine)", nodeType: "db", pos: { x: 1450, y: 50 }, icon: "📚", columns: [{ name: "LoadFromMod()" }, { name: "Actors", type:"Dictionary" }, { name: "Weapons", type:"Dictionary" }] },

        // ======================================================================
        //  DATA / SCHEMA LAYER
        // ======================================================================
        { id: 100, name: "vehicles.yaml (Rules)", path:"mods/ra/rules/", nodeType: "db", pos: { x: 1800, y: 50 }, icon: "📄", columns: [{ name: "1TNK: (Light Tank)" }, { name: "  Inherits: ^Vehicle" }, { name: "  Armament: Weapon: 25mm" }] },
        { id: 101, name: "weapons.yaml", path:"mods/ra/weapons/", nodeType: "db", pos: { x: 1450, y: 450 }, icon: "📄", columns: [{ name: "25mm:" }, { name: "  Projectile: ap" }, { name: "  Warhead: SpreadDamage" }] },
        { id: 102, name: "SpreadDamageWarhead.cs", path:"Warheads/", nodeType: "db", pos: { x: 1450, y: 750 }, icon: "💣", columns: [{ name: "DoImpact()" }, { name: "InflictDamage()" }] },
        
        // ======================================================================
        //  RENDERING LAYER
        // ======================================================================
        { id: 200, name: "RenderSprites.cs", path:"Traits/Render/", nodeType: "ui", pos: { x: 750, y: 450 }, icon: "🖼️", columns: [{ name: "Render()" }] },
        { id: 201, name: "WithSpriteBody.cs", path:"Traits/Render/", nodeType: "ui", pos: { x: 1100, y: 450 }, icon: "🧍", columns: [{ name: "DefaultAnimation" }] },
        { id: 202, name: "sequences/vehicles.yaml", path:"mods/ra/sequences/", nodeType: "db", pos: { x: 1450, y: 450 }, icon: "🎬", columns: [{ name: "1tnk: Filename: 1tnk.shp" }] },
        { id: 203, name: "1tnk.shp", path:"(asset)", nodeType: "service", pos: { x: 1800, y: 450 }, icon: "🎨", columns: [{ name: "Raw Sprite Data" }] }
    ],
    relationships: [
        // Input Flow
        { from: { table: "WorldInteractionControllerWidget.cs", column: "HandleMouseInput()" }, to: { table: "UnitOrderGenerator.cs", column: "Order()" }, type: "flow" },
        { from: { table: "UnitOrderGenerator.cs", column: "Order()" }, to: { table: "Order.cs", column: "OrderString" }, type: "write" },
        { from: { table: "Order.cs", column: "Subject (Actor)" }, to: { table: "Actor.cs (Core)", column: "TraitsImplementing<T>()" }, type: "flow" },
        
        // Order Resolution to Activity
        { from: { table: "Actor.cs (Core)", column: "TraitsImplementing<T>()" }, to: { table: "Mobile.cs (IResolveOrder)", column: "ResolveOrder('Move')" }, type: "read" },
        { from: { table: "Actor.cs (Core)", column: "TraitsImplementing<T>()" }, to: { table: "AttackFollow.cs (IResolveOrder)", column: "ResolveOrder('Attack')" }, type: "read" },
        { from: { table: "Mobile.cs (IResolveOrder)", column: "ResolveOrder('Move')" }, to: { table: "Actor.cs (Core)", column: "QueueActivity()" }, type: "flow" },
        { from: { table: "Actor.cs (Core)", column: "QueueActivity()" }, to: { table: "Move.cs (Activity)", column: "Tick()" }, type: "flow" },
        { from: { table: "AttackFollow.cs (IResolveOrder)", column: "ResolveOrder('Attack')" }, to: { table: "Actor.cs (Core)", column: "QueueActivity()" }, type: "flow" },
        { from: { table: "Actor.cs (Core)", column: "QueueActivity()" }, to: { table: "Attack.cs (Activity)", column: "Tick()" }, type: "flow" },

        // Activity Execution
        { from: { table: "Move.cs (Activity)", column: "Uses -> IMove" }, to: { table: "Mobile.cs (IResolveOrder)", column: "PathFinder.FindPath()" }, type: "read" },
        { from: { table: "Mobile.cs (IResolveOrder)", column: "PathFinder.FindPath()" }, to: { table: "HierarchicalPathFinder.cs", column: "FindPath()" }, type: "read" },
        { from: { table: "Attack.cs (Activity)", column: "Uses -> AttackBase" }, to: { table: "AttackFollow.cs (IResolveOrder)", column: "DoAttack()" }, type: "read" },
        { from: { table: "AttackFollow.cs (IResolveOrder)", column: "DoAttack()" }, to: { table: "Armament.cs", column: "CheckFire()" }, type: "read" },
        
        // Data Lookup Flow
        { from: { table: "Game.cs (Engine)", column: "LoadMod('ra')" }, to: { table: "Ruleset.cs", column: "LoadFromMod()" }, type: "flow" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod()" }, to: { table: "vehicles.yaml (Rules)", column: "1TNK: (Light Tank)" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "LoadFromMod()" }, to: { table: "weapons.yaml", column: "25mm:" }, type: "read" },
        { from: { table: "Armament.cs", column: "WeaponInfo" }, to: { table: "Ruleset.cs", column: "Weapons" }, type: "read" },
        { from: { table: "Ruleset.cs", column: "Weapons" }, to: { table: "weapons.yaml", column: "25mm:" }, type: "read" },
        { from: { table: "weapons.yaml", column: "  Warhead: SpreadDamage" }, to: { table: "SpreadDamageWarhead.cs", column: "DoImpact()" }, type: "read" },
        { from: { table: "SpreadDamageWarhead.cs", column: "InflictDamage()" }, to: { table: "Health.cs", column: "InflictDamage()" }, type: "write" },

        // Rendering Flow
        { from: { table: "Actor.cs (Core)", column: "Tick() -> Tick activities" }, to: { table: "RenderSprites.cs", column: "Render()" }, type: "flow" },
        { from: { table: "RenderSprites.cs", column: "Render()" }, to: { table: "WithSpriteBody.cs", column: "DefaultAnimation" }, type: "read" },
        { from: { table: "WithSpriteBody.cs", column: "DefaultAnimation" }, to: { table: "sequences/vehicles.yaml", column: "1tnk: Filename: 1tnk.shp" }, type: "read" },
        { from: { table: "sequences/vehicles.yaml", column: "1tnk: Filename: 1tnk.shp" }, to: { table: "1tnk.shp", column: "Raw Sprite Data" }, type: "read" }
    ]
};


// ===================================================================================
//  RENDERING LOGIC
// ===================================================================================
function initializeCanvas(canvasId, schema) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;
    const transformContainer = canvas.querySelector('.transform-container');
    const svg = canvas.querySelector('.connections');
    let state = { scale: 0.7, panX: 50, panY: 50, isPanning: false, lastMouse: { x: 0, y: 0 } };

    function render() {
        if (!transformContainer || !svg) return;
        transformContainer.innerHTML = '';
        transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
        
        schema.tables.forEach(table => {
            const node = document.createElement('div');
            node.id = `${canvasId}-${table.id}`;
            node.classList.add('node', `${table.nodeType}-node`);
            node.style.left = `${table.pos.x}px`;
            node.style.top = `${table.pos.y}px`;

            const header = document.createElement('h3');
            const pathSpan = table.path ? `<span class="path">${table.path}</span>` : '';
            header.innerHTML = `${pathSpan}${table.name}`;

            const list = document.createElement('ul');
            list.innerHTML = table.columns.map(col => {
                const colName = typeof col === 'string' ? col : col.name;
                const colType = typeof col === 'string' ? '' : col.type;
                const colId = colName.replace(/\s+/g, '-').replace(/[^\w-]/g, '');
                return `<li id="${canvasId}-${table.id}-${colId}"><span class="col-name">${colName}</span>${colType ? `<span class="col-type">${colType}</span>` : ''}</li>`;
            }).join('');
            
            if (table.icon) {
                const iconDisplay = document.createElement('div');
                iconDisplay.className = 'icon-display';
                iconDisplay.textContent = table.icon;
                node.appendChild(iconDisplay);
            }

            node.appendChild(header);
            node.appendChild(list);
            transformContainer.appendChild(node);
        });
        updateConnections();
    }
    
    function getElementPortPosition(elId) {
        const el = document.getElementById(elId);
        if (!el) return null;
        const parentNode = el.closest('.node');
        if (!parentNode) return null;
        const isNodeConnection = el.classList.contains('node');
        
        const parentRect = parentNode.getBoundingClientRect();
        const elRect = isNodeConnection ? parentRect : el.getBoundingClientRect();
        const canvasRect = canvas.getBoundingClientRect();
        
        const y = isNodeConnection ? (parentRect.top - canvasRect.top + parentRect.height / 2) : (elRect.top - canvasRect.top + elRect.height / 2);
        const leftX = (parentRect.left - canvasRect.left);
        const rightX = (parentRect.right - canvasRect.left);

        return { left: {x: leftX, y: y}, right: {x: rightX, y: y} };
    }

    function updateConnections() {
        if (!svg) return;
        svg.innerHTML = `<defs>
            <marker id="arrowhead-read-${canvasId}" markerWidth="10" markerHeight="7" refX="8" refY="3.5" orient="auto"><polygon points="0 0, 10 3.5, 0 7" fill="#6b7283" /></marker>
            <marker id="arrowhead-write-${canvasId}" markerWidth="12" markerHeight="9" refX="10" refY="4.5" orient="auto"><polygon points="0 0, 12 4.5, 0 9" fill="#86efac" /></marker>
            <marker id="arrowhead-flow-${canvasId}" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto"><polygon points="0 0, 8 3, 0 6" fill="#9ca3af" /></marker>
        </defs>`;
        
        const idMap = new Map(schema.tables.map(t => [t.name, t.id]));

        schema.relationships.forEach(rel => {
            const fromId = idMap.get(rel.from.table);
            const toId = idMap.get(rel.to.table);
            if(!fromId || !toId) return;

            let fromColName = typeof rel.from.column === 'string' ? rel.from.column : rel.from.column.name;
            let toColName = typeof rel.to.column === 'string' ? rel.to.column : rel.to.column.name;
            
            let fromElId = `${canvasId}-${fromId}-${fromColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(fromElId)) fromElId = `${canvasId}-${fromId}`;

            let toElId = `${canvasId}-${toId}-${toColName.replace(/\s+/g, '-').replace(/[^\w-]/g, '')}`;
            if (!document.getElementById(toElId)) toElId = `${canvasId}-${toId}`;
            
            const fromPorts = getElementPortPosition(fromElId);
            const toPorts = getElementPortPosition(toElId);
            if (!fromPorts || !toPorts) return;

            const fromPos = toPorts.right.x < fromPorts.left.x ? fromPorts.left : fromPorts.right;
            const toPos = toPorts.right.x < fromPorts.left.x ? toPorts.right : toPorts.left;
            
            const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
            line.setAttribute('x1', fromPos.x); line.setAttribute('y1', fromPos.y);
            line.setAttribute('x2', toPos.x); line.setAttribute('y2', toPos.y);

            if (rel.type === 'write') {
                line.setAttribute('stroke', '#86efac'); line.setAttribute('stroke-width', 4); line.setAttribute('marker-end', `url(#arrowhead-write-${canvasId})`);
            } else if (rel.type === 'flow') {
                line.setAttribute('stroke', '#9ca3af'); line.setAttribute('stroke-width', 2); line.setAttribute('stroke-dasharray', `6,6`); line.setAttribute('marker-end', `url(#arrowhead-flow-${canvasId})`);
            } else {
                line.setAttribute('stroke', '#6b7283'); line.setAttribute('stroke-width', 2); line.setAttribute('marker-end', `url(#arrowhead-read-${canvasId})`);
            }
            svg.appendChild(line);
        });
    }

    let activeNode = null, offset = { x: 0, y: 0 };
    canvas.addEventListener('mousedown', (e) => {
        const targetNode = e.target.closest('.node');
        if (targetNode) {
            activeNode = targetNode;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (!tableData) return;
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            offset.x = mouseX - tableData.pos.x;
            offset.y = mouseY - tableData.pos.y;
            activeNode.style.zIndex = 11;
        } else {
            state.isPanning = true;
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });

    document.addEventListener('mousemove', (e) => {
        if (activeNode) {
            e.preventDefault();
            const mouseX = (e.clientX - canvas.getBoundingClientRect().left - state.panX) / state.scale;
            const mouseY = (e.clientY - canvas.getBoundingClientRect().top - state.panY) / state.scale;
            const tableData = schema.tables.find(t => `${canvasId}-${t.id}` === activeNode.id);
            if (tableData) {
                tableData.pos.x = mouseX - offset.x;
                tableData.pos.y = mouseY - offset.y;
                activeNode.style.left = `${tableData.pos.x}px`;
                activeNode.style.top = `${tableData.pos.y}px`;
                updateConnections();
            }
        } else if (state.isPanning) {
            e.preventDefault();
            const dx = e.clientX - state.lastMouse.x;
            const dy = e.clientY - state.lastMouse.y;
            state.panX += dx;
            state.panY += dy;
            transformContainer.style.transform = `translate(${state.panX}px, ${state.panY}px) scale(${state.scale})`;
            updateConnections();
            state.lastMouse.x = e.clientX;
            state.lastMouse.y = e.clientY;
        }
    });
    
    document.addEventListener('mouseup', () => {
        if (activeNode) { activeNode.style.zIndex = 10; activeNode = null; }
        state.isPanning = false;
    });
    
    canvas.addEventListener('wheel', (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;
        const oldScale = state.scale;
        const zoomFactor = 1.1;
        state.scale *= e.deltaY < 0 ? zoomFactor : 1 / zoomFactor;
        state.scale = Math.max(0.1, Math.min(state.scale, 2.5));
        state.panX = mouseX - (mouseX - state.panX) * (state.scale / oldScale);
        state.panY = mouseY - (mouseY - state.panY) * (state.scale / oldScale);
        render();
    });

    render();
    return { render, updateConnections };
}

const canvases = [
    initializeCanvas('canvas-r2', schemaR2),
    initializeCanvas('canvas-r3', schemaR3),
    initializeCanvas('canvas-r4', schemaR4)
];

document.querySelectorAll('.splitter').forEach(splitter => {
    splitter.addEventListener('mousedown', (e) => {
        e.preventDefault();
        const prevPanel = splitter.previousElementSibling;
        const startX = e.clientX;
        const initialPrevWidth = prevPanel.getBoundingClientRect().width;
        
        const onMouseMove = (moveEvent) => {
            let newPrevWidth = initialPrevWidth + (moveEvent.clientX - startX);
            const MIN_WIDTH = 150;
            if (newPrevWidth < MIN_WIDTH) newPrevWidth = MIN_WIDTH;
            prevPanel.style.flex = `0 0 ${newPrevWidth}px`;
            canvases.forEach(c => c && c.render());
        };
        const onMouseUp = () => {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', onMouseUp);
        };
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    });
});

window.addEventListener('resize', () => {
    canvases.forEach(c => c && c.render());
});
</script>

</body>
</html>


