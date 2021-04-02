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
| sortBy | false | string | 排序规则:soldNum=销量 dewu_price=得物价格 goat_price=goat价格 cha_price=差价,默认cha_price,由dewu_price-goat_price计算而成 |
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
        "sell_date": "2017.01.01" //发售日期
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
| goods_id | true | int | 商品id |

### 返回结果示例
```   js
{
    "id": 201,
    "sku": "BV2667-010",
    "size": "XXXL",
    "us_size": "",
    "gender": "",
    "goat_price": 0,
    "dewu_price": 0,
    "cha_price": 0,
    "dewu_buy_log": [
        {
            "avatar": "https://cdn.poizon.com/du_app/2019/image/62560283_byte88212byte_eb6f472c09b74ecfbac79a68eed25af7_iOS_w750h750.jpg?imageView2/2/w/50/h/50",
            "userName": "爱*女",
            "formatTime": "4分钟前",
            "orderSubTypeName": "",
            "price": 24900,
            "propertiesValues": "XL"
        },
        {
            "avatar": "https://cdn.poizon.com/du_app/2020/image/65177046_byte192371byte_deb88196070fed74d1eff7cf9ec19943_iOS_w1125h1125.jpg?imageView2/2/w/50/h/50",
            "userName": "G*七",
            "formatTime": "4分钟前",
            "orderSubTypeName": "",
            "price": 24900,
            "propertiesValues": "XL"
        },
        {
            "avatar": "https://du.hupucdn.com/FhQbGz1UY2Znmn1GxpHedYSm-AWV?imageView2/2/w/50/h/50",
            "userName": "打*5",
            "formatTime": "5分钟前",
            "orderSubTypeName": "",
            "price": 24900,
            "propertiesValues": "M"
        },
        {
            "avatar": "https://cdn.poizon.com/node-common/TGFyazIwMjAwMzIwLTE4MzQ0OA==.png?imageView2/2/w/50/h/50",
            "userName": "聪*f",
            "formatTime": "29分钟前",
            "orderSubTypeName": "",
            "price": 22900,
            "propertiesValues": "XXL"
        },
        {
            "avatar": "https://cdn.poizon.com/node-common/TGFyazIwMjAwMzIwLTE4MzQ0OA==.png?imageView2/2/w/50/h/50",
            "userName": "俊*2",
            "formatTime": "45分钟前",
            "orderSubTypeName": "",
            "price": 24900,
            "propertiesValues": "L"
        }
    ],
    "dewu_sold_num": 118805,
    "dewu_link": "", //得物跳转连接,
    "goat_link": "", //goat跳转连接,
    "title": "Nike 徽标长袖圆领休闲卫衣 男款 黑色",
    "sub_title": "",
    "images": "https://cdn.poizon.com/pro-img/origin-img/20210119/985d0cd2caac4a75abfa3e7cb7327ef2.jpg",
    "sell_date": "2019夏季"
}
```

同步得物商品列表
======================
### URL

`/dewu/sync_list`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| brandId | true | int | 得物品牌id,nice=144 |


同步得物sku商品
======================
### URL

`/dewu/sync_item`

### 请求方法
| HTTP请求方式 | 说明 |
| ----- | ----- |
| method | get | 

### 请求参数
|    | 必选 | 类型及范围 | 说明 |
| ----- | ----- | ------- | ---------- |
| sku | true | string | sku |


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