---
id: learn-coretime-marketplace
title: Coretime Marketplace
sidebar_label: Coretime Marketplace
description: A brief overview of Coretime Marketplace, the coretime borker trade platform between parachains.
keywords: [blockspace, coretime]
slug: ../learn-hyperbridge
---

import RPC from "./../../components/RPC-Connection";

:::info Learn about Agile Coretime

To fully follow the material on this page, it is recommended to be familiar with the concepts of the [Agile Coretime Allocation](https://wiki.polkadot.network/docs/polkadot-direction#agile-coretime-allocation)

:::

## Coretime

<!-- Write the short summary of the coretime and the need of the Coretime Marketplace -->
As polkadot is moving toward an application-focused ecosystem where all cores are a resource to be consumed and used as needed by all applications. Previously, the auction mechanism was quite a competitive process to secure a slot on parachain. With coretime rental, there is no need for slot anymore, teams can purchase instantaneous or reserve bulk coretime. This greatly decreases the barrier-to-entry for software tinkerers and parachain teams and increase the accessibility to polkadot computation without wasting valuable blockspace.<br/>

The slot auction mechanism is not agile, creates high entry barriers, and is designed for long-running single applications (i.e., the original Polkadot vision proposed in the whitepaper). We depart from the classic lease auctions and propose an agile marketplace for coretime, where essentially coretime becomes a commodity that can be tokenized, sold, and traded. This setup maximizes the agility of Polkadot and lets the market figure out the best solution needed for applications to be successful.
***
### Coretime Chain

<!-- Breaking down the details of the concepts -->

The Coretime Chain is a proposed new system parachain within the Polkadot Network, responsible for the sale and management of Coretime. It is designed to handle the allocation of Bulk Coretime and track ownership of Coretime as non-fungible assets (NFTs). The Coretime Chain provides information to the Relay-chain regarding the number of cores available, the tasks running on each core, and accounting information for Instantaneous Coretime Credit. Additionally, it processes renewals and allows for various manipulations of Bulk Coretime, such as transfers, partitioning, interlacing, assignment to tasks, and pooling for Instantaneous Coretime.

#### Communication

The Coretime Chain and Relay Chain communicate through two protocols:

- **Upward Message Passing (UMP):**
  - Coretime Chain provides Relay Chain with the following information:
    - Number of cores to be made available.
    - Tasks running on each core and their ratios.
    - Accounting information for Instantaneous Coretime Credit.

- **Downward Message Passing (DMP):**
  - Relay Chain provides Coretime Chain with the following information:
    - Number of cores available to be scheduled.
    - Account information on Instantaneous Coretime Sales.

#### Specific Functions of the Coretime Chain

- **Transfer:**
  - Allows the transfer of Bulk Coretime between accounts.

- **Partition:**
  - Facilitates the partitioning of Bulk Coretime for specific use cases.

- **Interlace:**
  - Enables the interlacing of Coretime to optimize usage.

- **Assign:**
  - Permits the assignment of Coretime to specific tasks.

- **Pool:**
  - Supports pooling for Instantaneous Coretime, enhancing resource utilization.

#### Operations

- **Purchases:**
  - Handles the sale and acquisition of Coretime.

- **Renewals:**
  - Manages the renewal process for Coretime.

- **Instantaneous Coretime Credits:**
  - Provides functionality for handling Instantaneous Coretime Credits.
***
### Bulk Coretime

<!-- Breaking down the details of the concepts -->
**Definition**<br />
Bulk Coretime is a format of selling Coretime on Polkadot, where it is sold periodically on a specialized system chain called the Coretime Chain. It is allocated in advance of its usage and provides 28 days of a full core to its owner. Owners of Bulk Coretime are tracked on the Coretime Chain, and Bulk Coretime is represented as non-fungible assets (NFTs) that can be traded, partitioned, or used in various ways, such as being allocated to a particular parachain or task, or pooled into the Instantaneous Coretime Pool.

At the normal Bulk Coretime Marketplace the price depends on Demand.

Each month.<br />
Offered Cores (max to be sold).<br />
Ideal (Best if sold).<br />
If Sold > Ideal in current month, the price raises 5% for the next selling.


**Bulk Sales** <br />
- A sale of Bulk Coretime occurs on the Coretime-chain every `BULK_PERIOD` blocks.
- In every sale, a `BULK_LIMIT` of individual Regions are offered for sale.
- Each Region offered for sale has a different Core Index, ensuring that they each represent an independently allocatable resource on the Polkadot UC.
- The Regions offered for sale have the same span: they last exactly `BULK_PERIOD` blocks, and begin immediately following the span of the previous Sale's Regions. - The Regions offered for sale also have the complete, non-interlaced, Core Mask.
- The Sale Period ends immediately as soon as span of the Coretime Regions that are being sold begins. At this point, the next Sale Price is set according to the previous Sale Price together with the number of Regions sold compared to the desired and maximum amount of Regions to be sold. See Price Setting for additional detail on this point.
- Following the end of the previous Sale Period, there is an Interlude Period lasting `INTERLUDE_PERIOD` of blocks. After this period is elapsed, regular purchasing begins with the Purchasing Period.
- This is designed to give at least two weeks worth of time for the purchased regions to be partitioned, interlaced, traded and allocated.<br />

**Region**<br />
A Core bought by Bulk is made out of parts  Regions (Bulk Coretime Assets)
The Owner of the Core can
Assign a Task (like Parachain) to the Region
Transfer some Regions to another Owner
Put Regions on Instantaneous Coretime Marketplace (like Lastic or Cortego)
Split Regions into smaller pieces
***
### Instantaneous Coretime

<!-- Breaking down the details of the concepts -->
#### Instantaneous Coretime and Spot Price Multiplier

Instantaneous Coretime, previously referred to as "Parathreads" or "Pay-as-you-go Parachains," is sold on the Relay Chain immediately prior to its usage on a block-by-block basis. It allows for the immediate allocation of core resources for tasks without the need to pre-schedule or participate in periodic Bulk Coretime sales. Instantaneous Coretime credits are non-transferable and non-refundable, only usable for immediate core allocation.

#### Instantaneous Coretime Marketplace

On the Instantaneous Coretime Marketplace, the price will also depend on demand (% of OrderBook filled).

#### Spot Price Multiplier

##### Parameters:

- `traffic`: The previously calculated multiplier, can never go below 1.0.
- `queue_capacity`: The max size of the Order Book.
- `queue_size`: How many orders are currently in the Order Book.
- `target_queue_utilisation`: How much of the Queue_Capacity should be ideally occupied (expressed in %).
- `variability`: A variability factor, i.e., how quickly the Spot Price adjusts.

##### Formula:

$v = \frac{p}{k \times (1 - s)} $<br />

- \( p \): Desired ratio increase in Spot Price.
- \( k \): Number of blocks.
- \( s \): Target queue utilisation.
- \( v \): Variability.

##### Example:

$v = \frac{0.05}{20 \times (1 - 0.25)} = 0.0033$

The desired ratio increase in Spot Price is 0.05 (5%).
***
### Instantaneous Coretime Pool

<!-- Breaking down the details of the concepts -->
At the request of the owner, the Coretime-chain allows a single Bulk Coretime asset, known as a Region, to be used in various ways including transferal to another owner, allocated to a particular task (e.g. a parachain) or placed in the Instantaneous Coretime Pool. 
## Renewals & Migrations

<!-- Breaking down the details of the concepts -->

### Renewals

<!-- Breaking down the details of the concepts -->

### Migrations

<!-- Breaking down the details of the concepts -->

## Coretime Price Calculation

<!-- Breaking down the details of the concepts -->

## Instantaneous Coretime Market

<!-- Breaking down the details of the concepts -->

## Terminology

### Blockspace

<!-- Write the short definition of the blockspace -->

### Timeslice

<!-- Write the short definition of the timeslice -->

## Learn More

The information provided here is subject to change; keep up to date using the following resources:

- [Polkadot Direction - Agile Coretime Allocation](https://wiki.polkadot.network/docs/polkadot-direction#agile-coretime-allocation) - Polkadot Wiki
- [RFC-1: Agile Coretime](https://github.com/polkadot-fellows/RFCs/blob/6f29561a4747bbfd95307ce75cd949dfff359e39/text/0001-agile-coretime.md) - Agile periodic-sale-based model for assigning Coretime on the Polkadot Ubiquitous Computer.
- [Lastic Blockspace Marketplace Documentation](https://docs.lastic.xyz/)
- [Coretime Price Simulator](https://lastic.streamlit.app/)

<!-- Add more links about the pallet implementation and code reference -->
## References
This section will list the referneces to the information mentioned above:
- [RFC-1: Agile Coretime](https://github.com/polkadot-fellows/RFCs/blob/6f29561a4747bbfd95307ce75cd949dfff359e39/text/0001-agile-coretime.md)
