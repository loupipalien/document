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
#### 
