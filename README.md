# 詞彙

## Contract Layer：合約層，放置接口（Interface）、資料模型的地方。

| Vocabulary |                                                                                         |
|------------|-----------------------------------------------------------------------------------------|
| Model      | 命名空間（namespace），放置資料模型的地方。                                             |
| Data       | 命名空間（namespace），定義在 Model 底下，放置商業資料模型的地方。                      |
| Quests     | 命名空間（namespace），定義在 Model 底下，放置動態條件查詢用的模型。                    |
| FieldSets  | 命名空間（namespace），定義在 Model 底下，放置動態欄位更新用的模型。                    |
| Results    | 命名空間（namespace），定義在 Model 底下，放置 Logic Layer 所定義出來的方法回傳值模型。 |
| Aspects    | 命名空間（namespace），放置 AOP Advice 的地方。                                         |

## Logic Layer：邏輯層，放置商業邏輯的地方。

- 以領域為邊界構成一個 Logic Service
- Logic Service 經常以使用情境來設計，不以 Reusable 優先來思考設計方式。

| Vocabulary      |                                        |
|-----------------|----------------------------------------|
| -Service        | 邏輯層每個類別的結尾必定冠上 Service。 |
| LookupService   | 查找服務，取得系統設定、靜態選項。     |
| Create<br />Add | 新增行為                               |
| List            | 清單列表，僅回傳部分識別性資料。       |
| Get             | 取得一筆或多筆資料。                   |
| Save            | 儲存檔案及更新資料表行為               |
| Remove          | 移除行為                               |

## Physical Layer：實體層，又稱資料存取層，放置存取資料方法的地方。

- 用 IDataAccess<T> 來為每個資料表實作 Generic Data Access Object，僅處理單一資料表的 CRUD。
- Logic Service 要做 Transaction、JOIN 資料表、...等較複雜的 Query，必須搭配 XXXRepository 來處理，不與 Data Access Object 混在一起。
- Repository 可以寫純 SQL 或是注入 Data Access Object 來使用
- 禁止做商業邏輯的判斷、運算、轉換。

| Vocabulary  |                                                                                              |
|-------------|----------------------------------------------------------------------------------------------|
| Create      | 建立**檔案**                                                                                 |
| Insert      | 執行單一插入語句                                                                             |
| BatchInsert | 多個插入語句一同執行                                                                         |
| BulkInsert  | 執行批次插入語句，與 BatchInsert 不同的是 BulkInsert 使用資料庫批次寫入的語法。              |
| Count       | 依條件取得資料數量                                                                           |
| Exists      | 是否存在                                                                                     |
|             | 是否於某個條件下存在，為 Exists 的多載。                                                     |
| QueryOne    | 取得一筆資料（僅限 SELECT 的欄位是從單一資料表來的）。                                       |
|             | 取得一筆資料，包含指定的部分欄位，為 QueryOne 的多載。                                       |
| BundleQuery | 取得一筆資料，連同取回相關的資料。                                                           |
| Query       | 取得一或多筆資料。                                                                           |
|             | 取得一或多筆資料，包含指定的部分欄位，為 Query 的多載。                                      |
| Itemize     | 取得清單列表的項目。                                                                         |
| Update      | 執行單一更新語句，依照 PK 更新一筆資料。                                                     |
|             | 執行條件更新語句，為 Update 的多載。                                                         |
| BatchUpdate | 多個更新語句一同執行                                                                         |
| BulkUpdate  | 執行批次插入語句，與 BatchUpdate 不同的是 BulkUpdate 使用資料庫批次更新的語法。              |
| Delete      | 執行單一刪除語句，依照 PK 刪除一筆資料。                                                     |
|             | 執行條件刪除語句，為 Delete 的多載。                                                         |
| -Config     | 實際到資料來源取得系統設定、靜態選項的類別，通常實作 Singleton 模式，並 Cache 所取得的資料。 |

## Cross-cutting：分層的橫切面，被安插至各個物件的處理流程之中。

| Vocabulary           |                                      |
|----------------------|--------------------------------------|
| -Interceptor         | 欄截器                               |
| ExceptionInterceptor | 例外狀況攔截器                       |
| -Aspect              | 側面、局面。                         |
| LogAspect            | 日誌方面，在方法執行的前後加入日誌。 |

## ASP.NET MVC Action Naming Convention

| Vocabulary        | HTTP Verb |                                                  |
|-------------------|-----------|--------------------------------------------------|
| Index             | GET       | 資源的起始頁面                                   |
| List              | GET       | 依條件列出資料，若無條件則表示全部。             |
| Details           | GET       | 取得單筆資料                                     |
| New               | GET       | 新增資料用的頁面                                 |
| Create            | POST      | 新增資料用的方法                                 |
| Edit              | GET       | 編輯資料用的頁面                                 |
| Save              | PATCH     | 更新資料用的方法                                 |
| Destroy           | GET       | 刪除資料用的頁面                                 |
| Remove            | DELETE    | 移除單筆資料，若要依條件刪除資料就實作多載方法。 |
| Login             | GET       | 登入用的頁面                                     |
| Authenticate      | POST      | 驗證使用者所送出的登入資訊                       |
| Logout            | DELETE    | 登出                                             |

## REST 風格

| HTTP Verb | Safe? | Idempotent? |
|:---------:|:-----:|:-----------:|
|    GET    |   Y   |      Y      |
|    POST   |   N   |      N      |
|    PUT    |   N   |      Y      |
|   PATCH   |   N   |   N (or Y)  |
|   DELETE  |   N   |      Y      |

**Idempotent: 如果相同的操作再執行第二遍第三遍，結果還是跟第一遍的結果一樣（也就是說不管執行幾次，結果都跟只有執行一次一樣）*

- **GET**

> - 名詞資源視為受詞，命名優先使用複數型態，無論使用單數或複數型態，要一致。<br />EX: /members/123456, /member/123456
> - 動名詞資源也是一種名詞。
> - 不可數名詞無單複數型態，若要表達複數型態，則補充描述。<br />EX: :x:/feedbacks, :o:/feedback-list

- **PATCH**

> - 關於 PATCH 的 Non-Idempotent 特性，其實 PATCH 可以是 Idempotent，在這篇 [RFC5789](https://tools.ietf.org/html/rfc5789#section-2) 有說明。

- 更新資料，用 PATCH 取代 PUT。[文章一](http://www.ruanyifeng.com/blog/2011/09/restful.html)、[文章二](https://ihower.tw/blog/archives/6483)

- **參考文章**
> - [HTTP Verbs: 談 POST, PUT 和 PATCH 的應用](https://ihower.tw/blog/archives/6483)
> - [常見的HTTP METHOD的不同性質分析：GET,POST和其他4種METHOD的差別](https://data-sci.info/2015/10/24/%E5%B8%B8%E8%A6%8B%E7%9A%84http-method%E7%9A%84%E4%B8%8D%E5%90%8C%E6%80%A7%E8%B3%AA%E5%88%86%E6%9E%90%EF%BC%9Agetpost%E5%92%8C%E5%85%B6%E4%BB%964%E7%A8%AEmethod%E7%9A%84%E5%B7%AE%E5%88%A5/)

## 集合類型的命名

以名詞複數呈現或以 `-List`、`-Collection` 結尾，底下是遇到的一些特殊案例：

- xxxIds、xxxIDs：表達一個識別值的集合，建議改用 xxxIdentifiers 命名。

## 可以是動名詞的動詞

[`Add`](https://en.wiktionary.org/wiki/add#Noun)、[`Buy`](https://www.thefreedictionary.com/buy)、[`Capture`](https://www.thefreedictionary.com/capture)、[`Decrease`](https://www.thefreedictionary.com/decrease)、[`Edit`](https://www.thefreedictionary.com/edit)、[`Find`](https://www.thefreedictionary.com/find)、[`Finish`](https://www.thefreedictionary.com/finish)、[`Follow`](https://www.thefreedictionary.com/follow)、[`Increase`](https://www.thefreedictionary.com/increase)、[`Insert`](https://www.thefreedictionary.com/insert)、[`Listen`](https://www.thefreedictionary.com/listen)、[`Purchase`](https://www.thefreedictionary.com/purchase)、[`Pull`](https://www.thefreedictionary.com/pull)、[`Quote`](https://www.thefreedictionary.com/quote)、[`Reply`](https://www.thefreedictionary.com/reply)、[`Save`](https://www.thefreedictionary.com/save)、[`Sell`](https://www.thefreedictionary.com/sell)、[`Set`](https://www.thefreedictionary.com/set)、[`Start`](https://www.thefreedictionary.com/start)、[`Update`](https://www.thefreedictionary.com/update)、[`Upload`](https://www.thefreedictionary.com/upload)、[`Vote`](https://www.thefreedictionary.com/vote)

## 詞性的英文

| Vocabulary   |        |
|--------------|--------|
| noun         | 名詞   |
| verb         | 動詞   |
| plural       | 複數   |
| synonym      | 同義詞 |
| antonym      | 反義詞 |
| abbreviation | 縮寫   |

## 序數

| Vocabulary                                                                                                                                                                                 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Primary, Secondary, Tertiary, Quaternary, Quinary, Senary, Septenary, Octonary, Nonary, Denary.                                                                                            |
| First, Second, Third, Fourth, Fifth, Sixth, Seventh, Eighth, Ninth, Tenth, Eleventh, Twelfth, Thirteenth, Fourteenth, Fifteenth, Sixteenth, Seventeenth, Eighteenth, Nineteenth, Twentieth. |

## 易混淆的詞彙

| Vocabulary     |                                                                |
|----------------|----------------------------------------------------------------|
| Verification   | 查證；作證。（側重證明）                                       |
| Validation     | 批准；驗證。（側重批准）                                       |
| Authentication | 系統判斷使用者是否真的是他宣稱的那個人。（檢查 Id & Password） |
| Authorization  | 系統根據該帳號所擁有的權限驗証該使用者。（檢查使用權限）       |
| Certification  | 發證；檢定。                                                   |

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
| Rate       | 比率、比值，像是稅率、犯罪率、...，通常會有一個數字。                                                                                        |
| Ratio      | 兩個量的比值，其實是強調相比、相除的關係，但是它的兩個量必須在上下文出現。<br />E.g. 學生及老師比是 35:1。                                       |
| Proportion | 指比例關係，可代替 Ratio 使用，但是在相比的兩個量在上下文沒有出現是必須用 Proportion 而不能用 Ratio。<br />E.g. 只有小比例的人口感染 H7N9 病毒。 |

| Vocabulary |                                                                                                 |
|------------|-------------------------------------------------------------------------------------------------|
| Divide     | 將一個東西分成兩個或更多<br />E.g. 我將披薩分成好幾塊。                                         |
| Separate   | 分離多個物體並讓它們遠離彼此。<br />E.g. 我將兩個孩子分開。                                     |
| Split      | 相對上面兩個，是比較通用的用法。<br />E.g. 「我將木頭分成兩半。」、「我分開正在打架的兩個人。」 |

| Vocabulary |                                                                                                                                        |
|------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Failure    | 失敗是一種系統內部的意外事件，是意料之外的，會阻止系統繼續正常地運行。                                                                 |
| Error      | 錯誤是意料之中的，並且針對錯誤的情況進行了處理，將會作為程序的正常處理過程。（例如：在輸入驗證的過程中發現了錯誤，並且回應給客戶端。） |

| Vocabulary    |                                                                     |
|---------------|---------------------------------------------------------------------|
| Configure     | 動詞，改變可選項的動作。                                            |
| Settings      | 可選項的集合                                                        |
| Options       | 可選項                                                              |
|               | 一句話解釋以上三者的關係「Configure some options in the settings.」 |
| Configuration | 已確定 Configure 的結果                                             |
| Config        | Configure 或 Configuration 的縮寫                                   |
| Profile       | 指一系列對特定個體的不一定全面的描述，翻成輪廓更准確。              |
| Preferences   | 偏好、喜好。                                                        |

| Vocabulary    |                                                                                        |
|---------------|----------------------------------------------------------------------------------------|
| Options       | We have set some things already, and give you the option to change them.               |
| Settings      | View or modify the list of things that can be set                                      |
| Properties    | Change one or more properties of this item                                             |
| Configuration | We have defaults, but they're so barebones you probably want to configure it yourself. |
| Preferences   | Tell us how you prefer this to work                                                    |

| Vocabulary |                                                                                                                                                                                        |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Simulator  | 著重於整體模型的建造                                                                                                                                                                   |
| Emulator   | 著重於主要功能的實踐                                                                                                                                                                   |
|            | 一句話來解釋這兩者的差異「If a flight-simulator could transport you from A to B then it would be a flight-emulator.（如果一個飛行模擬器能將你從 A 傳送到 B，那麼它就是飛行仿真器。）」 |

| Vocabulary     |                                |
|----------------|--------------------------------|
| Before / After | 大於（>）/ 小於（<）           |
| Start / End    | 大於等於（>=）/ 小於等於（<=） |

## API 命名慣例

用 ASP.NET Core MVC 開發，會把畫面跟 Web API 混在同一個專案裡面，這邊列一些命名慣例來作為參考。

以某網站的某個頻道為例，假設這個頻道叫 Laojiao，Laojiao 底下有 Project，Project 底下有 Article，Article 底下有 Comment，Comment 底下有 Reply，擁有五層的階層關係，但是 Laojiao 是抽象的概念，沒有實體資料，有實體資料是從 Project 開始，所以實體階段關係只有四層，如果以 ASP.NET Core MVC 專案範本進行開發，Web API 的設計規劃會是怎樣？

### 頻道首頁（專案列表）
|      | Web API          | Area    | Controller | Action | 應用場景 |
|------|------------------|---------|------------|--------|----------|
| 畫面 | HttPGet /laojiao | Laojiao | Project    | Index  |          |

### 單一專案
|      | Web API                               | Area    | Controller | Action | 應用場景                                |
|------|---------------------------------------|---------|------------|--------|-----------------------------------------|
| 畫面 | HttpGet /laojiao/project/{projectId:int} | Laojiao | Project    | Project |                                         |
| 公開資料 | HttpGet /laojiao/project/{projectId:int}/info | Laojiao | Project    | Info    | 部分的資料，通常是無需權限即可存取。   |
| 詳細資料 | HttpGet /laojiao/project/{projectId:int}/details | Laojiao | Project    | Details  | 完整的資料，有時候需要權限才能存取。   |
| 專案留言列表 | HttpGet /laojiao/project/{projectId:int}/comment/list | Laojiao | Comment    | List    | 能存取文章時，留言連同回覆一併存取。   |

### 文章列表
|      | Web API                               | Area    | Controller | Action | 應用場景 |
|------|---------------------------------------|---------|------------|--------|----------|
| 畫面 | HttpGet /laojiao/project/{projectId:int}/article | Laojiao | Article    | Index   |          |
| 資料 | HttpGet /laojiao/project/{projectId:int}/article/list | Laojiao | Article    | List    |          |

### 單篇文章
|      | Web API                               | Area    | Controller | Action  | 應用場景                                |
|------|---------------------------------------|---------|------------|---------|-----------------------------------------|
| 畫面 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int} | Laojiao | Article    | Article |                                         |
| 公開資料 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int}/info | Laojiao | Article    | Info    | 部分的資料，通常是無需權限即可存取。   |
| 詳細資料 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int}/details | Laojiao | Article    | Details  | 完整的資料，有時候需要權限才能存取。   |
| 文章留言列表 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int}/comment/list | Laojiao | ArticleComment | List    | 能存取文章時，留言連同回覆一併存取。   |

### 單篇文章留言
|      | Web API                               | Area    | Controller | Action  | 應用場景 |
|------|---------------------------------------|---------|------------|---------|----------|
| 畫面 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int}/comment/{commentId:int} | Laojiao | ArticleComment | Comment |          |

### 單篇文章留言的回覆
|      | Web API                               | Area    | Controller | Action  | 應用場景 |
|------|---------------------------------------|---------|------------|---------|----------|
| 回覆列表 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int}/comment/{commentId:int}/reply/list | Laojiao | ArticleCommentReply | List    |          |
|  |  |  | ArticleComment | ReplyList |          |
| 單篇回覆 | HttpGet /laojiao/project/{projectId:int}/article/{articleId:int}/comment/{commentId:int}/reply/{replyId:int}/details | Laojiao | ArticleCommentReply | Details  |          |
|  |  |  | ArticleComment | ReplyDetails |          |

### 新增文章
|      | Web API                               | Area    | Controller | Action  | 應用場景 |
|------|---------------------------------------|---------|------------|---------|----------|
| 畫面 | HttpGet /laojiao/project/{projectId:int}/article/new | Laojiao | Article    | New     |          |
| 新增 | HTTP POST /project/{projectId:int}/article | Project | Article    | Create  |          |

### 編輯文章
|      | Web API                               | Area    | Controller | Action  | 應用場景 |
|------|---------------------------------------|---------|------------|---------|----------|
| 畫面 | HttpGet /laojiao/project/{projectId:int}/article/edit | Laojiao | Article    | Edit    |          |
| 儲存 | HttpPatch /project/{projectId:int}/article | Project | Article    | Save    |          |
| 儲存 | HttpPut /project/{projectId:int}/article | Project | Article    | Save    |          |

### 刪除文章
|      | Web API                               | Area    | Controller | Action  | 應用場景 |
|------|---------------------------------------|---------|------------|---------|----------|
| 刪除 | HttpDelete /project/{projectId:int}/article/{articleId:int} | Project | Article    | Remove  |          |

### 刪除多篇文章
|      | Web API                               | Area    | Controller | Action  | 應用場景 |
|------|---------------------------------------|---------|------------|---------|----------|
| 刪除 | HttpDelete /project/{projectId:int}/article?id=1,2,3 | Project | Article    | Remove  |          |

## API 參數命名慣例

| CRUD 動作       | 命名格式         | 命名範例                                                 | 適用情境                                                       |   |
|-----------------|------------------|----------------------------------------------------------|----------------------------------------------------------------|---|
| Create          | CreateXXXInput   | CreateAccountInput CreateArticleInput                    | 明確代表是新增資料（不涉及更新）                               |   |
| Create / Update | SaveXXXInput     | SaveUserProfileInput SavePhotographyPreferencesInput     | 用於新增或更新都可能的情境（多數一般表單）                     |   |
| Update          | UpdateXXXInput   | UpdateUserProfileInput UpdatePhotographyPreferencesInput | 明確代表是更新資料（不涉及新增）                               |   |
| Read (List)     | XXXListFilter    | ArticleListFilter UserListFilter                         | 針對列表查詢提供過濾條件（回傳部分資料的清單）                 |   |
| Read (複數資源) | XXXFilter (複數) | ArticlesFilter MembersFilter                             | 若資源本身語意為集合或明確強調複數                             |   |
| Remove          | RemoveXXXCommand | RemoveAccountCommand RemoveArticleCommand                | 用於需要傳遞刪除參數的 API（例如：POST /remove、DELETE /{id}） |   |
