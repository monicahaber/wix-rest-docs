SortOrder: 0
### About the Card Tokens API:
Used to tokenize credit card data against the PCI environment. 
This card token must be passed to [CreateTransaction](needs link to endpoint) when a credit/debit card is the chosen payment method.
> **Note:** A new `card_token` should be generated for each transaction request.
> If you are creating a recurring transaction (MIT/CIT) you only need to tokenize the `card_number` and pass it in the [CreateTransaction](needs link to endpoint) request together with [COF](link needed) flags.
