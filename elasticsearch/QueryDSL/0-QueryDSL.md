### 查询 DSL
ElasticSearch 提供一个基于 JSON 定义查询的完整查询 DSL (领域特定语言). 可将查询 DSL 看一个成查询的 AST (抽象语法树), 其包括两种类型的子句:
- 叶子查询子句
叶子查询子句在在特定的域中查找特定的值, 例如 [match(https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html), [term](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html), [range](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html) 查询. 这些查询可以被它们自己使用.
- 复合查询子句
复合查询包装了其他叶子或复合查询, 可以被用于以逻辑谓词组合多个查询 (例如 [bool](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html) 或 [dis_max](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-dis-max-query.html) 查询), 或者更改它们的行为 (例如 [constant_sore](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-constant-score-query.html) 查询)

查询子句的行为取决于它们是否被用于 [query context or filter context](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html). 
