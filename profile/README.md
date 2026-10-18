# Async Swap Labs

**A new frontier for large swaps.** AsyncSwap is an asynchronous limit-order book built as a Uniswap v4 hook, made for institutional-size trades that no pool can absorb without slippage. A maker posts an order on-chain with an exact input and the minimum output they accept; the input is escrowed in the PoolManager. The order then finds its counterparty asynchronously: matched against other resting orders by coincidence of wants, filled from pool liquidity when that satisfies the limit, or settled by an independent filler who pays the maker's price and keeps the spread.

> **Status.** The contracts on Arc and Unichain are live test deployments. They are not audited and not ready for production use. Try the mechanism, find bugs, open issues; do not put in funds you cannot lose. The official launch is coming soon.

## How it works

- **Orders live on-chain.** Every order, reprice, match, fill and cancel is a contract event. The book is a Patricia radix tree of price levels in the hook's storage; the best opposing price is always found first.
- **Matching by coincidence of wants.** A new order that crosses the book is matched against up to eight resting orders in the same PoolManager unlock, swapping the two escrows directly with no protocol fee. Partial fills rest on the book.
- **Application-specific sequencing.** Each pool chooses how same-price orders are ordered: FIFO, or USDC priority bids that rank first and are donated to the liquidity providers active at the current tick. Bids order the book, not the block.
- **Pool liquidity as fallback, fillers as the last resort.** What the book cannot match is tried against the pool's own v4 liquidity; what remains can be settled by anyone through the Router at the maker's live minimum plus a 0.3% fee.
- **Proved, not promised.** Escrow, conservation, price-time priority and non-negativity are machine-checked in Lean 4 and re-proved on every protocol commit by the [FormalMind](https://formal.team) swarm.

## Where to go

| | |
| --- | --- |
| Trade | [app.asyncswap.org](https://app.asyncswap.org) |
| Docs | [docs.asyncswap.org](https://docs.asyncswap.org) |
| API | [api.asyncswap.org](https://api.asyncswap.org) · [OpenAPI](https://api.asyncswap.org/openapi.json) |
| For AI agents | [llms.txt](https://app.asyncswap.org/llms.txt) · [capabilities](https://app.asyncswap.org/capabilities.md) · [skills](https://app.asyncswap.org/.well-known/agent-skills/index.json) · [MCP](https://app.asyncswap.org/.well-known/mcp/server-card.json) |
| Whitepaper | [Async Swap: Coincidence-of-Wants Matching on Uniswap v4](https://github.com/asyncswap/whitepaper) |
| Follow | [@asyncswap](https://x.com/asyncswap) |

## Repositories

**Protocol**

- [v2](https://github.com/asyncswap/v2): the new protocol: hook, Router, on-chain order book and matching engine (Solidity, Foundry). Live on Arc.
- [protocol](https://github.com/asyncswap/protocol): the original Async Swap contracts. Live on Unichain.
- [v2-proofs](https://github.com/asyncswap/v2-proofs): Lean 4 + Mathlib specification and machine-checked proofs, maintained by the FormalMind swarm.
- [whitepaper](https://github.com/asyncswap/whitepaper): Async Swap: Coincidence-of-Wants Matching on Uniswap v4 (work in progress).

**Tooling we built along the way**

- [solidity-language-server](https://github.com/asyncswap/solidity-language-server): a fast Solidity language server powered by solc and Foundry, with AI agents in mind.
- [skills](https://github.com/asyncswap/skills): Solidity LSP skills for coding agents.
- [lsp-bench](https://github.com/asyncswap/lsp-bench): a benchmarking framework for language servers.
- [eth-libs](https://github.com/asyncswap/eth-libs): Ethereum client RPC APIs in TypeScript: execution, engine, Flashbots relay, BuilderNet.
- [uv4-py](https://github.com/asyncswap/uv4-py): Uniswap v4 utilities in Python.
- [EthereumKit](https://github.com/asyncswap/EthereumKit): an Ethereum package for iOS.

## Deployments (test)

| Chain | Chain id | Hook | Router |
| --- | --- | --- | --- |
| Arc (new protocol) | 5042 | `0x911947FB822261F19Cb644191244bef29CAE6888` | `0x36f8e4a87c815Fd9Ae4112788DFF6EE64A7193f8` |
| Unichain (original) | 130 | `0x917b2e7C352af82c35C7a4477d7D055C54E76888` | `0x88A76b0991016754425093C8fB440744c5574b75` |

Always read configured addresses and fees from the contracts and from [app.asyncswap.org/llms-full.txt](https://app.asyncswap.org/llms-full.txt); deployments may be replaced before launch.

Async Swap Labs, Inc. is based in the United States and is an alumnus of the Atrium Academy Uniswap hook incubator, supported by the Uniswap Foundation.
