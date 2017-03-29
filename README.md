# 詞彙

### Contract Layer：合約層，放置接口（Interface）、資料模型的地方。

| Vocabulary    |                                                                        |
|---------------|------------------------------------------------------------------------|
| Model         | 命名空間（namespace），放置資料模型的地方。                            |
| Data          | 命名空間（namespace），定義在 Model 底下，放置商業資料模型的地方。     |
| Infos         | 命名空間（namespace），放置 Logic Layer 所定義出來的方法的參數模型。   |
| Results       | 命名空間（namespace），放置 Logic Layer 所定義出來的方法的回傳值模型。 |
| ServiceResult | 抽象類別，所有的回傳值模型必須是它的派生類。                           |
| Aspects       | 命名空間（namespace），放置 AOP Advice 的地方。                        |

### Logic Layer：邏輯層，放置商業邏輯的地方。

- 將取得的資料加以判斷、整理、運算、轉換。
- 以提供服務的主體為服務命名，例如：`MemberService` 服務的主體為 Member，其中取得 Member 所參與的 Club 的方法 - `GetWithClubsBy(string memberId)`，乍看之下這個方法應該由 `ClubService` 服務提供，但是 Member 所參與的 Club 只是附屬於 Member 的屬性，Member 才是服務的主體，因此應該由 MemberService 提供該方法。
- 以提供服務的意圖為方法命名，承上例子，`GetWithClubsBy(string memberId)` 展現主體服務行為的意圖。
- 命名要思考贅詞，承上例子，`MemberService` 的 `GetWithClubsBy(string memberId)` 方法，使用起來就會長這樣 `memberService.GetWithClubsBy 123456`，如果多了一些贅詞看起來就會像這樣 `memberService.GetMemberWithClubsByMemberId 123456`。
- Logic Service 經常以使用情境來設計，不優先以 Reusable 來思考設計方式。

| Vocabulary    |                                                                                             |
|---------------|---------------------------------------------------------------------------------------------|
| -Service      | 邏輯層每個類別的結尾必定冠上 Service。                                                      |
| LookupService | 查找服務，取得系統設定、靜態選項。                                                          |
| Create        | 新增行為                                                                                    |
| Get           | 取得行為                                                                                    |
| GetBy-        | 取得行為，By 後面接的名稱（或方法內參數名稱）描述主要的過濾條件，描述的過濾條件必須是名詞。 |
| Save          | 儲存檔案及更新資料表行為                                                                    |
| Remove        | 移除行為                                                                                    |

### Physical Layer：實體層，又稱資料存取層，放置存取資料方法的地方。

- 依照實際存放資料的實體來為 DAO 及方法命名，例如：`MemberService` 提供了一個 `GetUnreadInsideMailCount(string memberId)` 方法，實際存放資料的實體是 `InsideMail` 這個資料倉儲，因此其 DAO 應命名為 `InsideMailDataAccess` 與 MemberService 形成聚合關係。
- 禁止做資料的整理、判斷、運算、轉換，單純做接受參數、取資料、回傳資料的工作。
- DAO 元件經常會被多個不同的 Logic Service 使用，設計上要偏向 Reusable。

| Vocabulary                      |                                                                                                       |
|---------------------------------|-------------------------------------------------------------------------------------------------------|
| Create                          | 建立檔案                                                                                              |
| Insert                          | 執行單一插入語句                                                                                      |
| BatchInsert<br />MultiplyInsert | 多個插入語句一同執行                                                                                  |
| BulkInsert                      | 執行批次插入語句，與 MultiplyInsert 不同的是 BulkInsert 使用資料庫批次寫入的語法。                    |
| Update                          | 執行單一更新語句，依照 PK 更新一筆資料。                                                              |
| Delete                          | 執行單一刪除語句，依照 PK 刪除一筆資料。                                                              |
| DeleteBy-                       | 過濾條件執刪除語句，By 後面接的名稱（或方法內參數名稱）描述主要的過濾條件，描述的過濾條件必須是名詞。 |
| QueryByKey                      | 取得一筆資料，而且是唯一一筆，參數就是主鍵。                                                          |
| QueryBy-                        | 執行查詢語句，By 後面接的名稱（或方法內參數名稱）描述主要的過濾條件，描述的過濾條件必須是名詞。       |
| -Config                         | 實際到資料來源取得系統設定、靜態選項的類別，通常實作 Singleton 模式，並 Cache 所取得的資料。          |

### Cross-cutting：分層的橫切面，被安插至各個物件的處理流程之中。

| Vocabulary           |                                      |
|----------------------|--------------------------------------|
| -Interceptor         | 欄截器                               |
| ExceptionInterceptor | 例外狀況攔截器                       |
| -Aspect              | 側面、局面。                         |
| LogAspect            | 日誌方面，在方法執行的前後加入日誌。 |

### ASP.NET MVC Action Naming Convention

| Vocabulary   | HTTP Verb |                                                  |
|--------------|-----------|--------------------------------------------------|
| Index        | GET       | 資源的起始頁面                                   |
| List         | GET       | 依條件列出資料，若無條件則表示全部。             |
| Details      | GET       | 取得單筆資料                                     |
| New          | GET       | 新增資料用的頁面                                 |
| Create       | POST      | 新增資料用的方法                                 |
| Edit         | GET       | 編輯資料用的頁面                                 |
| Save         | PATCH     | 更新資料用的方法                                 |
| Destroy      | GET       | 刪除資料用的頁面                                 |
| Remove       | DELETE    | 移除單筆資料，若要依條件刪除資料就實作多載方法。 |
| Login        | GET       | 登入用的頁面                                     |
| Authenticate | POST      | 驗證使用者所送出的登入資訊                       |
| Logout       | DELETE    | 登出                                             |
