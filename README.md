# ThoughtWorks DID

This document is the ThoughtWorks DID specification.

## Table of Contents
  - [Abstract](#abstract)
  - [Motivation](#motivation)
  - [DID](#did)
    - [DID Definition](#did-definition)
    - [DID Document](#did-document)
    - [Create DID](#create-did)
    - [Read DID](#read-did)
    - [Update DID](#update-did)
    - [Revoke DID](#revoke-did)
    - [Privacy considerations](#privacy-considerations)
    - [Security considerations](#security-considerations)
  - [Verifiable Credential](#verifiable-claims)
    - [VC Definition](#vc-definition)
  - [Use Cases](#use-cases)
    - [Authentication](#authentication)
    - [Issue Claims](#issue-claims)
    - [Verify Claims](#verify-claims)
## Abstract

## Motivation
[去中心化身份试图解决的矛盾](https://www.zybuluo.com/lambeta/note/1695624)

## DID

### DID Definition
DID is a globally unique identifier that does not require a centralized registration authority because it is registered with distributed ledger technology (DLT) or other form of decentralized network. 
```
did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876

did       -> URL scheme identifier
tw        -> Identifier for the DID method
12AE66CDc592e10B60f9097a7b0D3C59fce29876 -> DID method-specific identifier is a checksumed address like Etheruem.
```

### DID Document
A DID document is the resource that is associated with a decentralized identifier (DID). DID documents typically express verification methods (such as public keys) and services that can be used to interact with a DID controller.



### Create DID
```json
POST /dids

{
  "did": "did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876",
  "publicKey": "0440b3fa8e848297ff26b04088263101fa87d3541ac48bbc32fe7b77b73246578241236ab6097d4012ac17a514272a54a7b728790e914bbbff431e49d421aa1eef" ,
  "signature": "54CnhKVqE63rMAeM1b8CyQjL4c8teS1DoyTfZnKXRvEEGWK81YA6BAgQHRah4z1VV4aJpd2iRHCrPoNTxGXBBoFw"
}
```
Returns `201`
### Read DID
```json
GET /dids/{did}
```
Returns `200` with Document Object in JSON-LD format
```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876",
  "created":"2020-04-20T10:27:27.326Z",
  "publicKeys": [
	{
	  "id": "did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876#keys-1",
	  "type": "Secp256k1",
	  "publicKey": "0440b3fa8e848297ff26b04088263101fa87d3541ac48bbc32fe7b77b73246578241236ab6097d4012ac17a514272a54a7b728790e914bbbff431e49d421aa1eef"
	}]
}
```

### Update DID
```json
PUT /dids/{did}
{
  "publicKey": "0189140b3fa8e848297ff26b04088263101fa87d3541ac48bbc32fe7b77b73246578241236ab6097d4012ac17a514272a54a7b728790e914bbbff431e49d421aa1f12",
  "signatures": [
    {
        "id":"did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876#keys-1",
        "type":"Secp256k1",
        "signature":"54CnhKVqE63rMAeM1b8CyQjL4c8teS1DoyTfZnKXRvEEGWK81YA6BAgQHRah4z1VV4aJpd2iRHCrPoNTxGXBBoFw"
    },
    {
        "id":"did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876#keys-2",
        "type":"Secp256k1",
        "signature":"26kkhZbQLSNvEKbPvx18GRfSoVMu2bDXutvnWcQQyrGxqz5VKijkFV2GohbkbafPa2WqVad7wnyLwx1zxjvVfvSa"
    }
  ]
}
```
Returns `200` with Document Object
```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876",
  "created":"2020-04-20T10:27:27.326Z",
  "updated":"2020-04-20T12:30:20.326Z",
  "publicKeys": [
	{
	  "id": "did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876#keys-1",
	  "type": "Secp256k1",
	  "publicKey": "0189140b3fa8e848297ff26b04088263101fa87d3541ac48bbc32fe7b77b73246578241236ab6097d4012ac17a514272a54a7b728790e914bbbff431e49d421aa1f12"
	}
  ]
}
```

### Revoke DID
```json
DELETE /dids/{did}
{
  "signatures": [
	{
	 "id":"did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876#keys-2",
	 "type":"Secp256k1",
	 "signature":"26kkhZbQLSNvEKbPvx18GRfSoVMu2bDXutvnWcQQyrGxqz5VKijkFV2GohbkbafPa2WqVad7wnyLwx1zxjvVfvSa"
	}
  ]
}
```
Returns `204`

### Privacy considerations

### Security considerations


## Verifiable Credential
Verifiable Credential is a set of one or more claims/assertions. It has `Metadata`, `Claim` and `Proof` properties. A alumniCredential, for example, the Metadata of it could include the credential's type, issuer, issue time, etc. A Claim could be **who is a alumni of an University** and the University then sign the credential in digital to give a proof for later verify.

![A verifiable credential example](./imgs/verifiable-credential.svg)
### VC Definition
Verifiable Credential follows the [JWT standard](https://tools.ietf.org/html/rfc7519).
```
header.payload.signature
```
#### Header
```
{
  "alg": "ES256",
  "typ": "JWT"
}
```
* alg: the signing algorithm
* typ: the type of Token
	* JWT: JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object.
    
#### Payload
```
{
   "@context": ["https://www.w3.org/2018/credentials/v1",
   "https://blockchain.thoughtworks.cn/credentials/v1"],
   "id": "xyzxyzxyz",
   "ver": "0.7.0",
   "iss": "did:tw:f340A40E197c77979bE698401fd03251741137eE",
   "iat": 1588059342,
   "exp": 1651131342,
   "typ": ["VerifiableCredential", "HealthyCredential"],
   "sub": {
     "id": "did:tw:12AE66CDc592e10B60f9097a7b0D3C59fce29876",
     "healthyStatus": {
       "typ": "HealthyStatus",
       "val": "HEALTHY"
     }
   }
}
```

#### Signature
JWT Digital Signature is way to give a proof for the verifiable credential.
```
signature = Base64(ECDSASHA256(Base64(header).Base64(payload), pubkey, prikey))
```

## Use Cases

### Authentication
![authenication flow](./imgs/authentication-flow.png)

### Issue Claims
![issue claims](./imgs/issue-claims.svg)

### Verify Claims
![verify claims](./imgs/verify-claims.svg)