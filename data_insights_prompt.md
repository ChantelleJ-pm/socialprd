# Stakeholder Data Insights Prompt

Use the prompt below with ChatGPT, Claude, Gemini, or another analysis model when you want to turn spreadsheet data into stakeholder-ready insights.

This version is tailored to the Recipe Clipper initiative context in the PRD, where the main business goals are to increase:

- saves per user
- collection creation
- repeat sessions / retention
- engagement with MyRecipes as a central recipe hub
- user acquisition via external recipe saving behavior

## Recommended input

Provide the model with:

1. the Google Sheet link
2. exported CSVs or pasted tab contents if the sheet is not directly accessible
3. a short note on the audience for the output
4. the decision you are trying to support
5. the relevant date range, if known

## Copy/paste prompt

```text
You are a senior product insights analyst helping me analyze a spreadsheet and turn it into a clear stakeholder-ready narrative.

Context:
- This analysis supports the MyRecipes Recipe Clipper initiative.
- The product goal is to make MyRecipes the central place users save recipes from across the web.
- The most important business outcomes are increased saves per user, more collection creation, stronger repeat usage / retention, and clearer evidence that the feature creates value for users and the business.
- The audience may include product, design, engineering, marketing, analytics, and leadership stakeholders.

Your job:
1. Review the spreadsheet data and first identify:
   - what tabs or sections exist
   - what each tab appears to measure
   - the date range covered
   - the key dimensions or segments available
   - any data quality issues, missing definitions, or inconsistencies
2. Build a concise KPI framework from the data:
   - north-star outcome
   - leading indicators
   - lagging indicators
   - diagnostic metrics
3. Analyze the data for:
   - major trends over time
   - meaningful increases or decreases
   - differences by segment, platform, source, audience, or cohort
   - funnel drop-off points
   - anomalies, outliers, and possible drivers
   - relationships between behavior metrics and business outcomes
4. Synthesize the analysis into stakeholder insights, not just observations.
   For each insight, explain:
   - what happened
   - why it matters
   - what evidence supports it
   - what the likely explanation is
   - how confident you are
   - what action or decision it suggests
5. Anchor the insights to the Recipe Clipper goals whenever possible:
   - saves per user
   - collection creation
   - repeat sessions / retention
   - adoption of saving recipes from outside MyRecipes
   - user value and business value
6. Separate signal from noise:
   - call out which findings are material enough for stakeholders to care about
   - note where the data is inconclusive
   - avoid overstating causality

Output format:

## 1) Executive summary
- 3 to 5 bullets with the most important takeaways in plain English
- each bullet should include the business implication

## 2) What the data says
Create a table with these columns:
- Insight
- Evidence
- Impact on users/business
- Confidence level
- Recommended action

## 3) Deep dive
Include:
- performance trends
- segment differences
- funnel or journey insights
- risks, surprises, and anomalies
- notable gaps in the data

## 4) Stakeholder narrative
Write a short narrative I could use in a stakeholder readout:
- what is working
- what is not working
- what we should investigate next
- what decisions or support are needed

## 5) Next-step recommendations
Prioritize recommendations as:
- do now
- investigate next
- monitor

## 6) Open questions
List the most important unanswered questions or data limitations.

Style requirements:
- be concise, specific, and evidence-based
- write for cross-functional stakeholders, not analysts
- translate metrics into product and business meaning
- use bullets over dense paragraphs where possible
- explicitly label assumptions
- if the sheet is missing context, start by asking me only the minimum clarifying questions needed

Important:
- If you cannot access the Google Sheet directly, tell me exactly what data export you need (for example: tab names, CSV exports, screenshots, or metric definitions) and then proceed once I provide it.
- If metric names are ambiguous, propose a likely interpretation but flag it as an assumption.
- If there are too many tabs, group them into themes before analyzing.
- If possible, end with a stakeholder-ready headline such as: "The strongest signal is...", "The main risk is...", and "The decision this points to is..."

Here is the spreadsheet to analyze:
https://docs.google.com/spreadsheets/d/1rOcdkynL7A1VZevbWkfIGQrDyZkSNjsFFX25gpAxVxM/edit?gid=503997832#gid=503997832

Optional extra context:
- Audience: [product/design/engineering/leadership/etc.]
- Main decision to support: [decision]
- Time period of interest: [date range]
- Known KPI definitions: [definitions]
```

## Shorter version

Use this when you want a faster answer:

```text
Analyze this spreadsheet like a senior product insights analyst and turn it into stakeholder-ready insights.

Focus on:
- major trends
- meaningful segment differences
- anomalies or drop-offs
- implications for saves per user, collection creation, repeat sessions, retention, and overall Recipe Clipper value

Return:
1. executive summary
2. top 5 insights with evidence and business implication
3. recommended actions
4. open questions / data gaps

If you cannot access the sheet directly, ask me for the minimum CSV exports or tab summaries you need.

Sheet: https://docs.google.com/spreadsheets/d/1rOcdkynL7A1VZevbWkfIGQrDyZkSNjsFFX25gpAxVxM/edit?gid=503997832#gid=503997832
Audience: [AUDIENCE]
Decision to support: [DECISION]
```

## Suggested usage notes

- If the sheet contains many tabs, start by asking the model to inventory the workbook and identify the most decision-relevant tabs first.
- If the sheet mixes qualitative and quantitative inputs, ask the model to keep them separate initially, then synthesize them in the final narrative.
- If you are preparing for a meeting, add: "End with 3 talking points and 3 likely stakeholder questions."
