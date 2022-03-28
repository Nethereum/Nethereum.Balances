# Nethereum.Balances
Easily get all your token balances at https://balance.nethereum.com/

![image](https://user-images.githubusercontent.com/562371/127461803-08825a23-35de-4831-9a5f-aee76c4dc04b.png)

# Nethereum features demonstrated
Nethereum now includes built in support to work with Multicall.sol, this enables much faster querying using many different functions from different smart contracts.
Just use your FunctionMessages input and outputs.

```csharp
  for (int i = startItem; i < totaItemsToFetch; i++)
            {
                var balanceOfMessage = new BalanceOfFunction() { Owner = account };
                var call = new MulticallInputOutput<BalanceOfFunction, BalanceOfOutputDTO>(balanceOfMessage,
                    _tokens[i].Address);
                callList.Add(call);
            }

            await _web3.Eth.GetMultiQueryHandler().MultiCallAsync(callList.ToArray()).ConfigureAwait(false);

            var currentBalances = new List<BalanceResult>();

            for (int i = startItem; i < totaItemsToFetch; i++)
            {

                var balance = ((MulticallInputOutput<BalanceOfFunction, BalanceOfOutputDTO>)callList[i - startItem]).Output.Balance;
                if (balance > 0)
                {
                    var balanceResult = new BalanceResult()
                    {
                        Balance = Web3.Web3.Convert.FromWei(balance, _tokens[i].Decimals),
                        TokenImage = _tokens[i].LogoURI,
                        TokenName = _tokens[i].Symbol,
                        TokenAdress = _tokens[i].Address,
                        GeckoToken = _geckoTokens.FirstOrDefault(x => x.Symbol.ToLower() == _tokens[i].Symbol.ToLower())
                    };

                    currentBalances.Add(balanceResult);
                }
            }

```

### Future of the application
This will be later on integrated in the other apps, like Nethereum Desktop and Explorer / Wallet (web, mobile, desktop, etc)

## Credits and Thanks
+ Everyone contributing to https://tokenlists.org/, the example currenlty uses the 1inch list (1000)
+ Coingecko for their great api https://www.coingecko.com/
