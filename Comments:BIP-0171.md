# Cerise ( by [straumat](https://github.com/straumat/) )
CERISE ([website](http://www.cerise.tech/) & [github](https://github.com/straumat/cerise)) provides :
 * [BIP-0171 specifications](http://www.cerise.tech/#specifications).
 * A [mocked BIP-0171 compliant server available online](http://api.cerise.tech/swagger-ui.html) you can use to understand the API and make calls.
 * A [mocked BIP-0171 compliant server as a Java application](https://github.com/straumat/cerise-server-mock/) you can use to develop your client application.
 * A [mocked BIP-0171 compliant server as a Docker image](https://hub.docker.com/r/straumat/cerise-server-mock/) you can use to develop your client application.
 * A [BIP-0171 library](https://github.com/straumat/cerise/) to transform your application in a BIP-0171 server.
 * A [server template project](https://github.com/straumat/cerise-server-template) to quickly write your implementation and automatically produce your BIP-0171 server.
 * A collection of [client libraries](http://www.cerise.tech/#clients) to call any BIP-0171 compliant server with your favorite language.

# Changes to make in the BIP ? ( by [straumat](https://github.com/straumat/) )
“a GET request to a common URI with parameters encoded in application/x-www-form-urlencoded format” 
May I ask you why parameters should be encoded this way ? From what I have seen in other projects, they also allow json for get method.

In the samples you provided, all returned fields are not set. For example, signature is never set. Of course it’s not a problem but I think it would be a good idea to have at least a result with all fields set so i could implement all the unit test cases.
I have the same comment for “archive” and “signature” fiedls in Currency-pair information.
I have the same comment for “minrate” and “maxrate” parameters in Current exchange rate and nonce in result.

“digits - The type of digits to use for the quote currency's numbers. "arabic" should be used for common 0-9 digits.” Don’t you think we should provide the list of supported digits ?

# Things I don’t understand ( by [straumat](https://github.com/straumat/) )
I don’t understand what means “XBTUSD-ver4” ? I don’t see what it means.
“Currency-pair information \ symbol : Any positive or negative symbols must be included in this prefix/suffix” I don’t understand this rule. Can you explain it more please ?