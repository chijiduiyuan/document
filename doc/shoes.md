> HOST http://api.zhang1009.com


商品列表
======================
### URL

`/goods/list`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| page_no | false | nuber | 页码,默认0 |
| gender | false | string | women=女性 men=男性 younth=青少年 infant=儿童 |
| size | false | string | 尺码，欧码 |
| sortBy | false | string | 排序规则:soldNum=销量 dewu_price=得物价格 goat_price=goat价格 cha_price=差价,默认cha_price |
| sortModel | false | number | 排序方式:0=从小到大 1=从大到小 |

### 结果示例
```   js
[
    {
        "sku": "BV0072-001", //通用sku,主货号
        "images": "[\"https://cdn.poizon.com/source-img/origin-img/20201205/0156207812a54a1e9a464f4185ce38db.jpg\"]", //商品主图
        "title": "Sacai x Nike Blazer Mid Black Blue 解构 白蓝", //商品名
        "dewu_price": 217900, //得物价格(分)
        "goat_price": 0, //goat价格(分)
        "cha_price": 217900, //差价(分)
        "size": null, //尺码
        "gender": "men", //身份
        "sold_num": 33976 //销量
    }
]
```


商品详情
======================
### URL

`/goods/detail`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| sku | true | string | 主货号 |

### 返回结果示例
```   js
{
    "sku": "394053-001",
    "images": "[\"https://cdn.poizon.com/source-img/origin-img/20201207/aaf94b73977f4c3fbd4aabe5ca912da1.jpg\"]",
    "title": "【周雨彤同款】Nike Initiator 灰银 女款",
    "dewu_price": 53900,
    "goat_price": 0,
    "cha_price": 53900,
    "size": null,
    "buy_log": null
}
```
