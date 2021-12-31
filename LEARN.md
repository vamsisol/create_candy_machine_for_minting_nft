# Creating your own NFT Minting Machine

Welcome to the Candy Machine Quest. In this quest you will learn how to create your own candy machine to mint NFTs on the Solana Network. We don't require you to have any background on Solana development. If you're someone who loves creating NFTs and has a hard time due to the high prices of minting, you are in the right place. 

In this Quest, we'll be building our own Candy Machine which will be used to mint NFTs on the Solana Network without paying the minting price. Minting a bunch of NFTs is really pricy on other networks and thus we will be minting our NFTs on Solana Network. Even then, if we are going to mint a large number of NFTs (say 10000) it will be very costly. Thus, we will be building our own Candy Machine and the price will be paid by the buyer of the NFT. The candy machine here can be considered analogous to real world candy machine where we fill in the machine with candies (NFTs) beforehand & the customers can buy the candy (NFT) by paying the price set earlier.

By the end of this quest, you will be able to mint your own NFTs on the Solana network with just a bunch of commands.


## Metaplex Introduction


Metaplex is a collection of tools, smart contracts, and more designed to make the process of creating and launching NFTs easier. 


Metaplex is a protocol built on top of Solana that allows:
- Creating/ Minting non-fungible tokens (NFTs)
- Starting a variety of auctions for primary/secondary sales
- visualizing NFTs in a standard way across wallets and applications


The candy-machine is an on chain Solana program (smart contract) that governs fair mints. It will help interact with the candy-machine program.  We will be using it to: 
- upload our images and metadata to arweave, then register them on the solana block-chain
- verify the state of our candy-machine 
- mint individual tokens


## Metaplex Setup


> Please have NodeJS and Git installed on your machine. Also, install yarn using `npm install yarn -g`

We can use the Metaplex github repo to get started. The link to the repository is [Metaplex Github](https://github.com/metaplex-foundation/metaplex)

Lets clone the repository on our device. Open the terminal in your desired location of project creation and type the following command.

```
git clone https://github.com/metaplex-foundation/metaplex.git

```
the
After a while, you will be having the metaplex folder on your machine. Typescript language is used in Metaplex and to run these files directly from the command line, we need to install one package globally on our machine called **ts-node** . The command for installing it is: 
```
npm install -g ts-node
```

Congratulations. You are done with the initial Metaplex setup.


## Solana Environment Setup

For interacting with the Solana network, we need to install Solana CLI tool. Below is the installation commands:

>For MacOS A& Linux Users
```
sh -c "$(curl -sSfL https://release.solana.com/v1.8.2/install)"
```



After successful updation, the terminal will have the following message:

```
downloading v1.8.2 installer
Configuration: /home/solana/.config/solana/install/config.yml
Active release directory: /home/solana/.local/share/solana/install/active_release
* Release version: v1.8.2
* Release URL: https://github.com/solana-labs/solana/releases/download/v1.8.2/solana-release-x86_64-unknown-linux-gnu.tar.bz2
Update successful

```

> For Windows Users
- Open a Command Prompt as an Administrator.
- Copy and paste the following command, then press Enter to download the Solana installer into a temporary directory:
```
curl https://release.solana.com/v1.8.2/solana-install-init-x86_64-pc-windows-msvc.exe --output C:\solana-install-tmp\solana-install-init.exe --create-dirs

```

*You can replace v1.8.2 with the release tag matching the software version of your desired release.*

Here, you have succesfully installed, the Solana CLI tool. Please type the below command in the terminal:
```
solana --version
```

If the installation was correct and successful, you will be getting the version of cli installed.


## Wallet Creation & Setup

There are a couple of networks on the solana blockchain. Devnet serves as a playground for begineers. Mainnet beta is the real network.
Depending upon where you want to setup the project, you can set the the network to either devnet or mainnetbeta. 
> For devnet
```
solana config set --url https://api.devnet.solana.com 
```
> For mainnet-beta
```
solana config set --url https://api.mainnet-beta.solana.com 
```
We suggest you to connect to the devnet for initial playaround 
To check the configuration, you can type:
```
solana config get
```
And then will get the RPC URL of the network set above.

After succesfull configuration, we will need to create awallet for minting. Make sure to store its credentials for further usage. 
We can generate a new `KeyPair` wallet from the CLI itself by:

```
solana-keygen new --outfile ~/config/solana/devnet-test.json
```
--outfile flag will be storing the private key of the wallet by creating a file at the location mentioned at the end of the command. The name of the file here is devnet-test.json .
It will ask us for a passphrase to be used as a password for the wallet. After entering the pass phrase, you will be provided with the PubKey and the secret recovery phrase of the wallet in the terminal. The private key of the wallet is stored in the file stated in command & is available in the form of base64 . You have to store the PubKey of the wallet & the secret phrase for future reference.

If successfully created, you will have a prompt in the terminal like this. 

![Wallet Creation](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/1_wallet_creation.png)

With just a single command, you have created a new wallet. Its time to set this wallet as your default one. You can configure to set the keypair and provide the file location which you used initially during wallet creation.
```
solana config set --keypair ~/config/solana/devnet-test.json

```
Successful setup will prompt a message like this:

![Wallet Setup](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/2_wallet_setup.png)


The public address of your default wallet will be used to handle deployment, transactions, airdop SOLs,etc. To know the address of the wallet, you can type:
```
solana address 
```

For more details of your account, you can use the following command with the address that you got from the last command.
```
solana account <your address from the last command>
```

You can see the balance of the wallet too. To explicitly view just the balance, you can use `solana balance`.

Time to get some SOL for the project to be used in minting (just a small amount independent of the amount of NFTs) . For those who are connected to the devnet, we can airdrop SOLs to the wallet. For users who are connected to the mainnet-beta, they can use the wallet address of the default wallet and transfer real SOL to the wallet. 
For devnet users, the command is:
```
solana airdrop 1
```
Successful airdrop will be be prompting message like this.
![Balance and Airdrop](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/3_check_balance.png)

## NFT METADATA

Every NFT minting on candy machine requires two information:
- The actual asset (image, gif, etc) of the NFT
- The metadata for that NFT (will be in the json format)

Now, we will be creating the actual assets for the minting. Create a folder named `assets` in the metaplex directory. This will contain the metadata for NFTs. In this project, we will be creating a NFT of an eye image. 

The image can be downloaded from [here](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/0.png).

We need to follow a sequence for the assets naming, the first one being 0. In this quest, we will be minting just one image thus, 0.png and 0.json will be the two file names. The json file will contain all the metadata related to the NFT.

A referrential json file can be found here: [0.json](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/0.json)

Depending upon the number of NFTs you want to have in the Candy Machine, you will need to have those many pairs of files.( an actual file and a JSON file for metadata).

### Creating the NFT Metadata
All these attributes which will be stated now are must have attributes in the metadata of NFT while minting using Candy Machine, You can use the json stated above. For better understanding, lets start from scratch. Create a file named `0.json` in the assets folder. JSON file has object like structure. Thus, initialize the file content as:
```
{
}
```
#####  Adding attibutes to the metadata
- Name, stands for the Name of the NFT. Lets name it as `My First NFT`
- Description- Human readable description of the NFT
- Image- URL to the image of the asset. PNG, GIF and JPG file formats are supported. Since, we have the image in the asset folder already, we can put the value as `image.png`.

After adding these attributes, the JSON file will look like this:
```
{
	"name":"My First NFT",
    "description":" Learning to mint NFTs using Candy Machine",
    "image":"image.png"
}
    
```
- Date- Javascript Date timestamp of the asset creation
- Symbol- The symbol of the asset. It can be empty if there is no specific symbol 
- Seller_fee_basis_points- Royalties percentage awarded to creators. A value of 250 means each creator will get a royalty fee of 2.5% per each NFT. We will be stating 0 royalty points. It means the creators won't be receiving any royalty for any transaction of the NFT. It will be an integer value. 

After adding these three attributes to the json object, the file will look like:
```
{
	"name":"My First NFT",
    "description":" Learning to mint NFTs using Candy Machine",
    "image":"image.png",
    "date": 633885941000,
 	"symbol": "",
 	"seller_fee_basis_points": 0
}
```
- Collection - Most popular NFTs are launched in collections. So, if your NFT belongs to any particular collection, the information can be provided here
- Properties
   - Files- This attribute is an Object array, where an object will contain the uri and type of the file that is part of the asset. The type should match the file extension. The array will also include files specified in image and animation_url fields, and any other that are associated with the asset.
   - Category - States the supported category of the assets in the machine
   - Creators - This attribute will contain the  public address of the creators along with the share each creator has. Here, we have provided the address of the wallet we have generated recently.
 - Attributes- It is an object array, where an object will contain trait_type and value fields. value can be a string or a number. Different assets will have different values for the same trait_types in a collection. As the image used as an asset here has different traits , all the trait_types are listed here with their value in the asset.

Adding all the other mentioned attributes in the json will be resulting into:
```
{
	"name":"My First NFT",
    "description":" Learning to mint NFTs using Candy Machine",
    "image":"image.png",
    "date": 633885941000,
 	"symbol": "",
 	"seller_fee_basis_points": 0,
    "collection": {
       "name": "My NFT Collection",
       "family": "My Family"
     },
	"properties": {
       "files": [
         {
           "url": "image.png",
           "type": "image/png"
         }
       ],
       "category": "image",
       "creators": [
         {
           "address": "PUBLIC ADDRESS OF THE CREATOR",
           "share": 100
         }
       ]
     },
    "attributes": [
       {
         "trait_type": "Background",
         "value": "Black"
       },
       {
         "trait_type": "Eyeball",
         "value": "Red"
       },
       {
         "trait_type": "Eye color",
         "value": "Pink"
       },
       {
         "trait_type": "Iris",
         "value": "Small"
       },
       {
         "trait_type": "Shine",
         "value": "Shapes"
       },
       {
         "trait_type": "Bottom lid",
         "value": "Low"
       },
       {
         "trait_type": "Top lid",
         "value": "Middle"
       }
     ]
}
```
`NOTE: ` 
- The attributes of the NFT can be anything. The trait_type remains common in a collection of NFTs and the value is something which varies across different NFTs.  
- Please follow the above structure and make sure to have all the mentioned attributes in their actual format. Failure doing so will be resulting in errors during uploading assets.

Kudos, you now know how to create metadata for NFTs which will be minted using the Candy Machine. 

`CHALLENGE: ` If you are looking to have one more NFT asset, add another file and create the metadata for it. The file names though will be 1.png and 1.json. 


## INSTALLING DEPENDENCIES

After cloning the repository, we need to install the packages. Now, in the terminal you will be available in the metaplex directory. Change to the js directory and then install all the required packages using yarn. 

```
cd js && yarn install
```
Now, metaplex has also has other dependencies linked externally. `Lerna` allows for installing and linking all the external/ shared dependencies. To install those run the command:
```
yarn bootstrap
```
It will execute the command stated under bootstrap in scripts in the `package.json` file in js directory. 
You will need to go back to the metaplex directory. You can do so by: `cd ../`. This command is used to move one level above in the file structure from the terminal. 

You are all set for uploading your assets to the candy machine. 

## UPLOAD NFT ASSETS

Now that your wallet is funded and your assets are organized and validated we can do the fun and the important part! Uploading the assets to the candy machine. It can be done so using the command: 
```
ts-node js/packages/cli/src/candy-machine-cli.ts upload ./assets --env devnet --keypair ~/config/solana/devnet-test.json

```
We specify that we are using the candy-machine cli. The assets folder location is provided in the command as `./assets` . We also specify the environment for minting (`devnet` in this case). For users following this quest on mainnet-beta, the value will be `mainnet-beta	` for the env flag. We will have to provide a wallet address for this process. I have used the wallet we created during this quest (by providing the address of the private key). 
A successful upload will have the prompt like this:

![Upload NFT Assets](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/4_upload_nft_assets.png)

The above command sends your asset files to arweave and also registers those files with your candy machine. You are now just a step away from creating your candy machine. 

A .cache folder is created which will contain  a file called devnet-temp. This contains all the information regarding the assets. If we open the link available in each index object (ex: Asset-0 Info), we will be able to see a json file similar to the .json file of Asset-0. The value of image attribute will is the location where the asset is available.

## CREATE CANDY MACHINE

With the assets in place , you can create your candy machine using the following command:
```
ts-node js/packages/cli/src/candy-machine-cli.ts create_candy_machine  --env devnet --keypair ~/config/solana/devnet-test.json --price 1

```
This will be creating your candy machine. You will again need to pass the network of creation and the wallet address for the machine. Price of each NFT is mentioned here in the command itself. In the above command, we have stated that the price of the NFT after the `--price` flag (which was uploaded via assets folder) as 1 SOL. Please note down the candy machine pub key and the wallet public key which gets generated. 
Successful creation of candy machine will prompt a message with the keys as:
![Create Candy Machine](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/5_create_candy_machine.png)

Congratulations, you have succesfully minted your NFT and created your candy machine. 

## UPDATE CANDY MACHINE

After successful creation of the machine, we will need to update the date of release of assets. If you intend to have the assets open for sale from now itself, a date before now should be provided. If planning to open the sale in future, provide the date accordingly. This provides the flexibility to choose the auction timings. 
```
ts-node js/packages/cli/src/candy-machine-cli.ts update_candy_machine  devnet --keypair ~/config/solana/devnet-test.json --date '31 Oct 2021 00:00:00 GMT'
```
Make sure to provide just the network name and not the `--env` flag in the command. Provide the wallet address and then provide the date of asset sale opening after the `--date` flag.

![Upload Candy Machine](https://raw.githubusercontent.com/vamsisol/create_candy_machine_for_minting_nft/main/learn_src/learn_assets/6_update_candy_machine.png)

And, your assets are open for sale. 

Congratulations, on successfully creating your own candy machine using metaplex. Now, you don't need to rely on applications to mint your NFTs. 

## WHAT'S NEXT

After learning how to create the candy minting website, we will build our own interface for selling the NFTs. We will be integrating the candy machine created during this quest with the interface. Meanwhile, you can proceed to [Layer3](https://alpha.layer3.xyz/task/create-an-nft-candy-machine) and claim your reward NFT for succesfully completing this quest!
