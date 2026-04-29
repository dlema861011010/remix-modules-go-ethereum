# EVM (Ethereum Virtual Machine) Implementation Milestone

## Summary

This milestone tracks the Ethereum Virtual Machine (EVM) implementation within the Remix Project monorepo, specifically covering the `remix-simulator` and `remix-debug` libraries that provide EVM execution, state management, and transaction debugging capabilities.

## Libraries

### `remix-simulator` (`libs/remix-simulator`)

Provides a full in-browser/in-memory EVM simulator exposing a JSON-RPC provider interface compatible with ethers.js and web3.js.

#### Key Components

- **`vm-context.ts`** — Core EVM context management:
  - `VMContext` class: Manages EVM lifecycle, block tracking, logs, and fork state
  - `StateManagerCommonStorageDump`: Extended state manager with storage dump support
  - `CustomEthersStateManager`: Mainnet forking via JSON-RPC provider
  - `VMCommon`: Hardfork-locked common configuration
  - Supports hardforks: `berlin`, `london`, `shanghai`, `osaka`, and others
  - Blockchain override with `findCommonAncestor` and `_rebuildCanonical` for fork management

- **`VmProxy.ts`** — Proxy wrapping the EVM for opcode-level trace capture
- **`provider.ts`** — JSON-RPC provider entry point with full eth_ method support
- **`server.ts`** — HTTP server exposing the provider
- **`methods/`** — Handlers for each JSON-RPC method group (accounts, blocks, events, transactions, etc.)

#### Supported JSON-RPC Methods

| Category | Methods |
|---|---|
| Accounts | `eth_accounts`, `eth_requestAccounts`, `eth_getBalance`, `eth_sign`, `personal_sign`, `eth_signTypedData` |
| Blocks | `eth_blockNumber`, `eth_getBlockByNumber`, `eth_getBlockByHash`, `eth_getGasPrice`, `eth_coinbase`, `eth_getBlockTransactionCountByHash`, `eth_getBlockTransactionCountByNumber`, `eth_getUncleCountByBlockHash`, `eth_getUncleCountByBlockNumber`, `eth_getStorageAt`, `eth_call`, `evm_mine` |
| Events | `eth_getLogs` |
| Transactions | `eth_sendTransaction`, `eth_sendRawTransaction` (type 0 legacy, type 2 EIP-1559) |
| Misc | `web3_clientVersion`, `eth_protocolVersion`, `eth_syncing`, `eth_mining`, `eth_hashrate`, `web3_sha3`, `eth_getCompilers` |

#### EthereumJS Stack

| Package | Version |
|---|---|
| `@ethereumjs/vm` | ^10.1.1 |
| `@ethereumjs/evm` | ^10.1.1 |
| `@ethereumjs/block` | ^10.1.1 |
| `@ethereumjs/blockchain` | ^10.1.1 |
| `@ethereumjs/common` | ^10.1.1 |
| `@ethereumjs/statemanager` | ^10.1.1 |
| `@ethereumjs/tx` | ^10.1.1 |
| `@ethereumjs/util` | ^10.1.1 |

---

### `remix-debug` (`libs/remix-debug`)

Provides EVM transaction replay, trace analysis, and source mapping for Solidity debugging.

#### Key Components

- **`Ethdebugger.ts`** — Top-level debugger orchestrating trace, state decoding, and source mapping
- **`trace/`** — EVM execution trace manager (`traceManager`)
- **`debugger/`** — Step-through debugger with breakpoint support
- **`solidity-decoder/`** — Decodes contract storage and memory from EVM state
- **`source/`** — Source location tracker for mapping EVM opcodes to Solidity source lines
- **`storage/`** — Storage slot decoder for mappings, arrays, and structs
- **`code/`** — Bytecode disassembler and code manager

---

## Test Results

### `remix-simulator` Tests

Ran with: `npm test` in `libs/remix-simulator`

```
31 passing (13s)
```

| Suite | Tests | Status |
|---|---|---|
| Accounts (`eth_getAccounts`, `eth_getBalance`, `eth_sign`, `personal_sign`, `eth_signTypedData`) | 5 | ✅ All passing |
| Blocks (`eth_getBlockByNumber`, `evm_mine`, `eth_getStorageAt`, `eth_call`, ...) | 17 | ✅ All passing |
| Events (`eth_getLogs`) | 1 | ✅ All passing |
| Misc (`web3_clientVersion`, `eth_syncing`, `web3_sha3`, ...) | 5 | ✅ All passing |
| Transactions (`eth_sendTransaction`, legacy tx type 0, EIP-1559 tx type 2) | 3 | ✅ All passing |
| **Total** | **31** | **✅ 31/31** |

### `remix-debug` Tests

Ran with: `npm test` in `libs/remix-debug`

```
677 tests passing
```

| Suite | Status |
|---|---|
| TraceManager | ✅ Passing |
| CodeManager | ✅ Passing |
| Disassembler | ✅ Passing |
| Source Location Tracker | ✅ Passing |
| Source Mapping Decoder | ✅ Passing |
| Debugger | ✅ Passing |
| Storage Decoder | ✅ Passing |
| Breakpoint Manager | ✅ Passing |
| **Total** | **✅ 677/677** |

---

## Milestone Status

| Item | Status |
|---|---|
| EVM context with hardfork support (berlin → osaka) | ✅ Implemented |
| Mainnet forking via JSON-RPC (`CustomEthersStateManager`) | ✅ Implemented |
| Full JSON-RPC provider interface | ✅ Implemented |
| Legacy transaction support (type 0) | ✅ Implemented |
| EIP-1559 transaction support (type 2) | ✅ Implemented |
| EVM execution trace capture (`VmProxy`) | ✅ Implemented |
| Transaction debugger with step-through | ✅ Implemented |
| Solidity storage/memory decoder | ✅ Implemented |
| Source-level breakpoints | ✅ Implemented |
| Unit tests: remix-simulator | ✅ 31/31 passing |
| Unit tests: remix-debug | ✅ 677/677 passing |
