---
rip: 77XX
title: Preinstall deterministic deployment factories
description: Proposal to add deployment factory contracts at common addresses to enable deterministic contract deployments
author: Richard Meissner (@rmeissner)
discussions-to: https://ethereum-magicians.org/t/eip-proposal-create2-contract-factory-precompile-for-deployment-at-consistent-addresses-across-networks/6083/29
status: Draft
type: Standards Track
category: Core
created: 2024-06-24
---

## Abstract

This proposal introduces a set of deployment factory contracts to be preinstalled at common addresses. This will enable deterministic contract deployment to the same address across different networks.

## Motivation

Many projects rely on deployment factories that make use of `create2` to increase the security and determinism of deployments. Utilizing `create2` has a couple of major advantages:
- The address does not depend on the deployment key, therefore reducing the need to manage a deployment key
- The address of the contract is tied to the deployment code, which provides strong guarantees on the deployed code
- Contracts can be redeployed in case of a selfdestruct

The downside is that it is still necessary to deploy these deployment factories. There are two common ways to do so:
- Utilize a randomly generated signature for fixed deployment transactions
- Manage a deployment key for the deployment factory.

Using a randomly generated signature for a fixed deployment transaction has the advantage that this is a fully trustless process, and no deployment key has to be managed. But the parameter of the signed deployment transaction cannot be changed, therefore it is not possible to adjust the gas price, gas limits or set the chain ID. 

Providing a stable way for deterministic and trustless deployments will become even more important with EIPs like EIP-7702. The strong guarantees provided by a deployment factory are extremely helpful in this case, as this EIP depends on the code at a specific address.

This RIP proposes aligning on a set of deterministic deployment factories and preinstalling these on all Rollups to enable developers to rely on such libraries. Some rollups, such as Optimism, already have a set of preinstalled contracts for deterministic deployments, and this proposal aims to extend this set to other Rollup stacks to prevent fragmentation.

## Specification

### Factories

The following factories should be added
 - [Deterministic Deployment Proxy](https://github.com/Arachnid/deterministic-deployment-proxy) at `0x4e59b44847b379578588920ca78fbf26c0b4956c`
 - [Safe Singleton Factory](https://github.com/safe-global/safe-singleton-factory) at `0x914d7Fec6aaC8cd542e72Bca78B30650d45643d7`
 - [CreateX](https://github.com/pcaversaccio/createx) at `0xba5Ed099633D3B313e4D5F7bdc1305d3c28ba5Ed`
 - [Create2 Deployer](https://github.com/pcaversaccio/create2deployer) at `0x13b0D85CcB8bf860b6b79AF3029fCA081AE9beF2`

### References

- [OP Stack Preinstalls](https://docs.optimism.io/builders/chain-operators/features/preinstalls)

## Rationale

### Why not align on one factory?

The listed factories are already in active use on multiple networks. To ensure future compatibility without having to redeploy existing contracts, it makes the most sense to enable a set of deployment factories that also cover a large part of the existing ecosystem.

## Backwards Compatibility

No backward compatibility issues were found.