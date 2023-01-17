---
id: submit-mapping-request
title: Mapping Tokens
description:  A guide on how to map tokens between Ethereum and Polygon Chains using the PoS Bridge
keywords:
  - docs
  - polygon wiki
  - token mapping
  - pos bridge
  - polygon
  - goerli
  - ethereum
  - testnet
  - mainnet
image: https://wiki.polygon.technology/img/polygon-wiki.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

Mapping is necessary in order to transfer your assets to and from the Ethereum and Polygon. We offer two bridges to do the same. More details on the bridge can be understood from [here](/develop/ethereum-polygon/getting-started.md).

:::tip

The Polygon PoS bridge is available for both Polygon Mainnet as well as Mumbai Testnet. More information about the bridge can be found [<ins>here</ins>](/develop/ethereum-polygon/pos/getting-started.md).

:::

## Steps to submit a mapping request

In order to map tokens between Ethereum and Polygon, you have to submit a mapping request. It has to be submitted on [https://mapper.polygon.technology/](https://mapper.polygon.technology/). Open the link and click on the **Map New Token** button on the top right corner to create a new mapping request.

<img src={useBaseUrl("img/token-mapping/mapping-tool.png")} />

**Step 1 &rarr;** Choose the network on which you want to map tokens. You can choose Goerli-Mumbai for Testnet, and Ethereum-Polygon for the Mainnet.

**Step 2 &rarr;** Select the type of token you are mapping from **ERC20**, **ERC721**, or **ERC1155**. For mapping any other token standard, you can reach out to the Polygon team on [Discord](https://discord.com/invite/0xPolygon) or create a ticket [here](https://support.polygon.technology/support/home) and keep "Token Mapping" in the ticket title.

**Step 3 &rarr;** Enter your **Ethereum / Goerli** token address in the **Ethereum Token Address** field. Make sure your token contract code has been verified on the [Ethereum](https://etherscan.io/) / [Goerli](https://goerli.etherscan.io/) blockchain explorers. if your root token is verified ✅, the **name**, **symbol**, and **decimal** fields will be automatically filled and these fields are uneditable.

In case of a custom token mapping, you can visit our [FxPortal](/develop/l1-l2-communication/fx-portal.md) and use the data provided to build your custom token.
