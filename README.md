# Nedos

A compact, high-performance and full-featured web framework based on nodejs. The
features include:

- [x] Application context
- [x] Middlewares like Koa.js
- [x] Built-in static service
- [x] Built-in routing service
- [x] Built-in template engine

## Installation

```
npm i nedos
```

## Get Started

```js
const app = require("nedos");

app
  .static("/assets")
  .engine({
    root: "template",
    imports: { globalName: "" },
  })
  .on("error", (err, ctx) => {
    console.error(err);
    ctx.body = {
      status: ctx.status,
      message: ctx.body,
    };
  })
  .use(async (ctx, next) => {
    ctx.custom = "xxxx";
    await next();
  })
  .get("/", async (ctx) => {
    console.log(ctx.query);
    console.log(ctx.params);
  })
  .listen();
```

## API Reference

### Methods

- `app.engine(options)` Enable template engine with options `{ root, imports }`.
- `app.static(path)` Serve static resources with the given `path`.
- `app.on("error", function)` Custom unified error handling.
- `app.use(function)` Add a middleware like koa.js.
- `app.get(path, [tmpl,] function)` Add dynamic route including `post`, `put`,
  `delete` and other standard request methods, it will auto-render template if
  `tmpl` parameter exists.
- `app.listen([port])` Create and start an application server on the specified
  port.
- `app.callback()` Return a request handler for node's native http server.

### Context

#### Properties

- `ctx.params` Get params in route path, wildcard supports
- `ctx.query` Get params in query string
- `ctx.method` Get request method
- `ctx.path` Get request path
- `ctx.url` Get request full href
- `ctx.protocol` Get request protocol
- `ctx.host` Get request host
- `ctx.hostname` Get request hostname
- `ctx.origin` Get request origin
- `ctx.headers` Get headers object
- `ctx.cookies` Get cookies object
- `ctx.status` Get response status code
- `ctx.body` Get response body
- `ctx.request` Get native request
- `ctx.response` Get native response
- `ctx.status=` Set response status code
- `ctx.body=` Set response body

#### Methods

- `ctx.get(name)` Get request headers by name
- `ctx.set(name, value)` Set response headers
- `ctx.cookie(name, value[, options])` Set cookies
- `async ctx.json()` Get request body in json
- `async ctx.text()` Get request body in text
- `async ctx.buffer()` Get request body in buffer
- `ctx.redirect(url[, status])` Redirect url with status default 301
- `ctx.view(path, data)` Render template file, only if engine enabled.
- `ctx.render(path, data)` Render template text, only if engine enabled.
- `ctx.throw(message, status)` Throw an error with status code

### Route Syntax

- `/static` static route
- `/*` Wildcard route, it will return `wildcard` variable in `ctx.params`
- `/:user`
- `/:user?`
- `/:user(\\d+)`

### Template Syntax

See https://github.com/metadream/tmplet
