# 詞彙

### Contract Layer：合約層，放置接口（Interface）、資料模型的地方。

|Vocabulary|   |
|:--|:--|
|Model|命名空間（namespace），放置資料模型的地方。|
|Data|命名空間（namespace），定義在 Model 底下，放置商業資料模型的地方。|
|Infos|命名空間（namespace），放置 Logic Layer 所定義出來的方法的參數模型。|
|Results|命名空間（namespace），放置 Logic Layer 所定義出來的方法的回傳值模型。|
|ServiceResult|抽象類別，所有的回傳值模型必須是它的派生類。|
|Aspects|命名空間（namespace），放置 AOP Advice 的地方。|

### Logic Layer：邏輯層，放置商業邏輯的地方。

|Vocabulary||
|:--|:--|
|-Service|邏輯層每個類別的結尾必定冠上 Service。|
|LookupService|查找服務，取得系統設定、靜態選項。|
|Add|新增行為|
|Get|取得行為|
|Save|儲存行為|
|Remove|移除行為|

### Physical Layer：實體層，又稱資料存取層，放置存取資料方法的地方。

- Create：執行單一插入語句。
- Insert：執行單一插入語句，與 Create 相同。
- MultiplyInsert：多個插入語句一同執行。
- BulkInsert：執行批次插入語句，與 MultiplyInsert 不同的是 BulkInsert 使用資料庫批次寫入的語法。
- Update：執行單一更新語句，依照 PK 更新一筆資料。
- Delete：執行單一刪除語句，依照 PK 刪除一筆資料。
- DeleteBy-：過濾條件執刪除語句，By 後面接著描述主要的過濾條件，描述的過濾條件必須是名詞。
- QueryBy-：執行查詢語句，By 後面接著描述主要的過濾條件，描述的過濾條件必須是名詞。
- -Config：實際到資料來源取得系統設定、靜態選項的類別，通常實作 Singleton 模式，並 Cache 所取得的資料。

### Cross-cutting：分層的橫切面，被安插至各個物件的處理流程之中。

- -Interceptor：欄截器
- ExceptionInterceptor：例外狀況攔截器
- -Aspect：側面、局面。
- LogAspect：日誌方面，在方法執行的前後加入日誌。
