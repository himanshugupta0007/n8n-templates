# AI Blog Generator (n8n Template)

An end-to-end n8n workflow that takes a blog topic and goals from a form, builds a focused writing brief, creates an SEO pack, drafts a full blog post, runs SEO + quality validation, and auto-revises until it passes or hits the retry limit. The final blog is converted to HTML and emailed.

## What This Workflow Does

1. Collects blog inputs via a form trigger (topic, audience, goal, tone, length, language).
2. Validates input values.
3. Uses Gemini to generate a structured writing brief.
4. Generates an SEO pack (intent, keywords, titles, meta, FAQs).
5. Drafts the full blog post with an OpenAI copywriter agent.
6. Parses and validates the generated JSON output.
7. Runs a strict SEO + quality evaluation (OpenAI).
8. If failed, revises the blog and retries (up to 5 times).
9. Converts final markdown to HTML and emails the result.

## Included Template

- `Blog Generator/AI Blog Generator.json`

## Inputs

From the form trigger:

- `topic` (required)
- `audience` (optional)
- `goal` (optional)
- `tone` (optional)
- `length_words` (600, 1000, 1500)
- `language` (English, Hindi)

## Outputs

- Final blog content in markdown (and HTML via the Markdown node)
- Email sent with the generated blog content

## Credentials You Need

Set these in n8n before running the workflow:

- Google Gemini (PaLM) API credentials for:
  - `Brief Builder`
  - `SEO Pack - Create`
- OpenAI API credentials for:
  - `Copywriter` (agent)
  - `SEO + Quality Validation`
  - `Copywriter Revision Agent`
- Gmail OAuth2 credentials for:
  - `Send a message`

## How to Use

1. Import `Blog Generator/AI Blog Generator.json` into n8n.
2. Add/verify credentials for Gemini, OpenAI, and Gmail.
3. Open the `On form submission` node and use the test form.
4. Run the workflow.
5. Check your email for the generated blog output.

## Key Logic Details

- **Strict JSON parsing** for brief, SEO pack, and blog output.
- **Validation gates** stop execution on malformed outputs.
- **Retry loop** for revisions: max 5 attempts.
- **SEO QA** enforces intent match, keyword usage, title/meta alignment, and readability.

## Notes and Limitations

- The workflow emails the blog to the configured Gmail address.
- If SEO validation fails repeatedly, the workflow stops after 5 revisions.
- Ensure your models are available in your n8n instance:
  - Gemini: `models/gemini-2.5-flash`
  - OpenAI: `gpt-5-mini`, `gpt-5.2`

## Suggested Customizations

- Add more languages in the form dropdown and validation logic.
- Adjust SEO rules (keyword limits, thresholds, or scoring weights).
- Send output to a CMS, Notion, or Google Docs instead of email.
