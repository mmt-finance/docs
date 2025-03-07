# Claim Fees and Rewards

### Claim all fees and rewards

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
const userAddress = '0x...';

const pools = await sdk.Pool.getAllPools();
const tx = new Transaction(userAddress, pools);

await sdk.Pool.collectAllPoolsRewards(userAddress, pools);

// Execute transaction
const result = await client.signAndExecuteTransaction({ signer: keypair, transaction: tx });
const responce = await client.waitForTransaction({ digest: result.digest });
```
