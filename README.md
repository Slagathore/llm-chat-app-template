# LLM chat app template

A small chat app that runs on Cloudflare Workers AI and streams the model's output back as it generates. Use it as a starting point for your own AI chat frontend.

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/Slagathore/llm-chat-app-template)

## What it does

- Streams responses live over Server-Sent Events (SSE)
- Model and system prompt both live in one file, so swapping them is quick
- Works with AI Gateway if you want rate limiting, caching, or analytics
- Plain HTML/CSS/JS frontend that keeps chat history on the client
- Observability logging is on by default

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/) v18 or newer
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/install-and-update/)
- A Cloudflare account with Workers AI access

### Install

1. Clone the repo:

   ```bash
   git clone https://github.com/Slagathore/llm-chat-app-template.git
   cd llm-chat-app-template
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Generate Worker type definitions:

   ```bash
   npm run cf-typegen
   ```

### Development

Start a local dev server:

```bash
npm run dev
```

It runs at http://localhost:8787.

Heads up: Workers AI hits your Cloudflare account even during local dev, so you get charged for usage.

### Deploy

```bash
npm run deploy
```

### Logs

Tail live logs from a deployed Worker:

```bash
npm wrangler tail
```

## Project structure

```
/
├── public/             # Static assets
│   ├── index.html      # Chat UI HTML
│   └── chat.js         # Chat UI frontend script
├── src/
│   ├── index.ts        # Main Worker entry point
│   └── types.ts        # TypeScript type definitions
├── test/               # Test files
├── wrangler.jsonc      # Cloudflare Worker configuration
├── tsconfig.json       # TypeScript configuration
└── README.md           # This documentation
```

## How it works

### Backend

A Cloudflare Worker that calls Workers AI to generate responses. The pieces:

1. API endpoint (`/api/chat`): takes POST requests with chat messages and streams the reply
2. Streaming over Server-Sent Events (SSE)
3. Workers AI binding connects to Cloudflare's AI service

### Frontend

Plain HTML/CSS/JS that:

1. Shows the chat interface
2. Sends messages to the API
3. Reads the streamed response as it arrives
4. Keeps chat history on the client

## Customization

### Change the model

Update the `MODEL_ID` constant in `src/index.ts`. Available models are listed in the [Cloudflare Workers AI docs](https://developers.cloudflare.com/workers-ai/models/).

### Use AI Gateway

There's commented gateway code in `src/index.ts`. To turn it on:

1. [Create an AI Gateway](https://dash.cloudflare.com/?to=/:account/ai/ai-gateway) in your Cloudflare dashboard
2. Uncomment the gateway config in `src/index.ts`
3. Replace `YOUR_GATEWAY_ID` with your gateway ID
4. Set the other options if you want them:
   - `skipCache`: `true` to bypass the gateway cache
   - `cacheTtl`: cache lifetime in seconds

More on [AI Gateway](https://developers.cloudflare.com/ai-gateway/).

### Change the system prompt

Edit the `SYSTEM_PROMPT` constant in `src/index.ts`.

### Styling

The CSS lives in the `<style>` block of `public/index.html`. Change the variables at the top to adjust the color scheme.

## Resources

- [Cloudflare Workers docs](https://developers.cloudflare.com/workers/)
- [Cloudflare Workers AI docs](https://developers.cloudflare.com/workers-ai/)
- [Workers AI models](https://developers.cloudflare.com/workers-ai/models/)
