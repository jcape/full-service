---
description: >-
  A receiver receipt contains the confirmation number and recipients can poll
  the receiver receipt for the status of the transaction.
---

# Receiver Receipt

## Attributes

| _Name_ | _Type_ | _Description_ |
| :--- | :--- | :--- |
| `object` | string, value is "receiver\_receipt" | String representing the object's type. Objects of the same type share the same value. |
| `public_key` | string | Hex-encoded public key for the TXO. |
| `tombstone_block` | string | The block index after which this TXO would be rejected by consensus. |
| `confirmation` | string | Hex-encoded confirmation that can be validated to confirm that another party constructed or had knowledge of the construction of the associated TXO. |
| `amount` | string | The encrypted amount in the TXO referenced by this receipt. |

## Example

```text
{
  "object": "receiver_receipt",
  "public_key": "0a20d2118a065192f11e228e0fce39e90a878b5aa628b7613a4556c193461ebd4f67",
  "confirmation": "0a205e5ca2fa40f837d7aff6d37e9314329d21bad03d5fac2ec1fc844a09368c33e5",
  "tombstone_block": "154512",
  "amount": {
    "object": "amount",
    "commitment": "782c575ed7d893245d10d7dd49dcffc3515a7ed252bcade74e719a17d639092d",
    "masked_value": "12052895925511073331"
  }
}
```

## Methods

### `check_receiver_receipt_status`

Check the status of a receiver receipt.

{% tabs %}
{% tab title="Request Body" %}
```text
curl -s localhost:9090/wallet \
  -d '{
        "method": "check_receiver_receipt_status",
        "params": {
          "address": "3Dg4iFavKJScgCUeqb1VnET5ADmKjZgWz15fN7jfeCCWb72serxKE7fqz7htQvRirN4yeU2xxtcHRAN2zbF6V9n7FomDm69VX3FghvkDfpq",
          "receiver_receipt": {
            "object": "receiver_receipt",
            "public_key": "0a20d2118a065192f11e228e0fce39e90a878b5aa628b7613a4556c193461ebd4f67",
            "confirmation": "0a205e5ca2fa40f837d7aff6d37e9314329d21bad03d5fac2ec1fc844a09368c33e5",
            "tombstone_block": "154512",
            "amount": {
              "object": "amount",
              "commitment": "782c575ed7d893245d10d7dd49dcffc3515a7ed252bcade74e719a17d639092d",
              "masked_value": "12052895925511073331"
            }
          }
        },
        "jsonrpc": "2.0",
        "id": 1
      }' \
  -X POST -H 'Content-type: application/json' | jq
```
{% endtab %}

{% tab title="Response" %}
```text
{
  "method": "check_receiver_receipt_status",
  "result": {
    "receipts_transaction_status": "TransactionSuccess",
    "txo": {
      "object": "txo",
      "txo_id": "fff4cae55a74e5ce852b79c31576f4041d510c26e59fec178b3e45705c5b35a7",
      "value_pmob": "2960000000000",
      "received_block_index": "8094",
      "spent_block_index": "8180",
      "is_spent_recovered": false,
      "received_account_id": "a4db032dcedc14e39608fe6f26deadf57e306e8c03823b52065724fb4d274c10",
      "minted_account_id": null,
      "account_status_map": {
        "a4db032dcedc14e39608fe6f26deadf57e306e8c03823b52065724fb4d274c10": {
          "txo_status": "spent",
          "txo_type": "received"
        }
      },
      "target_key": "0a209eefc082a656a34fae5cec81044d1b13bd8963c411afa28aecfce4839fc9f74e",
      "public_key": "0a20f03f9684e5420d5410fe732f121626352d45e4e799d725432a0c61fa1343ac51",
      "e_fog_hint": "0a544944e7527b7f09322651b7242663edf17478fd1804aeea24838a35ad3c66d5194763642ae1c1e0cd2bbe2571a97a8c0fb49e346d2fd5262113e7333c7f012e61114bd32d335b1a8183be8e1865b0a10199b60100",
      "subaddress_index": "0",
      "assigned_subaddress": "3Dg4iFavKJScgCUeqb1VnET5ADmKjZgWz15fN7jfeCCWb72serxKE7fqz7htQvRirN4yeU2xxtcHRAN2zbF6V9n7FomDm69VX3FghvkDfpq",
      "key_image": "0a205445b406012d26baebb51cbcaaaceb0d56387a67353637d07265f4e886f33419",
      "confirmation": null,
      "offset_count": 25
    }
  },
  "error": null,
  "jsonrpc": "2.0",
  "id": 1
}
```
{% endtab %}
{% endtabs %}

### `create_receiver_receipts`

After building a `tx_proposal`, you can get the receipts for that transaction and provide it to the recipient so they can poll for the transaction status.

| Required Param | Purpose | Description |
| :--- | :--- | :--- |
| `tx_proposal` |  |  |

{% tabs %}
{% tab title="Request Body" %}
```text
curl -s localhost:9090/wallet \
  -d '{
        "method": "create_receiver_receipts",
        "params": {
          "tx_proposal": '$(cat tx_proposal.json)',
        },
        "jsonrpc": "2.0",
        "id": 1
      }' \
  -X POST -H 'Content-type: application/json' | jq
```
{% endtab %}

{% tab title="Response" %}
```text
{
  "method": "create_receiver_receipts",
  "result": {
    "receiver_receipts": [
      {
        "object": "receiver_receipt",
        "public_key": "0a20d2118a065192f11e228e0fce39e90a878b5aa628b7613a4556c193461ebd4f67",
        "confirmation": "0a205e5ca2fa40f837d7aff6d37e9314329d21bad03d5fac2ec1fc844a09368c33e5",
        "tombstone_block": "154512",
        "amount": {
          "object": "amount",
          "commitment": "782c575ed7d893245d10d7dd49dcffc3515a7ed252bcade74e719a17d639092d",
          "masked_value": "12052895925511073331"
        }
      }
    ]
  },
  "error": null,
  "jsonrpc": "2.0",
  "id": 1
}
```
{% endtab %}
{% endtabs %}



