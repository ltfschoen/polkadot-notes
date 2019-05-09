# Polkadot

# Table of Contents
  * [Chapter 150 - Sample Runtime](#chapter-150)
  * [Chapter 200 - Debugging](#chapter-200)
  * [Chapter 209 - Substrate Dependencies](#chapter-205)
  * [Chapter 210 - External Dependencies](#chapter-210)
  * [Chapter 998 - Core Data Types](#chapter-998)
  * [Chapter 999 - Glossary](#chapter-999)
  * [Chapter 1000 - Community](#chapter-1000)
  * [Chapter 10000 - Appendix 1 - Example staging.json](#chapter-10000)

## Chapter 150 - Sample Runtime <a id="chapter-150"></a>

### Runtime Dependencies

* parity-codec / derive
* sr-io / runtime-io
* sr-primitives / runtime-primitives
* sr-std / rstd
* sr-version / version
* srml-aura / aura
* srml-balances / balances
* srml-client / client
* srml-consensus / consensus
* srml-executive / executive
* srml-indices / indices
* srml-sudo / sudo
* srml-support / support
* srml-system / system
* srml-timestamp / timestamp
* safe-mix
* serde_derive
* serde
* substrate-consensus-aura-primitives / consensus-aura
* substrate-consensus-authorities / consensus_authorities
* substrate-offchain-primitives / offchain-primitives
* substrate-primitives / primitives

### CLI Library

* Imports
  * chain_spec
  * service
  * cli

* Uses
  * substrate_cli
    * VersionInfo
    * IntoExit
    * error

* Functions
  * `run` - Run with cli, version info, and option to cli exit


## Chapter 200 - Debugging <a id="chapter-200"></a>

* Detailed logs
  ```
  RUST_LOG=debug RUST_BACKTRACE=1 cargo run -- --dev
  ```

## Chapter 209 - Substrate Dependencies <a id="chapter-209"></a>

* basic-authorship

* consensus

* ctrlc

* inherents

* network

* test-project-runtime

* primitives

* sr-io

* substrate-cli

* substrate-client

* substrate-executor

* substrate-service

* transaction-pool

## Chapter 210 - External Dependencies <a id="chapter-210"></a>

* error-chain

* exit-future

* future

* hex-literal

* log

* parity-codec

* parking_lot

* tokio

* trie-root

## Chapter 998 - Core Data Types <a id="chapter-998"></a>

* Core Datatypes
  * About
    * Part of Substrate "core"
    * Interface to work within Substrate
    * Each correspond to a Rust `trait`
    * Generic reference implementations for each are in the SRML
  * Types
    * `Hash` type encodes cryptographic digest of data (typically 256-bit)
    * `BlockNumber` type encodes all ancestors of a block (typically 32-bit)
    * `DigestItem` type must be able to encode at least one "hard-wired" alternative relevant to consensus and change-tracking, and any number of "soft-coded" variants, relative to specific modules in the runtime.
    * `Header` type represents cryptographically or otherwise all information relevant to a block. It includes a parent hash, storage root, extrinsics trie root, digest, and block number.
    * `Extrinsic` type represents a single piece of data external to the blockchain that is recognised by it. This typically involves one or more signatures, and some sort of encoded instruction (e.g. for transferring ownership of funds or calling into a smart contract).
    * `Block`, a combination of a `Header` and a series of `Extrinsic`s, together with a specification of the hashing algorithm to be used.
  * Modules
    * Module Aggregation by combining types across different runtimes
  * Usage
    * Option 1 - Running with Predesigned Bundled Substrate Node chain
      * Creating a "chainspec" JSON file by configuring the parameters of all the new chain's runtime modules (i.e. balances, staking, block-period, fees, and governance) that the the chain will include, an initial set of authorities/validators (identify accounts we trust to maintain the system and start authoring blocks), endowed accounts, and the runtime code itself for the genesis block prior to launching your own blockchain. Predefined  "chainspec" JSON files include:
        * "staging"
          ```
          substrate build-spec --chain=staging > ~/Desktop/staging.json
          ```
        * [Appendix 1 - Example staging.json](#chapter-10000)

      * This is the least customizable.
      * TODO - Tutorial is "Deploying a Substrate Node chain".
    * Option 2 - Running with the SRML
      * Configuring the Substrate client's block authoring logic
      * Other changes through on-chain logic, including:
        * Selecting and composing modules from the SRML
        * Adding new custom modules
        * Changing datatypes
      * Using the existing Substrate binary for block authoring and syncing
      * Tweaking the block authoring logic by creating a separate binary for use by validators
      * Polkadot Relay Chain is built this way
      * TODO - Tutorial is "Creating a custom Substrate chain"
    * Option 3 - Generic
      * Ignore the SRML
      * Design and implement a runtime from scratch that is compatible with Substrate Node's abstract block authoring logic, including possible alteration of the Substrate client's
      block authoring logic, header, and block serialization formats, in either Rust or another language that targets WebAssembly, and then contruct a new genesis block from the Wasm blob and launch your chain with the existing Rust-based Substrate client.
      * Highest development effort
      * Maximum freedom to innovate and a long-term upgrade path

## Chapter 999 - Glossary <a id="chapter-999"></a>

* https://docs.substrate.dev/docs/glossary
* Authorities vs Validators
  * "Authorities" is used in Substrate and low-level BFT/finality mechanisms refer to the set of keys "in charge" of consensus and have their own special type for identification `AuthorityId`.
  * "Validators" is used in higher-level contexts to identify one of the set of accounts in the system from which the authorities are derived, and dependent on the chain, other key groups for system maintenance. In our case, they are identified by `AccountId` and in some cases they may be binary compatible. Both are Ed25519 public keys. They are different types so conversion should be explicitely performed, and they cannot be assumed to be the same.

## Chapter 10000 - Appendix 1 - Example staging.json <a id="chapter-10000"></a>

<details><p><pre>
{
  "name": "Staging Testnet",
  "id": "staging_testnet",
  "bootNodes": [
    "/ip4/127.0.0.1/tcp/30333/p2p/QmQer2Q44aT66DZALSuUpM4S6YBm5gDsJV1jR7aocmU6mu"
  ],
  "telemetryEndpoints": [
    [
      "wss://telemetry.polkadot.io/submit/",
      0
    ]
  ],
  "protocolId": null,
  "consensusEngine": null,
  "properties": null,
  "genesis": {
    "runtime": {
      "system": null,
      "timestamp": {
        "minimumPeriod": 3
      },
      "consensus": {
        "authorities": [
          "5HBoHDLMR4jPwB6BCLyd2qfYBHytFhGs8fsa1h5PzhYd3WBq",
          "5HAWoPYfyYFHjacy8H2MDmHra7jVrPtBfFMPgd8CadpSqotL",
          "5ChJ5wjqy2HY1LZw1EuQPGQEHgaS9sFu9yDD6KRX7CzwidTN",
          "5HcLeWrsfL9RuGp94pn1PeFxP7D1587TTEZzFYgFhKCPZLYh"
        ],
        "code": "0x0061736d010000000"
      },
      "indices": {
        "ids": [
          "5GTG5We6twtoF6S4kUXJ77rWBsHBoHLS3JVf5KvvnxKdGQZr",
          "5Ef78yxqfaxVzrFCemYcSgwVtMV85ywykhLNm5WKTsZV22HZ",
          "5CqiScHtxUatcQpck1tUks51o3pSjKsdCi2CLEHvMM7tc4Qi",
          "5HpF9orzkmJ9ga3yrzNS9ckifxF3tbQjadEmCEiZJQ2fPgun",
          "5EFByrDMMa2m9hv4jrpykXaUyqjJ9XZH81kJE4JBa1Sz2psT"
        ]
      },
      "balances": {
        "existentialDeposit": 100000000000000,
        "transferFee": 1000000000000,
        "creationFee": 1000000000000,
        "transactionBaseFee": 1000000000000,
        "transactionByteFee": 10000000000,
        "balances": [
          [
            "5GTG5We6twtoF6S4kUXJ77rWBsHBoHLS3JVf5KvvnxKdGQZr",
            1000000000000000000000
          ],
          [
            "5Ef78yxqfaxVzrFCemYcSgwVtMV85ywykhLNm5WKTsZV22HZ",
            10000000000000000
          ],
          [
            "5CqiScHtxUatcQpck1tUks51o3pSjKsdCi2CLEHvMM7tc4Qi",
            10000000000000000
          ],
          [
            "5HpF9orzkmJ9ga3yrzNS9ckifxF3tbQjadEmCEiZJQ2fPgun",
            10000000000000000
          ],
          [
            "5EFByrDMMa2m9hv4jrpykXaUyqjJ9XZH81kJE4JBa1Sz2psT",
            10000000000000000
          ]
        ],
        "vesting": []
      },
      "session": {
        "validators": [
          "5HWfszmRMbzcjGmumYkkHtNJbi9y428JHgPeftVenvDgVUjh",
          "5HNZXnSgw21idbuegTC1J8Txkja97RPnnWkX68ewnrJDec2Z",
          "5HVwyfB3LRsFXm7frEHDYyhwdpTYDRWxEqDKBYVyLi6DsPXq",
          "5D22qQJsLm2JUh8pEfrKahbkW21QQrHTkm4vUteei67fadLd"
        ],
        "sessionLength": 50,
        "keys": [
          [
            "5HWfszmRMbzcjGmumYkkHtNJbi9y428JHgPeftVenvDgVUjh",
            "5HBoHDLMR4jPwB6BCLyd2qfYBHytFhGs8fsa1h5PzhYd3WBq"
          ],
          [
            "5HNZXnSgw21idbuegTC1J8Txkja97RPnnWkX68ewnrJDec2Z",
            "5HAWoPYfyYFHjacy8H2MDmHra7jVrPtBfFMPgd8CadpSqotL"
          ],
          [
            "5HVwyfB3LRsFXm7frEHDYyhwdpTYDRWxEqDKBYVyLi6DsPXq",
            "5ChJ5wjqy2HY1LZw1EuQPGQEHgaS9sFu9yDD6KRX7CzwidTN"
          ],
          [
            "5D22qQJsLm2JUh8pEfrKahbkW21QQrHTkm4vUteei67fadLd",
            "5HcLeWrsfL9RuGp94pn1PeFxP7D1587TTEZzFYgFhKCPZLYh"
          ]
        ]
      },
      "staking": {
        "validatorCount": 7,
        "minimumValidatorCount": 4,
        "sessionsPerEra": 12,
        "sessionReward": 2065,
        "offlineSlash": 1000000,
        "offlineSlashGrace": 4,
        "bondingDuration": 600,
        "invulnerables": [
          "5HWfszmRMbzcjGmumYkkHtNJbi9y428JHgPeftVenvDgVUjh",
          "5HNZXnSgw21idbuegTC1J8Txkja97RPnnWkX68ewnrJDec2Z",
          "5HVwyfB3LRsFXm7frEHDYyhwdpTYDRWxEqDKBYVyLi6DsPXq",
          "5D22qQJsLm2JUh8pEfrKahbkW21QQrHTkm4vUteei67fadLd"
        ],
        "currentEra": 0,
        "currentSessionReward": 0,
        "stakers": [
          [
            "5Ef78yxqfaxVzrFCemYcSgwVtMV85ywykhLNm5WKTsZV22HZ",
            "5HWfszmRMbzcjGmumYkkHtNJbi9y428JHgPeftVenvDgVUjh",
            10000000000000000,
            "Validator"
          ],
          [
            "5CqiScHtxUatcQpck1tUks51o3pSjKsdCi2CLEHvMM7tc4Qi",
            "5HNZXnSgw21idbuegTC1J8Txkja97RPnnWkX68ewnrJDec2Z",
            10000000000000000,
            "Validator"
          ],
          [
            "5HpF9orzkmJ9ga3yrzNS9ckifxF3tbQjadEmCEiZJQ2fPgun",
            "5HVwyfB3LRsFXm7frEHDYyhwdpTYDRWxEqDKBYVyLi6DsPXq",
            10000000000000000,
            "Validator"
          ],
          [
            "5EFByrDMMa2m9hv4jrpykXaUyqjJ9XZH81kJE4JBa1Sz2psT",
            "5D22qQJsLm2JUh8pEfrKahbkW21QQrHTkm4vUteei67fadLd",
            10000000000000000,
            "Validator"
          ]
        ]
      },
      "democracy": {
        "launchPeriod": 100,
        "minimumDeposit": 5000000000000000,
        "publicDelay": 100,
        "maxLockPeriods": 6,
        "votingPeriod": 100
      },
      "councilVoting": {
        "cooloffPeriod": 57600,
        "votingPeriod": 14400,
        "enactDelayPeriod": 0
      },
      "councilSeats": {
        "candidacyBond": 1000000000000000,
        "voterBond": 100000000000000,
        "presentSlashPerVoter": 1000000000000,
        "carryCount": 6,
        "presentationDuration": 14400,
        "inactiveGracePeriod": 1,
        "approvalVotingPeriod": 28800,
        "termDuration": 403200,
        "desiredSeats": 0,
        "activeCouncil": []
      },
      "grandpa": {
        "authorities": [
          [
            "5HBoHDLMR4jPwB6BCLyd2qfYBHytFhGs8fsa1h5PzhYd3WBq",
            1
          ],
          [
            "5HAWoPYfyYFHjacy8H2MDmHra7jVrPtBfFMPgd8CadpSqotL",
            1
          ],
          [
            "5ChJ5wjqy2HY1LZw1EuQPGQEHgaS9sFu9yDD6KRX7CzwidTN",
            1
          ],
          [
            "5HcLeWrsfL9RuGp94pn1PeFxP7D1587TTEZzFYgFhKCPZLYh",
            1
          ]
        ]
      },
      "treasury": {
        "proposalBond": 50000,
        "proposalBondMinimum": 100000000000000,
        "spendPeriod": 14400,
        "burn": 500000
      },
      "contract": {
        "transferFee": 1000000000000,
        "creationFee": 1000000000000,
        "transactionBaseFee": 1000000000000,
        "transactionByteFee": 10000000000,
        "contractFee": 1000000000000,
        "callBaseFee": 1000,
        "createBaseFee": 1000,
        "gasPrice": 1000000000,
        "maxDepth": 1024,
        "blockGasLimit": 10000000,
        "currentSchedule": {
          "version": 0,
          "put_code_per_byte_cost": 1,
          "grow_mem_cost": 1,
          "regular_op_cost": 1,
          "return_data_per_byte_cost": 1,
          "event_data_per_byte_cost": 1,
          "event_data_base_cost": 1,
          "sandbox_data_read_cost": 1,
          "sandbox_data_write_cost": 1,
          "max_stack_height": 65536,
          "max_memory_pages": 16
        }
      },
      "sudo": {
        "key": "5GTG5We6twtoF6S4kUXJ77rWBsHBoHLS3JVf5KvvnxKdGQZr"
      }
    }
  }
}
</pre></p></details>