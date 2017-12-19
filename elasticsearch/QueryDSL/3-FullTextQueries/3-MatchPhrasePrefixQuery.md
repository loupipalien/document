### 匹配短语前缀查询
`match_phrase_prefix` 类似于 `match_phrase`, 除了它允许在文本中的最后项做前缀匹配. 例如:
```
GET /_search
{
    "query": {
        "match_phrase_prefix" : {
            "message" : "quick brown f"
        }
    }
}
```
它接收类似于其他短语类型查询的参数. 额外的, 它还接收一个 `max_expansions` 的参数 (默认是 50), 此参数可以控制最后项的后缀可以扩展到多长. 强烈推荐为此设置一个可接收的值以控制查询执行的时间. 例如:
```
GET /_search
{
    "query": {
        "match_phrase_prefix" : {
            "message" : {
                "query" : "quick brown f",
                "max_expansions" : 10
            }
        }
    }
}
```

#### 重要
`match_phrase_prefix` 查询是个穷人版的自动补全. 它很容易使用, 能让你使用即时查询快速入门, 结果通常是足够好的, 但有时也会造成困惑.  
考虑查询字符串 `quick brown f`. 这个查询会从 `quick` 和 `brown` 中创建出一个短语查询 (即: `quick` 项必须存在并且后续是 `brown` 项). 然后查看已排序的词典项的前 50 项中已 `f` 开头的项, 然后添加这些项到短语查询中  
问题是前 50 项中可能并不包括 `fox` 项, 所以短语 `quick brown fox` 不会被找到. 这通常不是一个问题, 因为用户会键入更多的字符直到他们要找的词出现.  
即时查询更好的解决方法见 [completion suggester](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-suggesters-completion.html) 和 [Index-Time Search-as-You-Type](https://www.elastic.co/guide/en/elasticsearch/guide/master/_index_time_search_as_you_type.html)

>**参考:**
[Match Phrase Prefix Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase-prefix.html)
