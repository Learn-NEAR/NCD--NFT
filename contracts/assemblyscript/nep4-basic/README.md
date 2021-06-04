Project Documentation Team c
[ToC]

## Newspaper 3.0
Todays Correspondents work for a newspaper or News Organisation (associated press)
Newspaper editorial offices publish their articles. Image material must additionally be purchased from photo reporters.There are extensive laws that protect copyrights and protect against plagiarism.

**Newspaper 3.0** would be the newspaper of the OpenWeb. A new distinction is made between the different creators such as:
- Editorial office
- Editor
- Correspondent
- Photo agency
- News agency


## Idea
The idea now would be to build a content as a contract based on the Non-Fungible Token (NEP-171) standard.

A reader of such a content would buy the content. He collects NFT contents, so to speak, and can pass them on. NFT's could be published (offered) by different editors.

The use case is limited to a minimal example. A NFT can be created and when purchased two rewards should happen to the contributors correspondent and editor.


## NFT base foundation

:::info
:rocket:  The project bases the solution on  the Nonfungible Token Standard #4  ➜ [see on github](https://github.com/near/NEPs/pull/4) 
:::

However for the purpose there were some parts missing


1. Metadata: attach Metadata to the NFT, for that the core standard does help
https://github.com/near/NEPs/blob/master/specs/Standards/NonFungibleToken/Metadata.md
and ofc a (public?) getter function that should return them.
However, Metadatastructures have been derived from
https://nomicon.io/Standards/NonFungibleToken/Metadata.html

2. Ownership and Price: make the contract ownable, this means there's a role called admin (or owner), who has exclusive privileges to call some functions in the contract, like setting the Price of 1 NFT, or the price to be paid as subscription for publishers (denominated in NEAR)
3. Getterfunction: add getter functions for MAX_SUPPLY, current supply and price
4. Payablefunction: add a payable function: register_publisher(to: AccountId) 
it allows someone to pay the subscription fee and set someone to be considered a publisher (so add his account_id to some "publishers" map) and we can add a getter function that returns the list of publishers
5. Change mint_to function: The code currently allows anyone to mint (=make) his own copy of the NFT through the "mint_to" function, we can make that function private (internal), and add another function called buy, here's a possible signature:
nft_buy (amount :int , to :AccountId)
if the caller is a publisher it requires an attached amount of Near so that 
[(price_per_one * amount) > attached Amount] AND [current supply + Amount < MAX_SUPPLY]
if conditions satisfied then it will call the internal "mint_to" to mint  the specified amount to whoever called this function an ownable contract is a widely used pattern so there must be some example that defines roles and allows only certain roles to call some function otherwise we can save the admin account_id in some variable, and inside the function check if he's the one calling it, if yes then continue, otherwise revert and do the same to check if caller is a publisher..


## Final Code
:::info
:rocket:  The NCDL1-small-group-c project code is available 
  ➜ [here](https://github.com/NCDL1-small-group-c/NFT/blob/chluff/contracts/assemblyscript/nep4-basic/main.ts) 
:::


## Testnet
Contract is deployed on testnet -> nft.chluff1.testnet









Minimal NEP#4 Implementation
============================

This contract implements bare-minimum functionality to satisfy the [NEP#4](https://github.com/nearprotocol/NEPs/pull/4) specification


Notable limitations of this implementation
==========================================

* Anyone can mint tokens (!!) until the supply is maxed out
* You cannot give another account escrow access to a limited set of your tokens; an escrow must be trusted with all of your tokens or none at all
* You cannot name more than one account as an escrow
* No functions to return the maximum or current supply of tokens
* No functions to return metadata such as the name or symbol of this NFT
* No functions (or storage primitives) to find all tokens belonging to a given account
* Usability issues: some functions (`revoke_access`, `transfer`, `get_token_owner`) do not verify that they were given sensible inputs; if given non-existent keys, the errors they throw will not be very useful

Still, if you track some of this information in an off-chain database, these limitations may be acceptable for your needs. In that case, this implementation may help reduce gas and storage costs.


Notable additions that go beyond the specification of NEP#4
===========================================================

`mint_to`: the spec gives no guidance or requirements on how tokens are minted/created/assigned. If this implementation of `mint_to` is close to matching your needs, feel free to ship your NFT with only minor modifications (such as caller verification). If you'd rather go with a strategy such as minting the whole supply of tokens upon deploy of the contract, or something else entirely, you may want to drastically change this behavior.
