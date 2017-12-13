### 全文查询
高阶的全文查询经常被由于在例如邮件内容的全文本域上运行全文查询. 它们理解正在被查询的文本如何被分析, 并且在执行之前将每个域的分析器应用到查询文本上.
这个组别的查询如下:
- [match](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html) 查询
执行全文查询的标准查询, 包括模糊匹配, 短语匹配和近义匹配
- [match_phrase](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html) 查询
类似于 `match` 查询, 但是能够被用于匹配精确短语或近义词匹配
- [match_phrase_prefix](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase-prefix.html) 查询
穷人版的即时搜索, 类似于 `match_phrase` 查询, 但是会在最后一个词上做通配符搜索
- [common_terms](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-common-terms-query.html) 查询
一个更专业的查询, 更倾向于查询不常见的词
- [query_string](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html) 查询
支持紧凑的 Lucene [query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax), 允许你指定 AND|OR|NOT 条件, 并且可以在单个查询语句中搜索多个域. 仅供专业用户使用
- [simple_query_string](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-simple-query-string-query.html)
更简单更健壮的 `query_string` 语法的版本, 适合直接暴露给用户
