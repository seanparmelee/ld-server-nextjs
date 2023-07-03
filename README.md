This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

This project reproduces an error that occurs when trying to use `@launchdarkly/node-server-sdk` in a Next.js application.

### To reproduce the issue
Run the following commands:

```
npm install
npm run build
```

Which should result in

```
Failed to compile.

./lib/ld-server.ts
Module not found: Default condition should be last one

https://nextjs.org/docs/messages/module-not-found

Import trace for requested module:
./pages/index.tsx
```

### To resolve the issue
Open `node_modules/@launchdarkly/node-server-sdk/package.json` and for each entry in the `exports` field, move the `default` entry to the end.

For example, update:
```json
  "exports": {
    ".": {
      "default": "./dist/src/index.js",
      "types": "./dist/src/index.d.ts"
    },
    "./integrations": {
      "default": "./dist/src/integrations.js",
      "types": "./dist/src/integrations.d.ts"
    }
  },
```
to
```json
  "exports": {
    ".": {
      "types": "./dist/src/index.d.ts",
      "default": "./dist/src/index.js"
    },
    "./integrations": {
      "types": "./dist/src/integrations.d.ts",
      "default": "./dist/src/integrations.js"
    }
  },
```

and then run
```
npm run build
```
