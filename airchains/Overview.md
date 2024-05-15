# Overview

Airchains is a versatile and powerful framework for creating customized rollups with a variety of options. Our framework supports EVM, SVM, and CosmWasm-based rollups

![Banner Image](https://docs.airchains.io/~gitbook/image?url=https%3A%2F%2F123555353-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FSJC99oYNkNjVi7CHXhGM%252Fuploads%252Fsv9aJKidJc64fhXFIIki%252FDoc_cover_image2-01%2520%281%29.png%3Falt%3Dmedia%26token%3Dcd406a92-1cc2-487c-8d69-c0cf338c23a6&width=768&dpr=4&quality=100&sign=10b5a8f7f80269565a825e9d8eb7b6186c4878a3c76e021d7685affcf66c0cd1)

## Framework
Airchains Framework: Simplifying blockchain innovation with a versatile, secure, and user-friendly toolkit for developers

Airchains Framework is a versatile and powerful tool for building your own custom blockchain parts. It works with popular blockchain types like Ethereum, Solana, and Cosmos, and lets you pick where to store your data with options like Celestia and Avail. It's super secure thanks to special zero-knowledge proofs and fits perfectly with Airchains' main chain. This framework is all about making blockchain building easy, secure, and flexible for everyone.

![Banner Image](https://docs.airchains.io/~gitbook/image?url=https%3A%2F%2F123555353-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FSJC99oYNkNjVi7CHXhGM%252Fuploads%252FrnvxWDfp93cuhOeGoSzf%252Fexecution%2520Layer%2520%281%29%2520copy.png%3Falt%3Dmedia%26token%3D33f2335c-60d1-4855-8c41-014ccbffdf03&width=768&dpr=4&quality=100&sign=d18eda4227d87b0bde16fd3f1b57729296801866f8619f5e6a79f04fb30ecc34)

Our framework is designed to empower developers like you to create, innovate, and scale your blockchain solutions with ease and security. Here’s how we make it happen.

## Features of Airchains Framework
#### Multi-Virtual Machine Support

Our Framework is designed to be a versatile tool in the world of blockchain technology, offering support for a variety of virtual machines. This makes it incredibly flexible for different blockchain projects. Here’s what we offer:

- Ethereum Virtual Machine (EVM) Support: Recognizing Ethereum's widespread use, our framework is fully compatible with EVM. This means if you're working with Ethereum's technology, our framework fits right in, making it easier to build and expand your projects.

- Solana Virtual Machine (SVM) Integration: For those interested in using Solana's fast and efficient blockchain system, our framework integrates seamlessly with SVM. This allows for building powerful and scalable applications without a hitch.

- CosmWasm Compatibility: We also support CosmWasm, known for its ability to interconnect different blockchain systems. This is perfect for creating complex applications that work across multiple blockchain networks.

#### Flexible Data Availability
Choose how you store your blockchain data with leading layers like Celestia and Avail. We give you the freedom to select the best fit for your project's needs.
#### Advanced Security
Security is paramount in the blockchain world, and our framework doesn't take it lightly. With our implementation of zero-knowledge proofs, we ensure enhanced security and efficiency for your projects.

#### Smooth Integration with Airchains
Experience seamless integration as your rollups settle effortlessly on Airchains' settlement chain. This integration is the cornerstone of our framework, ensuring reliability and consistency.

## 🔐ZK Proofs
Proofs in the context of zk-rollups (zero-knowledge rollups) are a form of cryptographic evidence that validates all transactions in a rollup block without revealing the actual data of those transactions. Here’s a simplified breakdown:
{% code }
##### What Are zk-Rollups?
zk-Rollups are a type of Layer 2 scaling solution for blockchains like Ethereum. They help in scaling by processing transactions off the main chain (Layer 1) while ensuring the security and decentralization of the network.
{% endcode %}

## Zero-Knowledge Proofs

In blockchain, there are several types of zero-knowledge proofs (ZKPs) which vary in their interaction model and cryptographic techniques:

#### zk-SNARKs

zk-SNARKs are the most well-known ZKP protocol, valued for their quick verification times and minimal proof sizes. Initially, they required a "trusted setup," posing potential security risks, but newer versions have mitigated these concerns with trustless implementations using Multi-Party Computation (MPC).
#### Bulletproofs
Introduced as a scalable solution, Bulletproofs' proof sizes increase logarithmically with the number of inputs, improving over linear-sized proofs. They eliminate the need for a trusted setup, enhancing security, although they lack quantum resistance.
#### zk-STARKs
The latest in ZKP technology, zk-STARKs offer transparency and quantum resistance, without the need for a trusted setup. They are considered more scalable with simpler assumptions, at the cost of larger proof sizes than zk-SNARKs.

###### Feature | zk-SNARKs	  | Bulletproofs  | zk-STARKs

Trusted Setup|Required (traditional), optional (recent) | Not required| Not required
|   |   |   |
|   |   |   |
|   |   |   |
