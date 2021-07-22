# PXEC API
<a id="markdown-overview" name="overview"></a>

<!-- TOC -->

- [Overview](#overview)
- [History](#history)
- [1.系統代碼定義](#markdown-1.系統代碼定義)
    - [1.1 Http_Status_Code:200](#markdown-httpstatuscode200)
    - [1.2 Http_Status_Code:401](#markdown-httpstatuscode400)
    - [1.3 Http_Status_Code:400](#markdown-httpstatuscode401)
- [2.後台登入](markdown-2.共同規範)
    - [2.1 Header](#markdown-2.1Header)
- [3.管理後台](#markdown-管理後台)
    - [3.1 帳號密碼驗證](#markdown-帳號密碼驗證)
    - [3.2 新增帳號](#markdown-新增帳號)
    - [3.3 後台使用者列表](#markdown-後台使用者列表)
    - [3.4 更新後台使用者](#markdown-更新後台使用者)



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
| 200              | 2001 | 帳號或密碼錯誤                                    |             |

<br/>

### 1.2. Http_Status_Code:400
<a id="markdown-httpstatuscode400" name="httpstatuscode400"></a>

| Http status code | Code | Message  | Description |
| ---------------- | ---- | -------- | ----------- |
| 400              | 9999 | 系統異常 |             |

<br/>

### 1.3. Http_Status_Code:401
<a id="markdown-httpstatuscode401" name="httpstatuscode401"></a>

| Http status code | Code | Message                         | Description                                             |
| ---------------- | ---- | ------------------------------- | ------------------------------------------------------  |
| 401              | 4001 | 不合法授權/授權已逾期，請重新登入 | Token不合法或以逾期，請Refresh Token，如在不合法重新登入  |

<br/>

# 2. 共同規範
<a id="markdown-2.共同規範" name="共同規範"></a>

## 2.1 通用 Header
<a id="markdown-2.1Header" name="Header"></a>
    呼叫API時 Header 需帶參數
<br>

+ Header

    | Parameter    | Type   | Mandatory | Description                                     |
    | ------------ | ------ | --------- | ----------------------------------------------- |
    | platform     | string | Y         | GroupBuying(圈團消費者端)<br>GroupInitiator(團爸團媽端)<br>Shop(EC)<br>Backend(後台)|
    | authentication | string | Y       | 使用者token |


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