Base Currency (sourceCurrency in config\params.php) is MYR

Open Hub store preset currency exchange rate in the database. Cron job ‘currency updateCurrencyExchangeRate’ run daily, query the day exchange rate API from https://openexchangerates.org/ and store to local database. We are using FREE plan so it is very limited, we can only query a specific date and in base currency of USD.

There is an API provided to acquire conversion rate between CurrencyA and CurrencyB.

All currency stored in database in the international-standard 3-letter ISO currency codes format.

## Todo
Change source currency to USD