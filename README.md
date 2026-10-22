# Taylan Government — GLM-5.3-Flash government harness (prototype)

A runnable Java 17+ / Maven / SQLite event-driven prototype. **Not a complete import of your Whimsical board.** The sample CSV contains five illustrative offices. Replace it with the full registry after confirming each node represents a post (the atlas also contains descriptions, teams and unresolved draft rules).

## Setup (Windows PowerShell)

1. Install JDK 17+ and Maven (`java -version`, `mvn -version`).
2. In this folder, run `mvn package`.
3. Obtain a **general API** key from Z.ai, not a coding-plan-only key. In the current PowerShell session set `$env:GLM_API_KEY="YOUR_KEY"` (do not commit or share it).
4. Check your provider's current model availability and prices. The defaults use Z.ai's general OpenAI-compatible endpoint and `glm-5.3-flash` model ID. You can instead use OpenRouter with `$env:GLM_BASE_URL="https://openrouter.ai/api/v1"` and `$env:GLM_MODEL="z-ai/glm-5.3-flash"` and the corresponding key.
5. Set **verified** provider rates (USD per million tokens) using `$env:INPUT_USD_PER_M="0.15"` and `$env:OUTPUT_USD_PER_M="0.50"` (illustrative defaults, NOT verified live prices). Set `$env:MAX_USD="2.00"`.
6. `java -jar target/government-harness-0.1.0.jar import officials.sample.csv`
7. `java -jar target/government-harness-0.1.0.jar send crown lord-chancellor "Prepare an initial government programme and request an energy assessment."`
8. `java -jar target/government-harness-0.1.0.jar step 10`
9. `java -jar target/government-harness-0.1.0.jar report`

`next-day` advances simulated time; `stats` shows pending messages and estimated API spend. Run `step 10` repeatedly to process the queue. State persists in `taylan.db`.

## CSV format

`id,title,parent,remit,persona` with one person per post. IDs must be unique; parent references another ID. Importing an existing ID updates it. Do not import organization headings or descriptions as people. Sample data are illustrative, not the complete canonical hierarchy.

## Limitations and safety

- The **local estimated spend cap** uses the API's reported token counts and your configured rates; it is NOT a provider-enforced hard cap. A request may cross the cap, and provider fees/cached/reasoning token accounting may differ. Configure an actual provider-side limit if available. The harness makes requests sequentially, avoiding concurrent cap overshoot.
- This is a **prototype**, not a complete constitutional engine. It records proposed actions and communications but does not enact legislation, conduct votes, implement budgets, or prove constitutional legality. Those require explicit rules before activation.
- Model replies may be malformed. Invalid JSON is logged; no message is sent. A maximum of one outgoing message is allowed per decision. The inbox is finite but agents can generate loops; stop with the budget and decision caps.
- No API call is made by `import`, `send`, `stats`, `next-day`, or `report`; only `step` spends money.
- API access, pricing, and live responses were not tested here because no user API key was provided.
