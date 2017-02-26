# Encrypted Message Protocol 1.0 Draft A

## Introduction

Encrypted Message Protocol is a protocol that describes abstract structure of encrypted message that can be securely transfered between multiple compatible Software parts.

It is not aimed to provide complete and strict requirements for Software rather than to provide a set of flexible yet precise agreements. The purpose of the protocol is to simplify the development of compatible software using encrypted messages to exchange data.

## Warning

While this document stays in draft state - any protocol backward compatibility is not guaranteed.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### Message packing and data layout

Software implementing Encrypted Message Protocol (_EMP_) **MUST** produce well packed message that can be transfered to any compatible third-party software using any accessible transport or technology. This Software **MUST** document message packing method to ensure interoperability between any other _EMP_ compatible Software.

Message produced by _EMP_ compatible Software **MUST** cosist of clearly separated blocks of data. This Software **MUST** document blocks separation method to ensure interoperability between any other _EMP_ compatible Software.

It is **RECOMMENDED** to store finalized message in JSON because of proper blocks separation and wide format adoption. Finalized JSON _EMP_ message **MAY** be encoded using [RFC 4648 "base64url"](https://tools.ietf.org/html/rfc4648#page-7) to ensure pass-by-URL safety.

### Required data blocks

Message produced by _EMP_ compatible Software **MUST** contain at least two **REQUIRED** blocks of data: encryption algorithm alias block (_ALG_ block) and encrypted message block (_MSG_ block). Any other blocks or payload in message are **OPTIONAL** and can be added based on spesific needs or implementation.

_ALG_ block **MUST** contain short unique alias of algorithm used by Software to encrypt message. _ALG_ block **MUST** comprise only ASCII alphanumerics, period and hyphen [0-9A-Za-z-.]. It is **RECOMMENDED** to use official abbreviation of ecryption algorithm with **OPTIONAL** signing hash and its bits length number. Meaning of every _ALG_ block aliases **MUST** be documented to ensure overall _EMP_ implementing Software interoperability. Examples: 
* `RSA-SHA256` - for [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) based asymmetric encryption with SHA256 signature
* `ECDSA-SHA512` - for [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) based asymmetric encryption with SHA256 signature
* `AES192` - for [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) based symmetric ecryption using 24 bytes key without signature

_MSG_ block **MUST** contain properly encrypted message text. Message encryption algorithm **MUST** match _ALG_ block alias and _ALG_ block documentation to ensure successfull message text decryption by receiving side.

### Optional data blocks

It is **RECOMMENDED** to add signature block (_SIG_ block) within message, containing cryptographic signature, allowing additional message verification. _SIG_ block **MUST** contain valid cryptographic signature. Message singing algorithm **MUST** match _ALG_ block alias and documentation to ensure successfull message verification by receiving side.

### Reserved message block key names

If message packing method uses key-value notation to separate data blocks then these keys **MUST** be called `alg` and `msg` for _ALG_ and _MSG_ blocks respectively. If _SIG_ block presented in key-value notated message than this block key **MUST** be called `sig`. Key names `alg`, `msg` and `sig` are reserved by protocol and **MUST** be used only to store data blocks as described in previous section.

### Documentation

Every protocol implementation **SHOULD** be followed by implementation documentation. It is **RECOMMENDED** to store documentation alongside with implementation source code in file named `IMPLEMENTATION`.

## Summary

* Use any encoding/packing format you want, but try to use widely adopted, such as JSON, Protobuf, etc.
* Use any data layout you want, as long as you distinguish data blocks.
* Always specify message block algorithm in designated data block.
* It is recommended to add signature data block into your message to increase safety.
* Always provide documentation of data layout, supported cryptographic algorithms with their aliases, additional payload and message packing method.
* If you use key-value data layout try to use reserved key for respected values to lower adoption complexity and increase interoperability.

## Authors and attributions

The Encrypted Message Protocol is authored by [Georgiy Zuykov](https://github.com/bbrodriges). It has been inspired by [Alexander Baranov](https://github.com/sashabaranov)'s idea of [Encrypted Timeline](https://github.com/enctl) and partially based on [JSON Web Token](https://jwt.io) standard by 0Auth. This protocol text is partially based on Semantic Version 2.0.0 specification text by [Tom Preston-Werner](http://tom.preston-werner.com/).

You can open an issue on GitHub if you would like to leave feedback.

## License

Encrypted Message Protocol specification text distributed under Apache 2.0 license.
