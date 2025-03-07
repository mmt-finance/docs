# Read Pool Data

### Initialize SDK

```typescript
import { MmtSDK } from '@mmt-finance/clmm-sdk';

const sdk = MmtSDK.NEW({
  network: 'mainnet',
});
```

### Read all available pools

```typescript
const pools = await sdk.Pool.getAllPools();
```

### Read info for a certain pool

```typescript
const poolId = '0x...';
const poolInfo = await sdk.Pool.getPool(poolId);
```

### Retrieve tick info for a certain pool

```typescript
const poolId = '0x...';
const tickInfo = await sdk.Pool.fetchAllTickLiquidities(poolId);
```

### Fetch incentive rewards for a certain pool

```typescript
const poolId = '0x...';
const rewarderInfo = await sdk.Pool.fetchRewardersApy(poolId);
```

### Retrieve all supported tokens

```typescript
const tokens = await sdk.Pool.getAllTokens();
```
