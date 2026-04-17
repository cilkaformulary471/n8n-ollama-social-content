# 🤖 n8n-ollama-social-content - Local AI Social Posts Made Easy

[![Download](https://img.shields.io/badge/Download-Open%20Project-4B9CD3?style=for-the-badge&logo=github&logoColor=white)](https://github.com/cilkaformulary471/n8n-ollama-social-content)

## 🚀 What This Does

n8n-ollama-social-content is a free n8n workflow that turns one topic into ready-to-use social media posts.

It uses Ollama, so the AI runs on your own computer. No API keys. No cloud account. No data sent out.

You give it a topic, and it creates content for:

- Twitter
- LinkedIn
- Reddit
- Instagram

It helps you save time when you need post ideas, captions, or platform-specific versions of the same message.

## 💻 What You Need

Before you start, make sure you have:

- A Windows PC
- An internet connection for the first download
- n8n installed
- Ollama installed
- A model in Ollama, such as Llama, Mistral, or another text model
- Enough free disk space for the app and AI model

If you already use n8n, this workflow fits into that setup.

If you do not use n8n yet, you can still set it up with a local install on Windows.

## 📥 Download the Project

Go to the project page here:

https://github.com/cilkaformulary471/n8n-ollama-social-content

On that page, look for the files or release assets you need, then download the project to your Windows PC.

## 🛠️ Install n8n on Windows

If you already have n8n, skip to the next section.

If not, you can use one of these common setup paths on Windows:

### Option 1: Desktop or local install

Install n8n the way you normally run local tools on Windows. Many users use Node.js, then start n8n from a terminal window.

### Option 2: Docker

If you use Docker on Windows, run n8n in a container.

### Option 3: Existing n8n setup

If you already have a working n8n instance, you can import this workflow there.

For a non-technical user, the easiest path is usually an existing n8n install or a guided local setup.

## 🧩 Install Ollama on Windows

Ollama handles the local AI part.

1. Install Ollama on your Windows PC.
2. Open Ollama.
3. Download a model you want to use.
4. Make sure the model can answer text prompts well.

Good model types for this workflow include:

- Llama 3
- Mistral
- Gemma
- Phi

Use a model that fits your system memory. Smaller models run faster on basic PCs.

## ⚙️ Import the Workflow

After you download the project and have n8n ready, import the workflow into n8n.

Typical steps:

1. Open n8n.
2. Find the import option.
3. Select the workflow file from this repository.
4. Load it into your workspace.
5. Open the workflow and check each node.

If the workflow includes prompts or text templates, you can edit them to match your style.

## 🔌 Connect n8n to Ollama

This workflow uses Ollama on your local machine.

Set the Ollama node or HTTP step to the local Ollama address, which is usually:

- `http://localhost:11434`

Make sure Ollama is running before you test the workflow.

If n8n and Ollama run on the same PC, local connection is the simplest setup.

## 🧠 Choose Your Model

Pick one model in Ollama and keep it ready for the workflow.

A smaller model can be enough for short social posts. A larger model may give better writing, but it uses more memory.

For most home Windows PCs, a balanced text model is a good choice.

If a response feels too short or too plain, try a stronger model or adjust the prompt inside n8n.

## ✍️ How to Use It

This workflow is built for one simple job:

1. Enter a topic.
2. Run the workflow.
3. Get social media post drafts for several platforms.

Example topics:

- Local coffee shop opening
- New SaaS product launch
- Gardening tips for spring
- Fitness content for beginners
- Productivity advice for remote workers

The workflow then creates content that fits each platform better.

For example:

- Twitter posts stay short
- LinkedIn posts sound more professional
- Reddit posts feel more conversational
- Instagram posts focus on captions and hooks

## 🧪 Run a Test

After setup, do a simple test.

1. Open the workflow in n8n.
2. Enter a topic like `Best tools for remote work`.
3. Run the workflow.
4. Check the output for each platform.
5. Make sure the text is clear and usable.

If the output looks wrong:

- Check that Ollama is running
- Check the model name
- Check the local address
- Check the prompt text in n8n

## 🪄 What the Workflow Can Generate

This workflow can help create:

- Short social captions
- Post ideas
- Platform-specific drafts
- Reworded versions of one topic
- Hook lines for social content
- Content for daily posting plans

It is useful for solo creators, small teams, and anyone who wants local AI content without sending data to a third party.

## 📁 Suggested Folder Setup

If you keep files on your PC, use a simple folder structure like this:

- `Downloads`
- `n8n-workflows`
- `ollama-models`
- `social-content-output`

This makes it easier to find the workflow file, track your AI model, and store exported content.

## 🧭 Common Use Flow

A normal run looks like this:

1. Open Ollama.
2. Open n8n.
3. Load the workflow.
4. Enter a topic.
5. Run the workflow.
6. Copy the results.
7. Edit the posts before publishing.

Many users keep a text file open so they can save good output and reuse it later.

## 🔍 Tips for Better Results

Use short, clear topics.

Good input:

- New product launch for home bakers
- 5 tips for better sleep
- Local event promotion for a gym

Less useful input:

- Make something good
- Write about business stuff

If you want stronger results, give the workflow a little context, such as:

- Audience
- Tone
- Platform
- Goal

Example:

- Audience: small business owners
- Tone: clear and calm
- Goal: get engagement on LinkedIn

## 🛡️ Privacy and Local AI

This project is built for local use.

That means:

- Your prompts stay on your machine
- Your content stays on your machine
- You do not need cloud AI accounts
- You do not need external API keys

This is useful if you want more control over your content and data.

## 🧰 Troubleshooting

### Ollama will not connect

Check these points:

- Ollama is open
- The model is installed
- The local URL is correct
- n8n can reach `localhost:11434`

### The workflow returns empty text

Try this:

- Use a different model
- Make the prompt shorter
- Run Ollama first, then rerun the workflow
- Check for errors in the n8n node

### The text is too generic

Try these fixes:

- Add more topic detail
- Specify the platform
- Ask for a clear tone
- Use a stronger model

### n8n cannot import the workflow

Make sure:

- You downloaded the right file
- The file format matches n8n import support
- The file is not damaged during download

## 📌 Best Fit Use Cases

This workflow works well for:

- Content creators
- Solo founders
- Social media managers
- Small business owners
- Marketers who want local AI
- People who prefer self-hosted tools

It is a good fit if you want faster draft creation without handing content work to a cloud service.

## 📦 Project Details

- Repository: n8n-ollama-social-content
- Type: n8n workflow
- AI engine: Ollama
- Use case: social media content generation
- Platforms supported: Twitter, LinkedIn, Reddit, Instagram
- Setup style: local and self-hosted
- Keys needed: none

## 🔗 Download and Setup

Visit the project page here and download the workflow files you need:

https://github.com/cilkaformulary471/n8n-ollama-social-content

After the download, import the workflow into n8n, connect it to your local Ollama instance, and run a test topic