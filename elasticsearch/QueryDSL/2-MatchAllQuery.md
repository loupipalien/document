### Match All 查询
最简单的查询, 它匹配所有的文档, 并都给一个 1.0 评分的 `_source`
```
GET /_search
{
    "query": {
        "match_all": {}
    }
}
```
`_source` 可以使用 `boost` 参数更改值
```
GET /_search
{
    "query": {
        "match_all": { "boost" : 1.2 }
    }
}
```
### Match None 查询
这个 `match_all` 查询的反转, 它不匹配任何文档
```
GET /_search
{
    "query": {
        "match_none": {}
    }
}
```

>**参考:**
[Match All Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-all-query.html)
