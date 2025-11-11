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