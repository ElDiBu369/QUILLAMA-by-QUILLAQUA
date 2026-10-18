# QUILLAMA

Source code and input prompt template accompanying the QUILLAMA manuscript.

| File | Content |
| --- | --- |
| `quillama.json` | Node-RED source export of the QUILLAMA query service. |
| `prompt-template.json` | Complete input prompt template for the NL-to-SQL stage, extracted from the supplied current code. |

The prompt placeholders are `{{USER_QUESTION}}`, `{{DB_SCHEMA}}` and `{{SQL_EXAMPLES_FIRST_12000_CHARS}}`.

For execution, import the flow into Node-RED with `node-red-node-mysql` installed, and configure the MariaDB connection, Ollama endpoint and local resource-file paths referenced by the flow. This is a source-code export for inspection, not a self-contained deployment package.

The files describe the current supplied configuration; they are not asserted to be a frozen snapshot of the original S01-S54 executions.

The original operational database is not included. Its supplied registration identifiers are certificate **QUI-073079**, application **SENADI-2026-66205**.
