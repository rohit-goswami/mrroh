---
title: "zkFetch: Secure Data Fetching with Zero-Knowledge Proofs"
date: 2024-08-25
draft: false
author: mr-unomi
tags: ["zkFetch", "zero-knowledge proofs", "Reclaim Protocol"]
categories: ["tech", "Web3", "zkp"]
description: "zkFetch: Secure Data Fetching with Zero-Knowledge Proofs"
---


## Introduction

zkFetch is a library that extends the functionality of a standard HTTP fetch operation by adding a zero-knowledge proof component. The main goal is to allow users to fetch data from remote resources and generate a cryptographic proof of the fetch operation and its result, without revealing sensitive information like API keys or private headers.

## Core Concepts

### Zero-Knowledge Proofs (ZKP)

At the heart of zkFetch is the concept of zero-knowledge proofs. A ZKP allows one party (the prover) to prove to another party (the verifier) that a statement is true, without revealing any information beyond the validity of the statement itself. In our case, we're proving that a specific HTTP request was made and a specific response was received, without revealing sensitive details of the request.

### Reclaim Protocol

zkFetch is built on top of the [Reclaim Protocol](https://www.reclaimprotocol.org/), which provides the underlying infrastructure for generating and verifying zero-knowledge proofs. The Reclaim Protocol handles the complex cryptographic operations required for ZKPs.

## Library Architecture

The zkFetch library consists of several key components:

1. ReclaimClient class
2. zkFetch method
3. Utility functions
4. Error handling

Let's explore each of these in detail.

### 1.  ReclaimClient Class

The ReclaimClient is the main class that users interact with. It's initialized with an application ID and secret, which are used to authenticate with the Reclaim Protocol.

```typescript
export class ReclaimClient {
  applicationId: string;
  applicationSecret: string;
  logs?: boolean;
  sessionId: string;

  constructor(applicationId: string, applicationSecret: string, logs?: boolean) {
    validateApplicationIdAndSecret(applicationId, applicationSecret);
    this.applicationId = applicationId;
    this.applicationSecret = applicationSecret;
    this.sessionId = v4().toString();
    logger.level = logs ? "info" : "silent";
  }

  // ... zkFetch method goes here
}
```

The constructor validates the application ID and secret, sets up logging, and generates a unique session ID for each instance.

### 2. zkFetch Method

The zkFetch method is the core of the library. It performs the following steps:

1. Validates input parameters
2. Prepares the fetch options
3. Sends a log indicating the start of verification
4. Performs the actual fetch operation
5. Generates a zero-knowledge proof of the fetch operation
6. Sends a log indicating successful proof generation
7. Returns the proof

Here's a simplified version of the method:

```typescript
async zkFetch(url: string, options?: Options, secretOptions?: secretOptions, retries = 1, retryInterval = 1000) {
  // Input validation
  validateURL(url, "zkFetch");
  if (options !== undefined) {
    assertCorrectnessOfOptions(options);
  }
  if (secretOptions)  {
    assertCorrectionOfSecretOptions(secretOptions);
  }

  // Prepare fetch options
  const fetchOptions = {
    method: options?.method || HttpMethod.GET,
    body: options?.body,
    headers: { ...options?.headers, ...secretOptions?.headers },
  };

  // Start verification log
  await sendLogs({
    sessionId: this.sessionId,
    logType: LogType.VERIFICATION_STARTED,
    applicationId: this.applicationId,
  });

  // Perform fetch with retries
  let attempt = 0;
  while (attempt < retries) {
    try {
      const response = await fetch(url, fetchOptions);
      if (!response.ok) {
        throw new FetchError(`Failed to fetch ${url} with status ${response.status}`);
      }
      const fetchResponse = await response.text();

      // Generate proof
      const claim = await createClaimOnWitness({
        name: 'http',
        params: {
          method: fetchOptions.method as HttpMethod,
          url: url,
          responseMatches: [
            {
              type: "contains",
              value: fetchResponse,
            },
          ],
          headers: options?.headers,
          geoLocation: options?.geoLocation,
          responseRedactions: [],
          body: fetchOptions.body,
        },
        secretParams: {
          cookieStr: "abc=pqr",
          ...secretOptions,
        },
        ownerPrivateKey: this.applicationSecret,
        logger: logger,       
        client: {
          url: WITNESS_NODE_URL,
        }
      });

      // Log successful proof generation
      await sendLogs({
        sessionId: this.sessionId,
        logType: LogType.PROOF_GENERATED,
        applicationId: this.applicationId,
      });

      return claim;
    } catch (error: any) {
      // Handle retries
      attempt++;
      if (attempt >= retries) {
        await sendLogs({
          sessionId: this.sessionId,
          logType: LogType.ERROR,
          applicationId: this.applicationId,
        });
        logger.error(error);
        throw error;
      }
      await new Promise((resolve) => setTimeout(resolve, retryInterval));
    }
  }
}
```

### Proof Generation

The core of zkFetch is the proof generation, handled by the `createClaimOnWitness` function from the Reclaim Protocol. This function takes the details of the HTTP request and response and generates a zero-knowledge proof.

The proof includes:
- The method and URL of the request
- A hash of the response
- Public headers
- Any specified geoLocation

Importantly, it does not include any secret parameters (like API keys), ensuring that sensitive information remains private.

### 3. Utility Functions

Several utility functions support the main zkFetch operation:

- `validateApplicationIdAndSecret`: Ensures the provided application ID and secret are valid.
- `validateURL`: Checks that the provided URL is correctly formatted.
- `assertCorrectnessOfOptions`: Validates the options passed to zkFetch.
- `assertCorrectionOfSecretOptions`: Ensures secret options don't contain disallowed fields.
- `sendLogs`: Sends logs to a backend service for monitoring and debugging.

### 4. Error Handling

The library includes custom error classes to provide clear and specific error messages:

- `InvalidParamError`: For invalid input parameters.
- `DisallowedOptionError`: For attempts to use disallowed options.
- `InvalidMethodError`: For unsupported HTTP methods.
- `FetchError`: For failures in the fetch operation.
- `NetworkError`: For network-related issues.
- `ApplicationError`: For application-specific errors.

##  Verification Process

While not part of the zkFetch library itself, the verification process is a crucial part of the overall system. Verification is typically done using the Reclaim Protocol's `verifySignedProof` function:

```javascript
const isProofVerified = await Reclaim.verifySignedProof(proof);
```

This function checks the cryptographic signatures and recalculates the proof identifier to ensure the proof hasn't been tampered with.

## Use Cases and Benefits

zkFetch is particularly useful in scenarios where:

1. You need to prove you made a specific API call without revealing your API key.
2. You want to verify the integrity of data fetched from a third-party source.
3. You need to generate proofs of API responses for later verification.

The main benefits are:
- Privacy: Sensitive information like API keys remain secret.
- Integrity: The proof ensures the response hasn't been tampered with.
- Verifiability: Third parties can verify the fetch operation without access to private credentials.


----

zkFetch extends the capabilities of standard HTTP requests by adding a layer of cryptographic proof. It leverages zero-knowledge proofs and the Reclaim Protocol to allow for verifiable data fetching while maintaining the privacy of sensitive information. This makes it a powerful tool for scenarios requiring data integrity and verifiability without compromising on security.

Thank you for reading my tech blog :)