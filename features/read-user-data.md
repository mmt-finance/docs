# Read User Data

### Initialize SDK

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
```

### Read all user positions

```typescript
const userAddress = '0x...';
const positions = await sdk.Position.getAllUserPositions(userAddress);
```

### Read all user rewards (incentives and fees)

```typescript
const userAddress = '0x...';
const rewardsAndFees = await sdk.Position.fetchRewards(userAddress);
```

### Read user rewards for a position (incentives and fees)

```typescript
const positionId = '0x...';
const userAddress = '0x...';
const poolId = '0x...';

const pool = await sdk.poolModule.getPool(poolId);
const rewardsAndFees = await sdk.Position.fetchAllRewards(
  positionId,
  userAddress,
  pool,
);
```

### Read info from a position

```typescript
const positionId = '0x...';
const liquidity = await sdk.Position.getLiquidity(positionId);
const lowerTick = await sdk.Position.getTickLowerIndex(positionId);
const upperTick = await sdk.Position.getTickUpperIndex(positionId);
```

