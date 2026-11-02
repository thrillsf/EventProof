# Run EventProof

Python 3.10+ required. In your Codespaces terminal:

```bash
pip install -r requirements.txt
read -s -p "Fireworks API key: " FIREWORKS_API_KEY
export FIREWORKS_API_KEY
python extract.py 'https://YOUR-EVENT-URL' --output result.json --ics event.ics
```

The key is entered privately in the terminal. Do not paste it into code or commit it.
`.env.example` documents the variable; this program does not automatically load .env files.

Open result.json to review extracted values, quotes, warnings, source text, raw API
response, model identifier, token usage, model latency and total processing latency.
Download event.ics from the Codespaces file explorer and import it into your calendar.
The program creates a file; it does not directly modify Google Calendar or Outlook.

If a page requires JavaScript/login or blocks fetching, save the page's event text
and use the explicit fallback:

```bash
python extract.py 'https://YOUR-EVENT-URL' --text-file event-02-input.txt --output result.json
```

Use original event text, not a file containing another model's response. No network
fetch is performed in fallback mode. Include the original URL for attribution.

## Interpretation and limitations

- The open model extracts the whole event, with separate start/end dates.
- Exact source quotes are checked in code. This does not prove that a quote supports
  a field semantically. Review all extracted details before importing.
- Missing year, end time or event timezone blocks calendar export. A website clock
  is not event timezone evidence. Do not infer missing facts to force export.
- Warnings, including conflicting overall and agenda hours, block export. This
  conservative behavior reduces export coverage and must be reported in evaluation.
- JavaScript-only content, inaccessible pages, recurrence and all-day events are
  not fully supported in this first implementation. Large pages are rejected.
- The command uses paid Fireworks API calls with an 8192-token output cap; it does
  not retry calls automatically. A failed export can still incur extraction cost.
- Latency includes non-streaming completion. Cost is not calculated; use saved usage
  with the actual applicable provider prices and include reasoning tokens.
- Saved runs contain page content: review data permissions before publishing them.

## Evaluation status

Existing uploaded event examples are development data, not held-out benchmark
results. No reproducible frontier API comparison, cost advantage, or quality
improvement is claimed yet. Preserve original responses. Correct answer-key issues
with a documented scoring policy rather than changing model outputs.

Use the same saved fetched evidence for both systems in an extraction benchmark.
For end-to-end URL evaluation, separately report fetch failures and the fraction
of all inputs producing correct usable calendar files. Keep tuning examples and
held-out test examples separate; save model configuration and raw outputs.

## AI assistance and attribution

The initial extractor and these instructions were written with ChatGPT/Codex
assistance during this work session. It uses existing Fireworks gpt-oss-120b and
requests, Beautiful Soup, python-dateutil and icalendar libraries. No model training
was performed. The team must review and explain this code in its submission.

Fireworks API reference: https://docs.fireworks.ai/api-reference/post-chatcompletions
