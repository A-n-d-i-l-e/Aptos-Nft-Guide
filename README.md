# Aptos-Nft-Guide
This is a simple way of launch your Nft collection on the Aptos ecosytem
even without coding experience shout to the FTM labs team for this candy machine.

First lets setup our tools 

-Walllet 
https://chrome.google.com/webstore/detail/martian-aptos-wallet/efbglgofoippbgcjepnhiblaibcnclgk
https://pontem.network/pontem-wallet

The most important is our IDE which i recommend VS code but you can use any other IDE.https://code.visualstudio.com/

Python
You need python version 3.9 and above. https://www.python.org/downloads/

Git 
You need to get Git. https://git-scm.com/downloads

You need node. https://nodejs.org/en/download/

Images and metadata
Lets setup our Assets before we go to the candy machine code.
So if you don't have a image generator go to the hashlips art generator. https://github.com/HashLips/hashlips_art_engine
You can also watch the video and follow along.https://youtu.be/loQBFM4WULs

Assets/  
├─ Images/  
|  |- cover.png this is for your display for the mint site.so you can choose an image and name it as cover.png
│  ├─ 1.png  
│  ├─ 2.png  
│  ├─ 3.png  
│  ├─ ...  
├─ metadata/  
│  ├─ 1.json  
│  ├─ 2.json  
│  ├─ ...  

Ok lets get to the code.

Step 1

Open up vs code and had over to the terminal and paste this git clone https://github.com/FTM-Labs/Aptos-NFT-Mint.git
or you can just download the folder open it vs code.
After that go back to the terminal and paste this
 cd script/third_party
and paste this pip3 install -r requirements.txt

Step 2 
After intsalling your dependencies 
Now had over to the script/src folder and open the config file 
And you will see this dont worry you will only be changing a few values in this file

"collection": {
    // this is for where you assets are stored the hashlips folder
    "assetDir": "C:\Users\Desktop\Assets/images",
    "metadataDir": "C:\Users\Desktop\Assets/metadata",
    "collectionName": "MyCollection",
    "collectionDescription": "this is my nft collection...", //dont forget to change this 
    // you can leave this as it is cause you putting the cover image in the assets images folder the program will be able to find it.
    "collectionCover": "",
    // this is for the max supply 
    "collectionSize": 1111,
    // how many nfts can a wallet mint.
    "maxMintPerWallet": 10,
    // this is for 1 APT token
    "mintFee": 100000000,
    // this your royalty percentage
    "royalty_points_denominator": 1000,
    "royalty_points_numerator": 50,
    // checkout this site to calculate these values and you can use it to set the correct times you want https://www.unixtimestamp.com/
    "presaleMintTime": 100000000,
    "publicMintTime": 100000000,
    // this is for the presale so go to the assets folder and create a new txt file and name it to whitelist.txt
    after that you can add you wallets to the txt file and the last digit is for how many nfts a wallet can mint
    "whitelistDir": "/Users/Shared/assets"
}
 Now go back to the top section and lets edit the storage part

Had over  to the pinata website https://www.pinata.cloud/ and create an account if you don't have one already
go to the account icon and click and click on the apikeys and create API Key and give adim role after copy the values and change them to yours.

"storage": {
        "solution": "pinata",
        "pinata": {
            "pinataApi": "https://api.pinata.cloud/pinning/pinFileToIPFS",
            "pinataPublicKey": "your pinata public key",
            "pinataSecretKey": "your pinata secret key"
        },
Now this is our last section 
if so we just need to go get the address wallet and private key from our wallet.
add your public key and add your private key

 "candymachine": {
        "cmPublicKey": "your public key",
        "cmPrivateKey": "your secret key"
    },


Step 3 
we are almost done and ready to go
Switch network
Go to constants.py and change MODE to be 'test' for testnet ,
'dev' for devnet and 'mainnet' for using mainnet. Make sure the mode in candyMachineInfo.js is set to the same.
devnet is closed so you can use the testnet and mainnet.
 
After that run the create_candy_machine.py file
On testnet, we automatically deposited some aptos to the candy machine account.

If you need more aptos in devnet, check how to use faucet_client 
to fund account in prepareCandyMachineAccount in create_candy_machine.py. As an example, you can write a for loop:
for i in range(15):
    faucet_client.fund_account(alice.address(), 100000000)

Update WL will override the past WL configuration. If you want to add some more wl spots, just add below the previous file and upload it.

Step 4

Now we had over to the dapp so if you have ran the create_candy_machine.py file then you can do this 

Open up a new terminal and  type cd mint-site 
and paste this in the terminal npm install or yarn add all . so lets first install yarn so only do this if you get errors from npm 
this is the code to install yarn . npm install -g yarn this will install yarn globally

change the values in mint-site/helpers/candyMachineInfo.js - can find those info in your config.json:

export const candyMachineAddress = "No need to hard code this it will be automatically updated once you have deployed your candy machine";
export const collectionName = " MyCollection"; // Case sensitive!
Upload your collectionCover to your pinata and copy the link and add it here .you can watch videos on how to do this right.
export const collectionCoverUrl = "https://cloudflare-ipfs.com/ipfs/";
export const mode = "mainet"; // "dev" or "test" or "mainnet"

export let NODE_URL;
export const CONTRACT_ADDRESS = "your address";
export const COLLECTION_SIZE = 111
let FAUCET_URL;
if (mode == "dev") {
    NODE_URL = "https://fullnode.devnet.aptoslabs.com/v1";
    FAUCET_URL = "https://faucet.devnet.aptoslabs.com";
} else if (mode === "test") {
    NODE_URL = "https://fullnode.testnet.aptoslabs.com/v1";
    FAUCET_URL = "https://faucet.testnet.aptoslabs.com";
} else {
    NODE_URL = "https://fullnode.mainnet.aptoslabs.com/v1";
    FAUCET_URL = null;
}

We are finally done now had over to the terminal where the mint-site is open and paste this in 
npm run dev or yarn dev

You should see this and click the http://localhost:3000 link to see your site.
 
This is the end we have learnt how to mint nfts in the Aptos ecosystem.
