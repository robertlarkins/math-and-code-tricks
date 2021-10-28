# Fiddler

## Filtering traffic
- https://docs.telerik.com/fiddler/knowledge-base/filters
- https://stackoverflow.com/questions/4098877/filter-fiddler-traffic

## Remove Certificates
If you have enabled Decrypt HTTPS traffic, Fiddler will add its own certificate allowing it to inspect the content.
If you no longer need this ability it can be disabled by going

> Tools > Options > HTTPS > uncheck Decrypt HTTPS traffic
> Then click Actions > Remove Interception Certificates

The certificate that Fiddler adds to Windows Machine Root List is called _DO_NOT_TRUST_FiddlerRoot_ and it can be seen by running certmgr.msc (from the run command) and going
Action > Find Certificates...

Even after removing the certificate via Fiddler some parts of the DO_NOT_TRUST_FiddlerRoot certificate might remain. These can be deleted by the certmgr.msc.

See Also:
- https://stackoverflow.com/questions/16827901/how-do-you-remove-the-root-ca-certificate-that-fiddler-installs
