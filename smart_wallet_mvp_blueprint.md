# Smart Wallet MVP Blueprint

This blueprint defines a shippable first version of a self-custody EVM wallet while keeping the architecture ready for ERC-4337 smart accounts, sponsored gas, multisig, and social recovery.

## Product goal

Build a mobile-first crypto wallet that supports everyday wallet actions first, then progressively adds programmable-account features without forcing a future rewrite.

The MVP should prioritize:

- Fast wallet creation or import.
- Clear send and receive flows.
- Multi-chain EVM support.
- Dapp connection support.
- Token and NFT visibility.
- A smart-account-ready service boundary for later account abstraction.

## Recommended first release

The first release should be a React Native mobile app targeting iOS and Android, with EVM chains as the initial network family. The app should support externally owned accounts at launch while isolating account operations behind interfaces that can later route through ERC-4337 bundlers and paymasters.

### MVP scope

| Area | Included in MVP | Deferred |
| --- | --- | --- |
| Wallet setup | Create wallet, import wallet, backup prompts, local secure storage | Email/social recovery automation |
| Networks | Ethereum mainnet plus configurable EVM networks | Non-EVM chains |
| Assets | Native coin balances, ERC-20 token import, basic NFT gallery | Advanced portfolio analytics |
| Transactions | Send, receive, gas controls, activity history | Batch transactions and automation rules |
| Dapp access | WalletConnect-compatible session approval and signing screens | Browser extension support |
| Identity | ENS forward and reverse lookup where supported | Full profile/social graph layer |
| Smart accounts | Account abstraction interfaces, capability detection, placeholder policy engine | Production guardians, multisig, paymaster sponsorship |
| Swaps | UI stub and provider adapter boundary | Live aggregator routing and execution |
| Fiat | Provider-ready integration boundary | Production onramp/offramp compliance flow |

## Architecture

Use layered modules so wallet custody, network calls, signing, and smart-account routing stay replaceable.

```text
app/
  screens/              Mobile screens and navigation
  components/           Reusable wallet UI components
  features/
    onboarding/         Create/import/backup flows
    portfolio/          Token, NFT, and activity views
    send/               Transaction composition and confirmation
    receive/            Address and QR presentation
    dapp-connect/       Dapp session approval and signing prompts
    settings/           Networks, security, contacts, developer options
  wallet/
    accounts/           Account models and account registry
    custody/            Seed/private-key storage adapters
    signing/            Message and transaction signing interfaces
    smart-accounts/     ERC-4337-ready account operation abstractions
  chains/
    evm/                RPC clients, chain metadata, gas estimation
    registry/           Built-in chains and custom RPC imports
  services/
    balances/           Native and token balance fetching
    tokens/             Token metadata and import logic
    nfts/               NFT metadata fetching
    ens/                ENS resolution
    swaps/              Aggregator adapter boundary
    fiat/               Onramp/offramp provider boundary
```

## Smart-account-ready design

The app should not hard-code the assumption that every account submits raw EVM transactions directly. Instead, every send or signing action should pass through an account controller that can choose the correct execution path.

### Account execution modes

| Mode | Launch behavior | Future behavior |
| --- | --- | --- |
| EOA direct | Sign and broadcast a standard transaction | Continue supporting imported seed/private-key wallets |
| Smart account | Expose interfaces and capability flags | Build user operations, estimate through bundlers, request paymaster sponsorship |
| Multisig | Stub policy and approval states | Collect multiple approvals before execution |
| Social recovery | Stub guardian metadata model | Add recovery proposals, guardian approvals, and account key rotation |

### Core interfaces

The wallet layer should expose these stable interfaces from day one:

- `AccountController`: resolves the active account and its capabilities.
- `Signer`: signs typed data, personal messages, and transactions.
- `TransactionPlanner`: builds a user-readable transaction preview.
- `ExecutionAdapter`: submits either a raw transaction or a smart-account user operation.
- `PolicyEngine`: evaluates future spending limits, guardians, multisig rules, and sponsorship eligibility.

## Implementation phases

### Phase 1: MVP wallet

- Scaffold React Native app and navigation.
- Implement create/import wallet flows.
- Store secrets with platform secure storage.
- Add built-in EVM networks and custom RPC import.
- Show balances, token import, receive QR, send flow, and activity history.
- Add WalletConnect-compatible session management and signing approvals.
- Add ENS resolution.
- Add smart-account interfaces without enabling production ERC-4337 execution.

### Phase 2: Portfolio and transaction expansion

- Improve NFT gallery and token metadata.
- Add swap provider integration through the existing swap adapter.
- Add contact book and address risk warnings.
- Add hardware wallet adapter boundary.
- Improve transaction simulation and decoded contract calls.

### Phase 3: Smart account features

- Add ERC-4337 account factory support.
- Add bundler and paymaster provider configuration.
- Implement sponsored gas eligibility checks.
- Add guardian-based social recovery.
- Add multisig policies and approval UX.
- Add account upgrade flow.

### Phase 4: Fiat rails

- Add hosted onramp provider flow.
- Add hosted offramp provider flow.
- Add region and compliance gating.
- Add transaction status tracking for fiat orders.

## Initial technical choices

| Decision | Recommendation | Reason |
| --- | --- | --- |
| Client | React Native | Mobile-first delivery for iOS and Android from one codebase |
| Chains | EVM first | Matches ERC-4337, ENS, WalletConnect, and broad dapp compatibility |
| Account model | Hybrid EOA now, smart-account-ready later | Ships faster while avoiding an architectural dead end |
| Dapp connectivity | WalletConnect-compatible protocol layer | Broad dapp ecosystem support |
| Fiat | Hosted provider integration later | Reduces compliance and custody complexity for MVP |

## Open product decisions

Before implementation starts, the product owner should confirm:

1. Launch platforms: iOS, Android, web, or all three.
2. Launch chains: Ethereum only or Ethereum plus Base, Arbitrum, Optimism, Polygon, and BNB Chain.
3. Login model: seed phrase, passkey, email/social plus smart wallet, or hybrid.
4. Brand direction: MetaMask-like, Rainbow-like, Coinbase Wallet-like, or gaming-native.
5. First release scope: MVP only, or MVP plus swaps and NFT gallery.

## Key risks

- **Security:** Key storage, transaction signing, and recovery flows require security review before production.
- **Compliance:** Fiat onramp/offramp features need provider, region, sanctions, and KYC constraints.
- **Reliability:** RPC providers, indexers, NFT metadata services, and dapp sessions need fallback strategies.
- **User safety:** Transaction previews should decode approvals, spending allowances, contract interactions, and phishing risks.
- **Scope creep:** Smart account functionality should be built behind MVP-ready interfaces, not fully shipped in the first release.

## Definition of done for MVP

The MVP is ready when a user can:

- Create or import a wallet.
- Back up the wallet safely.
- View native and imported token balances.
- Add a custom EVM RPC network.
- Receive funds via address and QR code.
- Send a native token transaction with gas controls.
- Review transaction history.
- Connect to a dapp session and approve or reject signing requests.
- Resolve ENS names where available.
- Use the app without the UI depending on future smart-account features.
