# Create your own Pokemon NFT Minting Website

Welcome Questmasters. In this quest you will learn how to create your own NFT minting website. We don't require you to have any background on Solana development. If you're someone who loves creating NFTs and want to have your our own internet space of selling NFTs, you are in the correct place. 

In this Quest, we'll be building our own Website and use a Candy Machine which will be containing our NFT assets. You will now not require to depend on any other platforms to sell your NFTs.

By the end of this quest, NFT enthusiasts can buy your NFTs from your own website.



## Must-haves

For all the masters who are getting started with this quest, I assume that you have already followed the previous quest of Creating your own Candy Machine. If not I request you to follow that quest first and then get started with this, since we will be integrating a Candy Machine to the UI. If you have your own `Candy Machine` and all the things that will be listed below, then you can continue to the next Subquest. 
A list of things from the Candy Machine is required during this quest. Listing them below:
- Public Key for `Treasury Address` - Public Key address for the earnings from NFT. I will be referring to the wallet that was generated during the Candy Machine Creation as the treasury address.
- Public Key for `Candy Machine Config`- During the initialization of the config for the Candy Machine, a wallet is in use. The public key for that is required. It is an output when the assets for the NFTs are uploaded using the command:
```
ts-node js/packages/cli/src/candy-machine-cli.ts upload ./assets --env SOLANA_NETWORK --keypair ~/config/solana/FILENAME.json

```
- `Candy Machine ID` - The public key of the Candy Machine created is here in reference. It is a output of the following command while creating the candy machine:
```
ts-node js/packages/cli/src/candy-machine-cli.ts create_candy_machine  --env SOLANA_NETWORK --keypair ~/config/solana/FILENAME.json --price PRICE_PER_NFT_IN_SOL

```
- The auction of the NFTs can be set during the creation of the Candy Machine. The `Start Date` of the Auction (in Javascript Timestamp format) is in reference here. It is a output of the following command while updating the candy machine:
```
ts-node js/packages/cli/src/candy-machine-cli.ts update_candy_machine  SOLANA_NETWORK --keypair ~/config/solana/FILENAME.json --date 'START_DATE_OF_AUCTION'

```
It is mandatory to have these things before moving forward. 


`NOTE`
- For users who had didn't follow the Previous quest, please make sure that you have correctly setup your Candy Machine. And, have the above listed items available handy.
- A basic understanding of the ES6 notations and ReactJS will be very helpful.
- The views of the terminal in this quest will be from `Visual Studio Code Editor`
- I will be turning a Pikachu Pokemon into a NFT. The links to the metadata of the assets is: 
[JSON file](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/0.json) & ![Pikachu Img](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/0.png) 

## UI Setup


> I assume you have NodeJS and Git installed on your machine. 

`exiled-apes` provide a boiler-plate Candy Machine Mint app which will be used in this quest. To get started, you can clone the existing github repository . The link to the repository is [Candy Machine Mint Github](https://github.com/exiled-apes/candy-machine-mint)

Lets clone the repository on our device. Open the terminal in your desired location of project creation and type the following command.

```
git clone https://github.com/exiled-apes/candy-machine-mint

```
After a while, you will be having the candy-machine-mint folder on your machine. 

Change to that directory using the command `cd candy-machine-mint`. Open this in your favourite code editor (For VS Code users, `code .` opens the folder you are in  , in the Editor). 

Open the terminal in the candy-machine-mint folder location. It is a ReactJS application. Install the dependencies using the command `npm install`.

You have successfully setup the boiler plate Candy Mint App. 


## Candy Machine Integration

Lets integrate the candy machine to the UI. 

In the folder section for the `candy-machine-mint`, you will find a File called `.env.example`. Rename it to `.env`. Open the file.

You will find already existing variables starting with `REACT_APP_`. These are called environment variables. All the sensitive information related to the application is listed in `.env`. 

> If again you wish to add other environment variables, make sure you start the variable name with `REACT_APP` and have all the letters of the variable in capital & have `_` between each word. 

Changes to be made in the file is as follows:
- The value of the `REACT_APP_SOLANA_NETWORK` is devnet by default. You can change it to mainnet-beta if required.
- Depending upon the solana network, the `REACT_APP_SOLANA_RPC_HOST` will be changed. For devnet, the host will be "https://explorer-api.devnet.solana.com". For mainnet-beta, the host will be "https://explorer-api.mainnet-beta.solana.com".
- The treasury address mentioned as a must have above has to be the value of the variable `REACT_APP_TREASURY_ADDRESS`
- The Candy Machine ID mentioned as a must have above has to be the value of the variable `REACT_APP_CANDY_MACHINE_ID`
- The Candy Machine Config key mentioned as a must have above has to be the value of the variable `REACT_APP_CANDY_MACHINE_CONFIG`
- The Start Date mentioned as a must have above has to be the value of the variable `REACT_APP_CANDY_START_DATE`

After setting up all these variables, you have completed setting up the Candy Mint App. 

Let us run the application using the command `npm start`. After a while, the compilation will be successful, you will have the terminal view like this: 

![Running React App](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/1_successful_compilation.png)

A tab will be automatically opening in your default web browser, with the URL `localhost:3000`. All the react applications when started, become live on localhost on the available `PORT NUMBER`. The default one is `3000`. The view of the portal will be something like this:

![React App View](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/2_react_app.png)

## Buying NFT

The UI interface is now live and if deployed, others can buy NFT from this. As seen above, you will first need to connect with your wallet. `@solana/wallet-adapter-react`,`@solana/wallet-adapter-base`, `@solana/wallet-adapter-wallets` & `@solana/wallet-adapter-material-ui` are the npm packages which have helped integrate different wallets in the UI. Thus, a user can connect to the UI using either Phantom/ Slope / Solflare / Sollet / SolletExtension wallets to interact with our website. 

I will be connecting using the Phantom wallet. After successful login, the content of the website will now be having information regarding the Candy Machine. The website will look like this: 

![Wallet Login](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/3_wallet_login.png)

> Make sure to have some SOL in the wallet to buy the NFT . If on devnet, you can airdrop fake SOL to your wallet. 

The list of information of the connected wallet displayed is:
- Wallet Address of the connected wallet
- Balance of the connected wallet

The list of information about the candy machine displayed is:
- Total NFTs in the Candy Machine that was open for sale
- Number of NFTs already reedemed
- Number of NFTs remaining 

If there is balance in the wallet and there are NFTs remaining for sale, then the MINT button will still be enabled.

Clicking on the mint will initiate the transaction. As stated, the price for minting will be solely paid by the customer in case of Candy Machine, thus the total price of each NFT alongwith the minting price has to be paid. Initiating the mint will popup an approve transaction screen like this:

![Approve Transaction](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/4_mint_nft.png)

Upon approving the transaction, if the stated price is available in the wallet, the NFT will be successfully bought. You can view your NFT Collectible now in your collectibles. The NFT will be visible like this:

![Collectible NFT](https://github.com/vamsisol/create_nft_minting_website/blob/main/learn_src/learn_assets/5_collectible_nft.png)

Congratulations, a NFT is bought from your website successfully. 

## Enhancements on boilerplate

There are a lot of things which can be improved from the UI side of the minting website. Since the aim of this quest was to solely help you create your own NFT minting website, we won't be covering any UI enhacements topic here. 

People might also want to know the price of the NFT before proceeding to mint. But in this boilerplate app, we don't have the price returned. Since in candy machine the price for each NFT is same and is set during creation, we can fetch the NFT price from the Candy Machine State. 

In your candy-machine-mint folder you will be having a `src` folder which contains a file named `candy-machine.ts`. All the important functions related to the minting is available in this file. `getCandyMachineState` is the function which returns the actual state of the Candy Machine by which all the stats are visible on the UI. We can tweak it to return the price for each NFT too. 

```
const state: any = await program.account.candyMachine.fetch(candyMachineId);
```

This variable state stores the actual state of the candyMachine. We can get the price & store it in a variable as:
```
const pricePerNFT=(state.data.price.toNumber())/1000000000;
```
The price is available in Lamports and `1 SOL = 1000000000 Lamports`. Thus, we divide the number generated by `10^9` to get the price in SOL.

At the end of the function, modifying the return function to include our variable, resulting in:
```
return {
    candyMachine,
    itemsAvailable,
    itemsRedeemed,
    itemsRemaining,
    goLiveDate,
    pricePerNFT
  };
```

Here, you will now get the price too from the CandyMachine state. In the function `refreshCandyMachineState`, destructure it and you can now use it to display in the UI. The changed destructure will look like:
```
const {
        candyMachine,
        goLiveDate,
        itemsAvailable,
        itemsRemaining,
        itemsRedeemed,
        pricePerNFT
      } = await getCandyMachineState(
        wallet as anchor.Wallet,
        props.candyMachineId,
        props.connection
      );
```

## Key highlights in Home.tsx

Whatever is visible in the screen is just a single component of React called `Home.tsx` which is available in the src folder. 

The main functions in use in this component are:
- `refreshCandyMachineState` will be refreshing the state of the candy machine, during the initial load, & after performing any transactions.
- `onMint` will be performing the mint and buying the NFT for the payer. 

If the NFTs are already available for auction, then only the `MINT` button will be visible. Otherwise, if the NFTs will be open for sale on some time later, a counter will be rendered showing the remaining time. If no NFTs remaining, a disabled `SOLD OUT` button will be visible on the screen.

Alerts for all the actions will be available at the bottom of the screen. 


Congratulations, you have now your own NFT minting website.