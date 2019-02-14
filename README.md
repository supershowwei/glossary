# 詞彙

### Contract Layer：合約層，放置接口（Interface）、資料模型的地方。

| Vocabulary    |                                                                        |
|---------------|------------------------------------------------------------------------|
| Model         | 命名空間（namespace），放置資料模型的地方。                            |
| Data          | 命名空間（namespace），定義在 Model 底下，放置商業資料模型的地方。     |
| Meshes        | 命名空間（namespace），放置動態條件查詢用的模型。                      |
| FieldSets     | 命名空間（namespace），放置動態欄位更新用的模型。                      |
| Results       | 命名空間（namespace），放置 Logic Layer 所定義出來的方法的回傳值模型。 |
| ServiceResult | 抽象類別，所有的回傳值模型必須是它的派生類。                           |
| Aspects       | 命名空間（namespace），放置 AOP Advice 的地方。                        |

### Logic Layer：邏輯層，放置商業邏輯的地方。

- 要重構成一個 Logic Service 有幾個條件：資源需具有集合的特性、已設計或實作 CRU（U包含D）的行為。
- 將取得的資料加以判斷、整理、運算、轉換。
- 以提供服務的主體為服務命名，例如：`MemberService` 服務的主體為 Member，其中取得 Member 所參與的 Club 的方法 - `GetWithClubsBy(string memberId)`，乍看之下這個方法應該由 `ClubService` 服務提供，但是 Member 所參與的 Club 只是附屬於 Member 的屬性，Member 才是服務的主體，因此應該由 MemberService 提供該方法。
- 以提供服務的意圖為方法命名，承上例子，`GetWithClubsBy(string memberId)` 展現主體服務行為的意圖。
- 命名要思考贅詞，承上例子，`MemberService` 的 `GetWithClubsBy(string memberId)` 方法，使用起來就會長這樣 `memberService.GetWithClubsBy 123456`，如果多了一些贅詞看起來就會像這樣 `memberService.GetMemberWithClubsByMemberId 123456`。
- Logic Service 經常以使用情境來設計，不優先以 Reusable 來思考設計方式。

| Vocabulary      |                                          |
|-----------------|------------------------------------------|
| -Service        | 邏輯層每個類別的結尾必定冠上 Service。   |
| LookupService   | 查找服務，取得系統設定、靜態選項。       |
| Create<br />Add | 新增行為                                 |
| List (By)       | 清單列表，僅回傳部分識別性資料。         |
| Get (By)        | 取得一筆或多筆資料。                     |
| GetPart (By)    | 取得一筆或多筆資料，包含指定的部分欄位。 |
| GetXWithY (By)  | 取得完整的 X 合併一部分的 Y，回傳 X。    |
| GetXOfY (By)    | 取得完整的 X 合併一部分的 Y，回傳 Y。    |
| Save            | 儲存檔案及更新資料表行為                 |
| Remove          | 移除行為                                 |

**By 後面接的名稱（或方法內參數名稱）描述主要的過濾條件，描述的過濾條件必須是名詞。*

### Physical Layer：實體層，又稱資料存取層，放置存取資料方法的地方。

- 依照實際存放資料的實體來為 DAO 及方法命名，例如：`MemberService` 提供了一個 `GetUnreadInsideMailCount(string memberId)` 方法，實際存放資料的實體是 `InsideMail` 這個資料倉儲，因此其 DAO 應命名為 `InsideMailDataAccess` 與 MemberService 形成聚合關係。
- 禁止做資料的整理、判斷、運算、轉換，單純做接受參數、取資料、回傳資料的工作。
- DAO 元件經常會被多個不同的 Logic Service 使用，設計上要偏向 Reusable。
- 依鍵值撈取單一筆資料時，設計上傾向預設將連同有一對一關聯的資料一起撈出來。

| Vocabulary                      |                                                                                              |
|---------------------------------|----------------------------------------------------------------------------------------------|
| Create                          | 建立檔案                                                                                     |
| Insert                          | 執行單一插入語句                                                                             |
| BatchInsert<br />MultiplyInsert | 多個插入語句一同執行                                                                         |
| BulkInsert                      | 執行批次插入語句，與 MultiplyInsert 不同的是 BulkInsert 使用資料庫批次寫入的語法。           |
| CountBy                         | 依條件取得資料數量                                                                           |
| ExistsBy                        | 是否存在                                                                                     |
| ExistsIn...By                   | 是否於某個條件下存在                                                                         |
| ExtractBy                       | 取得一筆資料（僅限 SELECT 的欄位是從單一資料表來的）。                                       |
| ExtractPartBy                   | 取得一筆資料，包含指定的部分欄位。                                                           |
| BunchBy                         | 取得一筆資料，連同相關的資料整串取回。                                                       |
| ExtractXWithYBy                 | 取得完整的 X 合併一部分的 Y，回傳 X。                                                        |
| ExtractXOfYBy                   | 取得完整的 X 合併一部分的 Y，回傳 Y。                                                        |
| ItemizeBy                       | 取得清單列表的項目。                                                                         |
| QueryBy                         | 取得一或多筆資料。                                                                           |
| QueryPartBy                     | 取得一或多筆資料，包含指定的部分欄位。                                                       |
| Update                          | 執行單一更新語句，依照 PK 更新一筆資料。                                                     |
| UpdateBy                        | 執行條件更新語句。                                                                           |
| BatchUpdate<br />MultiplyUpdate | 多個更新語句一同執行                                                                         |
| Delete                          | 執行單一刪除語句，依照 PK 刪除一筆資料。                                                     |
| DeleteBy                        | 執行條件刪除語句。                                                                           |
| -Config                         | 實際到資料來源取得系統設定、靜態選項的類別，通常實作 Singleton 模式，並 Cache 所取得的資料。 |

**By 後面接的名稱（或方法內參數名稱）描述主要的過濾條件，描述的過濾條件必須是名詞。*

### Cross-cutting：分層的橫切面，被安插至各個物件的處理流程之中。

| Vocabulary           |                                      |
|----------------------|--------------------------------------|
| -Interceptor         | 欄截器                               |
| ExceptionInterceptor | 例外狀況攔截器                       |
| -Aspect              | 側面、局面。                         |
| LogAspect            | 日誌方面，在方法執行的前後加入日誌。 |

### ASP.NET MVC Action Naming Convention

| Vocabulary         | HTTP Verb |                                                  |
|--------------------|-----------|--------------------------------------------------|
| Index              | GET       | 資源的起始頁面                                   |
| List               | GET       | 依條件列出資料，若無條件則表示全部。             |
| Details<br /> Show | GET       | 取得單筆資料                                     |
| New                | GET       | 新增資料用的頁面                                 |
| Create             | POST      | 新增資料用的方法                                 |
| Edit               | GET       | 編輯資料用的頁面                                 |
| Save               | PATCH     | 更新資料用的方法                                 |
| Destroy            | GET       | 刪除資料用的頁面                                 |
| Remove             | DELETE    | 移除單筆資料，若要依條件刪除資料就實作多載方法。 |
| Login              | GET       | 登入用的頁面                                     |
| Authenticate       | POST      | 驗證使用者所送出的登入資訊                       |
| Logout             | DELETE    | 登出                                             |

### REST 風格

| HTTP Verb | Safe? | Idempotent? |
|:---------:|:-----:|:-----------:|
|    GET    |   Y   |      Y      |
|    POST   |   N   |      N      |
|    PUT    |   N   |      Y      |
|   PATCH   |   N   |   N (or Y)  |
|   DELETE  |   N   |      Y      |

**Idempotent: 如果相同的操作再執行第二遍第三遍，結果還是跟第一遍的結果一樣（也就是說不管執行幾次，結果都跟只有執行一次一樣）*

- **GET**

> - 名詞資源視為受詞，命名優先使用複數型態，無論使用單數或複數型態，要一致。<br />EX: :o:/members/123456, :x:/member/123456
> - 動名詞資源也是一種名詞。
> - 不可數名詞無單複數型態，不要假會，保持它不可數的樣子就好。<br />EX: :x:/feedbacks, :x:/feedbacklist, :o:/feedback

- **PATCH**

> - 關於 PATCH 的 Non-Idempotent 特性，其實 PATCH 可以是 Idempotent，在這篇 [RFC5789](https://tools.ietf.org/html/rfc5789#section-2) 有說明。

- 更新資料，用 PATCH 取代 PUT。[文章一](http://www.ruanyifeng.com/blog/2011/09/restful.html)、[文章二](https://ihower.tw/blog/archives/6483)

- **參考文章**
> - [HTTP Verbs: 談 POST, PUT 和 PATCH 的應用](https://ihower.tw/blog/archives/6483)
> - [常見的HTTP METHOD的不同性質分析：GET,POST和其他4種METHOD的差別](https://data-sci.info/2015/10/24/%E5%B8%B8%E8%A6%8B%E7%9A%84http-method%E7%9A%84%E4%B8%8D%E5%90%8C%E6%80%A7%E8%B3%AA%E5%88%86%E6%9E%90%EF%BC%9Agetpost%E5%92%8C%E5%85%B6%E4%BB%964%E7%A8%AEmethod%E7%9A%84%E5%B7%AE%E5%88%A5/)

### 集合類型的命名

以名詞複數呈現或以 `-List`、`-Collection` 結尾，底下是遇到的一些特殊案例：

- xxxIds、xxxIDs：表達一個識別值的集合，建議改用 xxxIdentifiers 命名。

### 可以是動名詞的動詞

[`Edit`](https://www.thefreedictionary.com/edit)、[`Finish`](https://www.thefreedictionary.com/finish)、[`Follow`](https://www.thefreedictionary.com/follow)、[`Listen`](https://www.thefreedictionary.com/listen)、[`Reply`](https://www.thefreedictionary.com/reply)、[`Save`](https://www.thefreedictionary.com/save)、[`Set`](https://www.thefreedictionary.com/set)、[`Update`](https://www.thefreedictionary.com/update)、[`Upload`](https://www.thefreedictionary.com/upload)、[`Vote`](https://www.thefreedictionary.com/vote)

### 詞性的英文

| Vocabulary   |        |
|--------------|--------|
| noun         | 名詞   |
| verb         | 動詞   |
| plural       | 複數   |
| synonym      | 同義詞 |
| antonym      | 反義詞 |
| abbreviation | 縮寫   |

### 易混淆的詞彙

| Vocabulary     |                                                                |
|----------------|----------------------------------------------------------------|
| Verification   | 查證；作證。（側重證明）                                       |
| Validation     | 批准；驗證。（側重批准）                                       |
| Authentication | 系統判斷使用者是否真的是他宣稱的那個人。（檢查 Id & Password） |
| Authorization  | 系統根據該帳號所擁有的權限驗証該使用者。（檢查使用權限）       |

| Vocabulary         |                                                                                                                                                                          |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dis-               | 與某事物分開或是遠離、分離。                                                                                                                                             |
| un-                | 是「不」或是與某事物相反，或是沒有某事物、缺乏某事物、一種遭到剝奪的感覺。                                                                                               |
| non-               | 基本上就是「not」                                                                                                                                                        |
| in-、im-、il-、ir- | 顛倒、相反。<br /> in-、im，不只代表「不」的意思，還有「朝向」或「導致」的意思。<br /> <br /> im-：接 b、m、p 開頭的字<br /> il-：接 l 開頭的字<br /> ir-：接 r 開頭的字 |
| mis-               | 是「錯誤地...」                                                                                                                                                          |

| Vocabulary |                                                                                                                                                  |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Percent    | 百分之(某數字)，一般都是有數字在前面。                                                                                                           |
| Percentage | 百分率，通常用在表達某個什麼的百分率，不會是一個數字。                                                                                           |
| Rate       | 比率、比值，像是稅率、犯罪率、...，通常可以會有一個數字。                                                                                        |
| Ratio      | 兩個量的比值，其實是強調相比、相除的關係，但是它的兩個量必須在上下文出現。<br />E.g. 學生及老師比是 35:1。                                       |
| Proportion | 指比例關係，可代替 Ratio 使用，但是在相比的兩個量在上下文沒有出現是必須用 Proportion 而不能用 Ratio。<br />E.g. 只有小比例的人口感染 H7N9 病毒。 |

| Vocabulary |                                                                                                 |
|------------|-------------------------------------------------------------------------------------------------|
| Divide     | 將一個東西分成兩個或更多<br />E.g. 我將披薩分成好幾塊。                                         |
| Separate   | 分離多個物體並讓它們遠離彼此。<br />E.g. 我將兩個孩子分開。                                     |
| Split      | 相對上面兩個，是比較通用的用法。<br />E.g. 「我將木頭分成兩半。」、「我分開正在打架的兩個人。」 |
