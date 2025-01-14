# TOP LEVEL
    about.md
    index.md

# CLI Tools [UNCHANGED] 
    ├── algokey
    ├── conduit
    ├── diagcfg
    ├── goal
    ├── indexer
    ├── kmd.md
    └── tealdbg

# Reference
    ├── accounts.md - collapse create/index/rekey to single page
    ├── assets.md
    ├── atomic-transfers
    │   ├── with_atc.md
    │   └── wthout_atc.md
    ├── conduit
    │   ├── exporters.md - Collapse to single pages for each
    │   ├── importers.md
    │   └── processors.md
    ├── smart-contracts 
    │   ├── avm - moved `teal` up 1 directory, some of these pages could probably be combined
    │   │   ├── index.md
    │   │   ├── guidelines.md
    │   │   ├── teal.md 
    │   │   ├── opcodes.md
    │   │   └── specification.md
    │   ├── pyteal.md   - single page, prefer to link to specific docs/videos/whatever
    │   ├── beaker.md   - single page, prefer to link to specific docs/videos/whatever
    │   ├── algokit.md  - single page, prefer to link to specific docs/videos/whatever
    │   ├── abi.md 
    │   ├── applications 
    │   │   ├── lifecycle.md - move frontend/apps.md to here
    │   │   ├── state.md - new page describing the state storage options and limitations
    │   │   ├── inner-txns.md - new page describing the ability to issue inner txns + describe c2c limits 
    │   │   └── index.md
    │   ├── debugging.md
    │   ├── guidelines.md 
    │   ├── index.md
    │   └── smartsigs
    │       ├── index.md - combine modes.md and index.md 
    │       └── walkthrough.md - move frontend/smartsigs.md to here
    ├── encoding.md
    ├── index.md
    ├── indexer.md - bit torn on this, we dont have a page for the other services like algod/kmd 
    ├── parameter_tables.md
    ├── stateproofs
    │   ├── index.md
    │   └── light_client.md
    ├── transactions
    │   ├── index.md
    │   ├── offline_transactions.md  
    │   ├── signatures.md
    │   └── transactions.md
    ├── ARCs - New directory where we can list ARCs and link off to those pages
    │   └── index.md
    └── wallets  - renamed walletconnect to more general `wallets`, we can describe how to create a Signer and list wallet options
        ├── index.md 
        ├── payment_prompts.md
        └── walletconnect-schema.md


# Overview 
    ├── basics
    │   ├── what_is_blockchain.md
    │   ├── where_to_start.md
    │   └── why_algorand.md
    ├── algorand-networks
    │   ├── betanet.md
    │   ├── index.md
    │   ├── mainnet.md
    │   └── testnet.md
    ├── algorand_consensus.md - Moved here from Reference
    ├── development 
    │   ├── index.md
    |   ├── algokit.md
    │   ├── devenv.md
    │   ├── pyteal.md
    │   ├── beaker.md
    ├── useful_resources.md - Moved here from Reference
    └── tokenization.md - collapse ft/nft/security into a single page describing ASAs, move code (if wanted) to Reference


# Rest APIs  [UNCHANGED]
    ├── algod.md
    ├── indexer.md
    ├── kmd.md
    └── restendpoints.md

# Run a Node  [UNCHANGED]
    ├── operations
    │   ├── catchup.md
    │   └── switch_networks.md
    ├── participate
    │   ├── generate_keys.md
    │   ├── index.md
    │   ├── offline.md
    │   ├── online.md
    │   └── renew.md
    ├── reference
    │   ├── artifacts.md
    │   ├── config.md
    │   ├── relay.md
    │   └── telemetry-config.md
    └── setup
        ├── indexer.md
        ├── install.md
        ├── node-troubleshooting.md
        └── types.md

# SDKs [UNCHANGED]
    ├── index.md
    ├── go
    │   └── index.md
    ├── java
    │   └── index.md
    ├── javascript
    │   └── index.md
    └── python
        └── index.md
