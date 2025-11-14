# tiny ~41% smaller than TOON

The shortest structured data format for LLM usage

This is a response to the TOON format:

## From the TOON examples

```json
{
  "users": [
    { "id": 1, "name": "Alice", "role": "admin" },
    { "id": 2, "name": "Bob", "role": "user" }
  ]
}
```

TOON conveys the same information with **even fewer tokens**:

```
users[2]{id,name,role}:
  1,Alice,admin
  2,Bob,user
```

**Count: 22 tokens**

## "tiny"

Start with good old LISP S-Expressions:

```scheme
(users (id name role)
  (1 Alice admin)
  (2 Bob user))
```

Remove any newlines:

```scheme
(users (id name role) (1 Alice admin) (2 Bob user))
```

Now replace `)(` with `|`

```scheme
(users(id name role|1 Alice admin|2 Bob user))
```

Drop the leading `(` and any number of trailing `)`

```scheme
users(id name role|1 Alice admin|2 Bob user
```

**Count: 13 tokens**


Which then can be decoded back into JSON:

```json
{
  "users": [
    { "id": 1, "name": "Alice", "role": "admin" },
    { "id": 2, "name": "Bob", "role": "user" }
  ]
}
```




## Other examples

### JSON

```json
{
  "metrics": [
    {
      "date": "2025-01-01",
      "views": 5715,
      "clicks": 211,
      "conversions": 28,
      "revenue": 7976.46,
      "bounceRate": 0.47
    },
    {
      "date": "2025-01-02",
      "views": 7103,
      "clicks": 393,
      "conversions": 28,
      "revenue": 8360.53,
      "bounceRate": 0.32
    },
    {
      "date": "2025-01-03",
      "views": 7248,
      "clicks": 378,
      "conversions": 24,
      "revenue": 3212.57,
      "bounceRate": 0.5
    },
    {
      "date": "2025-01-04",
      "views": 2927,
      "clicks": 77,
      "conversions": 11,
      "revenue": 1211.69,
      "bounceRate": 0.62
    },
    {
      "date": "2025-01-05",
      "views": 3530,
      "clicks": 82,
      "conversions": 8,
      "revenue": 462.77,
      "bounceRate": 0.56
    }
  ]
}
```

### TOON

```
metrics[5]{date,views,clicks,conversions,revenue,bounceRate}:
  2025-01-01,5715,211,28,7976.46,0.47
  2025-01-02,7103,393,28,8360.53,0.32
  2025-01-03,7248,378,24,3212.57,0.5
  2025-01-04,2927,77,11,1211.69,0.62
  2025-01-05,3530,82,8,462.77,0.56
```

**73 tokens**

### tiny

```scheme
metrics(date views clicks conversions revenue bounceRate|2025-01-01 5715 211 28 7976.46 0.47|2025-01-02 7103 393 28 8360.53 0.32|2025-01-03 7248 378 24 3212.57 0.5|2025-01-04 2927 77 11 1211.69 0.62|2025-01-05 3530 82 8 462.77 0.56
```

**Count: 43 tokens**

Again, ~41% smaller than TOON.


