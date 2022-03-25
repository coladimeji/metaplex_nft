Minting Solana NFTs with Metaplex 

 

 

Local Environment Setup 

 

Node.js version 16.13.0 or later 

Git version 2.32.0 or later 

Yarn version 1.22.17 or later yarn add @types/offscreencanvas

ts-node version 10.4.0 or later 

solana cli 
solana.exe --version
Page Break
curl https://release.solana.com/v1.9.13/solana-install-init-x86_64-pc-windows-msvc.exe --output C:\solana-install-tmp\solana-install-init.exe --create-dirs
 C:\solana-install-tmp\solana-install-init.exe v1.9.13
solana --version
Create a Project Folder 

 

Create a project folder and an ‘assets’ subfolder. The ‘assets’ subfolder will hold the NFT artwork files and associated metadata in json format. 

mkdir solana-nft 

mkdir assets 

Within the ‘assets’ folder put some png files (let’s make 3 files) named 0.png, 1.png, 2.png. 

These can be any image files. You can create some random art here: https://www.randomart.co.uk/random-art-generator.aspx 

 

Now create 3 json files named 0.json, 1.json, 2.json. Use the following template: 

     { 

    "name": "Number #0001", 
    "symbol": "NFT",

    "description": "My NFT collection total 10 in all This the first Image ", 

    "image": "8.png", 

    "attributes": [ 
{ "trait_type": "Layer-1", "value": "0"},
{ "trait_type": "Layer-2", "value": "0"},
{ "trait_type": "Layer-3", "value": "0"},
{ "trait_type": "Layer-4", "value": "1"}

],

   "properties": { 
"creators":[
{		
   "address": "54i7DsioFrJp2Go95rRsrDTikZqNa3VYBi5kymZKL428", 

                "share": 100 
}
],
        "files": [{ "uri": "0.png", "type": "image/png" }]
},
"collection": { "name": "numbers", "family" "numbers" }

}
 

 

For each file change the name, image and files to the appropriate number (in our example: 0,1,2) and in the address field put your Solana Phantom Wallet address. At this point your ‘assets’ directory should look like this: 

 

 

Metaplex 

 

Metaplex is a protocol built on top of Solana that allows: 

Creating/minting NFTs; 

Auctions for primary/secondary sales; 

Visualizing NFTs. 

Metaplex consists of 2 core components: an on-chain program, and a self-hosted frontend web3 application. Follow the instructions to clone the Metaplex repo here: 

https://github.com/metaplex-foundation/metaplex 

 

The steps are, from your project directory: 

git clone https://github.com/metaplex-foundation/metaplex.git 

cd metaplex/js 

yarn install 

yarn bootstrap 

yarn build 

yarn start 

Run the following to check that the install went smoothly: 

ts-node metaplex/js/packages/cli/src/candy-machine-v2-cli.ts --version 
mine

ts-node \Users\Chris\Desktop\metaplex_nft\metaplex\js\packages\cli\src\candy-machine-v2-cli.ts --version

Should return 0.0.2 

(your path may be different depending on where you installed metaplex) 

 

Solana 

 

Make sure the Solana CLI is installed: 

solana --version 

Create a new devnet wallet and store the keys (or use a pre-existing Phantom wallet): 

solana-keygen new --outfile .config/solana/devnet.json 

Wrote new keypair to .config/solana/devnet.json
=========================================================================   
pubkey: 7X7JuNPzAmc6bcKs41VTHsmceVURRdcmYSymKemh94RF
=========================================================================   
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
winner build science can present win lawsuit point reveal three order old   
=========================================================================   

Configure Solana to use devnet: 

solana config set --url https://api.devnet.solana.com 

Config File: C:\Users\Chris\.config\solana\cli\config.yml
RPC URL: https://api.devnet.solana.com 
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: \Users\Chris\solana-minty\metaplex\js\pokemong-key.json
Commitment: confirmed
PS C:\Users\Chris\Desktop\metaplex_nft> 

Airdrop some SOL into your wallet: 

solana airdrop 2 7X7JuNPzAmc6bcKs41VTHsmceVURRdcmYSymKemh94RF

 

Metaplex Candy Machine 

 

Metaplex uses a tool called Candy Machine for NFT creation. We need to configure it. 

In the root of your project, create a file called config.json. It should look like this: 

{ 

    "price": 1.0, 

    "number": 3, 

    "gatekeeper": null, 

    "solTreasuryAccount": "<YOUR DEVNET WALLET ADDRESS>", 

    "splTokenAccount": null, 

    "splToken": null, 

    "goLiveDate": "25 Dec 2021 00:00:00 GMT", 

    "endSettings": null, 

    "whitelistMintSettings": null, 

    "hiddenSettings": null, 

    "storage": "arweave-sol", 

    "ipfsInfuraProjectId": null, 

    "ipfsInfuraSecret": null, 

    "nftStorageKey": null, 

    "awsS3Bucket": null, 

    "noRetainAuthority": false, 

    "noMutable": false 

} 

 DIDI NOT WORK


{ 

    "price": 1.0, 

    "number": 3, 

    "gatekeeper": null, 

    "solTreasuryAccount": "7X7JuNPzAmc6bcKs41VTHsmceVURRdcmYSymKemh94RF", 

    "splTokenAccount": null, 

    "splToken": null, 

    "goLiveDate": "25 Dec 2021 00:00:00 GMT", 

    "endSettings": null, 

    "whitelistMintSettings": null, 

    "hiddenSettings": null, 

    "storage": "arweave-sol", 

    "ipfsInfuraProjectId": null, 

    "ipfsInfuraSecret": null, 

    "nftStorageKey": null, 

    "awsS3Bucket": null, 

    "noRetainAuthority": false, 

    "noMutable": false 

} 


It is important that the number setting value matches the number of items that will be in your Candy Machine. In our example we are uploading 3 NFTs, so we have the following: 

"number": 3. 

 

Uploading Assets to the Candy Machine 

 

First verify that your artwork and metadata is in the correct format, with the following command: 

ts-node metaplex/js/packages/cli/src/candy-machine-v2-cli.ts verify_assets ./assets 

mine
ts-node \Users\Chris\Desktop\metaplex_nft\metaplex\js\packages\cli\src\candy-machine-v2-cli.ts verify_assets ./assets

(this filepath may be different depending on where metaplex is stored) 

Should return a message saying: Verifying token metadata for 3 (img+json) pairs. 

 

Make sure that your images and metadata are located in the same ‘assets’ directory. 

Have some SOL in your devnet wallet, and ensure you have created a Candy Machine configuration file (config.json). 

Execute the following: 

ts-node metaplex/js/packages/cli/src/candy-machine-v2-cli.ts upload -e devnet -k /.config/solana/devnet.json -cp config.json -c nft-project ./assets 

Page Break
 

If successful a Candy Machine public key will be returned along with messages similar to the following: 

 

ts-node Users\tobog\Documents\solana-nft-project> ts-node metaplex/js/packages/cli/src/candy-machine-v2-cli.ts upload -e devnet -k .config/solana/devnet.json -cp config.json -c nft-project ./assets 

wallet public key: AfhWjFPuHBahwP8L9SBMjr4d9mNTUBDS7tS3VbMcAdN5 

(node:20204) ExperimentalWarning: buffer.Blob is an experimental feature. This feature could change at any time 

(Use `node --trace-warnings ...` to show where the warning was created) 

WARNING: On Devnet, the arweave-sol storage option only stores your files for 1 week. Please upload via Mainnet Beta for your final collection. 

 

Beginning the upload for 3 (img+json) pairs 

started at: 1647623704229 

initializing candy machine 

Candy machine address:  25iS1WV73ntAbCiYay5RjML4ShFZnzVMWyaHfvHjfbmC 

Collection metadata address:  7F7VXvL1QNTUrDsFW4PomN7UaXL2VDvxqvdEzpVWvqru 

Collection metadata authority:  AfhWjFPuHBahwP8L9SBMjr4d9mNTUBDS7tS3VbMcAdN5 

Collection master edition address:  B91xweWAdK4gXEDqHoN1nbBCtBQLwdFHeNDa7UgF1Dag 

Collection mint address:  FswboJtaJarWxvGvQDTiXegfyftdN3GcCipJ72C7qbjF 

Collection PDA address:  D9Qy7TaCD3Da5gqZwnAaLwbj2A2Xr8WWgbZ91cxifSii 

Collection authority record address:  8qB4ShtAEBZMbcaM2fS9hYANb9SkDWc6sPdBqnagboHS 

Collection:  { 

  collectionMetadata: '7F7VXvL1QNTUrDsFW4PomN7UaXL2VDvxqvdEzpVWvqru', 

  collectionPDA: 'D9Qy7TaCD3Da5gqZwnAaLwbj2A2Xr8WWgbZ91cxifSii', 

  txId: '2GCb9wyirCefBpzovzdfcdFiBHRURVkTjk75m62PqcWn3kWCiLjqVRwF5DT48pn1TMepLmRejma6rYjq86VLjqQL' 

} 

initialized config for a candy machine with publickey: 25iS1WV73ntAbCiYay5RjML4ShFZnzVMWyaHfvHjfbmC 

[0] out of [3] items have been uploaded 

Starting upload for [3] items, format {"mediaExt":".png","index":"0"} 

Saved bundle upload result to cache. 

Computed Bundle range, including 3 file pair(s) totaling 0.218MB. 

Processing file groups... 

Progress: [████████████████████████████████████████] 100% | 10/10 

Uploading bundle via Bundlr... in multiple transactions 

0.000088915 SOL to upload 0.440MB with buffer. Sending fund txn... 

Successfully funded Arweave Bundler, starting upload 

Progress: [████████████████████████████████████████] 100% | 30/30 

Bundle uploaded! 

Saved bundle upload result to cache. 

Upload done. Cleaning up... 

Writing all indices in 1 transactions... 

Progress: [████████████████████████████████████████] 100% | 1/1 

Done. Successful = true. 

ended at: 2022-03-18T17:15:49.128Z. time taken: 00:00:44 

Page Break
 

Check to verify the upload: 

ts-node metaplex/js/packages/cli/src/candy-machine-v2-cli.ts verify_upload -e devnet -k .config/solana/devnet.json -c nft-project 

You should see something like the following: 

 

wallet public key: AfhWjFPuHBahwP8L9SBMjr4d9mNTUBDS7tS3VbMcAdN5 

(node:12340) ExperimentalWarning: buffer.Blob is an experimental feature. This feature could change at any time 

(Use `node --trace-warnings ...` to show where the warning was created) 

Checking 3 items that have yet to be checked... 

Looking at key  0 

Looking at key  1 

Looking at key  2 

uploaded (3) out of (3) 

ready to deploy! 

 

Setup the Frontend Candy Machine Minter 

 

Open the file .env.example located in the folder metaplex/js/packages/candy-machine-ui and enter the Candy Machine program ID. You can find the ID in the .cache folder in your nft-project directory. Open that folder, and you’ll see a json file with information about your upload. The program.candyMachine property will show the ID. 

REACT_APP_CANDY_MACHINE_ID=<YOUR CANDY MACHINE PROGRAM ID> 

REACT_APP_SOLANA_NETWORK=devnet 

REACT_APP_SOLANA_RPC_HOST=https://metaplex.devnet.rpcpool.com/ 

Save the file as .env instead of .env.example. 

 

Page Break
 

After these changes are made, run the following commands inside the folder metaplex/js/packages/candy-machine-ui. 

yarn install 

yarn start 

 

This will start a local server (http://localhost:3000/) with a frontend store where you can mint your NFTs. 

 

 stop 47.04/1.30.35

