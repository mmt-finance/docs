# Open and Add Liquidity

### Initialize liquidity

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
```

### Open position and add liquidity

```typescript
import { TickMath } from '@mmt-finance/clmm-sdk';
import { Transaction } from '@mysten/sui/transactions';

const tx = new Transaction();
// Optional merge coins
tx.mergeCoins(...);

// Compute lower and upper price
const lowerSqrtPrice = TickMath.priceToSqrtPriceX64(new Decimal(lowerPrice), decimalX, decimalY);
const upperSqrtPrice = TickMath.priceToSqrtPriceX64(new Decimal(upperPrice), decimalX, decimalY);

// Open position
const position = positionModule.openPosition(
  tx,
  {
    objectId: poolId,
    tokenXType: coinAType,
    tokenYType: coinBType,
    tickSpacing: tickSpacing,
  },
  lowerSqrtPrice.toString(),
  upperSqrtPrice.toString(),
);

// Add liquidity
poolModule.addLiquidity(
  tx,
  {
    objectId: poolId,
    tokenXType: coinAType,
    tokenYType: coinBType,
    tickSpacing: tickSpacing,
  },
  position, // Position from previous tx
  coinA,
  coinB,
  BigInt(minAddAmountA), // Min a added
  BigInt(minAddAmountB), // Min b added
  senderAddress,
);

// Transfer position back to sender
tx.transferObjects([position], senderAddress);

// Execute transaction
const result = await client.signAndExecuteTransaction({ signer: keypair, transaction: tx });
const responce = await client.waitForTransaction({ digest: result.digest });
```
