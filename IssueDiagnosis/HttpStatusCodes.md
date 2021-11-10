# Http Response Status Codes

Useful Resources:
 - https://www.loggly.com/blog/http-status-code-diagram/

## HttpStatus 301 Headers
A 301 Http status code is _Moved Permamemtly_, which indicates that the requested resource has been definitively moved to the URL given by the Location headers.
When calling an endpoint that returns a 301, Postman will automatically handshake with the URL provided in the endpoint location,
and perform a redirect request with the original headers, including the Authorization header. This happens behind the scenes and appears to just work.
However, in Visual Studio when a 301 occurs a redirect request is still done, but the original Authorization header is not provided to the redirect.
This could result in a 401 occurring, even though the Authorization header appears to have been applied.

To diagnose this, using a packet sniffer, like Fiddler, helps show what is occurring, such as the 301 and subsequent request redirect.
The easiest fix is to determine the difference between the URL routes (the original and the redirect) and change your code to use the new URL location.
The difference between these two URLs that triggers the 301 could be as simple as a `/` being on the end of one and not on the other.

It appears that Visual Studio's behaviour is _by design_ as a security measure. If the redirect was to an untrusted third-party server, then without this behaviour,
your authorization token would be disclosed to the server and potentialy abused.

See:
 - https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301
 - https://community.k6.io/t/requests-that-redirect-with-301-response-code-sends-http-requests-without-header/569/2
 - https://stackoverflow.com/a/28671822/1926027

## 503 Service Temporarily Unavailable
This status occurred when trying to call external APIs from an AWS lambda. The external APIs were available and working fine when called locally (Postman, Visual Studio),
and also from a temporarily created lambda (using python code) within the same AWS environment. The cause of this was determined by adding the following logging:
```C#
var testClient = new HttpClient();
var apiKey = // Get API key
testClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", apiKey);

var response = await testClient.GetAsync("https://the.api.url.to.call");

var content = await response.Content.ReadAsStringAsync();

await _logger.Instance.LogInformationAsync("Internal http client start");
await _logger.Instance.LogInformationAsync("Internal http client response: " + JsonConvert.SerializeObject(response));
await _logger.Instance.LogInformationAsync("Internal http client response.content: " + JsonConvert.SerializeObject(content));
await _logger.Instance.LogInformationAsync("Internal http client end");
```
with the logging of the content capturing the HTML of a webpage. Copying this HTML, fixing the quotation marks and newlines, saving it to a file called index.html,
and opening it in a brower showed

> DDos protection by Cloudflare  
> Ray Id: _some identifier_

So what was happening was the external APIs were using Cloudflare, which some how determined that the requests from that one particular lambda was suspicious
and was essentially blocking the request. This was fixed by getting the owner of the API to disable Cloudflare or change its settings,
specifically they disabled a feature called _bot fighting_ in Cloudflare.
