# Talk Buddy

![CI](https://github.com/aayxshkumar/talk-buddy/actions/workflows/ci.yml/badge.svg)

A chat app where users message each other and can talk to bots backed by the OpenAI API. Chat rooms, users and real-time messaging come from [ChatEngine](https://chatengine.io); a small Express server handles sign-up and login against ChatEngine and forwards bot prompts to OpenAI, then posts the bot's reply back into the chat.

## Features

- Sign up and log in with a username and password (stored as ChatEngine users)
- Multi-chat window with real-time messages and file attachments
- The chat title prefix picks the bot that answers in that chat:
  - `Talk_` and `Analyze_`: general text bot (`/openai/text`)
  - `Code_`: code bot (`/openai/code`)
  - `Think_`: writing assistant that suggests how to finish the sentence you are typing (`/openai/assist`)
- Any other chat is a normal person-to-person chat

## Tech stack

- Client: React 18, Vite 4, Redux Toolkit Query, React Router 6, `react-chat-engine-advanced`, Sass
- Server: Node.js, Express 4, `openai` SDK v3, Axios, Helmet, Morgan

The server uses the v3 `openai` SDK with the `text-davinci-003` and `gpt-3.5-turbo` models. OpenAI has since retired `text-davinci-003`, so the text bot needs a model update before it will answer.

## Project structure

```
client/   React front end (Vite)
server/   Express API: /auth/login, /auth/signup, /openai/text, /openai/code, /openai/assist
```

## Getting started

Requires Node.js 22, a ChatEngine project and an OpenAI API key.

```bash
git clone https://github.com/aayxshkumar/talk-buddy.git
cd talk-buddy
```

Server:

```bash
cd server
cp .env.example .env   # then fill in the values
npm ci
npm run dev            # starts on PORT, default 9000
```

| Variable | Purpose |
| --- | --- |
| `PORT` | Port for the API (default 9000) |
| `OPEN_API_KEY` | OpenAI API key |
| `PROJECT_ID` | ChatEngine project ID |
| `PRIVATE_KEY` | ChatEngine private key, used to create users |
| `BOT_USER_NAME_1` | ChatEngine username that posts text-bot replies |
| `BOT_USER_NAME_2` | ChatEngine username that posts code-bot replies |
| `BOT_USER_SECRET` | Password for the bot users |

Client:

```bash
cd client
cp .env.example .env   # then fill in the values
npm ci
npm run dev
npm run lint
npm run build
```

| Variable | Purpose |
| --- | --- |
| `VITE_BASE_URL` | URL of the server (for example `http://localhost:9000`) |
| `VITE_PROJECT_ID` | ChatEngine project ID |

The client's `.npmrc` sets `legacy-peer-deps=true` because `react-chat-engine-advanced` declares a React 16/17 peer dependency while the app uses React 18.
