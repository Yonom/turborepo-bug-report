This is a bug report, explaining a regression that I experienced switching to Turbopack with NextJS.

## Repro

```sh
pnpm install
pnpm run dev --turbo
```

You will get the following error:

```
Module not found: Can't resolve '@/app/TestValue'
```

Running the same command without `--turbo` will work fine.

## Real world example

I use a shared `tsconfig.base.json` with path aliases like:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./*"],
      "package1": ["./packages/package1/src"],
      "package2": ["./packages/package2/src"]
    }
  }
}
```

Each project extends this base config and sets its own `baseUrl`:

```json
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

This setup works with `tsc`, but Turbopack fails to resolve paths like `@/app/TestValue`.
