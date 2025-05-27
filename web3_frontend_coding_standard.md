# 🧑‍💻 Blockchain Frontend Coding Standard

---

## 🔐 1. Security & Environment Management

Never hardcode sensitive or environment-specific data. Use `.env` files for the following:

- Smart contract addresses
- Private/public keys
- API keys
- ABI files (or paths to them)
- Chain IDs
- RPC endpoints
- Network names

---

## 🔤 2. Naming Conventions

### Folders

- Use `kebab-case`  
  **Example:** `transaction-modal`, `wallet-hooks`

### Components

- Use `kebab-case` for filenames  
  **Example:** `transaction-modal.tsx`, `wallet-connect-button.tsx`

### Hooks

- Use `camelCase` prefixed with `use`  
  **Example:** `useWallet()`, `useContractRead()`
- Use `kebab-case` for hook filenames  
  **Example:** `read-contract.tsx`, `write-contract.tsx`

### Utils

- Use `camelCase` for function names  
  **Example:** `parseAddress()`, `formatEther()`
- Use `kebab-case` for file names  
  **Example:** `parse-address.ts`, `parse-ether.ts`

### Types & Interfaces

- Use `PascalCase`  
  **Example:** `User`, `TransactionStatus`

### Constants

- Use `UPPER_SNAKE_CASE`  
  **Example:** `MAX_GAS_LIMIT`, `CHAIN_ID_MAINNET`

### ABI Files

- Use `kebab-case` with `.json` extension  
  **Example:** `erc20-abi.json`, `vault-abi.json`

---

## 🧩 3. Input Validation

Use [Zod](https://zod.dev/) for validating:

- Form inputs
- Wallet data
- Smart contract method inputs

---

## 🔁 4. Contract Interaction Templates

Create reusable hooks for smart contract operations:

- `useContractRead()`
- `useContractWrite()`

Isolate blockchain logic inside these hooks using **wagmi** or **viem**.

---

## 💬 5. Transaction Feedback UI

Create a reusable **transaction modal**.

Use a hook like `useTransactionModal()` to manage modal lifecycle:

- `pending`
- `success`
- `error`

Use a modal (not toast) for better visibility and clarity.

---

## 🔄 6. Data Fetching Standard

Use **React Query** for all async operations:

- `useQuery` → read
- `useMutation` → write (pair with modal feedback)

Configure:

- `refetchInterval`
- `retry`
- `cacheTime`
- `staleTime`

---

## 🔠 7. Types and Interfaces

Use strict types/interfaces for:

- Props, component state
- API responses
- Smart contract schemas (ERC20, NFTs, etc.)

Use Zod inference to sync with TypeScript:

```ts
type MyType = z.infer<typeof mySchema>;
```

---

## 🧱 8. Modular Architecture

This architecture uses a **modular, scalable structure** inspired by feature-driven development. It separates global and feature-specific concerns, enhancing maintainability, reusability, and collaboration—ideal for Web3 or modern frontend applications.

```yaml
src/
│
├── app/                  # Application entry (providers, layout)
├── assets/               # Static files (images, fonts)
├── components/           # Shared components (buttons, modals)
├── hooks/                # Shared custom hooks
├── config/               # wagmi/viem config, env
├── lib/                  # Reusable libraries
├── features/             # Feature-based modules
│   ├── api/              # Feature-specific API
│   ├── assets/           # Feature-specific images/icons
│   ├── components/       # Feature-specific components
│   ├── hooks/            # Feature-specific hooks
│   ├── stores/           # Feature-specific state
│   ├── types/            # Feature-specific types
│   └── utils/            # Feature-specific utilities
├── utils/                # Shared utilities (unit conversions, etc.)
├── layout/               # Page layouts
├── stores/               # Global state (Zustand)
├── types/                # Shared types/interfaces
├── testing/              # Test utilities and mocks
└── themes/               # Shadcn UI themes
```

## 🧠 9. UX Components

## Custom Button Component

A reusable custom button component should support the following props:

- Add `isLoading`, `isDisabled`, and `variant` props
- Prevent double-click with disabled state while loading

---

## Wallet-aware Components

Conditionally render UI elements based on the wallet state:

- Show wallet address or disconnect button if connected
- Show connect wallet button if not connected

**Integration:**

- Use [RainbowKit](https://www.rainbowkit.com/) for wallet connection modal

---

## 🗂 10. State Management

Use [Zustand](https://zustand-demo.pmnd.rs/) for global state management.

### Recommended Store Slices:

- **Wallet Info**:

  - `address`, `balance`

- **Transaction Modal State**:

  - Show/hide modal
  - Transaction status

- **UI Preferences**:

  - `darkMode`, `language`, etc.

- **Smart Contract States (Cached)**:
  - Useful if not using `react-query`

---

## 🪙 11. Crypto Unit Utilities

- **Build a utility library (/utils/units.ts) to handle unit conversions using viem or ethers.js**:
  - wei ↔ gwei ↔ eth

---

## Maintainers
- **[Yamin Raad](https://github.com/Raad05)**
- **[Antonin Islam](https://github.com/antonin686)**
