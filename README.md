# ApacheHttpClientUsage  
Using jSSLutils with the Apache HTTP client library. 

## Introduction
The Apache HTTP client library 3.1 provides a way to configure SSL via its SecureProtocolSocketFactory.
org.jsslutils.extra.apachehttpclient.SslContextedSecureProtocolSocketFactory is an implementation of SecureProtocolSocketFactory which:
allows you to set an SSLContext so as to configure trust and use of a client certificate, and
verifies the host name against CN fields in the subject distinguished name of the server certificate.
This class does not depend on jSSLutils. It is currently available in version 0.5, in the extra directory of the subversion repository. It's also available in its own Jar file from the download page.

## Example
```
// Building an SSLContext using jSSLutils
X509SSLContextFactory sslContextFactory =
    new X509SSLContextFactory(keyStore, "keypassword", trustStore);
SSLContext sslClientContext = sslContextFactory.buildSSLContext();

// Using SslContextedSecureProtocolSocketFactory
// This doesn't depend on the rest of jSSLutils and could use any
// other SSLContext.
HttpClient httpClient = new HttpClient();
SslContextedSecureProtocolSocketFactory secureProtocolSocketFactory =
   new SslContextedSecureProtocolSocketFactory(sslClientContext);

Protocol.registerProtocol("https", new Protocol("https",
   (ProtocolSocketFactory)secureProtocolSocketFactory, 443));


GetMethod method = new GetMethod("https://ssl.example.org/");
int statusCode = httpClient.executeMethod(method);
```
## Licence
SslContextedSecureProtocolSocketFactory is released under the Apache License 2 and LGPL 2, since it is based on the StrictSSLProtocolSocketFactory which comes under this dual licence. That class was contributed by Sebastian Hauer as an example in the Apache HTTP client project (it was already capable of verifying the host name).
