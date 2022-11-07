# typescript type vs interface

ref: https://ithelp.ithome.com.tw/articles/10216626

## summary

---

因為語意和一些限制，在特定情況下有不同的選擇，因此不限制

cases using type:

- 不擴充, 表達單一資料型態
- 原始型別, 列舉型別,元組型別
- support Intersection &, union |

cases using interface:

- 功能是可以被重複再利用，可能會被多方程式碼或第三方套件依賴
- 物件
- extend
