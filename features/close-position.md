# Close Position

### Close Position

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';
import { Transaction } from '@mysten/sui/transactions';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});

// Prepare data
const poolParams = { objectId: poolId, tokenXType, tokenYType };
const liquidity = await sdk.Position.getLiquidity(positionId);
const pool = await sdk.poolModule.getPool(poolId);
const rewarders = pool.rewarders;

const tx = new Transaction();

// Remove liquidity
await sdk.Pool.removeLiquidity(
  tx, 
  poolParams, 
  positionId, 
  liquidity, 
  min_amount_x, // Compute based on slippage settings
  min_amount_y, // Compute based on slippage settings
  userAddress,
);

// Claim rewards and fees
if (rewarders) {
  await sdk.Pool.collectAllRewards(tx, poolParams, rewarders, positionId, userAddress);
}
sdk.Pool.collectFee(tx, poolParams, positionId, userAddress);

// Close position
await sdk.Position.closePosition(tx, positionId);

// Execute transaction
const result = await client.signAndExecuteTransaction({ signer: keypair, transaction: tx });
const responce = await client.waitForTransaction({ digest: result.digest });
```
