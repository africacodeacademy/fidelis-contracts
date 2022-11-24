# fidelis-contracts

# Global schema

globalInts = 7

globalBytes = 12

Local schema

localInts = 2
localBytes = 1
      
      
# No_op txn functions

payment
reclaim
invest
config
force_close
check_default

Deployment function

# handle_creation requence

Assertions made

- check if loan amount is non zero number
- check if loan fee / interest fee is non zero number
- check if loan start time is a future date
- check if loan end time is a future data
- check if loan end time is ahead of loan start time

# Initialized data

- calculated loan balance (loan amount + loan fee)
- original loan amount
- loan fee
- loan start date
- loan end date
- pool address
- beneficiary address
- agent address (address where payout will be issued)
- stable_token (the loan payout ( currency ) token, usually USDC)
- loan_state (default value is "openToInvestment")
- staked amount (amount staked by beneficiary and backers)
- manager address

# escrow_config sequence

Sequence of optin  transactions for the contract escrow address. Enables the escrow address to work with USDC,  Fidelis trust and backer tokens
Only one assertion is made, the txn can only be made by the manager address


# Invest sequence

allow investors to (Bid) fund escrow account with backer and trust tokens
after successfull bid the investor account local state is updated with a record of the investment (investment amount, invested assetId and key)
If the minimum required investement stake for the contract is met, a payout is made to the agent address and the contract state is changed from openToInvestment to alive.

Assetions made

-  loan state == openToInvestment
-  contract enddate has not passed
-  txn type is assertTransfer
-  txn reciever is the contract address
-  txn amount (investment amount) is a non zero number
-  txn amount (investment amount) does not go above the agreed loan amount

# repay sequence

allow account to transfer USDCa tokens to liquidity pool as loan payment
if a partial payment is made, the payment is accepted and held in the contract untill full payment is made
if full payment is made,  loan state is changed to matured

Assertions made

- loan state == alive
- txn type is AssertTransfer
- txn receiver is the contract address
- txn amount is a non zero digit
- txn amount is not greater than the balance
- token sent is the expected (currency) stable token (USDC)


# Reclaim sequence

allow backers to relcaim staked points, update account local state with investment amount
account local state (amount) is reduced to 0 after payment
Assertions made

-  loan state == matured
-  account local state must have a none zero stake amount, a valid key, recongnised tokenId

