# atharaldenh

Public note. Nothing in this repo is posted to kaspanet/rusty-kaspa.

The note is [PR-1147-mempool-fee-signal.txt](PR-1147-mempool-fee-signal.txt). It is about [issue 1146](https://github.com/kaspanet/rusty-kaspa/issues/1146) and [pull request 1147](https://github.com/kaspanet/rusty-kaspa/pull/1147), [atharaldsen's comment on 13 October 2026](https://github.com/kaspanet/rusty-kaspa/pull/1147#issuecomment-6146955360), and why leftover mempool transactions are a fee signal for Testnet-10 applications.

Credit for the diagnosis and the revised fix goes to [atharaldsen](https://github.com/atharaldsen). The Testnet-10 fee readings are [oskrcl](https://github.com/oskrcl)'s. The outside chain count is Kaspa Pulse's ([@gokugalax](https://x.com/gokugalax)), published with his yes in [tn10-storm-2026-10-10](https://github.com/STP-KAS/tn10-storm-2026-10-10/blob/main/credits/KASPA-PULSE.md).

**Status 15 October 2026:** [analysis/2026-10-15-atharaldsen-1147-reply.md](analysis/2026-10-15-atharaldsen-1147-reply.md). #1147 is open. Its head is now `239556a6` (a merge of master), which still carries the full-revalidation patch that atharaldsen said he is dropping. The body-report design is still only in [his comment](https://github.com/kaspanet/rusty-kaspa/pull/1147#issuecomment-6146955360). Michael Sutton's CHANGES_REQUESTED of 13 October stands. #1146 is open. Master is now `7cc22fea`, and the IBD path described in the note is unchanged there. Related: [#1150](https://github.com/kaspanet/rusty-kaspa/pull/1150) (atharaldsen, open), on a transaction left in both the orphan pool and the transaction pool, and on the mempool-full assert.
