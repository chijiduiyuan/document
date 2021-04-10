> HOST http://api.zhang1009.com

#### 预估收益计算方式
> 当goat价格 小于 得物价格，默认goat购买，得物卖出。goat额外费用为9.1%税费 + 128元配送费，计算方式：商品价格*0.091 + 12800；dewu额外费用为6%手续费 + 33元操作服务费，计算方式：商品价格*0.06 + 3300。
> 当goat价格 大于 得物价格，默认goat卖出，得物购买。goat额外费用为9%手续费 + 14元运往深圳仓物流费 + 深圳仓发货物流费用(暂时未知) + goat提现手续费(暂时未知)，计算方式：商品价格*0.09 + 1400；dewu额外费用为物流费用14元，计算方式：1400.
> 物流费用受地域影响，不同地区会有差异。

登录注册
======================
### URL

`/user/login`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | post | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| account | true | string | 登录账号 |
| password | true | string | md5加密密码 |

### 返回结果头部信息
```
Authorization:Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjYsImlhdCI6MTYxNzk1ODY3NSwiZXhwIjoxNjIwNTUwNjc1fQ.KHExoMRYkRzZCXDNVG0XJKgvUOTXhVqLvCtAdCnwh6o
```

用户信息
======================
### URL

`/user/info`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 头部参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| Authorization | true | string | 登录获取的token |

### 返回结果示例
```
{
    "uid": 6,
    "username": "86-15070924883",
    "avatar": null
}
```



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
| sortBy | false | string | 排序规则:dewu_sold_num=得物销量 dewu_price=得物价格 goat_price=goat价格 cha_price=差价,默认cha_price,由dewu_price-goat_price计算而成 |
| sortModel | false | number | 排序方式:0=从小到大 1=从大到小 |

### 结果示例
```   js
[
    {
        "id":"1", //商品id
        "sku": "315122-111", //主货号
        "size": "43", //尺码
        "us_size": "9", //美码
        "gender": "men", //适用范围
        "goat_price": 11900, //goat价格(分)
        "dewu_price": 74900, //得物价格(分)
        "cha_price": 63000, //差价
        "dewu_buy_log": [ //得物购买记录
            {
                "avatar": "https://cdn.poizon.com/du_app/2020/image/50574925_byte91265byte_f846ef977e463f79adf65efd42874789_iOS_w828h828.jpg?imageView2/2/w/50/h/50",
                "userName": "今*么",
                "formatTime": "刚刚",
                "orderSubTypeName": "",
                "price": 72900,
                "propertiesValues": "42.5"
            },
        ],
        "dewu_sold_num": 593879, //得物销量,
        "dewu_link": "", //得物跳转连接,
        "goat_link": "", //goat跳转连接,
        "title": "【Jennie同款】Nike Air Force 1 07 low 空军一号 白色 板鞋", //标题
        "sub_title": "",
        "images": "https://cdn.poizon.com/pro-img/origin-img/20201228/9ba0d5f565f647c88f4885ee3921a082.jpg",
        "sell_date": "2017.01.01", //发售日期
        "amount": 88156, //预估收益(分)
        "goat_extra_price": 29562, //goat额外费用
        "dewu_extra_price": 22782 //得物额外费用
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
| sku | true | string | 商品sku |

### 返回结果示例
```   js
{
    "sku": "ML2002RC",
    "images": "https://cdn.poizon.com/source-img/origin-img/20201206/a966510bf2ab4e01bcfb90b958fbcfb9.jpg",
    "title": "New Balance 2002R 灰色",
    "sub_title": "",
    "sell_date": "2020秋季",
    "skuList": [
        {
            "sku": "ML2002RC",
            "size": "36",
            "us_size": "4",
            "gender": "",
            "goat_price": 0,
            "dewu_price": 0,
            "dewu_buy_log": "[{\"avatar\":\"http://thirdqq.qlogo.cn/g?b=oidb&k=lQC4wdVpaANH1Em84icHBNA&s=100?imageView2/2/w/50/h/50\",\"userName\":\"d*i\",\"formatTime\":\"20分钟前\",\"orderSubTypeName\":\"\",\"price\":123900,\"propertiesValues\":\"D宽 42\"},{\"avatar\":\"https://cdn.poizon.com/node-common/TGFyazIwMjAwMzIwLTE4MzQ0OA==.png?imageView2/2/w/50/h/50\",\"userName\":\"斯*s\",\"formatTime\":\"46分钟前\",\"orderSubTypeName\":\"\",\"price\":143900,\"propertiesValues\":\"D宽 43\"},{\"avatar\":\"https://cdn.poizon.com/node-common/TGFyazIwMjAwMzIwLTE4MzQ0OA==.png?imageView2/2/w/50/h/50\",\"userName\":\"很*f\",\"formatTime\":\"47分钟前\",\"orderSubTypeName\":\"\",\"price\":127900,\"propertiesValues\":\"D宽 44\"},{\"avatar\":\"https://cdn.poizon.com/node-common/TGFyazIwMjAwMzIwLTE4MzQ0OA==.png?imageView2/2/w/50/h/50\",\"userName\":\"魁*E\",\"formatTime\":\"1小时前\",\"orderSubTypeName\":\"\",\"price\":111900,\"propertiesValues\":\"D宽 41.5\"},{\"avatar\":\"https://cdn.poizon.com/du_app/2020/image/38487553_byte95846byte_1fe256429c96bf0bba1d9244ec9089ea_iOS_w828h828.jpg?imageView2/2/w/50/h/50\",\"userName\":\"七*_\",\"formatTime\":\"2小时前\",\"orderSubTypeName\":\"\",\"price\":127900,\"propertiesValues\":\"D宽 44\"}]",
            "dewu_sold_num": 23286,
            "dewu_link": "https://m.dewu.com/router/product/ProductDetail?spuId=1192115&sourceName=shareDetail",
            "goat_link": null,
            "amount": 88156, //预估收益(分)
            "goat_extra_price": 29562, //goat额外费用
            "dewu_extra_price": 22782 //得物额外费用
        },
    ]
}
```


创建仓库
======================
### URL

`/store/add_store`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | post | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| title | false | string | 仓库标题 |
| des | false | string | 仓库描述 |



录单
======================
### URL

`/store/add_store_goods`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | post | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| id | true | int | 仓库id |
| sku | true | int | 商品sku |
| size | true | int | 欧码 |
| us_size | false | string | 美码 |
| gender | false | string | 身份 |
| buy_price | true | int | 进价(分) |
| sell_price | true | false | 售价 |


仓库/仓库商品列表
======================
### URL

`/store/list`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| type | false | int | 0=仓库列表 1=仓库商品列表，默认0 |
| id | false | int | 仓库id，获取仓库商品列表时必须 |
| page_no | false | int | 页码，默认0 |


### 返回结果示例
```
//仓库列表
[
    {
        "id": 1, //仓库id
        "parent_id": 0,
        "uid": 6,
        "title": "默认仓库", //仓库标题
        "des": "", //仓库描述
        "type": 0,
        "content": null,
        "created_time": 1617964045,
        "updated_time": 1617964045
    }
]


//仓库商品列表
[
    {
        "sku": "315",
        "title": "",
        "images": "",
        "size": "36.5",
        "us_size": "6.5",
        "gender": "women",
        "buy_price": 1703, //进价(分)
        "sell_price": 0 //售价(分)
    }
]
```


同步全部商品goat信息
======================
### URL

`/goat/sync_list`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 


同步sku商品goat信息
======================
### URL

`/goat/sync_item`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| sku | true | string | sku |