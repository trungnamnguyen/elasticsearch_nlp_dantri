
PUT /my_search
{
  "settings": {
    "index":{
      "number_of_shards":1,
      "number_of_replicas":5
    },
    "analysis": {
      "analyzer": {
        "mysearch":{
          "tokenizer":"vi_tokenizer",
          "char_filter":  [ "html_strip" ],
            "filter": ["icu_folding"]
        }
      }
    }
  }
}

PUT my_search/_mapping/my_type
{
  "properties": {
    "title":{
      "type": "text",
      "analyzer": "mysearch"
    },
    "content":{
      "type": "text",
      "analyzer": "mysearch"
    },
    "url":{
      "type": "keyword"
    }
  }
}

curl -H "Content-Type: application/json" -XPOST "localhost:9200/my_search/my_type/_bulk?pretty" --data-binary "@elasimport0.json"

