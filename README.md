# 420Integrated - 420link token contract (Ethereum network resident)

This is the repository for the 420link token deployed on the Ethereum blockchain, and which will later act as a bridge between the 420coin and Ethereum networks. 

## Contract Details
- Creates/Mints 50 million 420link Tokens and holds them inside contract.
- 420link (ERC20) and Sale Contracts both reference each the other
- Converts payable ETH to 420link directly to user
- ETH payed to contract is forwarded to a different wallet (used for funding platforms within the 420coin Genesis Framework, see https://420integrated.com for more information.)
- Updateable ETH/420link rate
- Developers and Founders can allocate a specific amount of tokens to be released at later date.
- Ability to turn on/off transfer of coins. 
- Availiable balance through direct sale from contract to users ends at a specified Ethereum block number
- Token contrbutions will be removed from the Mint and transfered to the purchaser.
- Users that have tokens held can release them once time has passed without owner permission. 

# Basics

The Sale contract is deployed on the Ethereum main-net at address 0x1a2b3c4d5e6f.

The 420link Token contract is deployed on the Ethereum main-net at address 0xa1b2c3d4e5f6.

Users send ETH to the Sale contract address to receive 420link tokens. Purchases are processed using the extra data: `0xd7bb99ba` sent with a transaction to the Sale contract address. User must send ETH or the transaction will fail! 

Example: 1 ETH - Data: `0xd7bb99ba` - Gas Limit: `80000` creating transaction. This transaction sends 1 ETH to the referenced Ethereum wallet address maintained by 420Integrated and emits Tokens to the user. 

Once the full suite of 420link tokens have been sold, we will emit the `closeSale` function. 

This ICO-sale contract will allows us the flexibility to change owners, change the 420link token rate-per-ETH, hold tokens for a specific amount of time, and allow users that have tokens held to release those to their wallets for spending. The Sale contract allows for the function to lock a value of tokens, and once a set block height on the Ethereum network has been passed, the user can run `realeaseHeldCoins()` (`0x6ce5b3cf`) to the sale contract to receive their tokens.

### 420link Sale Contract
`function contribute() external payable` is the function for the purchaser to mint new tokens. (Data: `0xd7bb99ba`)

#### Minting 420link Tokens
The minting of 420link Tokens call comes from the Sale contract, when the ERC20 token contract is deployed it will force the Sale contract.
`function mintToken(address to, uint256 amount) external returns (bool success);`

Change "transfer" method from the Sale Contract. 
`function changeTransfer(bool allowed);`

### Hold Token Period

The Sale Contract allows us to set aside and hold a specific amount of tokens for an amount of time before being released directly to an address. The createHoldToken function uses an Ethereum address and the amount of 420link tokens released at end of a specified period.
```
function createHeldCoins() internal {
  createHoldToken(0x4f70Dc5Da5aCf5e71988c3a8473a6D8a7E7Ba4c9, 100000000000000000000000); // 100,000
  createHoldToken(0x323c82c7Ae55B48745f4eCcd2523450d291f2412, 250000000000000000000000); // 250,000
}
```

This time limit is adjustable, and the contract owner can adjust it using the call:
```
function createHoldToken(address _to, uint256 amount) internal {
...
  heldTimeline[_to] = block.number + 200000;
...
}
```

Once a time period as passed, the wallet owner can do a contract call to receive the tokens. (Data: `0x6ce5b3cf`)
```
function releaseHeldCoins()
```
