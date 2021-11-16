# # 分词器

## 类型

### simple

```shell
POST _analyze
{
  "analyzer": "simple",
  "text":     "Like X 国庆放假 的"
}

{
  "tokens" : [
    {
      "token" : "like",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "x",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "国庆放假",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "的",
      "start_offset" : 12,
      "end_offset" : 13,
      "type" : "word",
      "position" : 3
    }
  ]
}

```

### whitespace

`whitespace` （空白字符）分词器按空白字符 —— 空格、tabs、换行符等等进行简单拆分 —— 然后假定连续的非空格字符组成了一个语汇单元

```shell
POST _analyze
{
  "analyzer": "whitespace",
  "text":     "Like X 国庆放假的"
}

{
  "tokens" : [
    {
      "token" : "Like",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "X",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "国庆放假的",
      "start_offset" : 7,
      "end_offset" : 12,
      "type" : "word",
      "position" : 2
    }
  ]
}

```

### Standard

```shell
POST _analyze
{
  "analyzer": "standard",
  "text":     "Like X 国庆放假的"
}

{
  "tokens" : [
    {
      "token" : "like",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "x",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "国",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "庆",
      "start_offset" : 8,
      "end_offset" : 9,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "放",
      "start_offset" : 9,
      "end_offset" : 10,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "假",
      "start_offset" : 10,
      "end_offset" : 11,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "的",
      "start_offset" : 11,
      "end_offset" : 12,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    }
  ]
}

```



