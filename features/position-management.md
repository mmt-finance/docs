# Position Management

### Add Liquidity

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';
import { Transaction } from '@mysten/sui/transactions';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});

const poolParams = { objectId: poolId, tokenXType, tokenYType };

const tx = new Transaction();
// Optionally merge coins
tx.mergeCoins(...);

sdk.Pool.addLiquidity(
  tx, 
  poolParams,
  positionId, 
  coinX, 
  coinY, 
  minX, // Min X amount based on slippage settings
  minY, // Min Y amount based on slippage settings
  userAddress,
);
// Execute transaction
const result = await client.signAndExecuteTransaction({ signer: keypair, transaction: tx });
const responce = await client.waitForTransaction({ digest: result.digest });
```

### Remove liquidity

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';
import { Transaction } from '@mysten/sui/transactions';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});

const poolParams = { objectId: poolId, tokenXType, tokenYType };
const liquidity = await sdk.Position.getLiquidity(positionId);

const tx = new Transaction();

sdk.Pool.removeLiquidity(
  tx, 
  poolParams, 
  positionId, 
  removeLiquidity, // < liquidity
  minX, // Min X amount based on slippage settings
  minY, // Min Y amount based on slippage settings
  userAddress,
);
// Execute transaction
const result = await client.signAndExecuteTransaction({ signer: keypair, transaction: tx });
const responce = await client.waitForTransaction({ digest: result.digest });
```
