# Examples

A comprehensive set of examples has been provided to demonstrate the fundamental usage of the SDK. Please refer to the [detailed guidelines](https://github.com/mmt-finance/clmm-sdk/tree/main/examples) for further information&#x20;

{% embed url="https://github.com/mmt-finance/clmm-sdk/blob/main/examples/README.md" %}

A set of typescript scripts are provided to walk through the basic usage of @mmt-finance/clmm-sdk.

#### Running an example

Install dependencies

```typescript
yarn
```

Run a specific example (E.g. swap.ts)

```typescript
yarn scripts examples/swap.ts
```

#### Execute transaction

By default, transaction is evaluated in dryRun mode. To execute transaction, update the script as follows:

```typescript
const mnemonic = ''; // Replace mnemonic here
const signer = Ed25519Keypair.deriveKeypair(mnemonic); // Define the user's mnemonic (should be replaced with an actual mnemonic)
const resp = await executeTxExample({
  tx,
  sdk,
  execution: { dryrun: false, signer: signer },
});
```

#### Set network to Mainnet

Configure SDK to run on Mainnet network:

```typescript
const sdk = MmtSDK.NEW({ network: 'mainnet' });
```

\
