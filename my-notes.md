- don't run tests from top level
  - instead, go to e.g. src/examples/getting-started and then run `npm test`
- to avoid importing `{it, expect}` etc in every test file, in `vitest.config.ts` add:

```typescript
export default defineConfig({
//...
  test: {
//...
    globals: true
  }
/...
})
```

## Github actions

- steps run sequentially, jobs and workflows run in parallel
- example of test and build jobs running in parallel (with caching)

```yml
name: Build and Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  unit-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm'
      - run: npm ci
      - run: npm test

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm'
      - run: npm ci
      - run: npm run build
```
