---
title: "A Brief Summary of Networking in iOS"
date: 2020-08-29T17:10:00+11:00
draft: false
ads: true
categories:
  - Notes
tags:
  - ios
  - networking
---
# A Brief Summary of Networking in iOS

I was reading materials regarding iOS networking and taking notes while reading, this article is a summary of all the materials I've read so far. It's not a complete summary of networking or iOS, so you should not rely purely on this article to learn from scratch; However, having said this, you may find this article useful if you are already familiar with iOS and/or networking.

[Basics](#basics)  
[Security](#security)  
[Networking in iOS](#networking-in-ios)

## Basics

I found it helpful to first get familiar with some networking fundamentals, imagine you are reading this article in a browser on your Linux machine, the whole networking implementation can be described by Open System Interconnection (OSI), developed by the International Organization for Standardization, which consists of 7 layers listed below.

(Is it a coincident that OSI is developed by ISO, where the 7 layers might be more understandable if one reads it in reverse order?)

1. __Physical__: the hardware that transmits raw bit stream, e.g. Ethernet, RJ45, cables
2. __Data Link__: at this layer, data packets are encoded into and decoded from bits, the layer manages and handles errors in the physical layer including flow control and frame synchronization. The data link layer has two sub layers:
    - Media Access Control (MAC): controls how a device on the network gains data access and permission for transmitting.
    - Logical Link Control (LLC): frame synchronization, flow control and error handling
3. __Network__: this layer provides switching and routing to determine which physical path the data will take, it breaks segments of data from the transport layer into smaller units, called packets for sending, and resembles these packets on receiving device.
4. __Transport__: this layer is responsible for data transmission between two devices by breaking data from _session layer_ into smaller units called segments; Its job includes end-to-end error recovery which involves requesting for retransmission if data received is not complete, also determines an optimal speed of transmission based on connection quality of the two ends.
5. __Session__: this layer creates, manages and terminates connections between applications, the time between when the communication opens and closes, is known as _session_. The layer ensures session stays open for all data to be transmitted then closes the session.
6. __Presentation__: as the name suggests, this layer makes data presentable for _application_ layer as well as translating data to network format for _session_ layer.
7. __Application__: this layer provides protocols for client applications that are visible to the user, e.g. web browser, email clients. Protocols include HTTP for web browsing as well as SMTP for email.

After OSI, there's the TCP/IP model which is sometimes considered as a simplified version of OSI model:

1. __Network Access__: also called the Link or Network Interface layer, combines layer 1 and 2 in OSI model
2. __Internet__: similar to OSI model layer 3
3. __Transport__: similar to OSI model layer 4
4. __Application__: also called the Process layer, combines OSI model's layer 5, 6 and 7

Several common terminologies that are familiar to front-end (or even back-end) developers, if you feel unfamiliar, get yourself familiar:

- Client/Server: two ends in network communication activity, the end that provides resource or server is called server, the other end that _requesting_ data or service, is called client.
- HTTP(HyperText Transfer Protocol): _application layer_ protocol, it defines a set of request method (or verbs) to indicate action required to be performed:
  - _GET_: retrieve data
  - _POST_: sending data to server, _traditionally_ used for creating a new location
  - _PUT_: updates data at a specific location
  - _DELETE_: deletes data at a specific location
  - _HEAD_: similar to GET but only requests the headers, used when meta-data is needed
- REST (Representational State Transfer): usually used with HTTP (or HTTPS) for web APIs, more details in the next section
- JSON: stands for JavaScript Object Notation, a lightweight format for data representation

HTTP communication consists of text messages between a client and a server, the client can be a browser, or an app in the context of iOS development. Both request and response should have header fields and some request methods also have message body. Among all the request headers, there are a few that are commonly used:

- __status code__: in response header determines the status of the request, there are predefined status codes like 200 means success and 404 means not found, do your own research to find out more
- __content type__: specifies the type of media in the HTTP message, commonly used are:
  - text: `application/json`, `application/x-www-form-urlencoded` and `text/html`
  - binary: `application/pdf`, `image/png`, `image/jpeg`, `image/gif` and `multipart/formdata`

While many servers are using HTTP/1.1, there's HTTP/2 with quite a few of advantages, do your own research to find out more.

## Security

Security is a big topic, here I only summarise the thing I've encoutnered so far which are related to iOS.

### SSL

SSL stands for Secure Socket Layer, it's a protocol for creating an encrypted connection between client and server. The steps taken for client to establish connection to server are sometimes referred to as _SSL handshake_.

1. client connects to server and request certificate
2. upon receiving server certificate including a public key, client checks if it's valid
3. if the certificate is valid, client creates a symmetric key and encrypt it with received public key, sends it to server
4. server then receives client's symmetric key, encrypted with the public key in step 2, decrypt it using corresponding private key, then sends acknowledgement to client
5. client receives ACK then start the session

### TLS

SSL is now deprecated and replaced by TLS, TLS stands for Transport Layer Security.

- SSL version 1 was never released
- SSL version 2 has many flaws
- SSL version 3 rewrite version 2 with limited improvements
- TLS version 1 improves SSL version 3
- TLS version 1.1 has minor changes over version 1
- TLS version 1.2 has significant changes
- TLS version 1.3 refined the whole process

TSL connection consists of three phases:

1. client connects to server, providing supported TLS versions along with the cipher suite it can use; Server responds back with specified cipher suite and one or more certificates; Client validates the certificates from server, also verifies if server is authentic.
2. client generates a pre-master secret key, encrypts it with server's public key, upon receiving the encrypted public key, server decrypts it with its private key. Both server and client generate the master secret key and session keys based on the pre-master secret key.
3. the master secret key is then used to decrypt and encrypt the information that server and client exchange.

### Digital Certificate

A digital certificate is a data file that contains a public key and other information, such like subject, issuer, valid from/to; The standard for the structure of certificate is X.509.

Depends on encoding method, X.509 certificates can have different formats:

- __Privacy Enhanced Mail (PEM)__: a base-64 encoding with extension of `.pem`
- __Distinguished Encoding Rule (DER)__: a binary encoding with extensions of `.cer`, `.der` and `.crt`
- __Public Key Cryptography Standards (PKCS)__: used to exchange public and private objects in a single file, with extensions of `.p12`, `.pfx`, `.p7b` and `.p7c`

### Certificates Chain

For an SSL certificate to be trusted, the connecting device (e.g. iOS app) checks if the certificate is issued by a CA that's included in the device's trusted store, if not, continue to check if the certificate of the issuing CA is issued by a trusted CA, until a trusted CA is found. The list of certificates, from root certificate to end user certificate, is called __certificate chain__.

Click on the padlock next to browser's URL field, if the certificates are valid, you probably can see the chain of certificates.

### Certificate Pinning

The purpose of certificate pinning is to prevent man-in-the-middle attach, where someone captures tranffic between two ends and injects its own self-signed certificate in the middle. It's done by comparing server certificates with pinned certificates or public keys bundled in the app, and only establish connection if they match.

There are two types of certificate pinning:

1. Pin the certificate: server certificate is bundled in the app
2. Pin the public key: the public key of server certificate is hardcoded in the app

## Networking in iOS

In consumer apps one common use case of networking is to communicate with a server, in iOS, this can be done by `URLSession`. The list below is an overview of how `URLSession` API works:

1. use a `URLSessionConfiguration` to create an instance of `USLSession`
    - `URLSessionConfiguration` lets you configure things like timeout, caching, etc
2. use `URLSession` to create `URLSessionTask` to transfer data between app and server
    - one `URLSession` can create multiple `URLSessionTask` if their configuration is the same
    - for different configurations or delegate, multiple `URLSession` can be used
    - a completion closure can be specified to handle server respose, or
    - a delegate can be specified to handle response, monitor progress and handle authentication challenge
3. `URLSession.shared` is a singleton for simple request without configuration and delegate
    - thus `URLSessionTask` has to have a completion closure if you care about the response

### URLSessionConfiguration

`URLSessionConfiguration` can be created by using one of the predefined class variables, each returns an instance of `URLSessionConfiguration` with default configuration predefined for that class variable, and can be further customized.

- `default`: disk persistence for cache, credentials stored in keychain
- `ephemeral`: like `default` but doesn't store cookies, credentials or cache data to disk
- `background(withIdentifier:)`: transfer data while app is in background
  - upon backgrounding the app it handover data transfer to system which handles it in a differnet process
  - `identifier` is used for reconstructing the session when relaunching the app

Note, changing configurations must be done before creating the `URLSession`:

```swift
let configuration = URLSessionConfiguration.default
configuration.allowsCellularAccess = true // this works
let session = URLSession(configuration: configuration)
configuration.allowsCellularAccess = false // this won't work
```

For more details you should consult Apple's documentation of these classes, below are a few commonly used properties:

- `allowsCellularAccess`: whether cellular access is allowed, consider disabling this for large data download or consult user if they intend to use cellular when WIFI isn't available
- `waitsForConnectivity`: new in iOS 11, for non-background session, setting this to true allows the system to check for connection and retry, in case when network connectivity is lost
- `multipathServiceType`: specifies the Multipath TCP connection policy for transmitting data over Wi-Fi and cellular interfaces, this is useful when mobile devices move in and out of range of WIFI, switching to cellular network and back again.
  - apps that use Multipath TCP need to have _multipath capability_
  - also the server need to be MPTCP-capable

### URLSessionTask

Create tasks by calling task creation method on a `URLSession` instance, there are three concrete classes of URLSession tasks:

- `URLSessionDataTask`: response returned in memory, not supported in background session
- `URLSessionUploadTask`: requires a request body
- `URLSessionDownloadTask`: response saved in a file, and returns the location of the file

There are four ways to create a `URLSessionDataTask`, choose one that best suits your need. Providing `url` defaults to _GET_ request.

```swift
func dataTask(with url: URL) -> URLSessionDataTask
func dataTask(with: URL, completionHandler: (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask
func dataTask(with: URLRequest) -> URLSessionDataTask
func dataTask(with: URLRequest, completionHandler: (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask
```

While tasks created by `URLSession` inherits the configuration from `NSURLSessionConfiguration`, `URLRequest` can overwrite some of the configurations.

- `URLRequest` can be used to override HTTP method, headers and body
- `URLRequest` can be used to override configurations such like `timeoutInterval` and `cachePolicy`

__note:__ `URLRequest` can only override configurations if configurations from `NSURLSessionConfiguration` are less restrictive, for example, if `NSURLSessionConfiguration` allows celluar access (ie. `allowsCellularAccess = true`), `URLRequest` can disable it, but if `allowsCellularAccess` is `false` in `NSURLSessionConfiguration`, `URLRequest` cannot allow it.

### Certificate Pinning Implementation

In response to an authentication request from server, `NSURLSession` asks its delegate to handle the challenge by calling delegate method `didReceiveChallenge:completionHandler:delegate`. Note, there's also the task-level delegates defined by `NSURLSessionTaskDelegate`, `NSURLSessionDataDelegate`, and `NSURLSessionDownloadDelegate`, for task specific events.

```swift
func urlSession(_ session: URLSession, 
     didReceive challenge: URLAuthenticationChallenge, 
        completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void) {
    guard let serverTrust = challenge.protectionSpace.serverTrust else {
        completionHandler(URLSession.AuthChallengeDisposition.cancelAuthenticationChallenge, nil)
        return
    }

    var secResult = SecTrustResultType.invalid
    let status = SecTrustEvaluate(serverTrust, &secResult)

    if (errSecSuccess == status) {
        if let serverCertificate = SecTrustGetCertificateAtIndex(serverTrust, 0) {
            let serverCertificateData = SecCertificateCopyData(serverCertificate)
            let data = CFDataGetBytePtr(serverCertificateData);
            let size = CFDataGetLength(serverCertificateData);
            let serverCert = NSData(bytes: data, length: size)
            if let file = Bundle.main.path(forResource: "certfile", ofType: "cer") {
                if let bundledCert = NSData(contentsOfFile: file) {
                    if serverCert.isEqual(to: bundledCert as Data) {
                        completionHandler(.useCredential, URLCredential(trust: serverTrust))
                        return
                    }
                }
            }
        }
    }
    completionHandler(.CancelAuthenticationChallenge, nil)
}
```

References:

[URL Loading System](https://developer.apple.com/documentation/foundation/url_loading_system)  
[Improving Network Reliability Using Multipath TCP](https://developer.apple.com/documentation/foundation/urlsessionconfiguration/improving_network_reliability_using_multipath_tcp)  
[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)  
[What is the SSL Certificate Chain?](https://support.dnsimple.com/articles/what-is-ssl-certificate-chain/)
