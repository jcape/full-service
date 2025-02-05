---
description: >-
  The balance of an account, which includes additional information about the
  syncing status needed to interpret the balance correctly.
---

# Balance

## Attributes

| Name | Type | Description |
| :--- | :--- | :--- |
| `object` | String, value is "balance" | String representing the object's type. Objects of the same type share the same value. |
| `network_block_index` | String \(uint64\) | The block height of MobileCoin's distributed ledger. The `local_block_index` is synced when it reaches the `network_block_index`. |
| `local_block_index` | String \(uint64\) | The local block height downloaded from the ledger. The local database will sync up to the `network_block_index`. The `account_block_index` can only sync up to `local_block_index`. |
| `account_block_index` | String \(uint64\) | The scanned local block height for this account. This value will never be greater than the `local_block_index`. At fully synced, it will match `network_block_index`. |
| `is_synced` | Boolean | Whether the account is synced with the `network_block_index`. Balances may not appear correct if the account is still syncing. |
| `unspent_pmob` | String \(uint64\) | Unspent pico MOB for this account at the current `account_block_index`. If the account is syncing, this value may change. |
| `pending_pmob` | String \(uint64\) | Pending, out-going pico MOB. The pending value will clear once the ledger processes the outgoing TXOs. The `pending_pmob` will reflect the change. |
| `spent_pmob` | String \(uint64\) | Spent pico MOB. This is the sum of all the TXOs in the wallet which have been spent. |
| `secreted_pmob` | String \(uint64\) | Secreted \(minted\) pico MOB. This is the sum of all the TXOs which have been created in the wallet for outgoing transactions. |
| `orphaned_pmob` | String \(uint64\) | Orphaned pico MOB. The orphaned value represents the TXOs which were view-key matched, but which can not be spent until their subaddress index is recovered. |

## Example

```text
{
  "account_block_index": "152003",
  "is_synced": false,
  "local_block_index": "152918",
  "network_block_index": "152918",
  "object": "balance",
  "orphaned_pmob": "0",
  "pending_pmob": "0",
  "secreted_pmob": "0",
  "spent_pmob": "0",
  "unspent_pmob": "110000000000000000"
}
```

