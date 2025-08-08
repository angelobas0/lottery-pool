# Lottery Pool - Smart Contract Documentation

## Overview
Lottery Pool is a no-loss lottery protocol where users deposit funds to buy tickets, deposits generate yield, and winners receive prizes while all participants get their deposits back.

## Problem Solved
- **Traditional Lottery Loss**: Players keep deposits regardless of outcome
- **Low Savings Incentive**: Gamifies saving with prize potential
- **Unfair Odds**: Transparent on-chain randomness
- **Yield Optimization**: Deposits generate returns for prizes

## Key Features

### Core Functionality
- No-loss guarantee (get deposits back)
- Yield-generated prizes
- Fair randomness using block hashes
- Multiple tickets per user
- Automatic prize calculation

### Lottery Mechanics
- Fixed ticket price
- Maximum tickets per user
- Time-based pools
- Guaranteed winner selection
- Separate deposit withdrawal

## Contract Functions

### Pool Operations

#### `create-pool`
- **Parameters**: duration
- **Returns**: pool-id
- **Access**: Owner only

#### `buy-tickets`
- **Parameters**: pool-id, ticket-count
- **Returns**: ticket numbers
- **Effect**: Deposits funds, assigns tickets

#### `draw-winner`
- **Parameters**: pool-id
- **Returns**: winner address
- **Effect**: Selects winner, allocates prize

#### `claim-prize`
- **Parameters**: pool-id
- **Effect**: Winner claims prize
- **Requirements**: Must be winner

#### `withdraw-deposit`
- **Parameters**: pool-id
- **Effect**: Withdraw original deposit
- **Requirements**: Pool completed

### Admin Functions
- `set-ticket-price`: Adjust ticket cost
- `set-max-tickets`: Limit per user
- `set-yield-rate`: Prize generation rate
- `toggle-emergency-pause`: Emergency control

### Read Functions
- `get-pool`: Pool details
- `get-user-tickets`: User's tickets
- `get-user-stats`: Historical stats
- `calculate-pool-prize`: Current prize estimate
- `get-current-pool`: Active pool ID

## Usage Flow

```clarity
;; 1. Create lottery pool (7 days)
(contract-call? .lottery-pool create-pool u10080)

;; 2. Buy tickets (5 tickets)
(contract-call? .lottery-pool buy-tickets u1 u5)

;; 3. Draw winner after period ends
(contract-call? .lottery-pool draw-winner u1)

;; 4. Winner claims prize
(contract-call? .lottery-pool claim-prize u1)

;; 5. All users withdraw deposits
(contract-call? .lottery-pool withdraw-deposit u1)
```

## Economic Model

### Prize Calculation
```
yield = total_deposits * yield_rate
treasury_fee = yield * treasury_rate
prize = yield - treasury_fee
```

### Ticket Distribution
- 1 STX = 1 ticket (configurable)
- Max 100 tickets per user
- Linear probability increase

## Randomness Generation
- Block hash-based
- Seed mixing for unpredictability
- Modulo for ticket selection
- On-chain verifiable

## Security Features
1. **No-Loss Guarantee**: Deposits always returnable
2. **Time-Locked Draws**: Prevents manipulation
3. **Emergency Withdrawal**: Crisis recovery
4. **Maximum Ticket Limits**: Prevents domination
5. **Transparent Randomness**: Verifiable selection

## Pool Lifecycle
1. **Active**: Accepting deposits
2. **Drawing**: Ready for winner selection
3. **Completed**: Winner drawn, withdrawals open
4. **Emergency**: Pause mode for issues

## Deployment
1. Deploy contract
2. Set initial parameters
3. Create first pool
4. Market to users
5. Draw winners regularly

## Testing Checklist
- Pool creation and timing
- Ticket purchase limits
- Random winner selection
- Prize calculations
- Deposit withdrawals
- Emergency procedures
- Multi-user scenarios

## Yield Strategies
- 0.5% base yield rate
- Compound between pools
- Treasury accumulation
- Sustainable prize growth

## Use Cases
- **Savings Incentive**: Fun way to save
- **Community Pools**: Group savings
- **Charity Lotteries**: Fundraising mechanism
- **DeFi Gamification**: Engagement tool
