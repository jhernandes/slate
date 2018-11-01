# (Marketplace) - Transaction Split

## Submitting a Split Request

`POST /service/v1`

> Integration example

```shell
curl -X POST \
  'https://sandbox.ipag.com.br/service/v1?ctrl=zoopsplit&tid=9dd605c47d5144d58c0f091b397fb741' \
  -H 'authorization: Basic am9uYXRoYW46REM4QS00QzE2OU87789S1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: application/json' \
  -d '{
    "split": [
        {
            "seller_id": "06e004cf5f034e09bb0481a160969d81",
            "amount": "10.01"
        },
        {
            "seller_id": "074e381d9a0c46a790991ac65eae384c",
            "amount": "4.00"
        },
        {
            "seller_id": "edb36507a5864e0996de42d286f865f4",
            "amount": "4.99"
        }
    ]
}'
```

### The fields below must be sent as parameters in the URL

Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | **zoopsplit**
tid | string | sim | Transaction TID on iPag (transId)

> The JSON below must be sent in the body of the Request:

```json
{
  "split": [
    {
      "seller_id": "06e004cf5f034e09bb0481a160969d81",
      "amount": "10.01"
    },
    {
      "seller_id": "074e381d9a0c46a790991ac65eae384c",
      "amount": "4.00"
    },
    {
      "seller_id": "edb36507a5864e0996de42d286f865f4",
      "amount": "4.99"
    }
  ]
}
```

### Split's JSON fields

Field | type | Required | description
------ | ----- | ----------- | ---------
split | container | sim | Container
seller_id | string | sim | Seller ID
amount | double | sim | Absolute value that will be passed on to the seller

## Successful Split Response

> Successful Split Response

```json
[
    {
        "seller_id": "06e004cf5f034e09bb0481a160969d81",
        "amount": "10.01",
        "status": "success",
        "rule": "527940e2065b446484f3825a688e332f"
    },
    {
        "seller_id": "074e381d9a0c46a790991ac65eae384c",
        "amount": "4.00",
        "status": "success",
        "rule": "4d48ceb222854fde8818dba581bb990d"
    },
    {
        "seller_id": "edb36507a5864e0996de42d286f865f4",
        "amount": "4.99",
        "status": "success",
        "rule": "b8fdc6097e11417e98a4e683de5627f5"
    }
]
```


## Query Split Transaction

<aside class = "notice">
   You can perform a query before sending a Split, this way you will have the total net value available to perform Split.
</aside>

`GET /service/v1`

```shell
curl -X GET \
  'https://sandbox.ipag.com.br/service/v1?ctrl=zoopsplitquery&tid=9dd605c47d5144d58c0f091b397fb741' \
  -H 'authorization: Basic am9uYXRoYW46REM4QS00QzE2OU87789S1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: application/json'
```

### The fields below must be sent as parameters in the URL

Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | **zoopsplitquery**
tid | string | sim | Transaction TID on iPag (transId)

### Split query response

> Split query response

```json
{
    "available": "0.20",
    "can_split": 1,
    "zoop_fee": "0.80",
    "split": [
        {
            "seller_id": "074e381d9a0c46a790991ac65eae384c",
            "amount": "4.00",
            "rule": "4d48ceb222854fde8818dba581bb990d"
        },
        {
            "seller_id": "edb36507a5864e0996de42d286f865f4",
            "amount": "4.99",
            "rule": "b8fdc6097e11417e98a4e683de5627f5"
        },
        {
            "seller_id": "06e004cf5f034e09bb0481a160969d81",
            "amount": "10.01",
            "rule": "527940e2065b446484f3825a688e332f"
        }
    ]
}
```




