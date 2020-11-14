+++
title = "Lighthouse Run"
author = ["Aimee Z"]
description = "Lighthouse first try."
date = 2020-08-12
tags = ["blockchain", "lighthouse"]
categories = ["blockchain"]
draft = false
+++

## 2020-08-12 Testing {#2020-08-12-testing}

```shell
validator_client_1  | Your wallet's 12-word BIP-39 mnemonic is:
validator_client_1  |
validator_client_1  | 	enlist whip budget awkward gold glad toast demise grass radar chicken link
validator_client_1  |
validator_client_1  | This mnemonic can be used to fully restore your wallet, should
validator_client_1  | you lose the JSON file or your password.
validator_client_1  |
validator_client_1  | It is very important that you DO NOT SHARE this mnemonic as it will
validator_client_1  | reveal the private keys of all validators and keys generated with
validator_client_1  | this wallet. That would be catastrophic.
validator_client_1  |
validator_client_1  | It is also important to store a backup of this mnemonic so you can
validator_client_1  | recover your private keys in the case of data loss. Writing it on
validator_client_1  | a piece of paper and storing it in a safe place would be prudent.
validator_client_1  |
validator_client_1  | Your wallet's UUID is:
validator_client_1  |
validator_client_1  | 	44b3b046-628a-4e4b-9e3c-db614554c973
validator_client_1  |
validator_client_1  | You do not need to backup your UUID or keep it secret.
validator_client_1  | Running account manager for medalla testnet


beacon_node_1       | Aug 12 17:15:03.191 INFO Libp2p Service                          peer_id: PeerId("16Uiu2HAmMCq2UrX3QR1MUTYPhmn5WMa7jf58zUw5uv5Yj6DYve97"), service: libp2p
beacon_node_1       | Aug 12 17:15:03.191 INFO ENR Initialised                         tcp: Some(9000), udp: None, ip: None, id: 0x54d7..f90b, seq: 1, enr: enr:-KO4QEiEj3uKV2pqOiuKDYPo0LKtXHl0ZXpx7iZJXFwBD_-1Gc2tnqTgYA2oVNYAuOzjktYL_-hmh14zOjD3uqCRGs8Bh2F0dG5ldHOIAAAAAAAAAACEZXRoMpDnp11aAAAAAf__________gmlkgnY0iXNlY3AyNTZrMaEDfwWJzY-NbCoPfSXUuXm6XcmKqRSPK4UmaraCOoeEngKDdGNwgiMo, service: libp2p
beacon_node_1       | Aug 12 17:15:03.192 INFO Listening established                   address: /ip4/0.0.0.0/tcp/9000/p2p/16Uiu2HAmMCq2UrX3QR1MUTYPhmn5WMa7jf58zUw5uv5Yj6DYve97, service: libp2p


validator_client_1  | 1/1	0x8bd5d025f7b5b46d622ffe26b80da6886f1567da0e3a3a38ca2b6dc9bae23da3bc0300d5d32b9c608a4dbcfa21acca73
validator_client_1  | Aug 12 17:15:04.471 WARN Ethereum 2.0 is pre-release. This software is experimental.
validator_client_1  | Aug 12 17:15:04.471 INFO Lighthouse started                      version: Lighthouse/v0.2.2-8a1a4051+
validator_client_1  | Aug 12 17:15:04.471 INFO Configured for testnet                  name: medalla
validator_client_1  | Aug 12 17:15:04.474 INFO Starting validator client               datadir: "/root/.lighthouse/validators", beacon_node: http://beacon_node:5052
validator_client_1  | Aug 12 17:15:04.517 INFO Completed validator discovery           new_validators: 1
validator_client_1  | Aug 12 17:15:05.298 INFO Enabled validator                       voting_pubkey: 0x8bd5d025f7b5b46d622ffe26b80da6886f1567da0e3a3a38ca2b6dc9bae23da3bc0300d5d32b9c608a4dbcfa21acca73
validator_client_1  | Aug 12 17:15:05.298 INFO Initialized validators                  enabled: 1, disabled: 0
validator_client_1  | Aug 12 17:15:05.310 INFO Connected to beacon node                version: Lighthouse/v0.2.2-8a1a4051+/x86_64-linux
validator_client_1  | Aug 12 17:15:05.311 INFO Genesis has already occurred            seconds_ago: 706497
validator_client_1  | Aug 12 17:15:05.333 INFO Loaded validator keypair store          voting_validators: 1
validator_client_1  | Aug 12 17:15:05.335 INFO Block production service started        service: block
validator_client_1  | Aug 12 17:15:05.335 INFO Attestation production service started  next_update_millis: 2664, service: attestation
```