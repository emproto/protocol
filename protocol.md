# Encrypted Message Protocol 1.0 Draft A

## Introduction

TBD

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

### Message packing and data layout

Software implementing Encrypted Message Protocol (_EMP_) **MUST** produce well packed message that can be transfered to any compatible third-party software using any accessible transport or technology. _EMP_ compatible software **MUST** document message packing method to ensure overall _EMP_ implementing Software interoperability.

Message produced by _EMP_ compatible Software **MUST** cosist of clearly separated blocks of data. _EMP_ compatible software **MUST** document blocks separation method to ensure overall _EMP_ implementing Software interoperability. It is **RECOMMENDED** to store finalized message in JSON format to ensure proper blocks separation and overall interoperability. Finalized JSON _EMP_ message **MAY** be encoded using RFC 4648 "base64url" to ensure pass-by-URL safety.

### Required data blocks

Message produced by _EMP_ compatible Software **MUST** contain at least two **REQUIRED** blocks of data: encryption algorithm alias block (_ALG_ block) and encrypted message block (_MSG_ block). Any other blocks or payload in message are **OPTIONAL** and can be added based on spesific needs or implementation.

_ALG_ block **MUST** contain short unique alias of algorithm used by Software to encrypt message. _ALG_ block **MUST NOT** contain any non-ASCII or non-printable characters. It is **RECOMMENDED** to use official abbreviation of ecryption algorithm with **OPTIONAL** signing hash bits length number. Meaning of every _ALG_ block aliases **MUST** be documented to ensure overall _EMP_ implementing Software interoperability. Examples: 
* `RSA256` - for RSA based asymmetric encryption with SHA256 signature
* `ECDSA512` - for ECDSA based asymmetric encryption with SHA256 signature
* `AES192` - for AES based symmetric ecryption using 24 bytes key

_MSG_ block **MUST** contain valid encrypted message text. Message encryption algorithm **MUST** match _ALG_ block alias and _ALG_ block documentation to ensure successfull message text decryption by receiving side.

### Optional data blocks

It is **RECOMMENDED** to add signature block (_SIG_ block) within message, containing cryptographic signature, allowing additional message verification. _SIG_ block **MUST** contain valid cryptographic signature. Message singing algorithm **MUST** match _ALG_ block alias and documentation to ensure successfull message verification by receiving side.

### Reserved message block key names

If message packing method uses key-value notation to separate data blocks than keys **MUST** be called "alg" and "msg" for _ALG_ and _MSG_ blocks respectively. If _SIG_ block presented in key-value notated message than this block key **MUST** be called "sig". Key names "alg", "msg" and "sig" are reserved by protocol and **MUST** be used only to store data blocks as described in previous section. Example of JSON packed EMP message:

```json
{
  "alg": "RSA256",
  "msg": "YJ73Pj8FTced....VwAE0zsN7CQg",
  "sig": "Cs78Km+QYUjD....S5hJFR2ctlBA",
  "sender": "bbrodriges"
}
```

## Authors and attributions

The Encrypted Message Protocol is authored by Georgiy Zuykov. It has been inspired by Alexander Baranov's idea of Encrypted Timeline and partially based on JSON Web Token standard. This protocol text is partially based on Semantic Version 2.0.0 specification text by Tom Preston-Werner.

You can open an issue on GitHub if you would like to leave feedback.

## License

Encrypted Message Protocol text distributed under Apache 2.0 license.
