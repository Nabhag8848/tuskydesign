# Tuskydesign Monorepo

This repository is managed with Nx and contains several improvements and features added over time. Below is a summary of the main work done, with direct references to the codebase:

## Summary of Work

### 🚦 Task Pipeline Creation

- **Centralized task configuration** in [`nx.json`](./nx.json):
  ```jsonc
  "targetDefaults": {
    "serve": { "dependsOn": ["build"] },
    "build": { "dependsOn": ["^build"], "outputs": ["{projectRoot}/dist"], "cache": true },
    "typecheck": { "cache": true },
    "lint": { "cache": true }
  }
  ```
- Ensures all projects share consistent build, serve, typecheck, and lint behavior.

### 🔄 Project Sync & Inferred Tasks

- **TypeScript project references and sync** via the [`@nx/js/typescript`](https://nx.dev/packages/js) plugin in [`nx.json`](./nx.json):
  ```jsonc
  "plugins": [
    {
      "plugin": "@nx/js/typescript",
      "options": { ... }
    }
  ]
  ```
- Keeps all `tsconfig.json` and `tsconfig.lib.json` files in sync and up-to-date with inferred dependencies:
  - Example: [`packages/names/tsconfig.lib.json`](./packages/names/tsconfig.lib.json)
  - Example: [`packages/animals/tsconfig.lib.json`](./packages/animals/tsconfig.lib.json)

### 🧰 Shared Utility Module

- **Reusable logic** in [`@tuskdesign/util`](./packages/util):
  - [`getRandomItem`](./packages/util/src/lib/util.ts):
    ```ts
    export function getRandomItem<T>(arr: T[]): T {
      return arr[Math.floor(Math.random() * arr.length)];
    }
    ```
  - Used by [`@tuskdesign/animals`](./packages/animals/animals.ts) and [`@tuskdesign/names`](./packages/names/names.ts) for random selection.

### 🚀 NPM Release Setup

- **Package structure ready for npm:**
  - Each package has its own [`package.json`](./packages/animals/package.json) with `main`, `module`, and `types` fields.
  - The root [`nx.json`](./nx.json) includes:
    ```jsonc
    "release": {
      "projects": ["packages/*"]
    }
    ```
- Enables easy publishing and versioning for all packages in `packages/*`.

### ☁️ Nx Cloud Integration

- **Distributed caching and CI/CD acceleration** via Nx Cloud:
  - Enabled by the `nxCloudId` in [`nx.json`](./nx.json):
    ```jsonc
    "nxCloudId": "..."
    ```
- Speeds up builds and tests by sharing computation results across machines.

---

## Getting Started

- Install dependencies:
  ```sh
  npm install
  ```
- Run builds for all packages:
  ```sh
  npx nx run-many -t build
  ```
- Explore Nx commands:
  ```sh
  npx nx --help
  ```

---

For more details, see individual package documentation or the Nx documentation at [nx.dev](https://nx.dev/).
