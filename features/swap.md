# Swap

### Initialize SDK

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
```

### Execute swap (x to y)

If swapping token from `TokenXType` to `TokenYType`  for a certain pool

```typescript
import { Transaction } from '@mysten/sui/transactions';
import { TickMath } from '@mmt-finance/clmm-sdk';

const tx = new Transaction();
const userAddress = '0x...';

// Optional, prepare the coin to be transferred
tx.mergeCoins(...);

// Prepare the swap data
const limitSqrtPrice = TickMath.priceToSqrtPriceX64(
  new Decimal(priceLimit), // Price limit after slippage
  decimalX, // decimal of token X 
  decimalY, // deciaml of token Y
)
const swapAmount = BigInt(...) // Number of token X to be swapped

// Execute swap in tx
sdk.Pool.swap(
  tx,
  {
    objectId: poolId,
    tokenXType,
    tokenYType,
    tickSpacing,
  },
  swapAmount,
  coin, // Prepared coin in previous tx
  true,
  userAddress,
  limitSqrtPrice,
)

// Execute the transaction
const res = await suiClient.signAndExecuteTransaction({
  signer,
  transaction: tx,
});
const fin = await suiClient.waitForTransaction({ digest: res.digest });

```

### Execute swap (y to x)

```typescript
import { Transaction } from '@mysten/sui/transactions';
import { TickMath } from '@mmt-finance/clmm-sdk';

const tx = new Transaction();
const userAddress = '0x...';

// Prepare the coin to be transferred
const [coin] = tx.splitCoins(...);

// Prepare the swap data
const limitSqrtPrice = TickMath.priceToSqrtPriceX64(
  new Decimal(priceLimit), // Price limit after slippage
  decimalX, // decimal of token Y 
  decimalY, // deciaml of token X
)
const swapAmount = BigInt(...) // Number of token X to be swapped

// Execute swap in tx
sdk.Pool.swap(
  tx,
  {
    objectId: poolId,
    tokenXType,
    tokenYType,
    tickSpacing,
  },
  swapAmount,
  coin, // Prepared coin in previous tx
  false,
  userAddress,
  limitSqrtPrice,
)

// Execute the transaction
const res = await suiClient.signAndExecuteTransaction({
  signer,
  transaction: tx,
});
const fin = await suiClient.waitForTransaction({ digest: res.digest });
```
