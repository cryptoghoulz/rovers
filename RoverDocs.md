# MOONBASE & YOU: A PRIMER FOR \$BASED PARTNERS

Welcome, friends!

### WHAT ARE ROVERS?

Rovers are special smart contracts that are created and deployed trough BasedGod.sol, which acts as a Rover Factory. There are 3 types of Rover deployments: *RoverVault (rVault)*, *FarmingRover*, and *FarmingRover/RewardPool*.

All Rovers are subject to the same 1-year linear vesting schedule. This means that only a fraction of the underlying asset can be gradually swapped out over the course of year. The asset is swapped for an equivalent amount of \$BASED and the resulting \$BASED is distributed proportionally within the Moonbase.

The total amount of assets that can be swapped at a given time is calculated according to this formula:

`withdrawableTokenQuantity = secondsSinceRoverStart/(365 * 24 * 60 * 60) * totalTokensReceived - totalTokensWithdrawn`

### WHICH TYPE OF ROVER SHOULD YOU USE?

Probably rVault, as this is the easiest to use, but really, it varies from project to project. Below, we describe the 3 different types and provide instructions on how to use them.

### HOW TO DEPLOY A ROVER

#### 1. RoverVault (rVault)**
You deploy it and transfer tokens to it. This can be a lump sum that is deposited all at once or multiple sums that are deposited over an arbitrary period of time, even after the Rover is live.

##### How to deploy a RoverVault (rVault):

**Step 1:**
From BasedGod.sol, call:

```
function createNewRoverVault(
    address _rewardToken,
    string calldata _pair
)
```

Where `_rewardToken` is the address of your asset and `_pair` is ether "sUSD" or "WETH" as a string value. This `_pair`  is referenced during a swap in order to create the appropriate path between you asset and \$BASED via Uniswap.

**Step 2:**
Deposit tokens.

**Step 3:**
When you're ready to launch, call:

`function startRover()` on the Rover contract to start the Rover.

**Step 4:**
Ape in.

#### 2. FarmingRover**
The Rover mints 1 unique token which is then staked in a custom pool that you've made specifically for this token. All rewards from this yield go into the Rover.

##### How to deploy a FarmingRover:

**Step 1:**
From BasedGod.sol, call:

```
function createNewFarmingRover(
    address _rewardToken,
    string calldata _pair
)
```

Where the input values are the same as those provided in RoverVault.

**Step 2:**
Set up your reward pool using Rover as LP token.

**Step 3:**
When you're ready to launch, call:

`function startRover(address _rewardPool)`

Where `_rewardPool` is the address of your existing reward pool.

**Step 4:**
Stay BASED.

#### 3. FarmingRover + RewardPool**
Deploys a FarmingRover and a custom distribution reward pool in one transaction. This option is provided for convenience. The distribution reward pool is based on the Synthetix StakingRewards.sol contract.


##### How to deploy a FarmingRover + RewardPool:

**Step 1:**
From BasedGod.sol, call:

```
function createNewFarmingRoverAndPool(
    address _rewardToken,
    address _distributor,
    string calldata _pair,
    uint256 _duration
)
```

Where `_distributor` is the address that notifies the reward pool of new rewards. This address can be an account, multisig, DAO, or whatever else.

**Step 2:**
Deposit asset in RewardPool and then call the following function on `_rewardPool` from `_distributor` to notify `_rewardPool` of incoming rewards:

`function notifyRewardAmount(uint256 reward)`

**Step 3:**
Call function `startRover(address _rewardPool` on Rover to initiate.

**Step 4:**
Just market buy.
