### 匹配查询
`match` 查询接收 text/numerics/dates 类型, 可以分析它们并构造一个查询. 例如:
```
GET /_search
{
    "query": {
        "match" : {
            "message" : "this is a test"
        }
    }
}
```
注意, `message` 是域的名字, 你可以替换为任何域的名字
#### 匹配
`match` 查询是 `boolean` 类型的. 这意味着分析被提供的文本并在分析过程中用提供的文本构造一个布尔查询. `operator` 标识可以是 `or` 或 `and` 控制布尔子句 (默认是 `or`). `should` 子句匹配可以使用 [minimum_should_match](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-minimum-should-match.html) 参数设置可选项的最小值
设置 `analyzer` 可以控制分析文本时使用那个分析器. 默认在映射中为域显示定义, 或者使用默认搜索分析器.  
`lenient` 参数可以设置为 `true` 以忽略有数据类型失配造成的异常, 例如试图使用文本查询串查询数字域. 默认是 `false`
#### 模糊性
`fuzziness` 允许基于被查询的域类型模糊匹配. 对于允许的设置见 [Fuzziness](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#fuzziness)  
为控制模糊处理的场景下可以设置 `prefix_length` 和 `max_expansions`. 如果查询设置了模糊参数, 将会使用 `top_terms_blended_freqs_${max_expansions}` 作为它的 [rewrite method](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-multi-term-rewrite.html) 参数, 这个参数允许去控制查询如何被重写.
模糊置换 (ab -> ba) 默认是被允许的, 但是能够通过将 `fuzzy_transpositions` 设置为 `false` 来禁止.  
这有一个当提供额外参数的示例 (注意结构的细微变化, `message` 是域的名字):
```
GET /_search
{
    "query": {
        "match" : {
            "message" : {
                "query" : "this is a test",
                "operator" : "and"
            }
        }
    }
}
```
#### 零项查询
如果被使用的分析器像 `stop` 过滤器那样移除了查询中的所有符号, 默认的行为就是完全不匹配任何文档. 为了改变这种行为可以使用 `zero_terms_query` 选项, 它接收 `none` (默认的) 和对应着一个 `match_all` 查询的 `all` 参数.
```
GET /_search
{
    "query": {
        "match" : {
            "message" : {
                "query" : "to be or not to be",
                "operator" : "and",
                "zero_terms_query": "all"
            }
        }
    }
}
```
#### 频次底线
匹配查询支持 `cutoff_frequency` 指定一个绝对的或相对的文档频次, 高频次项会被移到一个可选的子查询中, 并且只有在 `or` 操作符中的一个低频次匹配项 (低于底线) 或 在 `and` 操作符中的所有低频次匹配才评分.
查询允许在运行时动态处理 `stopwords`, 它是域独立的并且不需要停止词文件. 它避免评分/迭代高频次项, 并且只在很重要/低批次项匹配一个文档时才考虑这些项. 如果所有的查询项都高于设置的 `cutoff_frequency`, 为确保更快的执行,查询会被自动转化为一个纯净的合取 (并) 查询.  
`cutoff_frequency` 可以是相对的如果文档的总数在[0, 1)区间内,或绝对的如果大于或等于1.0.  
以下是只由停止词组成的查询示例
```
GET /_search
{
    "query": {
        "match" : {
            "message" : {
                "query" : "to be or not to be",
                "cutoff_frequency" : 0.001
            }
        }
    }
}
```
#### 重要
`cutoff_frequency` 选项在分片层级操作. 着意味着当在测试索引中使用低文档号时, 你应该遵循 [Relevance is broken](https://www.elastic.co/guide/en/elasticsearch/guide/master/relevance-is-broken.html) 中的建议.
#### 和 query_string/field 比较
匹配查询家族不会经过 "查询解析" 过程. 它不支持域名前缀, 通配符或其他 "高级" 特性.由于这个原因, 它失败的可能性非常小或不存在, 并且当它来分析并运行该文本作为查询行为时 (通常是文本搜索框所做的) 提供了良好的性能. 当然, `phrase_prefix` 类型查询可以提供良好的 "即时输入" 的性能来自动加载搜索结果.

>**参考:**
[Match Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html#query-dsl-match-query-cutoff)
