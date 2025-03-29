# Getting Started

### Installation

```
npm install @mmt-finance/clmm-sdk
```

### SDK Initialization

Our SDK has pre-configured network settings that allows you to connect to MOMENTUM CLMM on both mainnet and testnet. Use `MmtSDK.NEW` method to swiftly initialize the configuration.

```
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
```

Now, you can start using MMT SDK.
