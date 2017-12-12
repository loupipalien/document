### 查询和过滤上下文
查询子句的行为依赖于它是否在查询上下文或过滤上下文中被使用
- 查询上下文
在查询上下文中使用的查询子句用于回答 "这个文档和这个查询子句的匹配程度?" 的问题, 除了决定文档是否匹配, 查询子句也计算了一个 `_source` 来代表文档的匹配程度, 相对于其他文档而言.
当查询子句被传递给 `query` 参数时查询上下文时有效的, 例如在 [search](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-query.html) API 中的 `query` 参数
- 过滤上下文
在过滤上下文中, 查询子句用于回答 "这个文档是否匹配这个查询子句?" 的问题, 答案就是简单的 **是** 或 **否** - 不会计算评分. 过滤上下文大多数被用于过滤结构性数据, 例如:
  - `timestamp` 是否在 2015 到 2016 的区间 ?
  - `status` 域的值是否是 "published" ?
  频繁被使用的过滤器将会被 ElasticSearch 自动缓存, 为了加速性能. 当查询子句被传递给 `filter` 参数时过滤上下文时有效的, 例如 在 [bool](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html) 查询中的 `filter` 或 `must_not` 参数, 在 [constant_sore](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-constant-score-query.html) 查询或在 [filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-filter-aggregation.html) 聚合中的 `query` 参数

下面是在 `search` API 的查询和过滤上下文中使用查询子句的示例. 这个查询将会匹配满足查询子句条件的文档:
- `title` 域包含 `search` 单词
- `content` 域包含 `elasticsearch` 单词
- `status` 域的值是单词 `published`
- `publish_date` 域是从 2015-01-01 以后的日期
```
GET /_search
{
  "query": { 1
    "bool": { 2
      "must": [
        { "match": { "title":   "Search"        }}, 3
        { "match": { "content": "Elasticsearch" }}  4
      ],
      "filter": [ 5
        { "term":  { "status": "published" }}, 6
        { "range": { "publish_date": { "gte": "2015-01-01" }}} 7
      ]
    }
  }
}
```
- 1: `query` 参数表示查询上下文
- 2,3,4: `bool` 和两个 `match` 子句在查询上下文中被使用, 这意味着它们被用于给每个文档的匹配程度评分
- 5: `filter` 参数表示过滤上下文
- 6,7: `term` 和 `range` 子句在过滤上下文中被使用, 它们将过滤掉不匹配的文档, 但是它们不会影响匹配文档的评分
#### 小技巧
在查询上下文中使用的查询子句条件会影响匹配文档的评分 (例如: 文档的匹配程度), 并在过滤上下文中使用其余的查询子句

>**参考:**
[Query and filter context](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html)
