---
title: ES查询语法
date: 2019-11-03 00:00:00
tags: ["ES"]
cover: /images/IMG_1937.JPG
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
