# PXEC API
<a id="markdown-overview" name="overview"></a>

<!-- TOC -->

- [PXEC API](#pxec-api)
- [History](#history)
- [1. 系統代碼定義](#1-系統代碼定義)
    - [1.1. Http_Status_Code:200](#11-http_status_code200)
    - [1.2. Http_Status_Code:400](#12-http_status_code400)
    - [1.3. Http_Status_Code:401](#13-http_status_code401)
- [2. 共同規範](#2-共同規範)
  - [2.1 通用 Header](#21-通用-header)
- [3. 管理後台](#3-管理後台)
  - [共同規格](#共同規格)
    - [3.1 帳號密碼驗證](#31-帳號密碼驗證)
      - [3.1.1 Diagram](#311-diagram)
      - [3.1.2 Request](#312-request)
      - [3.1.3 Response](#313-response)
    - [3.2 新增帳號](#32-新增帳號)
      - [3.2.1 Diagram](#321-diagram)
      - [3.2.2 Request](#322-request)
      - [3.2.3 Response](#323-response)
    - [3.3 後台使用者列表](#33-後台使用者列表)
      - [3.3.1 Diagram](#331-diagram)
      - [3.3.2 Request](#332-request)
      - [3.3.3 Response](#333-response)
    - [3.4 更新後台使用者](#34-更新後台使用者)
      - [3.4.1 Diagram](#341-diagram)
      - [3.4.2 Request](#342-request)
      - [3.4.3 Response](#343-response)
    - [3.5 檔案上傳](#35-檔案上傳)
      - [3.5.1 Diagram](#351-diagram)
      - [3.5.2 Request](#352-request)
      - [3.5.3 Response](#353-response)
- [4. 後台商品管理](#4-後台商品管理)
    - [4.1 列出30天商品草稿上傳記錄](#41-列出30天商品草稿上傳記錄)
      - [Request](#request)
      - [Response](#response)


<!-- /TOC -->

# History
<a id="markdown-history" name="history"></a>
<br/>

| Version | Date       | Author    | Description                                        |
| ------- | ---------- | --------- | -------------------------------------------------- |
| 1.00    | 2021-07-21 | Evan Chien| First Ver                                          |


# 1. 系統代碼定義
<a id="markdown-1.系統代碼定義" name="系統代碼定義"></a>
<br/>

### 1.1. Http_Status_Code:200
<a id="markdown-httpstatuscode200" name="httpstatuscode200"></a>

| Http status code | Code | Message                                          |Description  |
| ---------------- | ---- | ------------------------------------------------ | ----------- |
| 200              | 0000 | success                                          |             |
| 200              | 1000 | 欄位資料驗證錯誤                                  |             |
| 200              | 2001 | Service沒有使用權限                               | 對服務       |
| 200              | 2002 | 沒有使用權限                                      | 對使用者    |
| 200              | 3001 | 此員工Email尚未註冊，請與內部人員聯繫註冊          |             |
| 200              | 3002 | 此帳號已禁止登入，請與管理員聯繫重設密碼。          |             |
| 200              | 3003 | 密碼輸入錯誤，請重新輸入!錯誤次數{n}次             |             |
| 200              | 3004 | 無法取得登入Token。                               |             |
| 200              | 3005 | Email帳號已存在                                  |             |
| 200              | 3006 | 請設定一組新密碼                                  |             |

<br/>

### 1.2. Http_Status_Code:400
<a id="markdown-httpstatuscode400" name="httpstatuscode400"></a>

| Http status code | Code | Message  | Description |
| ---------------- | ---- | -------- | ----------- |
| 400              | 9999 | 系統異常 |             |
| 400              | 2003 | Token不合法或以逾期，請Refresh Token            |如再不合法重新登入|
| 400              | 2004 | Refresh Token 不合法，請重新登入                  |             |
| 400              | 2005 | Refresh Token 已失效，請重新登入                  |             |
| 400              | 2006 | 驗證失敗，請重新登入                              |             |
| 400              | 2007 | 驗證會員失敗，請重新登入                          |             |
| 400              | 2008 | 驗證使用者失敗，請重新登入                        |             |

<br/>

### 1.3. Http_Status_Code:401
<a id="markdown-httpstatuscode401" name="httpstatuscode401"></a>

| Http status code | Code | Message                         | Description                                             |
| ---------------- | ---- | ------------------------------- | ------------------------------------------------------  |
| 401              | 2003 | 不合法授權/授權已逾期，請重新登入 | Token不合法或以逾期，請Refresh Token，如在不合法重新登入  |

<br/>

# 2. 共同規範
<a id="markdown-2.共同規範" name="共同規範"></a>

## 2.1 通用 Header
<a id="markdown-2.1Header" name="Header"></a>
    前端呼叫API時 Header 需帶參數
<br>

+ Header

    | Parameter    | Type   | Mandatory | Description                                     |
    | ------------ | ------ | --------- | ----------------------------------------------- |
    | platform_id  | string | Y         | GroupBuying(圈團消費者端):a2f9f7fa-529a-4e18-99a5-33f625fb71b7<br>GroupInitiator(團爸團媽端):b4bfc8a8-5944-428a-8849-17b7ad480e00<br>Backend(後台):d603b103-ef07-43d3-a5a2-0beea3796411|
    | authentication | string | Y       | 使用者token, 格式: Bearer XXXXX |
    |  |


# 3. 管理後台
<a id="markdown-管理後台" name="管理後台"></a>

## 共同規格

**角色Role**

|   role  | value | description |
| ------- | ----- | ----------- |
| Product |   1   | 圈團-製檔        |
| Approve |   2   | 圈團-審核批准    |
| Customer|   3   | 圈團-客服        |
|  admin  |  999  | 系統管理    |


### 3.1 帳號密碼驗證
<a id="markdown-帳號密碼驗證" name="帳號密碼驗證"></a>
    驗證後台使用者帳號密碼
 <br/>

#### 3.1.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 3.1.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /backend/login ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```
+ Header
    <br>
    `不需要帶 authentication`
+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | account      | string | Y         | 帳號                         |
    | password     | string | Y         | 密碼                         |

+ Example
    ```json
    {
        "account": "evan",
        "password" :"abcd-1234"
    }
    ```

#### 3.1.3 Response
<a id="markdown-response" name="response"></a>

+ Content Type : ``` application/json ```
+ Body

    | Parameter   | Type   | Mandatory | Descrption                                                     |
    | ----------- | ------ | --------- | ------------------------------------------------------------- |
    | code        | string | Y         | response code <br/> 1000: 欄位資料驗證錯誤<br/> 9999: 系統異常  |
    | src         | string | Y         | 服務代碼                                                       |
    | message     | string | Y         | response message                                              |
    | data        | object | Y         | Auth Token                                                    |

    **data**
    
    | Parameter | Type | Mandatory | Descrption     |
    | ------------- | ------ | --------- | ------------- |
    | access_token  | string | Y         | Auth token    |
    | refresh_token | string | Y         | Refresh token |

+ Example

    ```json
    {
        "code": "0000",
        "srv": "BKE",
        "message": "success",
        "data":
        { 
            "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTAwMDEiLCJzdWIiOiIxMjIwMjk0ODk0IiwianRpIjoiNjk1ZWFjZDktMjM3Yi00MTcyLWFlZGQtYmIxNzBlMzVhYjQ1IiwiaXNzIjoiUFhFQyIsInNjb3BlIjoiRUMsR3JvdXAiLCJuYmYiOjE2MjY4NDUxODQsImV4cCI6MTYyNjg4MTE4NCwiaWF0IjoxNjI2ODQ1MTg0fQ.wR3QyDafLZnB75wcGKfw_fXnt8mPuxPBSbBdU1DSNyc",
            "refresh_token": "79dd3da50bbc4107a2b5ad3de6347d70.246c38be05504daeb780c822690fa1f8",
        }
    }
    ```

### 3.2 新增帳號
<a id="markdown-新增帳號" name="新增帳號"></a>
    新增後台使用者
 <br/>

#### 3.2.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 3.2.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /pxec/manager/add ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```

+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | employee_no  | string | Y         | 新增帳號                      |
    | name         | string | Y         | 密碼                         |
    | email        | string | Y         | Email (登入帳號)              |
    | first_login_password  | string    | Y | 首次登入密碼             |
    | role         | int[]  | Y         | 角色ID                       |


+ Example
    ```json
    {
        "employee_no": "2999999",
        "name" : "福利雄",
        "email" : "fulibear@pxmart.com.tw",
        "first_login_password" : "abcd-1234",
        "role" : [1,2,3]
    }
    ```

#### 3.2.3 Response
<a id="markdown-response" name="response"></a>

+ Content Type : ``` application/json ```
+ Body

    | Parameter   | Type   | Mandatory | Descrption                                                     |
    | ----------- | ------ | --------- | ------------------------------------------------------------- |
    | code        | string | Y         | response code <br/> 1000: 欄位資料驗證錯誤<br/> 9999: 系統異常  |
    | src         | string | Y         | 服務代碼                                                       |
    | message     | string | Y         | response message                                              |
    | data        | object | Y         | null                                                          |

+ Example

    ```json
        {
            "code": "0000",
            "srv": "BKE",
            "message": "success",
            "data": null
        }

    ```

### 3.3 後台使用者列表
<a id="markdown-後台使用者列表" name="後台使用者列表"></a>
    後台帳號管理使用者列表
 <br/>

#### 3.3.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 3.3.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /backend/manager/query ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```
+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | create_time_s| string |           | 建立帳號時間(起)              |
    | create_time_e| string |           | 建立帳號時間(訖)              |
    | employee_no  | string |           | 員工編號                     |
    | name         | string |           | 姓名                         |

#### 3.3.3 Response
<a id="markdown-response" name="response"></a>

+ Content Type : ``` application/json ```
+ Body

    | Parameter   | Type   | Mandatory | Descrption                                                     |
    | ----------- | ------ | --------- | ------------------------------------------------------------- |
    | code        | string | Y         | response code <br/> 1000: 欄位資料驗證錯誤<br/> 9999: 系統異常  |
    | src         | string | Y         | 服務代碼                                                       |
    | message     | string | Y         | response message                                              |
    | data        | object | Y         | 員工資料                                                          |

    **data**
    | Parameter         | Type        | Mandatory | Descrption                                     |
    | ----------------- | ----------- | --------- | ---------------------------------------------- |
    | id                | int         | Y         | Key      |
    | create_time       | Datetime    | Y         | 建立時間  |
    | employee_no       | string      | Y         | 員工編號  |
    | email             | email       | Y         | 員工Email |
    | name              | name        | Y         | 姓名      |
    | modify_time       | modify_time | Y         | 最後編輯時間 |
    | is_setting_pwd    | bool        | Y         | 是否已設定密碼 |
    | is_disable        | bool        | Y         | 是否帳號停用   |

+ Example

    ```json
        {
            "code": "0000",
            "srv": "BKE",
            "message": "success",
            "data": [
                {
                    "id" : "1001",
                    "create_time" : "2022-01-10 12:00:00",
                    "employee_no" : "2100001",
                    "email": "fulibear@pxmart.com.tw",
                    "name": "福利雄",
                    "modify_time": "2922-02-01 12:00:00",
                    "is_setting_pwd" : true,
                    "is_disable" : false
                },
                {
                    "id" : "1002",
                    "create_time" : "2022-02-01 12:00:00",
                    "employee_no" : "2100002",
                    "email": "appledog@pxmart.com.tw",
                    "name": "蘋狗",
                    "modify_time": "2922-03-01 12:00:00",
                    "is_setting_pwd" : true,
                    "is_disable" : true
                }
            ]
        }

    ```

### 3.4 更新後台使用者
<a id="markdown-更新後台使用者" name="更新後台使用者"></a>
    後台帳號管理更新帳號
 <br/>

#### 3.4.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 3.4.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /backend/manager/update ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```
+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | employee_no  | int    | Y         | 員工編號                     |
    | name         | string |           | 姓名                         |
    | role         | int[]  |           | 角色                         |
    | password     | string |           | 重設密碼需給值 (初始密碼)     |
    | is_reset_pwd | bool   | Y         | 是否重設密碼                  |
    | Set_disable  | bool   | Y         | 是否停用帳號(離職)            |

#### 3.4.3 Response
<a id="markdown-response" name="response"></a>

+ Content Type : ``` application/json ```
+ Body

    | Parameter   | Type   | Mandatory | Descrption                                                     |
    | ----------- | ------ | --------- | ------------------------------------------------------------- |
    | code        | string | Y         | response code <br/> 1000: 欄位資料驗證錯誤<br/> 9999: 系統異常  |
    | src         | string | Y         | 服務代碼                                                       |
    | message     | string | Y         | response message                                              |
    | data        | object | Y         | null                                                          |

+ Example

    ```json
        {
            "code": "0000",
            "srv": "BKE",
            "message": "success",
            "data": null
        }

    ```
### 3.5 檔案上傳
<a id="markdown-檔案上傳" name="檔案上傳"></a>
    後台帳號管理更新帳號
 <br/>

#### 3.5.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 3.5.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /service/1.0/file/upload ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```
+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | PlatformId   | Guid   | Y         | 平台ID                       |
    | user_id      | int    | Y         | 上傳者ID                     |
    | base64       | string | Y         | 檔案Base64編碼               |
    | is_watermark | bool   | Y         | 是否壓上浮水印                |

#### 3.5.3 Response
<a id="markdown-response" name="response"></a>

+ Content Type : ``` application/json ```
+ Body

    | Parameter   | Type   | Mandatory | Descrption                                                     |
    | ----------- | ------ | --------- | ------------------------------------------------------------- |
    | code        | string | Y         | response code <br/> 1000: 欄位資料驗證錯誤<br/> 9999: 系統異常  |
    | src         | string | Y         | 服務代碼                                                       |
    | message     | string | Y         | response message                                              |
    | data        | object | Y         |上傳結果                                                |


     **data**

    | Parameter         | Type        | Mandatory | Descrption                                     |
    | ----------------- | ----------- | --------- | ---------------------------------------------- |
    | file_id           | int         | Y         | Key                   |
    | file_name         | string      | Y         | 上傳後檔名             |
    | full_name         | string      | Y         | 上傳後含資料夾名稱檔名  |


+ Example

    ```json
        {
            "code": "0000",
            "srv": "UPL",
            "message": "success",
            "data": {
                "file_id"：1,
                "file_name": "63763518463338.jpg",
                "full_name": "202108\63763518463338.jpg"
            }
        }

    ```


# 4. 後台商品管理
<a id="markdown-後台商品管理" name="後台商品管理"></a>

### 4.1 列出30天商品草稿上傳記錄
<a id="markdown-列出30天商品草稿上傳記錄" name="列出30天商品草稿上傳記錄"></a>

#### Request
+ Url Format : ``` /backend/1.0/product/listProductDraftsWithin30Days ```
+ Http Method: [ **GET** ]
+ Parameters: ```無參數```

#### Response
+ Content Type : ``` application/json ```
+ Body

    | Parameter   | Type   | Mandatory | Descrption              |
    | ----------- | ------ | --------- | ----------------------- |
    | createTime  | string | Y | 上傳時間，格式: 2021/07/26 16:15 |
    | productName | string | Y | 商品名稱                        |
    | itemNo      | string | Y | 條碼                            |
    | barcode     | string | Y | 貨號                            |

+ Example

    ```json
    {
        "code": "0000",
        "message": "Success",
        "result": [
        {
            "createTime": "2021/07/26 16:15",
            "productName": "WFT平底鍋",
            "itemNo": "A0001",
            "barcode": "106612001"
        },
        {
            "createTime": "2021/07/26 16:15",
            "productName": "WTF叉子",
            "itemNo": "A0002",
            "barcode": "106612002"
        }]
    }

    ```