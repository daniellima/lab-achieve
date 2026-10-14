# ADR 0002: Choose React and Vite for the frontend

- **Status:** Accepted
- **Date:** 2026-10-14

## Context

Achieve is a personal todo-list web app. Its frontend needs to be straightforward to develop and inexpensive to host, while remaining portable if hosting changes or the app is self-hosted in the future. The app is expected to be a browser-based interface and does not currently require server-side rendering.

The frontend framework and build tool are separate choices. A framework structures the user interface; a build tool provides the local development server and produces deployable assets.

## Options considered

### Frontend frameworks

#### React

React provides a component-based model for building interfaces. Its large ecosystem, broad adoption, and abundance of learning resources make it easy to find examples and libraries. It pairs well with TypeScript and Vite.

The trade-off is that React leaves some architectural choices to the app, and its ecosystem can offer many competing libraries and patterns.

#### Vue

Vue is a mature component framework with cohesive conventions and an approachable template syntax. It is a reasonable choice for this app and can offer a gentler learning curve.

The trade-off is that its ecosystem and pool of examples are smaller than React's, though still substantial.

#### Svelte

Svelte offers concise components and compiles much of its behavior into JavaScript during the build.

The trade-off is a smaller ecosystem and fewer examples and third-party integrations than React.

#### Angular

Angular provides a comprehensive framework with strong conventions and integrated tools.

The trade-off is additional structure and concepts that are likely unnecessary for a small personal todo app.

#### Vanilla JavaScript

Building without a UI framework avoids framework dependencies and concepts.

The trade-off is that the app must manage UI structure and updates itself as features such as lists, filters, and forms grow.

### Build and development tools

#### Vite

Vite provides a fast local development server and a production build that outputs static assets. It works with React and TypeScript and can deploy to static hosting such as Cloudflare Pages.

The trade-off is that Vite is a build tool, not a full-stack framework; server-rendering and server-side features would need a separate solution if they become requirements.

#### Parcel

Parcel offers a low-configuration development and build workflow.

The trade-off is that Vite was selected as the preferred default for this project; Parcel remains a suitable alternative if minimal configuration is the overriding priority.

#### Webpack

Webpack is mature and highly configurable, with a broad plugin ecosystem.

The trade-off is that its flexibility and configuration can add complexity for this app.

#### Rsbuild

Rsbuild is an app-oriented build tool with fast builds and built-in conventions.

The trade-off is that Vite is the chosen simpler fit for a small static frontend.

## Decision

Build the frontend with **React and TypeScript**, using **Vite** for local development and production builds. Deploy the resulting static assets to **Cloudflare Pages**, as described in [ADR 0001](./0001-cloud-hosting.md).

React is selected for its mature ecosystem, wide adoption, and learning resources, while recognizing that Vue or Svelte could also meet the app's needs. Vite is selected because the current app can be a static browser frontend and does not need server rendering.

## Consequences

### Benefits

- React components provide a clear way to organize the todo interface as it grows.
- TypeScript supports safer changes to UI and data structures.
- Vite offers a fast development workflow and produces static files that can be hosted independently of the development tool.
- Static assets can be moved between suitable cloud and self-hosted web servers without requiring a Vite-specific runtime.
- The common React and TypeScript ecosystem makes learning resources and integrations easy to find.

### Trade-offs and risks

- React's flexibility means the project should keep its component and state-management patterns deliberate rather than adding libraries prematurely.
- Vite does not provide server-side rendering, a backend, or hosting. If those become requirements, evaluate a framework or separate server-side service rather than expecting Vite to provide them.
- The frontend's static files are portable, but backend integrations and provider-specific SDK usage can still create migration work; keep backend-specific behavior deliberate.
