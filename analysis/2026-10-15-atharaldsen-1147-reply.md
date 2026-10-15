# atharaldsen's answers on #1147: status on 15 October 2026

This is a public note. Nothing in it was posted to kaspanet/rusty-kaspa. It updates [PR-1147-mempool-fee-signal.txt](../PR-1147-mempool-fee-signal.txt) (13 Oct, commit `570e2c4`). Everything was read from the GitHub API on 15 Oct 2026 at about 16:34 UTC. No node was built or run for this note, and it makes no build or test claims.

## What has been said on #1147 since 13 Oct

- **atharaldsen's answer is still [comment 6146955360](https://github.com/kaspanet/rusty-kaspa/pull/1147#issuecomment-6146955360)** (13 Oct, 13:44:11 UTC). He has not written anything on #1147 or #1146 since then.
- **The review stands.** Michael Sutton's [review 5502542535](https://github.com/kaspanet/rusty-kaspa/pull/1147#pullrequestreview-5502542535) (13 Oct, 11:04:27 UTC, CHANGES_REQUESTED, on head `b4df2d8f`) and his [comment 6143871731](https://github.com/kaspanet/rusty-kaspa/pull/1147#issuecomment-6143871731) (10:48:39 UTC) have had no reply from a maintainer. On #1146 his only addition is [comment 6143875980](https://github.com/kaspanet/rusty-kaspa/issues/1146#issuecomment-6143875980), which points to 6143871731. #1146 is still open.
- **The only later comment** is [6150595699](https://github.com/kaspanet/rusty-kaspa/pull/1147#issuecomment-6150595699) (13 Oct, 16:48:33 UTC). STP-KAS posted it, and it links this repo and tn-master.

## The pull request's code has not moved to the new design

- The head is now [`239556a6`](https://github.com/kaspanet/rusty-kaspa/commit/239556a60db4de0233b079a9a6611a543eccbe61) (15 Oct, 05:41:40 UTC). It is a merge of master into the branch. Its parents are the PR's previous head [`b4df2d8f`](https://github.com/kaspanet/rusty-kaspa/commit/b4df2d8f) and master [`67686482`](https://github.com/kaspanet/rusty-kaspa/commit/67686482) (14 Oct). It has 7 files, +460/−11.
- The merge brings in only master's changes, none of them in `mining/` or the IBD flow.
- The PR diff still adds `revalidate_all_transactions` (full revalidation) in `mining/src/manager.rs`. That is the design he said on 13 Oct he is dropping.
- The body-chunk report through `handle_new_block_transactions` has not been pushed. No review exists on `239556a6`.

## Master has moved, but the path in the note has not

- Master is now [`7cc22fea`](https://github.com/kaspanet/rusty-kaspa/commit/7cc22feadb36cbb9678bffed5896aebe1a30b14d) (15 Oct, the #991 merge).
- The [compare 12db0f35...7cc22fea](https://github.com/kaspanet/rusty-kaspa/compare/12db0f35...7cc22fea) (3 commits) touches no file under `mining/` or `protocol/flows/src/ibd/`.
- At `7cc22fea`, [`protocol/flows/src/ibd/flow.rs`](https://github.com/kaspanet/rusty-kaspa/blob/7cc22feadb36cbb9678bffed5896aebe1a30b14d/protocol/flows/src/ibd/flow.rs) still has `sync_missing_block_bodies` (L911) and `revalidate_orphans` (L219), and no call to `handle_new_block_transactions`.
- [`mining/src/mempool/config.rs`](https://github.com/kaspanet/rusty-kaspa/blob/7cc22feadb36cbb9678bffed5896aebe1a30b14d/mining/src/mempool/config.rs) still has a 24 h expiry (L7), the 120 s accepted-id expiry (L9) and a count cap of 1,000,000 (L3).
- So sections 2, 3 and 5 of the 13 Oct note describe master at `7cc22fea` as well. Only the commit they cite (`12db0f35`) is older.

## Point by point against the 13 Oct note

| Note section | Status on 15 Oct | Source |
| --- | --- | --- |
| 1. "Pull request #1147 is the attempted fix. It is open." Sutton requested changes. | **Agrees.** It is still open and the CHANGES_REQUESTED stands. "The same path is still on master at 12db0f35" is **stale only in the hash**: the path is unchanged at `7cc22fea`. | review 5502542535; compare above |
| 1. atharaldsen "drops the original patch and proposes telling the mempool about each IBD block body". | **Agrees with his words, but the code has not followed yet.** The head `239556a6` still carries the dropped full-revalidation patch. | 6146955360; `239556a6` |
| 2. How leftovers get in (IBD skips `on_new_block`). | **Agrees.** It is unchanged on master `7cc22fea`. | flow.rs at `7cc22fea` |
| 2. Leftovers stay because `handle_new_block_transactions` never runs for IBD bodies. | **Agrees, and there is now a second, separate path.** [#1150](https://github.com/kaspanet/rusty-kaspa/pull/1150) (atharaldsen, 14 Oct 22:07:47 UTC, head [`7ada0025`](https://github.com/kaspanet/rusty-kaspa/commit/7ada0025978719b98c2e6746650c2d3882c6de88), open, unreviewed) says a relayed transaction can sit in both the orphan pool and the transaction pool. In that case `handle_new_block_transactions` skips the pool removal when it is mined, so the transaction stays until expiry. That is the author's description, with a unit test in the PR; it has not been verified here. | #1150 body and diff |
| 7. His replacement design (body chunks, skip when pools are empty, report unorphaned blocks, pruning residual). | **Agrees.** The note quotes it correctly. It is still only a proposal in a comment. | 6146955360 |
| 7. "He ends by offering to close the pull request …" | **Still current.** There is no maintainer answer yet. | thread read 16:34 UTC |
| 8. "Keep the assertion he already added … commit b4df2d8" | **Still current.** `b4df2d8f` is still a parent of the head. Retargeting it at the new path has not happened, because the new path is not in the code. | `239556a6` parents |
| 8. "The ram-scale panic at 100,001 is a neighbour, not this bug … should stay a reject instead of an assert." | **Partly superseded.** #1150 says it reproduces "the mempool-full assert panic" through double-pool presence: `limit_transaction_count` counts the orphan copy as a freed slot, and the assert in `post_validate_and_insert_transaction` fires. The fix in #1150 removes the orphan copy on insert ([`validate_and_insert_transaction.rs`](https://github.com/kaspanet/rusty-kaspa/pull/1150/files), +4 lines). It keeps the assert. Whether this is the same trigger as the 25 Sep panic at `--ram-scale=0.1` is not established here. The assert is still on master `7cc22fea` (L123 of `validate_and_insert_transaction.rs`). | #1150; master at `7cc22fea` |
| 10. Advice for the pull request. | **Still current.** The PR needs a push of the body-report design before the review can move. | head state above |
| Sources: "All of these were opened … on 13 October 2026." | **Dated.** The hashes `12db0f35` remain valid links. This file adds the 15 Oct reads. | |

## What is stale in this repo

- The README said nothing about the state after 13 Oct. Its status line is updated in this commit.
- The note's master hash `12db0f35` is older than master `7cc22fea`, but the cited code is unchanged. The note is left as it was written on 13 Oct, and this file is the dated update.
- Nothing in the note conflicts with atharaldsen's current answer. The one gap is between his answer and his code. The other new item is #1150 (his, 14 Oct), which touches the note's "neighbour" panic paragraph and adds a second way for mined transactions to linger.
