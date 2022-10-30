# note

## Value for rework recipe formula/alarm rules maintain product

---

as-is:

- 使用 public visual tool, no access/control
- manually import/edit data by IT according to PE/EE's request
- manually validate formula variable value

to-be:

- login through cloud
  - PE/EE can send edit data by themselves
  - reduce communication cost
- add approvement process
  - Manager can check all requests on the system
- auto validate all change
  - no typo issue

## question for po

---

- original system

  - 原本有 manager 是如何驗證 change
  - 平時處理一個要求所花費的時間

- system design

  - alarm rule/rework recipe formula relation?
  - alarm rule 包含在 rework recipe system?
  - request 是否包含多個 changes
  - 是否還要支援 import csv?

- database design

  - 為什麼 map table 幾乎都使用 compost PK, 有什麼有優缺點
    - pros
      - you don't need a special algorithm to make it unique,
  - production database 使用量
  - 目前參與的 table 所有 column 都需要驗證，所以會需要相關的規則和所有參考的 database access
  - 現行 sit 測試環境沒有都可以直接修關改, production 環境也是如此嗎

- gui behavior/feature
  - 是否支援多個 update/delete?
  - 可以限定 update 的 column?
  -

## restful api design

---

questions:

- replace post with get to search data when query parameters are too many
  - don't do it(?)
- multiple parameters in query parameters
  - LHS Brackets
  - RHS Colon
  - Pagination
- how to design multiple update

  ex:

  [
  {
  methods: "update"|"archive"|"delete,
  object: {},
  body: {}

      }

  }
  ]

- how to design multiple delete(無 body) -> 包含在 put 之中
- how to design multiple update

note

- api route naming 不要用 abbreviation
- schema naming: Pascal case and camel case
- system response 500 不需要
- system response 204 body 完全空的連 status meesge 都沒有
- response body 是要包含物件 naming
  ex{
  status
  }
- add validate api
- schema 可能會需要 user and user role table -> 這樣才能分辨 PE/EE 的 manager 還有 PE/EE 之間的關係

## sql note

- composite primary key
  - pros
  - cons
  - index
