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
