# Open and Add Liquidity

### Initialize liquidity

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
```

### Open position and add liquidity(The number of coinX is fixed)

```typescript
import { TickMath } from '@mmt-finance/clmm-sdk';
import { Transaction } from '@mysten/sui/transactions';

const tx = new Transaction();

// Compute lower and upper price
const lowerSqrtPrice = TickMath.priceToSqrtPriceX64(new Decimal(lowerPrice), decimalX, decimalY);
const upperSqrtPrice = TickMath.priceToSqrtPriceX64(new Decimal(upperPrice), decimalX, decimalY);

// Estimate liquidity using the given price range and amount
const liquidity = estimateLiquidityForCoinA(upperSqrtPrice, lowerSqrtPrice, new BN(amontX));

// Calculate the required coin amounts based on estimated liquidity
const coinAmounts = getCoinAmountFromLiquidity(
  liquidity,
  new BN(currentSqrtPrice),
  lowerSqrtPrice,
  upperSqrtPrice,
  false,
);

// Get the coinX split coin
const coinA = prepareSplitCoin(coinAmounts.coinX)

// Get the coinY split coin
const coinB = prepareSplitCoin(coinAmounts.coinY)

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

```typescript
export function estimateLiquidityForCoinA(
  sqrtPriceX: BN,  // Square root price of token X
  sqrtPriceY: BN,  // Square root price of token Y
  coinAmount: BN   // Amount of token A (in BN)
) {
  // Determine the lower and upper square root price boundaries
  const lowerSqrtPriceX64 = BN.min(sqrtPriceX, sqrtPriceY);
  const upperSqrtPriceX64 = BN.max(sqrtPriceX, sqrtPriceY);

  // Compute the numerator: coinAmount * upperPrice * lowerPrice
  const num = MathUtil.fromX64_BN(coinAmount.mul(upperSqrtPriceX64).mul(lowerSqrtPriceX64));

  // Compute the denominator: upperPrice - lowerPrice
  const dem = upperSqrtPriceX64.sub(lowerSqrtPriceX64);

  // Return the estimated liquidity
  return num.div(dem);
}

```

```typescript
export function getCoinAmountFromLiquidity(
  liquidity: BN,        // Total liquidity provided
  curSqrtPrice: BN,      // Current square root price of the pool
  lowerSqrtPrice: BN,    // Lower bound of the price range
  upperSqrtPrice: BN,    // Upper bound of the price range
  roundUp: boolean,      // Whether to round up the result
): {
  coinX: string;         // Amount of token X
  coinY: string;         // Amount of token Y
} {
  // Convert BN values to Decimal for precise calculations
  const liq = new Decimal(liquidity.toString());
  const curSqrtPriceStr = new Decimal(curSqrtPrice.toString());
  const lowerPriceStr = new Decimal(lowerSqrtPrice.toString());
  const upperPriceStr = new Decimal(upperSqrtPrice.toString());

  let coinX;
  let coinY;

  // Case 1: Current price is below the lower price, only token X is required
  if (curSqrtPrice.lt(lowerSqrtPrice)) {
    coinX = MathUtil.toX64_Decimal(liq)
      .mul(upperPriceStr.sub(lowerPriceStr))
      .div(lowerPriceStr.mul(upperPriceStr));
    coinY = new Decimal(0); // No token Y is needed

  // Case 2: Current price is within the range, both token X and token Y are required
  } else if (curSqrtPrice.lt(upperSqrtPrice)) {
    coinX = MathUtil.toX64_Decimal(liq)
      .mul(upperPriceStr.sub(curSqrtPriceStr))
      .div(curSqrtPriceStr.mul(upperPriceStr));

    coinY = MathUtil.fromX64_Decimal(liq.mul(curSqrtPriceStr.sub(lowerPriceStr)));

  // Case 3: Current price is above the upper price, only token Y is required
  } else {
    coinX = new Decimal(0); // No token X is needed
    coinY = MathUtil.fromX64_Decimal(liq.mul(upperPriceStr.sub(lowerPriceStr)));
  }

  // Round up or round down based on the parameter
  if (roundUp) {
    return {
      coinX: coinX.ceil().toString(),
      coinY: coinY.ceil().toString(),
    };
  }
  return {
    coinX: coinX.floor().toString(),
    coinY: coinY.floor().toString(),
  };
}

```
