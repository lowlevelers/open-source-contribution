# Pallet Broker Walkthrough

- Link to the pallet crate: https://github.com/paritytech/polkadot-sdk/blob/master/substrate/frame/broker/README.md
- Deep dive Wiki page: [Core Time Marketplace (Draft version)](./drafts/w3f-%235475-coretime-marketplace-draft.md)

## Description

Brokerage tool for managing Polkadot Core scheduling.

Properly described in [RFC-0001 Agile Coretime](https://github.com/polkadot-fellows/RFCs/blob/main/text/0001-agile-coretime.md).

## Storage Definitions

| Name          | Description                        | Type                         |
| ------------- | ---------------------------------- | ---------------------------- |
| Configuration | Configuration of the broker pallet | StorageValue(`ConfigRecord`) |

### Struct type: ConfigRecord

```rust
/// Configuration of this pallet
#[derive(Encode, Decode, Clone, PartialEq, Eq, RuntimeDebug, TypeInfo, MaxEncodedLen)]
pub struct ConfigRecord<BlockNumber, RelayBlockNumber> {
	/// The number of Relay-chain blocks in advance which scheduling should be fixed and the
	/// `Coretime::assign` API used to inform the Relay-chain.
	pub advance_notice: RelayBlockNumber,
	/// The length in blocks of the Interlude Period for forthcoming sales.
	pub interlude_length: BlockNumber,
	/// The length in blocks of the Leadin Period for forthcoming sales.
	pub leadin_length: BlockNumber,
	/// The length in timeslices of Regions which are up for sale in forthcoming sales.
	pub region_length: Timeslice,
	/// The proportion of cores available for sale which should be sold in order for the price
	/// to remain the same in the next sale.
	pub ideal_bulk_proportion: Perbill,
	/// An artificial limit to the number of cores which are allowed to be sold. If `Some` then
	/// no more cores will be sold than this.
	pub limit_cores_offered: Option<CoreIndex>,
	/// The amount by which the renewal price increases each sale period.
	pub renewal_bump: Perbill,
	/// The duration by which rewards for contributions to the InstaPool must be collected.
	pub contribution_timeout: Timeslice,
}
pub type ConfigRecordOf<T> = ConfigRecord<BlockNumberFor<T>, RelayBlockNumberOf<T>>;
```

### Struct type: SaleInfoRecord

**Bulk Coretime** is sold **periodically** on a specialised system chain known as the **Coretime-chain** and allocated in advance of its usage, whereas **Instantaneous Coretime** is sold **on the Relay-chain immediately prior to usage on a block-by-block basis.**

```rust
/// The status of a Bulk Coretime Sale.
#[derive(Encode, Decode, Clone, PartialEq, Eq, RuntimeDebug, TypeInfo, MaxEncodedLen)]
pub struct SaleInfoRecord<Balance, BlockNumber> {
	/// The local block number at which the sale will/did start.
	pub sale_start: BlockNumber,
	/// The length in blocks of the Leadin Period (where the price is decreasing).
	pub leadin_length: BlockNumber,
	/// The price of Bulk Coretime after the Leadin Period.
	pub price: Balance,
	/// The first timeslice of the Regions which are being sold in this sale.
	pub region_begin: Timeslice,
	/// The timeslice on which the Regions which are being sold in the sale terminate. (i.e. One
	/// after the last timeslice which the Regions control.)
	pub region_end: Timeslice,
	/// The number of cores we want to sell, ideally. Selling this amount would result in no
	/// change to the price for the next sale.
	pub ideal_cores_sold: CoreIndex,
	/// Number of cores which are/have been offered for sale.
	pub cores_offered: CoreIndex,
	/// The index of the first core which is for sale. Core of Regions which are sold have
	/// incrementing indices from this.
	pub first_core: CoreIndex,
	/// The latest price at which Bulk Coretime was purchased until surpassing the ideal number of
	/// cores were sold.
	pub sellout_price: Option<Balance>,
	/// Number of cores which have been sold; never more than cores_offered.
	pub cores_sold: CoreIndex,
}
pub type SaleInfoRecordOf<T> = SaleInfoRecord<BalanceOf<T>, BlockNumberFor<T>>;
```
