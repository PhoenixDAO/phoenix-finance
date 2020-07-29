# Phoenix Finance Manager
This is a smart contract that stores your credit cards, bank and investment accounts in one solidity contract named PhoenixFinance.sol using encryption to secure data on the blockchain.

## Workings of the contract
It receives the credit card data, encrypts it using your unique EIN identifier and stores that information into your user account. You can then generate the encryption result off-chain using `web3.utils.soliditySha3()` and your required parameters, to generate the hash that you can use to verify your account.
This is the only method to securely protect data on the open blockchain since we can't store sensitive information without encryptions.

## Understanding the functions

The contract is made of 9 functions and the constructor:
- The constructor requires the identity registry address to be deployed since it works with EIN addresses.
- The function `addCard()` is used to add a new card to the system. If you don't have a user associated with your financial data, it will be created automatically. You need to pass the cardNumber, the expiry date in timestamp, the name written in your card and the secure CVV verification code on the back of the card.
- The function `addBank()` is used to add a new bank account to the system. If you don't have a user associated with your financial data, it will be created automatically. You need to pass the bank number or IBAN and the name of your bank account.
- The function `addInvestmentAccount()` is used to add a new investment account to the system. If you don't have a user associated with your financial data, it will be created automatically the first time you use it. You need to pass the investment account number and the investment account name.
- The function `removeAccount()` deletes your existing account and resets the data so that it's lost forever.
- The function `checkAndCreateUser()` is an internal function used to verify is you have a user account yet or not. In such case, it creates a new one.
- The function `checkCard()` is a getter function that returns true if the parameters passed are valid or false if not. It is used to verify that the encrypted credentials uploaded to the blockchain are valid or not. When you add a credit card, it encrypts the data by combining your ein, card number, expiry and cvv using the keccak256 function. You need to pass all those parameters plus the encrypted hash at the beginning.
- The function `checkBank()` does the same thing as the check card but with your bank. You need to pass your encrypted bank hash as the first parameter, the bank number and the bank name.
- The function `checkInvestment()` does the same thing but with your investment account. You need to pass your encrypted investment hash as the first parameter, the investment number and the investment name.
- The function `getUserData()` is a function that returns your user data, specifically your EIN, Ethereum address used when creating the account, your encrypted cards in an array of bytes32 hashes, your encrypted bank accounts in an array of bytes32 hashes and your encrypted investments in an array of bytes32 hashes.

## Deploying your own Phoenix Finance
To deploy the contract, you must first deploy a Identity Registry Phoenix contract which is used to manage user EIN identities. Then, you can create a unique EIN account with the function: `identityRegistry.createIdentity()` and deploy your Phoenix Finance by setting the up the identity contract address in the constructor.

## Runing the tests
You can run the tests easily by starting a private ethereum instance using ganache with 8.5 million gas limit so that you have plenty of space to manage. Run ganache with: `ganache-cli -l 0x81B320`.

Then run the tests with: `truffle test` and you'll see that all tests are passing.

Run ganache-cli with 8.5 million gas for the testing with:
```
    ganache-cli -l 0x81B320
```
