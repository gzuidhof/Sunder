# Sunder
[![CI](https://github.com/gzuidhof/Sunder/workflows/CI/badge.svg)](https://github.com/gzuidhof/Sunder/actions)
[![NPM badge](https://img.shields.io/npm/v/sunder)](https://www.npmjs.com/package/sunder)
[![Documentation](https://img.shields.io/badge/Read%20the-documentation-1abc9c.svg)](https://gzuidhof.github.io/Sunder/docs)

**Sunder** is a minimal server-side framework for ServiceWorker environments. It is intended for websites and APIs built on [Cloudflare Workers](https://workers.cloudflare.com/).

Think of it as Express or Koa for serverless which allows you to quickly build websites and APIs in a modern async structure. It is easy to write tests for.

<!-- Technologies it pairs with especially well:

* [Serverless worker environments](https://workers.cloudflare.com/).
* [ESBuild](https://esbuild.github.io/) for <50ms builds, allowing for very fast iteration.
* [lit-html](https://https://lit-html.polymer-project.org/) for both [serverside](https://github.com/popeindustries/lit-html-server) and clientside templating. This enables websites without a runtime/front-end framework.
* [Typescript](https://https://www.typescriptlang.org/). Sunder is especially strict when it comes to path parameters: parameters in routes are statically checked at build time!

All four of these are optional in your own project. -->

## Documentation
Read the documentation [**here**](https://gzuidhof.github.io/Sunder/docs).

## Installation
```bash
npm i --save sunder
# or
yarn add sunder
```

## Example

```typescript
import {Sunder, Router, Context} from "sunder";

const app = new Sunder();
const router = new Router();

// Example route with a named parameter
router.get("/hello/:username", ({response, params}) => {
    response.body = `Hello ${params.username}`;
    response.headers.set("content-type", "text/plain");
});

// Example middleware
app.use(async (ctx, next) => {
    const start = Date.now();
    await next();

    const ms = Date.now() - start;
    ctx.response.headers.set('X-Response-Time', `${ms}ms`);    
});
app.use(router.middleware);

addEventListener('fetch', (event) => {
    app.handleEvent(event);
});
```

## Highlight feature: strict route parameters
![Strict routes gif](https://i.imgur.com/XeOyoxF.gif)

## Inspiration

The Sunder framework was inspired by Elixir's [plug](https://github.com/elixir-plug/plug), Node's [Koa](https://koajs.com/) framework, [tiny-request-router]() and [cfworker](https://github.com/cfworker/cfworker)'s web package.

Sunder is only a few hundred lines of code. It has little magic and doesn't impose too much structure.

## License
MIT