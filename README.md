# org
The governing body in charge of the project. Home of research papers, discussion and ideas for the BridgeDAO project.
The project was applied for Aragon Nest grant. The proposal which includes the project description follows:

# Aragon Nest Proposal: BridgeDAO
## Abstract
[DAOs face issues interacting with world beyond blockchain](https://github.com/aragon/nest/issues/48), eg. transacting with fiat currency. The reason lies in the general incompatibility of two systems, blockchains and the physical world. This incompatibility extends to virtually all cases. Fiat transactions are the simplest and most impactful example of operating with physical world. None of the established interfaces on either side are sufficient for the interaction of DAOs and traditional financial institutions without either of them losing some of their key properties. Finding a solution which doesn’t break the rules of how these systems operate would unlock the huge potential of DAOs. 

Here we explore the possibility of such a solution - called the bridge DAO. It is a concept of a DAO whose sole purpose is the ability to reliably interact with the physical world. Rather than being premissioned and legally entrenched (and thus breaking the core of it being a DAO), bridge DAO uses tools of decentralization, cryptography and economics for achieving this goal. The idea is that DAO can reliably enforce its decisions over any sufficiently collateralized physical actor. These actors can be final users, but they can also serve as proxy services. More information on this concept can be found here: [Collateralized DAO Proxy Services as an Aragon Network use case - Economics - Aragon Research](https://research.aragon.org/t/collateralized-dao-proxy-services-as-an-aragon-network-use-case/137)

Any DAO can easily use any established Bridge DAO to interact with the physical world. Any developer can use the original Bridge DAO code to deploy their own special-use bridge DAO to meet the needs of the market. This lack of barriers of entry creates opportunities for entrepreneurs on many sides, but also enables market efficiency, exactly what Aragon Network stands for.

We propose building the first implementation of Bridge DAO. It would explore the viability of the concept within a narrow use case of fiat transactions. Tackling this problem would have the biggest impact and is the easiest to overcome, but the problem is basically the same with any other use case. A solution built for this problem should be applicable (with certain adjustments) to other physical assets.

The ability to reliably operate with fiat is important for Ethereum mass adoption because most users and businesses earn, transact and save in fiat. Besides Ethereum growth, DAOs and cryptocurrency transacting businesses present significant opportunities for entrepreneurs and should be kept out of the reach of governments and banks with conflicted interests.

The first Bridge DAO implementation used for the specific purpose of prototyping, called Paperclip, is built for providing a trustless, cheap, fast, and real-time interface for managing fiat currency. Paperclip can be described as a 2nd layer solution on top of the traditional banking system. It provides an incentive mechanism for anyone to perform fiat transactions on behalf of its users.

## Deliverables
1. Documentation (technical, economical, legal)
2. Proof of concept
A minimal viable system with following participants:
* Node - IBAN account holder, executes transactions on behalf of the Coordinator
* Coordinator - DAO, facilitates all the transactions, collateral and fee payouts
* User - user of services, transacts with other users or DAOs
* Services - banks, provide platform for fiat transactions for a fee
3. Alpha version
The software suite consisting of Dashboard, Analytics, Consumer, Whisperbook, Watcher, Ramp and Ping.

## Grant size
Funding: Up to $100k in ETH, split into chunks paid out over achieved deliverables.  
Success reward: Up to $50k in ANT, given out when all deliverables are ready.

## Application requirements
1. [A technical whitepaper](https://docs.google.com/document/d/1SIcf5Atb6kiZ7GeOKzxk5qWO-GYHzTRzr7e1FnzrokI/edit?usp=sharing)
2. [High level overview of Paperclip](https://docs.google.com/document/d/1ApUs7deTU0xwQ6pKW_P4Yj_fzFW6x1Pc0D9rWf20m3k/edit?usp=sharing)
3. [Details of the team members](https://github.com/bridgedao/nest/blob/master/grants/bridgeDAO/team.md)
4. Budget
5. [Roadmap](https://github.com/bridgedao/nest/blob/master/grants/bridgeDAO/roadmap.md)
6. Legal structure to be adopted: not sure yet


## Development timeline
The development timeline will be the following one in regards to each deliverable:

1. Nov 2018: *Documentation (technical, economical, legal)*
2. Feb 2019: *System Link, User Link, First accounts testing the system, PoC*
3. Apr 2019: *Ropsten, Ramp app, Analytics, Dashboard, Suite combination on VM*

