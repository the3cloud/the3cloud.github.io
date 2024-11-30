# Architecture

The3Cloud is built on three core components:
1. The3Cloud zkTLS implementation
    * Implements full TLS stack verification via ZkVM with Risc0 and SP1 backends.
    * Ensures:
        - **Authentication of Communication and Identity**: Verifies certificate chains.
        - **Data Integrity**: Maintains trust in data exchanges while preserving confidentiality.
2. The3Cloud zkTLS Smart Contracts
    * **zkTLS Gateway**: Facilitates dApp-prover interactions.
    * **zkTLS Account**: Manages zkTLS client accounts.
    * **zkTLS Manager**: Handles account creation and fee management.
3. The3Cloud Dashboard
    * Smart Accounts: Enables OAuth-based social identity verification with EIP-4337 support.
    * dApp client account management: handles for dApp registration and management with The3Cloud.
    * Proof Scan: Allows to look up proofs submitted by provers on blockchains, ensuring transparency and trust.