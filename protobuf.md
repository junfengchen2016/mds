# protobuf

标签（空格分隔）： 未分类

---

## data type
|.proto类型|Java 类型|C++类型|备注|
|-|-|-|-|
|double|double|double||
|float|float|float||
|int32|int|int32|使用可变长编码方式。编码负数时不够高效——如果你的字段可能含有负数，那么请使用sint32。|
|int64|long|int64|使用可变长编码方式。编码负数时不够高效——如果你的字段可能含有负数，那么请使用sint64。|
|uint32|int[1]|uint32|Uses variable-length encoding.|
|uint64|long[1]|uint64|Uses variable-length encoding.|
|sint32|int|int32|使用可变长编码方式。有符号的整型值。编码时比通常的int32高效。|
|sint64|long|int64|使用可变长编码方式。有符号的整型值。编码时比通常的int64高效。|
|fixed32|int[1]|uint32|总是4个字节。如果数值总是比总是比228大的话，这个类型会比uint32高效。|
|fixed64|long[1]|uint64|总是8个字节。如果数值总是比总是比256大的话，这个类型会比uint64高效。|
|sfixed32|int|int32|总是4个字节。|
|sfixed64|long|int64|总是8个字节。|
|bool|boolean|bool||
|string|String|string|一个字符串必须是UTF-8编码或者7-bit ASCII编码的文本。|
|bytes|ByteString|string|可能包含任意顺序的字节数据。|

## protoc
```
protoc351 --cpp_out=. common.proto
protoc351 --csharp_out=. common.proto
protoc351 --python_out=. common.proto
```

## c++
```
#include <google/protobuf/util/json_util.h>
#include "./proto/mark.pb.h"
    
mark::image_mark image_mark=...;

string json;
mark::image_mark image_mark_json;
google::protobuf::util::MessageToJsonString(image_mark, &json);
google::protobuf::util::JsonStringToMessage(json, &image_mark_json);

vector<unsigned char> bytes(image_mark.ByteSize());
mark::image_mark image_mark_bytes;
image_mark.SerializeToArray(bytes.data(), bytes.size());
image_mark_bytes.ParseFromArray(bytes.data(), bytes.size());
```

## c(#)
```
using Google.Protobuf;

Mark::image_mark image_mark=...;

string json;
Mark.image_mark image_mark_json;
json = image_mark.ToString();
image_mark_json = Mark.image_mark.Parser.ParseJson(json);

byte[] bytes = null;
Mark.image_mark image_mark_bytes;
bytes = image_mark.ToByteArray();
image_mark_bytes = Mark.image_mark.Parser.ParseFrom(bytes);
```

## python
```
import mark_pb2
from google.protobuf import json_format

image_mark = mark_pb2.image_mark()
image_mark.ParseFromString(...)

image_mark_json = mark_pb2.image_mark()
json = json_format.MessageToJson(image_mark, including_default_value_fields=True)
json_format.Parse(json, image_mark_json, ignore_unknown_fields=True)

image_mark_bytes = mark_pb2.image_mark()
bytes = image_mark.SerializeToString()
image_mark_bytes.ParseFromString(bytes)
```

## nodejs
```
const protobufjs = require("protobufjs")

var image_mark = image_markMessage.decode(...);

var json = JSON.stringify(image_mark);
var image_mark_json = JSON.parse(json);

var root = protobufjs.loadSync("./mark.proto");
var image_mark_message = root.lookup("mark.image_mark");
var bytes = image_mark.encode(image_mark_message).finish();
var image_mark_bytes = image_markMessage.decode(bytes);
```



