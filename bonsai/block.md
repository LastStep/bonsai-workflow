<!--
bonsai/block.md: the workflow pack's part of the instruction block Bonsai writes into a linked project's CLAUDE.md.
Every session reads it, so it is at most four lines: the six roles and how each starts, the lanes, the templates.
The engine writes the lines after this comment; the comment stays in the pack. README.md ("The block") says more.
-->
Roles (workflow): the session the person talks to is the orchestrator and opens `workflow:orchestrate` when work starts; builder and verifier (leads) and researcher, auditor and runner (helpers) are called with their brief, or started as `claude --agent workflow:<role>`.
Lanes light, full and director, each with its pipeline: `workflow:lanes`. Document templates: `workflow:plan`, `workflow:feature`, `workflow:decision`, `workflow:options`, `workflow:spec`, `workflow:playtest`, `workflow:bugs`, and the daily brief's in `workflow:brief`.
