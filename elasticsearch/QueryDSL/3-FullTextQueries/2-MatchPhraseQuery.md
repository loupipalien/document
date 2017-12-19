### 匹配短语查询
`match_phrase` 查询分析文本并从分析的文本中构建一个 `phrase` 查询. 例如:
```
GET /_search
{
    "query": {
        "match_phrase" : {
            "message" : "this is a test"
        }
    }
}
```
短语查询匹配一个以任何顺序的可配置的 `slop` 项 (默认是 0). 转换项有一个为 2 的溢出
可以设置 `analyzer` 以控制使用哪个分析器在处理文本时执行分析, 默认是在域的映射中显示指定, 或者使用默认的搜索分析器, 例如:
 ```
GET /_search
{
    "query": {
        "match_phrase" : {
            "message" : {
                "query" : "this is a test",
                "analyzer" : "my_analyzer"
            }
        }
    }
}
 ```

 >**参考:**
 [Match Phrase Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html)
