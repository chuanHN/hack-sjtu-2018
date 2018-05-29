# hack-sjtu-2018

Grammar checker API instruction and examples for hack-sjtu-2018

目前提供服务支持英文语法检错和拼写检查功能， 可以通过HTTPS请求来调用服务。

调用地址： http://grammar.llsapp.com/llcup/compute

输入数据格式为

```
{
  "type": "writing",  //  fixed
  "text": "content"   
}
```

输出数据格式

```
{
  "content": [
    {
      // origin text
      "origin": [ 
        "he",
        "go",
        "to",
        "school"
      ],
      // corrected text
      "corrected": [
        "He",
        "goes",
        "to",
        "school"
      ]
    }
  ],
  // the key score is useless.
  "score": {								
    "coherence": 90,
    "total": 90,
    "grammar": 60,
    "vocabulary": 88
  },
  // detail grammar error information 
  "grammar": [
    {
    	// origin token
      "origin": "go", 
      // corrected token
      "correction": "goes",
      // error type
      "error_type": "sva",
      // sentence id
      "sentence_id": 0,
      // location in the sentence
      "error_loc": 1
    }
  ]
}

the error types are sva(主谓一致)， nn(名词单复数)， article(冠词错误)， adj_adv(形容词副词错误)， ofto(of 和 to 混淆使用), verb_form(动词形态错误), capitalize(大小写错误)， spell(拼写错误)。
```

调用示例
```
curl -X POST 
http://grammar.llsapp.com/llcup/compute
 -H 'Content-Type: application/json' 
 -d '{
 "type": "writing",
 "text": "he go to schol."
  }'
```

返回结果

```
{
  "content": [
    {
      "origin": [
        "he",
        "go",
        "to",
        "schol",
        "."
      ],
      "corrected": [
        "He",
        "goes",
        "to",
        "school",
        "."
      ]
    }
  ],
  "score": {
    "coherence": 4,
    "total": 0,
    "grammar": 2,
    "vocabulary": 4
  },
  "grammar": [
    {
      "origin": "go",
      "correction": "goes",
      "error_type": "sva",
      "sentence_id": 0,
      "error_loc": 1,
      "time": [
        0,
        0
      ]
    },
    {
      "origin": "schol",
      "correction": "school",
      "error_type": "spell",
      "sentence_id": 0,
      "error_loc": 3
    },
    {
      "origin": "he",
      "correction": "He",
      "error_type": "capitalize",
      "sentence_id": 0,
      "error_loc": 0
    }
  ]
}
```