# Sample ZeppelinOS project linked to gnosis-safe

This is an example ZeppelinOS application that connects to the gnosis-safe rinkeby contracts. It allows a user to easily create an upgradeable instance of a gnosis-safe, using only the zOS CLI. It also reuses the existing master deployments made by the gnosis team, removing the need to deploy new versions of the logic contracts.

## gnosis-safe library

This project uses the fork [spalladino/safe-contracts](https://github.com/spalladino/safe-contracts) of the gnosis-safe official repository, which only includes the [`zos.json`](https://github.com/spalladino/safe-contracts/blob/zeppelin_os/zos.json) file describing the project structure, and the [`zos.rinkeby.json`](https://github.com/spalladino/safe-contracts/blob/zeppelin_os/zos.rinkeby.json) file with the addresses of the deployments in Rinkeby (published [here](https://github.com/gnosis/safe-contracts/releases/tag/v0.0.1)).

Note that no changes were required to the contracts of the original gnosis-safe repository, and the same deployments were used as-is.

## How this project was set up

After npm-installing the `gnosis-safe` package from the `spalladino/safe-contracts#zeppelin_os` remote, we only need to initialize a new ZeppelinOS application via the CLI, and link to the `gnosis-safe` library.

```bash
npx zos init zos-sample-with-gnosis-safe 0.0.1
npx zos link gnosis-safe --no-install
npx zos session --network rinkeby --from $FROM
npx zos push
```

This creates our ZeppelinOS app, pushes the Application to the Rinkeby testnet, and links it to the gnosis-safe deployment address (published [in the package](https://github.com/spalladino/safe-contracts/blob/zeppelin_os/zos.json)).

## Creating a new gnosis-safe

Now can directly create a new instance of a gnosis-safe contract as part of our project, simply by running `zos create` and initializing the contract via the `setup` function:

```bash
> npx zos create GnosisSafePersonalEdition --init setup --args "[0xf0A9eD2663311CE436347Bb6F240181FF103CA16,0x3B9861c7D3e7BBd41602d9FfaCEF10BC04867Bc0,0x8C7623AC7Fe2E635Fa256791C25dA2c8851c5F08],3,0x0,0"

Using session with network rinkeby, sender address 0xf0a9ed2663311ce436347bb6f240181ff103ca16
Creating GnosisSafePersonalEdition proxy and calling setup with: 
 - _owners (address[]): ["0xf0A9eD2663311CE436347Bb6F240181FF103CA16","0x3B9861c7D3e7BBd41602d9FfaCEF10BC04867Bc0","0x8C7623AC7Fe2E635Fa256791C25dA2c8851c5F08"]
 - _threshold (uint256): "3"
 - to (address): "0x0"
 - data (bytes): "0"
 TX receipt received: 0x55cf4b70135a8565db1f9ae8869420f9fe271914159bb7581b35f175d889f309
 GnosisSafePersonalEdition proxy: 0xf67dfef8dd417d4a9c34cbb85c525b03fb099e51
```

This creates a new upgradeable instance (ie a proxy) of the gnosis-safe personal edition contract, that we can now interact with:

```js
truffle(rinkeby)> GnosisSafePersonalEdition.at('0xf67dfef8dd417d4a9c34cbb85c525b03fb099e51').getOwners()
[ '0xf0a9ed2663311ce436347bb6f240181ff103ca16',
  '0x3b9861c7d3e7bbd41602d9ffacef10bc04867bc0',
  '0x8c7623ac7fe2e635fa256791c25da2c8851c5f08' ]
```