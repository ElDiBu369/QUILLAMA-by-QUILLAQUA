# QUILLAMA

Source code and input prompt template accompanying the QUILLAMA manuscript.

| File | Content |
| --- | --- |
| `quillama.json` | Node-RED source export of the QUILLAMA query service. |
| `prompt-template.json` | Complete input prompt template for the NL-to-SQL stage, extracted from the current code. |

The prompt placeholders are `{{USER_QUESTION}}`, `{{DB_SCHEMA}}` and `{{SQL_EXAMPLES_FIRST_12000_CHARS}}`.

For execution, import the flow into Node-RED with `node-red-node-mysql` installed, and configure the MariaDB connection, Ollama endpoint and local resource-file paths referenced by the flow. This is a source-code export for inspection, not a self-contained deployment package.

The files describe the current supplied configuration; they are not asserted to be a frozen snapshot of the original S01-S54 executions.

The original operational database is not included. Its supplied registration identifiers are certificate **QUI-073079**, application **SENADI-2026-66205**.

## Semantic similarity assessment

This section documents the supplementary semantic similarity analysis performed during manuscript revision on the 32 operational cases preserved in Appendices A and B.

- Model: sentence-transformers/all-MiniLM-L6-v2.
- Model revision: 1110a243fdf4706b3f48f1d95db1a4f5529b4d41.
- Runtime: ONNX FP32 on CPU; onnxruntime 1.31.0, tokenizers 0.23.3, NumPy 2.5.4.
- Encoding: maximum 256 wordpieces; attention-mask-weighted mean pooling; float64 for pooling and cosine calculations; L2-normalised embeddings.
- Metric: cosine similarity. For Spearman correlation, categories were encoded as Incorrect = 0, Partial = 1, Correct = 2.

Questions use the English formulations in Appendix A. Editorial annotations were removed; T, RH and VPD were expanded to temperature, relative humidity and vapour pressure deficit, respectively. A1--A8 was written as A1 to A8.

SQL descriptions were constructed using fixed rules for selected columns, aggregates, temporal conditions, ordering and limits. Aliases were ignored. HA was marked as an undefined column according to the documented schema. Manual classification labels were used only to summarise and compare the completed scores.

| ID | Question used | SQL description used | Manual class | Cosine similarity |
| --- | --- | --- | --- | --- |
| S01 | What is the current temperature? | Return temperature from the latest reading. | Correct | 0.545073275 |
| S02 | What is the current relative humidity? | Return average relative humidity where the record timestamp equals midnight at the start of the current calendar day. | Incorrect | 0.626016009 |
| S03 | What is the current relative humidity? | Return average relative humidity over all recorded history. | Incorrect | 0.676525299 |
| S04 | What is the current vapour pressure deficit? | Return maximum vapour pressure deficit over all recorded history. | Incorrect | 0.813870990 |
| S05 | Give me the current absolute humidity value. | Return absolute humidity from the latest reading. | Correct | 0.640175054 |
| S06 | Give me the current dew-point temperature. | Return dew-point temperature from the latest reading. | Correct | 0.817848765 |
| S07 | Give me the current heat-index value. | Return heat index from the latest reading. | Correct | 0.733196544 |
| S08 | At what time was the latest reading recorded? | Return record timestamp from the latest reading. | Correct | 0.586071321 |
| S09 | Average temperature today. | Return average temperature for the current calendar day. | Correct | 0.661217451 |
| S10 | Average humidity today. | Return average relative humidity for the current calendar day. | Correct | 0.732446126 |
| S11 | Trend of temperature over the last hour. | Return record timestamp and temperature for the last 1 hour, ordered chronologically. | Correct | 0.507879886 |
| S12 | Trend of temperature and relative humidity in November. | Return earliest record timestamp, latest record timestamp, minimum temperature, maximum temperature, minimum relative humidity and maximum relative humidity for records from November, without restricting the year. | Partial | 0.444715566 |
| S13 | What was the maximum temperature during the last week? | Return maximum temperature for the last 1 week. | Correct | 0.730381097 |
| S14 | What was the minimum relative humidity during the last week? | Return minimum relative humidity for the last 1 week. | Correct | 0.776107781 |
| S15 | How many records are there today? | Return number of recorded samples for the current calendar day. | Correct | 0.486898172 |
| S16 | What is the average temperature over the last 30 minutes? | Return average temperature for the last 30 minutes. | Correct | 0.849635905 |
| S17 | What is the average relative humidity over the last 30 minutes? | Return average relative humidity for the last 30 minutes. | Correct | 0.847492320 |
| S18 | Which external variables are recorded? | Return record timestamp, external temperature from OpenWeatherMap and external relative humidity from OpenWeatherMap from the 10 latest readings, ordered from newest to oldest. | Partial | 0.403897606 |
| S19 | What is the most recent complete reading? | Return all recorded variables from the latest reading. | Correct | 0.445994283 |
| S20 | What is the current state of actuators A1 to A8? | Return state of actuator A1, state of actuator A2, state of actuator A3, state of actuator A4, state of actuator A5, state of actuator A6, state of actuator A7, state of actuator A8 and record timestamp from the latest reading. | Correct | 0.591147643 |
| S21 | What is the current light level? | Return light level and record timestamp for records with the latest timestamp. | Correct | 0.442134151 |
| S22 | Average light level today. | Return average light level for the current calendar day. | Correct | 0.691836398 |
| S23 | What was the maximum light level during the last week? | Return maximum light level for the last 7 days. | Correct | 0.677157273 |
| S24 | Is the fan currently active? | Return state of actuator A1 and record timestamp for records with the latest timestamp. | Incorrect | 0.022881580 |
| S25 | Is the extractor currently active? | Return state of actuator A5 and record timestamp for records with the latest timestamp. | Incorrect | 0.071304316 |
| S26 | Is relay 3 currently active? | Return state of actuator A3 and record timestamp for records with the latest timestamp. | Correct | 0.156489327 |
| S27 | How many times was the misting unit activated today? | Return sum of undefined column HA for the current calendar day. | Incorrect | 0.099873319 |
| S28 | How many times was the humidifier activated today? | Return number of recorded samples for the current calendar day for samples where undefined column HA equals 1. | Incorrect | 0.240253095 |
| S29 | How many times was the humidifier activated today? | Return number of recorded samples for the current calendar day for samples where humidifier state (HA10) equals 1. | Partial | 0.525774492 |
| S49 | What was the system like yesterday? | Return average temperature, average relative humidity and average vapour pressure deficit for the last 1 day. | Partial | 0.367612394 |
| S50 | Give me the average. | Return average temperature and average relative humidity over all recorded history. | Incorrect | 0.318112600 |
| S51 | Trend. | Return record timestamp and temperature over all recorded history, ordered chronologically. | Incorrect | 0.083732352 |

Mean cosine similarity: Correct = 0.627325409; Partial = 0.435500015; Incorrect = 0.328063284.

Spearman correlation: 0.528771999.

These scores represent textual similarity and should not be interpreted as SQL accuracy percentages.
