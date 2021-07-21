# Overview
<a id="markdown-overview" name="overview"></a>

<!-- TOC -->

- [Overview](#overview)
- [History](#history)
- [1. 系統代碼定義](#markdown-1.系統代碼定義)
    - [1.1. Http_Status_Code:200](#markdown-httpstatuscode200)
    - [1.2. Http_Status_Code:401](#markdown-httpstatuscode400)
    - [1.3. Http_Status_Code:400](#markdown-httpstatuscode401)
- [2. 後台登入](#markdown-2.後台登入)
    - [2.1. ]
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

## 2. 後台登入
<a id="markdown-登入後台" name="登入後台"></a>

### 2.1 帳號密碼驗證
<a id="markdown-確認使用者狀態" name="確認使用者狀態"></a>
    驗證後台使用者帳號密碼
 <br/>

#### 2.1.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 2.1.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /pxec/backend/login ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```

+ Header

    | Parameter    | Type   | Mandatory | Description                                     |
    | ------------ | ------ | --------- | ----------------------------------------------- |
    | platform     | string | Y         | GroupBuying(圈團消費者端)<br>GroupInitiator(團爸團媽端)<br>Shop(EC)<br>Backend(後台)|

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

#### 2.1.3 Response
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

### 2.2 新增帳號
<a id="markdown-新增帳號" name="新增帳號"></a>
    新增後台使用者
 <br/>

#### 2.2.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 2.2.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /pxec/backend/login ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```

+ Header

    | Parameter    | Type   | Mandatory | Description                                     |
    | ------------ | ------ | --------- | ----------------------------------------------- |
    | platform       | string | Y       | GroupBuying(圈團消費者端)<br>GroupInitiator(團爸團媽端)<br>Shop(EC)<br>Backend(後台)|
    | authentication | string | Y       | 使用者token |

+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | employee_id  | string | Y         | 新增帳號                      |
    | name         | string | Y         | 密碼                         |
    | email        | string | Y         | Email (登入帳號)              |
    | first_login_password  | string    | Y | 首次登入密碼             |
    | role_id      | int[]  | Y         | 角色ID                       |


+ Example
    ```json
    {
        "employee_id": "2999999",
        "name" : "福利雄",
        "email" : "fulibear@pxmart.com.tw",
        "first_login_password" : "df",
        "role_id" : [1,2,3]
    }
    ```

#### 2.2.3 Response
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

### 2.3 帳號(使用者)列表
<a id="markdown-新增帳號" name="新增帳號"></a>
    後台帳號管理使用者列表
 <br/>

#### 2.3.1 Diagram
<a id="markdown-diagram" name="diagram"></a>

#### 2.3.2 Request
<a id="markdown-request" name="request"></a>

+ Url Format : ``` /pxec/backend/login ```
+ Http Method: [ **Post** ]
+ Content Type : ``` application/json ```

+ Header

    | Parameter    | Type   | Mandatory | Description                                     |
    | ------------ | ------ | --------- | ----------------------------------------------- |
    | platform       | string | Y       | GroupBuying(圈團消費者端)<br>GroupInitiator(團爸團媽端)<br>Shop(EC)<br>Backend(後台)|
    | authentication | string | Y       | 使用者token |

+ Parameters

    | Parameter    | Type   | Mandatory | Description                  |
    | ------------ | ------ | --------- | ---------------------------- |
    | employee_id  | string | Y         | 新增帳號                      |
    | name         | string | Y         | 密碼                         |
    | email        | string | Y         | Email (登入帳號)              |
    | first_login_password  | string    | Y | 首次登入密碼             |
    | role_id      | int[]  | Y         | 角色ID                       |


+ Example
    ```json
    {
        "employee_id": "2999999",
        "name" : "福利雄",
        "email" : "fulibear@pxmart.com.tw",
        "first_login_password" : "df",
        "role_id" : [1,2,3]
    }
    ```

#### 2.3.3 Response
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