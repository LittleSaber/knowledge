## 约定
### 通讯协议

使用HTTP（GET、POST、PUT、DELETE）协议

### 数据格式
输入：http 参数
输出：json

### URL约定
{host}/api/

### header约定

Accept: application/vnd.chekuaikuai.**==v1==**+json

Authorization: Bearer **==token==**

*v1 为接口版本号*

*Authorization 只有==需要登录==的接口需要*



### 分页数据格式

| 参数            | 值      | 描述                     |
| ------------- | ------ | ---------------------- |
| total         | int    | 总数                     |
| per_page      | int    | 每页条数                   |
| current_page  | int    | 当前页码                   |
| last_page     | int    | 最后一页                   |
| next_page_url | string | 下一页url（如果为null表示没有上一页） |
| prev_page_url | string | 上一页url（如果为null表示没有下一页） |



### 其它通用格式说明

| 参数    | 值      | 描述                                       |
| ----- | ------ | ---------------------------------------- |
| page  | int    | 分页传参数页码字段,在对应请求的接口url后加?page=xxx或&page=xxx参数 |
| phone | string | 订单相关接口搜索字段,在对应请求的接口url后加?phone=133xxxxxxx或&phone=133xxxxxxx参数 |



### 异常消息格式

| 参数          | 值      | 描述               |
| ----------- | ------ | ---------------- |
| message     | string | 异常消息描述           |
| status_code | int    | 异常状态码            |
| code        | int    | 内部异常码            |
| debug       | object | 调试信息，可忽略，正式环境不输出 |



### 内部异常

| 数值    | 描述             |
| ----- | -------------- |
| 10001 | 登陆错误次数太多       |
| 10002 | 登陆失败           |
| 10003 | 注册失败           |
| 10004 | 注册验证码校验失败      |
| 20001 | 账户被禁用          |
| 20002 | 资金账户为主账户（不可操作） |
| 20003 | 资金账户操作参数异常     |
| 20004 | 资金账户余额不足       |



## 接口列表

### 省市县接口

1. 获取省份列表
   描述：获取城市列表
   URL：GET /province
   参数：

   | 参数   | 值    | 是否必填 | 描述   |
   | ---- | ---- | ---- | ---- |
   |      |      |      |      |

   返回：
   http 200

   | 参数        | 值      | 描述      |
   | --------- | ------ | ------- |
   | id        | int    | 省份ID    |
   | name      | string | 身份名称    |
   | enName    | string | 省份名称拼音  |
   | firstName | string | 省份名称首字母 |

2. 获取城市列表
   描述：获取城市列表
   URL：GET /city
   参数：

   | 参数   | 值    | 是否必填 | 描述   |
   | ---- | ---- | ---- | ---- |
   | id   | int  | 是    | 省份ID |

   返回：
   http 200

   | 参数        | 值      | 描述      |
   | --------- | ------ | ------- |
   | id        | int    | 城市ID    |
   | name      | string | 城市名称    |
   | enName    | string | 城市名称拼音  |
   | firstName | string | 城市名称首字母 |

3. 获取县级列表
   描述：获取县级列表
   URL：GET /region
   参数：

   | 参数     | 值    | 是否必填 | 描述   |
   | ------ | ---- | ---- | ---- |
   | cityId | int  | 是    | 县级ID |

   返回：
   http 200

   | 参数        | 值      | 描述      |
   | --------- | ------ | ------- |
   | id        | int    | 县级ID    |
   | name      | string | 县级名称    |
   | enName    | string | 县级名称拼音  |
   | firstName | string | 县级名称首字母 |

4. 获取省市县列表
   描述：获取县级列表
   URL：GET /getAreaList
   参数：

   | 参数   | 值    | 是否必填 | 描述   |
   | ---- | ---- | ---- | ---- |
   |      |      |      |      |

   返回(省级列表-has_many_city(市级列表)-has_many_region(区县列表))：
   http 200

   | 参数                         | 值      | 描述          |
   | -------------------------- | ------ | ----------- |
   | id                         | int    | ID          |
   | name                       | string | 县级名称        |
   | provinceId或cityId或regionId | string | 省id、市id、县id |

5. 获取当前城市
   描述：获取当前城市
   URL：GET /locate
   参数：
   
   | 参数    | 值    | 是否必填 | 描述   |
   | ---- | ---- | ---- | ---- |
   | lat  | float| 是 | 纬度 |
   | long | float| 是 | 经度 |
   
   返回：
   http 200
   
   | 参数 | 值 | 描述 |
   | ---- | ---- | ---- |
   | id | int | 城市id |
   | name | string | 城市名称 |


### 品牌厂商接口

1. 获取全部品牌接口
   描述：根据品牌字母分组获取品牌【如果需要全部品牌自己添加为 全部品牌：0】
   URL：GET /brand
   参数：

   | 参数   | 值    | 描述   |
   | ---- | ---- | ---- |
   |      |      |      |

   返回：
   http 200

   | 参数            | 值      | 描述                     |
   | ------------- | ------ | ---------------------- |
   | 参数            | 值      | 描述                     |
   | isCache       | bool   | 是否可以读取本地缓存（true、false） |
   | brandList     | array  | 以下为品牌列表                |
   | id            | int    | 品牌ID                   |
   | brandName     | string | 品牌名称                   |
   | brandInitials | string | 品牌首字母                  |
   | fileSrc       | string | 品牌logo                 |

2. 获取品牌下厂商接口
   描述：获取品牌下厂商接口【如果需要全部品牌自己添加为 全部厂商：0】
   URL：GET /facture
   参数：

   | 参数   | 值    | 是否必填 | 描述   |
   | ---- | ---- | ---- | ---- |
   | id   | int  | 是    | 品牌ID |

   返回：
   http 200

   | 参数           | 值      | 描述   |
   | ------------ | ------ | ---- |
   | id           | int    | 厂商ID |
   | brandId      | int    | 品牌ID |
   | brandName    | string | 品牌名称 |
   | facturerName | string | 厂商名称 |



### 授权接口

1. 登录
   描述：登录接口
   URL：POST /auth/login
   参数：

   | 参数       | 值      | 是否必填 | 描述                |
   | -------- | ------ | ---- | ----------------- |
   | phoneNum | string | 是    | 用户手机号             |
   | password | string | 是    | 用户密码(使用rsa加密后的密码) |

   返回：
   http 200

   | 参数           | 值      | 描述                                       |
   | ------------ | ------ | ---------------------------------------- |
   | id           | int    | 用户id                                     |
   | phoneNum     | string | 手机号码                                     |
   | token        | string | 用户token                                  |
   | status       | int    | 状态。0：正常(未认证)；1：禁用；2：实名认证通过；3：实名认证中；4：实名认证不通过 |
   | verifyReason | string | 审核失败原因                                   |
   | type         | int    | 用户类型 2为企业用户                              |
   | gender       | int    | 用户性别(0:未知；1：女；2：男)                       |
   | hasShop      | int    | 是否提交过店铺信息1:提交过 0未提交                      |

2. 注册
   描述：注册接口
   URL：POST /auth/register
   参数：

   | 参数         | 值      | 是否必填 | 描述                |
   | ---------- | ------ | ---- | ----------------- |
   | phoneNum   | string | 是    | 用户手机号             |
   | password   | string | 是    | 用户密码(使用rsa加密后的密码) |
   | verifyCode | string | 是    | 手机验证码             |

   返回：
   http 200

   | 参数    | 值      | 描述      |
   | ----- | ------ | ------- |
   | token | string | 用户token |

3. 获取验证码
   描述：获取手机验证码(测试环境填000000)
   URL：GET /auth/getVerifyCode
   参数：

   | 参数       | 值      | 是否必填 | 描述    |
   | -------- | ------ | ---- | ----- |
   | phoneNum | string | 是    | 用户手机号 |

   返回：
   http 200

   | 参数   | 值    | 描述   |
   | ---- | ---- | ---- |
   |      |      |      |

4. 验证验证码
   描述：验证验证码(测试环境填000000)
   URL：GET /auth/checkVerifyCode
   参数：

   | 参数         | 值      | 是否必填 | 描述    |
   | ---------- | ------ | ---- | ----- |
   | phoneNum   | string | 是    | 用户手机号 |
   | verifyCode | string | 是    | 验证码   |

   返回：
   http 200

   | 参数   | 值    | 描述   |
   | ---- | ---- | ---- |
   |      |      |      |

5. 图片上传(测试地址)
   描述：图片上传地址
   URL：POST [http://file.tchekk.com/fileServer/fileServer/save](http://file.tchekk.com/fileServer/fileServer/save)
   参数：

   | 参数   | 值      | 是否必填 | 描述        |
   | ---- | ------ | ---- | --------- |
   | file | object | 是    | 以form形式上传 |

   返回：
   http 200

   | 参数   | 值    | 描述                           |
   | ---- | ---- | ---------------------------- |
   | data | int  | 图片id (用于企业信息店铺信息中图片上传中的图片字段) |

6. 检测用户是否注册
   描述：检测用户是否在垫付宝,轻易贷等平台注册
   URL：GET /auth/checkUserExists
   参数：

   | 参数     | 值      | 是否必填 | 描述    |
   | ------ | ------ | ---- | ----- |
   | mobile | string | 是    | 用户手机号 |

   返回：
   http 200

   | 参数   | 值    | 描述   |
   | ---- | ---- | ---- |
   |      |      |      |
