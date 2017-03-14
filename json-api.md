
# 项目 JSON API 规范

## 文档中的关键字依据 [RFC2119](https://tools.ietf.org/html/rfc2119) 规范解释。 
+ MUST
+ MUST NOT
+ REQUIRED
+ SHALL
+ SHALL NOT
+ SHOULD
+ SHOULD NOT
+ RECOMMENDED
+ MAY
+ OPTIONAL
    
## 文档结构

JSON 对象必须***[MUST]***位于每个JSON API文档的根级，这个对象定义文档的"top level"。
文档必须（MUST）包含以下至少一种top-level键，data键和errors键不能***[MUST NOT]***在一个文档中同时存在。：

* data: 文档的"primary data"。(对象或数组)
* errors: 错误对象列表。(只可以是数组)
* meta: 包含非标准元信息的元对象。

## Errors Objects

错误对象提供执行操作时遇到问题的额外信息。 在JSON API文档顶层，错误对象必须[MUST]被作为errors键对应的数组返回。
错误对象可能[MAY]有以下成员:

* id - 特定问题的唯一标识符。
    * links - 一个包括以下成员的连接对象:
* about - 指向特定问题更多具体内容的连接。
* __status__ - 适用于这个问题的HTTP状态码，使用字符串表示。
* __code__ - 应用特定的错误码，以字符串表示。
* __title__ - 简短的，可读性高的问题总结。除了国际化本地化处理之外，不同场景下，相同的问题，值是不应该变动的。
* __detail__ - 针对该问题的高可读性解释。与title相同,这个值可以被本地化。
* source - 包括错误资源的引用的对象。它也可以包括以下任意成员:
    * pointer - 一个指向请求文档中相关实体的JSON Pointer。
    * parameter - 指出引起错误的查询对象的字符串。
* meta - 一个包括关于错误的非标准元信息的元对象。


