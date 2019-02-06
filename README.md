# Ethereum DAO Hack Study

This smart contract was used to experiment with the Ethereum DAO hack smart contract vulnerability as part of a Udacity course.  Hackers breached the smart contract for $60M of Ether ($ETH)

The DAO aimed to operate as a venture capital fund — allowing users to donate towards ideas they wised to support.  The value proposition with blockchain intended to prevent a significant amount of fees centralized venture capital sites take, such as Kickstarter and Indiegogo.  This is a common theme to use blockchain to combat business models based on economic rent-seeking. 

The contract contained a vulnerability known as re-entrancy.  
(expand)

From the fundraiser contract, the withdrawCoins() function is calling the payout() function from the wallet contract.  The wallet contract payout() function is then in turn calling the withdrawCoins() function from the fundraiser.  This prevents the withdrawCoins() function from reaching the balances[msg.sender] = 0; statement which zeroes out the balance of the wallet.  

```
function withdrawCoins(){
        uint withdrawAmount = balances[msg.sender];
        Wallet wallet = Wallet(msg.sender);
        wallet.payout.value(withdrawAmount)();
        balances[msg.sender] = 0;
    }
```


## Running this example:

Head over to the [ethereum remix IDE](http://remix.ethereum.org/):

### Deploy Contracts

1. Create a new file, paste in the smart contract code (.sol)
1. Switch to the run tab
1. Select the Fundraiser contract, click deploy
1. Under the deployed contracts section, to the right of the newly deployed Fundraiser contract, click the copy address button
1. Select the Wallet contract in the dropdown, paste the address next to the deploy button, then click deploy

### Exploit Contract Vulnerability

1. Under the deployed contracts section, expand both contracts
1. Add ether to the Fundraiser contract -- under value try 200 wei and click the (fallback) function button
1. Verify the balance -- click the getBalance() function
1. Add 100 wei to the Wallet contract and verify the balance
1. From the wallet contract, contribute 5 wei, check the balances
1. To implement the hack: from the wallet contract, click the payout function, then check the balances (note this will take longer due to the loop)
1. Note the payout can't be used again immediately because the balance of the wallet address was finally set to zero