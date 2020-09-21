SortOrder: 5
# Pagination

The standard Wix API pagination includes:

- **limit**: amount of items per response, must be between `1` and `1000` (default is `50`)  

- **offset**: number of items to skip default is (`0`)

The following examples:

```
?limit=100&offset=20
```

and

```json
    "limit": 100, 
    "offset": 20 
```

Should return items 21-120 in the results.