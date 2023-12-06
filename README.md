# PGP Testing

[![license](https://img.shields.io/github/license/interlok-testing/testing_pgp.svg)](https://github.com/interlok-testing/testing_pgp/blob/develop/LICENSE)
[![Actions Status](https://github.com/interlok-testing/testing_pgp/actions/workflows/gradle-build.yml/badge.svg)](https://github.com/interlok-testing/testing_pgp/actions/workflows/gradle-build.yml)

This project tests interlok-pgp features.

## What it does

This project contains various workflows that each demonstrates an interlok-pgp service.

```mermaid
graph LR
  subgraph PGP Encrypt
    direction LR
    FS11[File System] --> FS_C1(FS Consumer)
    FS_C1 --> SE(PGP Encrypt)
    SE --> FS_P1(FS Producer)
    FS_P1 --> FS12[File System]
  end
  subgraph PGP Decrypt
    direction LR
    FS21[File System] --> FS_C2(FS Consumer)
    FS_C2 --> SD(PGP Decrypt)
    SD --> FS_P2(FS Producer)
    FS_P2 --> FS22[File System]
  end
  subgraph PGP Sign
    direction LR
    FS31[File System] --> FS_C3(FS Consumer)
    FS_C3 --> SS(PGP Sign)
    SS --> FS_P3(FS Producer)
    FS_P3 --> FS32[File System]
  end
  subgraph PGP Verify
    direction LR
    FS41[File System] --> FS_C4(FS Consumer)
    FS_C4 --> SV(PGP Verify)
    SV --> FS_P4(FS Producer)
    FS_P4 --> FS42[File System]
  end

  style FS_C1 fill:#FF6C6C
  style FS_C2 fill:#FF6C6C
  style FS_C3 fill:#FF6C6C
  style FS_C4 fill:#FF6C6C
  style FS_P1 fill:#6C79FF
  style FS_P2 fill:#6C79FF
  style FS_P3 fill:#6C79FF
  style FS_P4 fill:#6C79FF
  style SE fill: #F89347
  style SD fill: #F89347
  style SS fill: #F89347
  style SV fill: #F89347
```

Each workflow is made up of:

* A fs-consumer polling a given directory every 5 seconds
* A service from the interlok-pgp component that will perform an action
* A fs-producer which outputs the result to a file

## Getting started

* `./gradlew clean build`
* `(cd ./build/distribution && java -jar lib/interlok-boot.jar)`
* Generate a PGP key
* Load the config from this directory
  - Update the PGP public key location in the encrypt & verify workflows
  - Update the PGP private key location and passphrase in the decrypt &
    sign workflows
* Ensure that the directories are created (`messages/encrypt-in`, `messages/decrypt-in`, `messages/sign-in`, `messages/verify-in`)

## PGP Encrypt

* Copy a file into `messages/encrypt-in`
* Confirm that a PGP encrypted file is placed in `messages/decrypt-in`

## PGP Decrypt

* After encrypting a file, the decrypt workflow should then pick it up
  and decrypt it and place the result in `messages/decrypt-out`

## PGP Sign

* Copy a file into `messages/sign-in`
* Confirm that the file is placed into `messages/verify-in` with an
  inline signature

## PGP Verify

* After signing a file, the verify workflow should then pick it up and
  verify the inline signature, placing the result into
  `messages/verify-out`
