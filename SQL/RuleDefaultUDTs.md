###### tags: `SQL`、`RuleDefaultUDTs`、`CHbb`
# RuleDefaultUDTs語法

> ### CHECK
```
CREATE TABLE 員工薪資
(
 編號 int IDENTITY PRIMARY KEY, 
 薪資 smallmoney,
 CHECK (薪資 > 0 AND 薪資 <= 50000)
)
```
* CHECK ==> 限制用來約束欄位中的可用值，以保證該欄位中的資料值都會符合您設定的條件。
<br>

> ### CREATE RULE
```
CREATE RULE Price_rule
AS @price >= 1 AND @price <= 50000 
```
```
CREATE RULE charset_rule 
AS @charset LIKE 'F0[1-9][1-9]-[A-E]_'
```
```
CREATE RULE Gender_rule
AS @gender IN ('男', '女')
```
```
CREATE RULE payday_rule
AS @payday >= getdate()
```
* CREATE RULE
1. 建立一個稱為規則的物件。
2. 當繫結到資料行或別名資料型別時，規則會指定能夠插入這個資料行的可接受的值。
3. 資料行或別名資料型別只能有一個繫結的規則。
<br>

> ### CREATE DEFAULT
```
CREATE DEFAULT 性別_df
AS '男' 
```
```
CREATE DEFAULT 地點_df
AS '台灣'
```
* CREATE DEFAULT ==> 建立一個稱為預設值的物件。
1. 當繫結到某個資料行或別名資料型別，且在插入作業期間未明確提供任何值時，預設值會指定要插入物件所繫結之資料行 (如果是別名資料型別，則是所有資料行) 的值。

詳細的參考網址：https://docs.microsoft.com/zh-tw/sql/t-sql/statements/create-default-transact-sql?view=sql-server-2017
<br>

> ### CREATE TYPE
```
CREATE TYPE Phone
FROM char(12) 
NOT NULL 
```
```
CREATE TYPE PayDay
FROM date
NULL
```
```
CREATE TYPE MyBook AS TABLE   -- 必須使用 AS 而不是 FROM
(書籍編號 int PRIMARY KEY, 
 書籍名稱 varchar(50))
```
* CREATE TYPE ==> 定義一個新的資料類型。

詳細的參考網址：http://twpug.net/docs/postgresql-doc-8.0-zh_TW/sql-createtype.html
<br>

> ### EXEC
#### RULE
```
EXEC sp_bindrule Gender_rule, '員工.性別'
```
```
EXEC sp_unbindrule '員工.性別'
```
#### DEFAULT
```
EXEC sp_bindefault 性別_df, '員工.性別'
```
```
EXEC sp_unbindefault '員工.性別'
```
* EXEC ==> 執行系統預存程序
1. 比對系統程序名稱時，會使用呼叫資料庫定序。
2. 系統程序會以 sp_ 前置詞開頭。
3. 以邏輯的方式出現在所有使用者與系統定義的資料庫中，因此可以從任何資料庫中執行，而不必完整限定程序名稱。
<br>

> ### DROP
#### RULE
```
DROP RULE Price_rule, Gender_rule
```
#### DEFAULT
```
DROP DEFAULT 地點_df
```
#### TYPE
```
DROP TYPE Phone
```
```
DROP TYPE PayDay 
```
* DROP ==> 從目前資料庫移除一或多個使用者自訂的規則。
<br>

> ### DECLARE
```
DECLARE @mybook MyBook  -- 以『使用者定義資料表類型』宣告 table 變數

INSERT @mybook                
SELECT 書籍編號, 書籍名稱
FROM 書籍

SELECT * FROM @mybook 
```
* DECLARE ==> 可以同時宣告多個變數，變數跟變數之間，用逗號來分隔

詳細的參考網址：https://ithelp.ithome.com.tw/articles/10009411

-----
其彥影片
(UTTS使用者自訂資料型別)
1.建立的RULE，可以在選擇的資料庫中/可程式性/規則看到
2.建立的DEFAULT，可以在選擇的資料庫中/可程式性/預設值看到
3.建立TYPE，在資料庫/可程式性/類型/使用者定義資料類型/(右鍵)/建立使用者定義資料型別(預設值跟規則都可以用第(1)(2)的來使用
4.建立完TYPE後，CREATE TABLE 資料類型可使用(3)所建立的資料型別


注意!!!
假如是出生日期 要寫成 <= getdate()
購買東西日期 >= getdate()
getdate()為資料庫內建，自動抓取日期時間的