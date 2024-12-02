# zkTLS Contracts

The zkTLS core contracts consist of three major components: the ZkTLSManager for account and prover management, the ZkTLSGateway for handling TLS requests and emitting events for relayering, and the ZkTLSAccount/ZkTLSClient for dApp interactions. Currently deployed on Linea Sepolia testnet.

## Deployment and Interaction Flow

The zkTLS contract system follows a deterministic deployment pattern and controlled interaction flow:

1. **Deterministic Deployment**: Both the ZkTLSGateway and ZkTLSManager are deployed using Create2 for deterministic addresses across different networks, ensuring consistent contract locations for better interoperability.

2. **Account Creation**: ZkTLSAccount/Client contracts are exclusively created through the ZkTLSManager, which maintains control over account creation and management.

3. **dApp Integration**: 
   - dApps can interact with the system through beacon proxy patterns for consistent interface upgrades
   - Access control is managed through the manager proxy, ensuring secure and controlled interactions with the system

---

More details about contracts API can be found at [zkTLS Contracts Documentation](https://docs.the3.cloud/zktls-contracts/).
