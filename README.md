# Achieve (Todo list app)
Project created to learn AI-native development flows.

Achieve is a todo list app that runs on browsers and offer a simple way to manage todos.

Documentation (including ADRs and requirements) can be found on context folder.

## Installing

The frontend uses **React**, **TypeScript**, and **Vite**. It builds to static
assets for deployment on **Cloudflare Pages**. See
[ADR 0002](context/adr/0002-frontend-stack.md) for the frontend decision and
[ADR 0001](context/adr/0001-cloud-hosting.md) for hosting.

Requirements: Node.js 20.19+ or 22.12+ and npm.

```sh
npm install
```

## How to run

Start the local development server:

```sh
npm run dev
```

Create a production build in `dist/`:

```sh
npm run build
```

Preview the production build locally:

```sh
npm run preview
```

## Authors

Daniel Lima (daniellima.pessoal at gmail dot com)