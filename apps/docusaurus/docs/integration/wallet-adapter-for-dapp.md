---
title: "For Dapps"
id: "wallet-adapter-for-dapp"
---

# Wallet Adapter For Dapp Builders

Imagine you have a great idea for a dapp and you want to start building it. Eventually, you will need to integrate a wallet or multiple wallets so your users can interact with the Aptos blockchain.
Implementing wallet integration can be difficult in supporting all edge cases, new features, unsupported functionality. And it can be even harder to support multiple wallets.

In addition, different wallets have different APIs, and not all wallets share the same naming convention. For example, maybe all wallets have a `connect` method, but not all wallets call that method `connect`; that can be tricky to support.

Luckily, Aptos built a wallet adapter, created and maintained by the Aptos team, to help you ramp up development and standardize where possible.

The Aptos Wallet Adapter provides:

- Easy wallet implementation - no need to implement and support code for multiple wallets.
- Support for different wallet APIs.
- Support for features not implemented on the wallet level.
- Detection for uninstalled wallets (so you can show users that a wallet is not installed).
- Auto-connect functionality and remembers the current wallet state.
- Listens to wallet events, such as account and network changes.
- A well-developed and maintained reference implementation by the Aptos ecosystem team.

## Install

Currently, the adapter supports a _React provider_ for you to include in your app.

Install wallet dependencies you want to include in your app. You can find a list of the wallets in the Aptos Wallet Adapter [README](https://github.com/aptos-labs/aptos-wallet-adapter#supported-wallet-packages).

Install the React provider:

```bash
npm install @aptos-labs/wallet-adapter-react
```

## Import dependencies

In the `App.jsx` file:

Import the installed wallets:

```js
import { PetraWallet } from "petra-plugin-wallet-adapter";
```

Import the `AptosWalletAdapterProvider`:

```js
import { AptosWalletAdapterProvider } from "@aptos-labs/wallet-adapter-react";
```

Wrap your app with the Provider, pass it the plugins (wallets) you want to have on your app as an array, and include an autoConnect option (set to false by default):

```js
const wallets = [new PetraWallet()];
<AptosWalletAdapterProvider plugins={wallets} autoConnect={true}>
  <App />
</AptosWalletAdapterProvider>;
```

### Use

On any page you want to use the wallet properties, import `useWallet` from `@aptos-labs/wallet-adapter-react`:

```js
import { useWallet } from "@aptos-labs/wallet-adapter-react";
```

You can then use the exported properties:

```js
const {
  connect,
  account,
  network,
  connected,
  disconnect,
  wallet,
  wallets,
  signAndSubmitTransaction,
  signTransaction,
  signMessage,
} = useWallet();
```

Finally, use the [examples](https://github.com/aptos-labs/aptos-wallet-adapter/tree/main/packages/wallet-adapter-react#examples) on the package README file to build more functionality into your dapps.

## AIP-62 Wallet Standard

The AIP-62 Wallet Standard is a chain-agnostic set of interfaces and conventions that aim to improve how applications interact with injected wallets. Read more about it [here](https://github.com/aptos-foundation/AIPs/blob/main/aips/aip-62.md).

### Why integrating with the AIP-62 wallet standard?

:::note
In the near future and as wallets onboard to the new standard, the wallet adapter will deprecate the legacy standard and keep only the AIP-62 wallet standard support.
:::

The AIP-62 wallet standard eliminates the current issues dapps have:

- The dapp detecting process logic can create a race condition risk in the case the dapp loads before a wallet and the dapp is not aware of the new wallets
- The legacy standard is deeply integrated within the Aptos wallet adapter, and any change can cause breaking changes for dApps and wallets, creating endless maintenance work by requiring a dApp or wallet to implement these changes.
- The legacy standard supports only the legacy TS SDK input, types, and logic. That means that it doesn't enjoy the features and enhancements of the new TS SDK. In addition, the legacy TS SDK does not receive any more support or new features.

In addition, the new wallet standard described in AIP-62 provides many benefits to dApps:

- The wallet adapter handles the maintenance and installation of wallet packages instead of the dApp which prevents potential supply chain attacks
- The new adapter version provides better validation and error handling support
- The new adapter version uses the new TS SDK which is more reliable, fast, and is actively maintained and updated with new features (the legacy sdk, i.e `npm i aptos`, is no longer actively maintained and developed)

### How to integrate with the AIP-62 wallet standard?

The latest version of the wallet adapter supports AIP-62 Wallet Standard integration by resolving wallets that support both standards.

Since the AIP-62 wallet standard uses event communication between a dapp and a wallet, the dapp does not need to install, maintain, and pass to the wallet adapter multiple wallet plugins (packages). Furthermore, for any wallet that compatible with the AIP-62 wallet standard, the wallet adapter will detect the wallet by default.

#### Support only AIP-62 wallet standard

:::tip
If you already use the wallet adapter with the legacy standard, uninstall and remove any AIP-62 wallet standard compatible wallet dependencies.
:::

To only support the AIP-62 wallet standard, dapp should not include the `plugins` prop in the React provider

```js
<AptosWalletAdapterProvider autoConnect={true}>
  <App />
</AptosWalletAdapterProvider>
```

Wallets compatible with AIP-62 wallet standard

- [Nightly](https://chromewebstore.google.com/detail/nightly/fiikommddbeccaoicoejoniammnalkfa)

#### Support both standards

To support both standards, dapp should provide a wallets array to the React provider. That way the adapter will resolve both wallet standards and provide to the end user with the dapp's installed wallets and with the AIP-62 wallet standard compatible wallets.

```js
const wallets = [new PetraWallet()];
<AptosWalletAdapterProvider plugins={wallets} autoConnect={true}>
  <App />
</AptosWalletAdapterProvider>;
```
