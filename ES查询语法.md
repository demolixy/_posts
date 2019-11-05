---
title: ES查询语法
---

```
{
  "query": {
    "bool": {
      "must": [

        {
          "term": {
            "global_id": {
              "value": "4329543139",
              "boost": 1.0
            }
          }
        }
      ],
      "disable_coord": false,
      "adjust_pure_negative": true,
      "boost": 1.0
    }
  }
}
```
