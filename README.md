# tiny
The shortest structured data format for LLM usage

> Roughly 40% fewer tokens than the TOON version.

This is a response to the TOON format:

From the TOON examples:

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

```lisp
(users (id name role)
  (1 Alice admin)
  (2 Bob user))
```

Without the newlines:

```lisp
(users (id name role) (1 Alice admin) (2 Bob user))
```

Now replace `)(` with `|`

```lisp
(users(id name role|1 Alice admin|2 Bob user))
```

Drop the leading `(` and any number of trailing `)`

```lisp
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
