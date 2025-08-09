# Contract Event

## CLMM Events Documentation <a href="#clmm-events-documentation" id="clmm-events-documentation"></a>

This document describes all user-related events emitted by the CLMM contract. These events provide transparency and allow external systems to track user interactions with the protocol.

### Table of Contents <a href="#table-of-contents" id="table-of-contents"></a>

* [Pool Management Events](contract-event.md#pool-management-events)
* [Trading Events](contract-event.md#trading-events)
* [Liquidity Management Events](contract-event.md#liquidity-management-events)
* [Fee Collection Events](contract-event.md#fee-collection-events)
* [Flash Loan Events](contract-event.md#flash-loan-events)

***

### Pool Management Events <a href="#pool-management-events" id="pool-management-events"></a>

#### PoolCreatedEvent <a href="#poolcreatedevent" id="poolcreatedevent"></a>

Emitted when a new pool is created.

```move
public struct PoolCreatedEvent has copy, drop, store {
    sender: address,        // Address of the pool creator
    pool_id: ID,           // Unique identifier for the pool
    type_x: TypeName,      // Type name of token X
    type_y: TypeName,      // Type name of token Y
    fee_rate: u64,         // Fee rate for the pool (in basis points)
    tick_spacing: u32,     // Tick spacing for the pool
}
```

***

### Trading Events <a href="#trading-events" id="trading-events"></a>

#### SwapEvent <a href="#swapevent" id="swapevent"></a>

Emitted when a swap operation is executed.

```move
public struct SwapEvent has copy, drop, store {
    sender: address,           // Address of the trader
    pool_id: ID,              // ID of the pool being traded
    x_for_y: bool,            // True if swapping X for Y, false if Y for X
    amount_x: u64,            // Amount of token X involved in the swap
    amount_y: u64,            // Amount of token Y involved in the swap
    sqrt_price_before: u128,  // Square root price before the swap
    sqrt_price_after: u128,   // Square root price after the swap
    liquidity: u128,          // Current liquidity in the pool
    tick_index: i32::I32,     // Current tick index
    fee_amount: u64,          // Total fee amount charged
    protocol_fee: u64,        // Protocol fee amount
    reserve_x: u64,           // Reserve of token X after swap
    reserve_y: u64            // Reserve of token Y after swap
}
```

***

### Liquidity Management Events <a href="#liquidity-management-events" id="liquidity-management-events"></a>

#### OpenPositionEvent <a href="#openpositionevent" id="openpositionevent"></a>

Emitted when a new position is opened.

```move
public struct OpenPositionEvent has copy, drop, store {
    sender: address,          // Address of the position owner
    pool_id: ID,             // ID of the pool
    position_id: ID,         // Unique identifier for the position
    tick_lower_index: I32,   // Lower tick index of the position range
    tick_upper_index: I32,   // Upper tick index of the position range
}
```

#### ClosePositionEvent <a href="#closepositionevent" id="closepositionevent"></a>

Emitted when a position is closed.

```move
public struct ClosePositionEvent has copy, drop, store {
    sender: address,          // Address of the position owner
    position_id: ID,         // ID of the position being closed
}
```

#### AddLiquidityEvent <a href="#addliquidityevent" id="addliquidityevent"></a>

Emitted when liquidity is added to a position.

```move
public struct AddLiquidityEvent has copy, drop, store {
    sender: address,          // Address of the liquidity provider
    pool_id: ID,             // ID of the pool
    position_id: ID,         // ID of the position
    liquidity: u128,         // Amount of liquidity added
    amount_x: u64,           // Amount of token X added
    amount_y: u64,           // Amount of token Y added
    upper_tick_index: I32,   // Upper tick index of the position
    lower_tick_index: I32,   // Lower tick index of the position
    reserve_x: u64,          // Reserve of token X after adding liquidity
    reserve_y: u64           // Reserve of token Y after adding liquidity
}
```

#### RemoveLiquidityEvent <a href="#removeliquidityevent" id="removeliquidityevent"></a>

Emitted when liquidity is removed from a position.

```move
public struct RemoveLiquidityEvent has copy, drop, store {
    sender: address,          // Address of the liquidity provider
    pool_id: ID,             // ID of the pool
    position_id: ID,         // ID of the position
    liquidity: u128,         // Amount of liquidity removed
    amount_x: u64,           // Amount of token X removed
    amount_y: u64,           // Amount of token Y removed
    upper_tick_index: I32,   // Upper tick index of the position
    lower_tick_index: I32,   // Lower tick index of the position
    reserve_x: u64,          // Reserve of token X after removing liquidity
    reserve_y: u64           // Reserve of token Y after removing liquidity
}
```

***

### Fee Collection Events <a href="#fee-collection-events" id="fee-collection-events"></a>

#### FeeCollectedEvent <a href="#feecollectedevent" id="feecollectedevent"></a>

Emitted when fees are collected from a position.

```move
public struct FeeCollectedEvent has copy, drop, store {
    sender: address,          // Address of the fee collector
    pool_id: ID,             // ID of the pool
    position_id: ID,         // ID of the position
    amount_x: u64,           // Amount of token X fees collected
    amount_y: u64,           // Amount of token Y fees collected
}
```

#### CollectPoolRewardEvent <a href="#collectpoolrewardevent" id="collectpoolrewardevent"></a>

Emitted when pool rewards are collected.

```move
public struct CollectPoolRewardEvent has copy, drop, store {
    sender: address,          // Address of the reward collector
    pool_id: ID,             // ID of the pool
    position_id: ID,         // ID of the position
    reward_coin_type: TypeName, // Type of the reward token
    amount: u64,             // Amount of reward tokens collected
}
```

***

### Flash Loan Events <a href="#flash-loan-events" id="flash-loan-events"></a>

#### FlashLoanEvent <a href="#flashloanevent" id="flashloanevent"></a>

Emitted when a flash loan is initiated.

```move
public struct FlashLoanEvent has copy, drop, store {
    sender: address,          // Address of the flash loan borrower
    pool_id: ID,             // ID of the pool
    amount_x: u64,           // Amount of token X borrowed
    amount_y: u64,           // Amount of token Y borrowed
    reserve_x: u64,          // Reserve of token X after flash loan
    reserve_y: u64           // Reserve of token Y after flash loan
}
```

#### RepayFlashLoanEvent <a href="#repayflashloanevent" id="repayflashloanevent"></a>

Emitted when a flash loan is repaid.

```move
public struct RepayFlashLoanEvent has copy, drop, store {
    sender: address,          // Address of the flash loan borrower
    pool_id: ID,             // ID of the pool
    amount_x_debt: u64,      // Amount of token X debt
    amount_y_debt: u64,      // Amount of token Y debt
    actual_fee_paid_x: u64,  // Actual fee paid in token X
    actual_fee_paid_y: u64,  // Actual fee paid in token Y
    reserve_x: u64,          // Reserve of token X after repayment
    reserve_y: u64,          // Reserve of token Y after repayment
    fee_x: u64,              // Fee amount in token X
    fee_y: u64               // Fee amount in token Y
}
```

#### RepayFlashSwapEvent <a href="#repayflashswapevent" id="repayflashswapevent"></a>

Emitted when a flash swap is repaid.

```move
public struct RepayFlashSwapEvent has copy, drop, store {
    sender: address,          // Address of the flash swap borrower
    pool_id: ID,             // ID of the pool
    amount_x_debt: u64,      // Amount of token X debt
    amount_y_debt: u64,      // Amount of token Y debt
    paid_x: u64,             // Amount of token X paid back
    paid_y: u64,             // Amount of token Y paid back
    reserve_x: u64,          // Reserve of token X after repayment
    reserve_y: u64           // Reserve of token Y after repayment
}
```

\
