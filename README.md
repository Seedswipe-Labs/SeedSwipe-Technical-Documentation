# SeedSwipe-Technical-Documentation
For a detailed explanation of the SeedSwipe ecosystem we encourage you read the whitepaper: [link]

SeedSwipe is a digital equity ecosystem designed for the internet age. Our protocol enables founders to fundraise seamlessly; from inviting investors, to negotiating term, to issuing equity all in a single flow. We lower the barrier to equity investment by leveraging our mobile and web platform to enable investors to realise unprecedented liquidity on their investments by making secure investments on chain, trading equity with global peers in a single click.

We’ve developed the SeedSwipe protocol, a digital equity infrastructure for the backend of our app made up of three main components: SeedShare, SeedClaim and SeedBase. These work together to create a seamless transfer infrastructure for private equity. 


### SeedSwipe


### SeedShare
SeedShare is a toolkit using Soroban smart contracts for asset tokenisation, allowing entrepreneurs to create and manage tokenised equity, automate compliance and raise venture funding. For investors, SeedShare provides a platform to realise unprecedented liquidity on assets that would have previously been immobile, enabling equity Shares to be traded like any other crypto asset.

For our working PoC of SeedShare, we developed a working prototype using Solidity Smart Contracts, but for the final release, we will be developing using Soroban as mentioned in the whitepaper because Soroban offers enhanced performance and scalability, crucial for handling the high volume of transactions in equity trading. The lower operational costs, due to minimal transaction fees, make it economically viable for both founders and investors. Additionally, Soroban's focus on security and Stellar's compliance features ensure that our platform operates within the necessary regulatory frameworks, offering peace of mind to our users.

#### SeedShare.rs
SeedShare is an ERC1155 Based tokenised equity contract. It provides all the functions traditionally associated with an ERC1155 token contract, plus additional issuer controls designed (and legally required in some jurisdictions) to support for equity Shares. These features include forced transfers and Share recovery in the event that a Shareholder has lost access to their wallet.

#### SeedCoreCompliance.rs
SeedCoreCompliance works in tandem with SeedShare and the SeedBaseClaimRegistry, recording which attributes a prospective Shareholder must have in order to receive Shares. These attributes are known as claims. Unless a user is whitelisted, when a Share transfer is initiated the SeedCoreCompliance contract iterates through the necessary claims, comparing them against the claims held by the prospective Shareholder in the SeedBaseClaimRegistry.

#### SeedCoreRegistry.rs
SeedCoreRegistry keeps an on-chain record of the Shareholders of its corresponding SeedShare contract. It then uses this record to enforce limit-Based compliance checks, such as ensuring that a Share transfer does not result in too many Shareholders, fractional Shareholdings or that a Shareholder has not been frozen by the owner-operator.

#### Advanced Issuer Controls
SeedShare includes several advanced issuer controls designed to facilitate effective and secure equity tokenization, including:
pause/unpause
recover
unfreezePartialShares/freezePartialShares
batchFreezePartialShares/batchUnfreezePartialShares
setAddressFrozen
batchSetAddressFrozen

#### MetaData
SeedSwipe introduces a metadata model designed to make the essential terms of an agreement easily accessible and interpretable by machines. This approach significantly improves the compatibility and functionality of third-party applications, as it extracts and organizes crucial information from the agreement. Consequently, external machine systems can index and use this information efficiently.
```
{ 
"name": "Acse Ordinary Shares", 
"symbol": "ACSE", 
"description": "Asce limited fully paid ordinary shares", 
"image": "ipfs:./QmW78TSUVA2343HCADk..../acselogo.png", 
"agreement": "ipfs:./QmWS1VAdMD353A6SDk..../agreement.md" "agreementMetadata": {
"$class": "org.seedswipe.tokenHolderAgreement.tokenHolderClause", "companyName": "ASCE LIMITED", 
"companyNumber": "123456789", 
"companyIdentity": "acse.seedswipe", 
"acceptedCountries": ["United Kingdom", "France", "Germany", "Sweden"], "signatureRequired": "True", "asset": { "$class": "org.seedswipe.assets.ordinaryShare", 
"shareFullyPaid": "True", 
"shareFractional": "False", 
"shareTransferLimit": "False", 
"shareHolderLimit": "False", 
"clauseId": "N234JKHKNM-8791-2146-AD7Y-8YRjgK24121L4K " 
}, 
"clauseId": "45KNKL43NL-8932-5434-231n-083kjn21kjn3w" 
} 
}
```


#### Encoding
In the creation and encoding of smart legal contracts, SeedSwipe establishes a secure connection between various elements. It achieves this by forming a two-way link, effectively mapping out the intended relationship between the legal contract and its smart contract counterpart, noted in the token URI. This method ensures a cryptographically secure integration, providing a safeguard that guarantees the integrity of information recorded off-chain, making it resistant to tampering.

An example of a token holders agreement, the steps are:

The user inputs data via the application.
This data is then used to create a structured metadata model using JSON.
A new token is created in the SeedShare token contract with a unique identifier. 
The metadata model is updated with the terms of the agreement to be enforced on-chain from the structured data model, referencing the token identifier.
The compliance contract is updated with the terms of the agreement to be enforced on-chain from the structured data model, referencing the token identifier.
The metadata is used to render the legal agreement in Markdown.
The legal Markdown file is uploaded to IPFS.
The metadata is updated with the legal agreement IPFS URI.
The metadata JSON file is uploaded to IPFS.
The SeedShare token is updated with the structured metadata model IPFS URI.


### SeedClaim


### SeedBase
