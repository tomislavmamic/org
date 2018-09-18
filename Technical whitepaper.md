# Paperclip Technical Draft
A node commits a certain amount of cryptocurrency into a smart contract. This is their stake. To avoid volatility, the stake is converted into a reliable stablecoin, e.g. DAI.
A single operator can have as many nodes as they wish, but each has to have a collateral stake - there’s no staking one amount for all nodes but one “node” encompasses several accounts (one in each bank, and one staked amount applies for all of them together).
All node operators are given a software suite to install and use on joining the system. The software is open source and can be modified, but will be made generic and easy to use enough for almost anyone to join.

## Software suite
The software suite consists of:
1. Dashboard
2. Analytics
3. Consumer
4. Whisperbook
5. Watcher
6. Ramp
7. Ping

For security, all this software will be installed and run locally on people’s computers. There will be no single point of failure, and there will be no trust issues because we won’t be holding anyone’s API keys or tokens for them. Each node is running its own security according to best practices and advice from us (recommendation will be to get a dedicated OS X or Linux computer set up for this, preferably with each of the tools running in a VM - instructions will be provided, VM is already ready).

### Dashboard
A browser-based or native dashboard which combines and includes all the tools below in an intuitive user friendly interface.
Analytics
A software package which keeps track of executed and failed transactions, along with collected fees, across any number of accounts / nodes. 
This software is used only for analytics and accounting.  

### Consumer
A set of API clients and scripts to trigger the various internet banking features of certain banks in order to automate the transaction process as much as possible. The goal is to have this tool abstracted enough to be applicable to most banks with any kind of API, but to also have specific out-of-the-box implementations for the more popular banks.
This tool will also have support for reading the various tokens and 2FA mechanisms offered by banks in order to make things as automatic as possible where PSD2 is not available.  

### Whisperbook
The Whisperbook is an “order book” based on Ethereum Whisper or similar technology which publishes desired transactions (see Ramp, below). A node can grab transactions from this list manually or automatically (based on pre-set rules), and thereby it commits to executing them. Given that Whispers are not logged as Eth transactions, they are temporary - ideal for this use case - but the software can still log a node’s dropped transaction and amass complaints against it to alter its reputation score (see Reputation and Reliability below).  
### Watcher
Watcher is an optional daemon which nodes can install to get up to date notifications and automated Analytics entries when a transaction occurs. Without watcher, entering received income for transaction forwarding is manual (e.g. “I confirm that I got X USD from A and will now proceed to send [the amount - % fee] to B”).
Most banks do not have any kind of read-only access to client accounts so it’s hard to read up to date incoming payments. This tool would help with that. It’s an independent tool we would open source for usage even outside of Paperclip as a way to get free marketing and a way to allow unrelated companies and individuals to read their transactions without having to constantly export XLS reports or ping web UIs of various banks. Think of it as an always-on crawler for net-banking accounts.  
### Ramp
The only application an end user (non-node) would need to use the system (mobile friendly, too). The Ramp is an on-ramp and off-ramp into the system. 

#### Usage flow:
Person A wants to send XXX USD in local currency value to B.
Ramp UI offers: input amount, input sender currency, input recipient IBAN, input recipient currency. If IBAN bank num is different, input recipient name (still needed in EU for now). Next step: input maximum total fee you’re willing to pay (in % or sender currency fiat amount). System will auto-calculate this based on list of available nodes and broadcasted fees, plus the conversion rate (if necessary) based on receiving bank rates of that day.
Upon submitting, the Ramp creates a Whisper for the Whisperbook, publishing this request for all to see (only the first two digits of the IBAN are published to preserve privacy). Once a node with the satisfactory conditions ( fee < max fee) grabs this request, the user is notified of payment information (i.e. send XXX USD to IBAN XXXX). This notification is still up for debate: it’s possible to embed metadata into the whisper which would contain the email of person A and notify them so they could effectively send the request and turn the computer off, but given that we expect transactions to be filled quickly, I think a notification system through the Ramp app itself is quite fine, and maximizes privacy - the user will simply have to stay online and in the app until given information where to send money). The “order” looks like this: | FROM BANK | TO BANK | CURRENCY PAIR | AMOUNT | MAX FEE |. That’s it - no other info is publicly provided.
Once the node has received the money (which it either checks manually, custom through Consumer, or automatically through Watcher), it is its duty to forward it to its destination within 5 minutes.
Success scenario:
The node forwards the money to its destination. The recipient receives the money and marks the transaction complete. 

#### Limitation: 
There will be lag between the time the money arrives and the recipient marks it received. This lag needs to be discussed, but the idea is to reduce the max transfer limit of the node which did the transaction by the transactionAmount value until confirmed. However, this opens the door to abuse by other nodes - if I’m running a competing node, I can send a transaction maxAmount through a competitor to myself, and lock them up until I confirm receiving the money (indefinitely), grabbing all TXs in the meanwhile and farming fees. A better way would be to create a sort of trust system where at the beginning confirmation is required from the recipient, but later on it no longer is, or where other trusted nodes can vouch for the node which did the transfer, but they themselves have to earn enough reputation first etc. A super-node ecosystem can also be put into place where pre-verified KYC-ed nodes are auto-trusted, to retain some kind of constant availability level.
*Failure case 1: Node Fails to Confirm Receiving Money*
TBD
*Failure case 2: Sender confirms having sent the money but node fails to confirm forwarding money*
Only when manual. If Consumer or Watcher is used to automatically issue a confirmation, this case does not occur. 

A confirmation from the recipient will also nullify this failure and turn it into a success.
If no confirmation arrives from recipient (indicating money was not received) and no confirmation is sent by node (indicating it kept the money or never received it from Sender), fallback mechanisms need to be put into place.

A warning mechanism can be implemented to trigger after 5 minutes, and another at 10 minutes at which point the node is temporarily suspended.

To maximize experience and availability towards the sender, Person A, a special Whisper is re-created with the same conditions, only a part of the corrupt node’s collateral is now used to cover the transaction cost - only nodes with enough cash in the account to facilitate the transaction while still accepting the stablecoin collateral as input instead of input from Person A are eligible to grab this reWhisper.
Failure case 3: Money fails to arrive due to bank issue
TBD - depends on where the money got stuck and why.
Failure case 4: Recipient Never Confirms Receiving Money
Sender bears responsibility. Sending to the wrong person is in this case just as honest a mistake as if sending to the wrong person via direct bank transfer.
Failure 5: Sender bails - Sender never confirms having sent the money, or never sends the money
TBD - depends on where the money got stuck and why.

### Ping
The Ping is a daemon which sends out a node’s proof of life, communicating with all others. This way, Analytics and Ramp will have up to date real-time information on which nodes are up and running, and which are idle or offline.
Reputation and Reliability
Every transaction increases a node’s reputation score in the smart contract which holds the collateral.
Every transgression is also logged but can be deleted when contested and voted legit (dispute resolution).
This score can be seen by senders and recipients (third parties) but has no effect on them - senders cannot chose through whom they wish to send, nodes pick transactions from a list. However, this score will be taken into account when offering transaction to nodes - the system will identify them automatically and recommend transactions. This score will also be useful during dispute resolution. 
Other uses are possible.
Bank Score
Due to the IBAN format, we’ll have direct insight into success rates with particular banks (complaints, duration, fees). We’ll thus be able to rank banks by crypto-friendliness, responsiveness, etc. This will help us get crypto-bank partnerships later on, as new Ramp users will actually prefer to open accounts in those banks.

## Smart contracts
The DAOBank will be a smart contract on the Ethereum blockchain, powered by Loom Dappchains or an alternative similar solution for speed and reliability.
Loom Dappchains
Loom is a company developing a dappchain ecosystem for the Ethereum blockchain. To use the SDK for Loom Dappchains one needs to own one Loom token (a kind of entry fee). The dappchains and their SDK are in public beta. The nodes of the dappchain (decentralized application sidechain) run a dPoS consensus configured to work only with the participants of the network. In effect, a dappchain is a small blockchain connected to the main Ethereum blockchain, but which separately processes all of its transactions. Anyone can run a dappchain node - the Loom company is not a single point of failure. More info here.

The dCUR tokens will be transacted with on the DAOBank dappchain, but will be movable to the main chain through Plasma Exits - transfers to the main Ethereum blockchain. This will allow these tokens to be sold and bought on exchanges.

## Front End
https://www.ingress.one/ will possibly be used to P2P host the front-end of the DAOBank’s website. This makes sure the mobile app and the website stay as robust as the blockchain itself.
API Server
The API server issuing requests to the PSD2 endpoints will reside on a cluster owned and operated by the DAOBank organization until there is a meaningfully reliable way to do so from within the blockchain (if ever). The tech stack for this part of the system is irrelevant, but will likely be a concoction of Go code.
