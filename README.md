# Free n8n Workflow: AI Social Media Content Generator (Ollama)

**Generate platform-optimized social media posts with local AI -- no API keys, no costs, no data leaving your machine.**

Enter a topic, get ready-to-post content for Twitter/X, LinkedIn, Reddit, and Instagram. Includes a second AI review pass for quality and engagement optimization.

> This is **1 of 11 production-ready workflows** from the [Self-Hosted AI Workflow Pack for n8n + Ollama](https://bonskari.github.io/n8n-ai-workflows/). The full pack ($39, one-time) includes blog writing, email auto-response, lead scoring, document summarization, competitor monitoring, and more.

---

## What This Workflow Does

```
You enter a topic
    |
    v
AI generates content for 4 platforms (Twitter, LinkedIn, Reddit, Instagram)
    |
    v
AI reviews & polishes everything for quality
    |
    v
You get ready-to-post content for each platform
```

- One click, four platforms
- Brand voice and audience targeting built in
- AI quality review catches errors and improves engagement
- Runs 100% locally with Ollama -- zero API costs

---

## Requirements

| Requirement | Details |
|---|---|
| **n8n** | Self-hosted, v1.0+ ([install guide](https://docs.n8n.io/hosting/)) |
| **Ollama** | Running on localhost:11434 ([install guide](https://ollama.ai/download)) |
| **Model** | Any Ollama model -- `llama3:8b` recommended |

---

## Setup (5 minutes)

### 1. Install Ollama and pull a model

```bash
# Install Ollama (Linux)
curl -fsSL https://ollama.ai/install.sh | sh

# Pull the recommended model
ollama pull llama3:8b

# Verify it's running
ollama list
```

### 2. Import the workflow into n8n

1. Copy the **entire JSON block** below
2. In n8n, go to **Workflows** > **Add Workflow** > **...** menu (top-right) > **Import from JSON**
3. Paste the JSON and click **Import**

### 3. Customize your content settings

After importing, open the **"Content Parameters"** node and edit:

- **topic** -- The subject you want content about
- **brand_voice** -- How your brand sounds (e.g., "Professional but approachable, data-driven")
- **target_audience** -- Who you're writing for (e.g., "startup founders, marketers")

### 4. Run it

Click **"Execute Workflow"** and check the output of the final node. You'll get JSON with platform-ready content for all four platforms.

---

## Workflow JSON

Copy this entire block and import it into n8n:

```json
{
  "name": "AI Social Media Content Generator (Ollama)",
  "nodes": [
    {
      "parameters": {},
      "id": "trigger",
      "name": "Manual Trigger",
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [240, 300]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "topic",
              "name": "topic",
              "value": "Benefits of self-hosting AI models",
              "type": "string"
            },
            {
              "id": "brand_voice",
              "name": "brand_voice",
              "value": "Tech-savvy, slightly irreverent, helpful. We love open source and privacy.",
              "type": "string"
            },
            {
              "id": "target_audience",
              "name": "target_audience",
              "value": "developers, sysadmins, tech enthusiasts",
              "type": "string"
            }
          ]
        }
      },
      "id": "config",
      "name": "Content Parameters",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [460, 300]
    },
    {
      "parameters": {
        "url": "http://localhost:11434/api/generate",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ JSON.stringify({ model: 'llama3:8b', prompt: 'Generate social media content for the topic: \"' + $json.topic + '\"\\n\\nBrand voice: ' + $json.brand_voice + '\\nTarget audience: ' + $json.target_audience + '\\n\\nReturn ONLY valid JSON with this structure:\\n{\\n  \"twitter\": \"Tweet (max 280 chars, include 2-3 relevant hashtags)\",\\n  \"linkedin\": \"LinkedIn post (3-4 paragraphs, professional tone, include hashtags)\",\\n  \"reddit_title\": \"Reddit post title (engaging, not clickbait)\",\\n  \"reddit_body\": \"Reddit post body (informative, genuine value, no hard sell)\",\\n  \"instagram_caption\": \"Instagram caption (engaging, with emoji, 5-10 hashtags at end)\"\\n}', stream: false, options: { temperature: 0.7, num_predict: 2000 } }) }}",
        "options": { "timeout": 120000 }
      },
      "id": "generate",
      "name": "Generate All Platform Content (Ollama)",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [680, 300]
    },
    {
      "parameters": {
        "jsCode": "const response = JSON.parse($input.first().json.data).response;\n// Extract JSON from response\nconst match = response.match(/\\{[\\s\\S]*\\}/);\nif (match) {\n  const content = JSON.parse(match[0]);\n  return [{ json: { ...content, topic: $('Content Parameters').first().json.topic, generated_at: new Date().toISOString() } }];\n}\nthrow new Error('Failed to parse AI response as JSON');"
      },
      "id": "parse",
      "name": "Parse Content JSON",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [900, 300]
    },
    {
      "parameters": {
        "url": "http://localhost:11434/api/generate",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ JSON.stringify({ model: 'llama3:8b', prompt: 'Review this social media content for quality. Check for:\\n1. Grammar and spelling errors\\n2. Tone consistency with brand voice\\n3. Platform-appropriate formatting\\n4. Hashtag relevance\\n\\nContent:\\nTwitter: ' + $json.twitter + '\\nLinkedIn: ' + $json.linkedin + '\\nReddit: ' + $json.reddit_title + ' — ' + $json.reddit_body + '\\nInstagram: ' + $json.instagram_caption + '\\n\\nIf any issues found, return corrected versions as JSON (same structure). If all good, return the same content. Return ONLY JSON.', stream: false, options: { temperature: 0.2, num_predict: 2000 } }) }}",
        "options": { "timeout": 120000 }
      },
      "id": "review",
      "name": "Review & Polish (Ollama)",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [1120, 300]
    },
    {
      "parameters": {
        "jsCode": "const response = JSON.parse($input.first().json.data).response;\nconst match = response.match(/\\{[\\s\\S]*\\}/);\nconst original = $('Parse Content JSON').first().json;\nif (match) {\n  try {\n    const reviewed = JSON.parse(match[0]);\n    return [{ json: { ...original, ...reviewed, reviewed: true } }];\n  } catch(e) {}\n}\nreturn [{ json: { ...original, reviewed: false } }];"
      },
      "id": "final-parse",
      "name": "Final Content Output",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [1340, 300]
    }
  ],
  "connections": {
    "Manual Trigger": {
      "main": [[{ "node": "Content Parameters", "type": "main", "index": 0 }]]
    },
    "Content Parameters": {
      "main": [[{ "node": "Generate All Platform Content (Ollama)", "type": "main", "index": 0 }]]
    },
    "Generate All Platform Content (Ollama)": {
      "main": [[{ "node": "Parse Content JSON", "type": "main", "index": 0 }]]
    },
    "Parse Content JSON": {
      "main": [[{ "node": "Review & Polish (Ollama)", "type": "main", "index": 0 }]]
    },
    "Review & Polish (Ollama)": {
      "main": [[{ "node": "Final Content Output", "type": "main", "index": 0 }]]
    }
  },
  "settings": { "executionOrder": "v1" },
  "tags": [{ "name": "AI" }, { "name": "Ollama" }, { "name": "Social Media" }, { "name": "Content" }]
}
```

---

## Example Output

When you run the workflow with the default topic ("Benefits of self-hosting AI models"), you'll get output like:

```json
{
  "twitter": "Why send your data to the cloud when you can run AI locally? Self-hosted models = zero API costs, full privacy, no rate limits. Your hardware, your rules. #SelfHosted #AI #OpenSource",
  "linkedin": "We've been running self-hosted AI models for our entire content pipeline, and the results speak for themselves...",
  "reddit_title": "I switched from OpenAI API to self-hosted Ollama. Here's what changed.",
  "reddit_body": "After 3 months of running llama3 locally instead of paying for GPT-4 API calls...",
  "instagram_caption": "Your AI, your rules. Self-hosting means your data never leaves your machine...",
  "topic": "Benefits of self-hosting AI models",
  "generated_at": "2026-03-24T12:00:00.000Z",
  "reviewed": true
}
```

---

## Tips for Better Results

- **Be specific with topics.** "5 ways remote teams use async video" beats "remote work."
- **Match brand voice to your actual tone.** Read your last 5 posts and describe the style.
- **Try different models.** `mistral:7b` tends to be more concise; `llama3:8b` is more detailed.
- **Use a larger model for quality.** If your hardware supports it, `llama3:70b` produces noticeably better content.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Connection refused" error | Make sure Ollama is running: `ollama serve` |
| Timeout errors | Increase the timeout in the HTTP Request nodes, or use a smaller model |
| JSON parse errors | Some models struggle with structured output -- try `llama3:8b` which handles JSON well |
| Empty or garbled output | Pull the model again: `ollama pull llama3:8b` and restart Ollama |

---

## Want All 11 Workflows?

This is just one workflow from the full pack. The **Self-Hosted AI Workflow Pack** includes 11 production-ready n8n templates:

| # | Workflow | What It Does |
|---|---|---|
| 1 | **AI Blog Writer Pipeline** | Research, outline, draft, and edit blog posts |
| 2 | **AI Social Media Generator** | Multi-platform content from a single topic (this one) |
| 3 | **AI YouTube-to-Newsletter** | Turn any YouTube video into an email newsletter |
| 4 | **AI Content Repurposer** | One blog post becomes 6 platform-specific pieces |
| 5 | **AI Lead Scorer** | Score and enrich incoming leads automatically |
| 6 | **AI Competitor Monitor** | Weekly competitor intelligence briefings on autopilot |
| 7 | **AI Email Auto-Responder** | Classify emails, filter spam, draft replies |
| 8 | **AI Support Ticket Router** | Triage tickets by category, priority, and sentiment |
| 9 | **AI Document Summarizer** | Summarize documents with Q&A generation |
| 10 | **AI Meeting Notes** | Structured summaries with action items from transcripts |
| 11 | **AI Data Extractor** | Pull structured JSON from unstructured text |

All workflows run locally with Ollama. No API keys. No monthly fees. One-time purchase.

**[Get the full pack for $39](https://bonskari.github.io/n8n-ai-workflows/)**

---

## License

This free sample workflow is released under the MIT License. Use it however you like.

The full workflow pack is sold under a standard commercial license -- see the [product page](https://bonskari.github.io/n8n-ai-workflows/) for details.

---

## Star This Repo

If this workflow saved you time, give it a star. It helps others find it.
