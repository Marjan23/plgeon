***
I just released CERISE ([website](http://www.cerise.tech/) & [github](https://github.com/straumat/cerise)) a project with the following artifacts : 
* A mocked server implementation to start developing clients : [https://github.com/straumat/cerise](https://github.com/straumat/cerise).
* A live & documented API to understand and directly calls the methods : [http://api.cerise.tech/docs](http://api.cerise.tech/docs).
* Specifications of the four methods : [Enumerating supported currency-pair tokens](http://www.cerise.tech/specifications/0.3-SNAPSHOT/supportedCurrencyPairTokensAPI.html), [Currency-pair information](http://www.cerise.tech/specifications/0.3-SNAPSHOT/currencyPairInformationAPI.html), [Current exchange rate](http://www.cerise.tech/specifications/0.3-SNAPSHOT/currentExchangeRateAPI.html) & [Historical exchange rates](http://www.cerise.tech/specifications/0.3-SNAPSHOT/historicalExchangeRatesAPI.html).
* 16 client librairies for various langages (Java, PHP, c++, Rusty, Ruby….) : [http://www.cerise.tech/#clients](http://www.cerise.tech/#clients).
* and i’m working on a template project to allow developers to quickly build a BIP171 compliant server without having to worrying about parameters validation, security, rest mechanics… will be done in september.

Stéphane Traumat ( @straumat )
***
**Change to make in the BIP ?**
“a GET request to a common URI with parameters encoded in application/x-www-form-urlencoded format” 
May I ask you why parameters should be encoded this way ? From what I have seen in other projects, they also allow json for get method.

There is no description on how to return error messages ?
For the moment, for any error, I return this message structure : {"message":"Currency-pair should be no longer than 255 characters","errors":["‘AAAAA.AAAA’ should be no longer than 255 characters", "‘BBBBB.BBBB’ should be no longer than 255 characters"]}
The message field contains the error message and you can use errors to detail every errors found.

In the samples you provided, all returned fields are not set. For example, signature is never set. Of course it’s not a problem but I think it would be a good idea to have at least a result with all fields set so i could implement all the unit test cases.
I have the same comment for “archive” and “signature” fiedls in Currency-pair information.
I have the same comment for “minrate” and “maxrate” parameters in Current exchange rate and nonce in result.

“digits - The type of digits to use for the quote currency's numbers. "arabic" should be used for common 0-9 digits.” Don’t you think we should provide the list of supported digits ?

**Things I don’t understand.**
I don’t understand what means “XBTUSD-ver4” ? I don’t see what it means.
“Currency-pair information \ symbol : Any positive or negative symbols must be included in this prefix/suffix” I don’t understand this rule. Can you explain it more please ?
“grouping” : can you provide more informations about how it works ? I don’t understand if it’s only 3 fields or if you can have more ?