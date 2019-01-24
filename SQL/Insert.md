###### tags: `SQL`、`INSERT`、`UPDATE`、`DELETE`、`CH08`
# INSERT語法
### ★此處的SELECT皆為確認資料是否成功新增，有特例會額外說明★
### ★此處的表如果前面有"#"皆為區域性的暫存資料表，並非真的存在★

> ### 基本語法
```
INSERT 圖書室借用記錄 ( 員工編號, 書名, 數量, 附註 )
VALUES ( 3, 'Windows 架站實務', 1, '寫作參考用' )
```
* INSERT ==> 新增資料到某資料表
* VALUES ==> 即將新增的資料
<br>

> ### IDENTITY、SET IDENTITY_INSERT ON/OFF
```
SET IDENTITY_INSERT 圖書室借用記錄 ON
INSERT 圖書室借用記錄 ( 編號, 員工編號, 書名 ) 
VALUES ( 0, 5, 'Word 手冊')
SET IDENTITY_INSERT 圖書室借用記錄 OFF
SELECT *  FROM 圖書室借用記錄
```
*  IDENTITY => 自動編號
   ex:IDENTITY(10,5) ==> 從10開始，以5為單位(10,15,20...)
*  SET IDENTITY_INSERT ON => 想要將值插入到自動編號中，必須做此設定
<br>

> ### 利用SELECT的資料來INSERT資料
```
INSERT  圖書室借用記錄  ( 員工編號, 書名 )
SELECT   3, 書籍名稱
FROM    書籍
WHERE   書籍編號 < 4

SELECT * FROM 圖書室借用記錄
```
* INSERT 圖書室借用記錄  ( 員工編號, 書名 )
* SELECT   3, 書籍名稱 FROM 書籍  WHERE  書籍編號 < 4
  說明：會在書籍(表)去搜尋書籍編號(欄位) < 4的資料
* SELECT完成後，會直接INSERT到表中(新增進去的資料會接在舊資料後面)
<br>

> ### 利用SELECT的資料來INSERT資料
```
INSERT  圖書室借用記錄  ( 員工編號, 書名 )
SELECT   3, 書籍名稱
FROM    書籍
WHERE   書籍編號 < 4

SELECT * FROM 圖書室借用記錄
```
### **說明**
會在書籍(表)去搜尋書籍編號(欄位) < 4的資料
SELECT完成後，會直接INSERT到表中(新增進去的資料會接在舊資料後面)
![](https://i.imgur.com/9FfQmZV.jpg)
<br>

> ### CREATE TABLE、EXEC
```
CREATE TABLE #HELPDB
   ( 名稱          nvarchar(24) ,
     空間大小  nvarchar(13) ,
     擁有者      varchar(24) ,
     DBID           smallint ,
     建立日期  smalldatetime ,
     狀態          text ,
     相容性層級  tinyint )

INSERT #HELPDB 
   EXEC sp_helpdb

SELECT * FROM #HELPDB
```
* CREATE TABLE ==> 新增表
* EXEC  ==> 調用另一個有傳回值的預存程序（必須獲得傳回值）
<br>

> ### SELECT INTO
```
SELECT   姓名 AS 借閱者, 書名, 數量 AS 本數
INTO     借閱清單
FROM     圖書室借用記錄, 員工
WHERE    圖書室借用記錄.員工編號 = 員工.編號
```
* SELECT INTO ==> 用來從某資料表查詢所得之資料集結果新增到另一個新建的資料表中。此一指令常用來複製備份資料表，或將資料表輸出至另一資料庫中。
### **說明**
此處的"借閱清單"，原是並不存在於資料庫中，透過語法將其新增進來，並從其他表查詢資料後，再塞進該表中。
<br>

> ### WHERE  1 = 0 
```
SELECT  * 
INTO    聯絡名冊 
FROM    員工
WHERE  1 = 0 

SELECT * FROM 員工
SELECT * FROM 聯絡名冊
```
* WHERE  1 = 0  ==> 網路上說，只是很單純的新增結構，並不包含資料
<br>

> ### UPDATE、SET
```
UPDATE  圖書室借用記錄
SET     員工編號 = 6 , 
        附註 = NULL 
WHERE   員工編號 = 3

SELECT * FROM 圖書室借用記錄
```
* UPDATE  ==> 更新資料
* SET ==> 欲改變的值
### **說明**
此處的語法為，使用者要更新圖書室借用記錄(表)中的員工編號(欄位)及附註(欄位)，該資料原本為員工編號的編號3，更新後會如下圖。
![](https://i.imgur.com/SXHdf1M.jpg)
員工編號原本為3，更新後改成6。
<br>

> ### DELETE
```
DELETE 圖書室借用記錄
WHERE  書名 = 'Word 手冊'

SELECT * FROM 圖書室借用記錄
```
* DELETE  ==> 刪除指定的資料行
<br>

> ### 進階DELETE
```
DELETE  圖書室借用記錄
FROM    員工
WHERE   圖書室借用記錄.員工編號 = 員工.編號
        AND 員工.姓名 = '楊咩咩'

SELECT * FROM 圖書室借用記錄
```
### **說明**
此處的語法為，如果圖書室借用記錄.員工編號 = 員工.編號 AND 員工.姓名 = '楊咩咩'之條件是符合，則會在圖書借用紀錄刪除該行資料。
<br>

> ### TRUNCATE TABLE 
```
TRUNCATE TABLE  圖書室借用記錄
```
* TRUNCATE TABLE ==> 清除該表格所有的資料(欄位及結構皆會保留)
<br>

> ### OUTPUT INSERTED
```
INSERT 圖書室借用記錄 ( 員工編號, 書名 )
OUTPUT INSERTED.*
VALUES ( '12', 'SQL 語法辭典' ),
       ( '25', 'Windows 使用手冊' ),
       ( '13', 'Linux 架站實務'),
       ( '12', 'VB 程式設計')

```
* OUTPUT ==> 可帶回兩種資訊(另外一種在下面)
  INSERTED.欄位：取回INSERT 、 UPDATE、MERGE完成後，被影響資料的資訊 
* 此處的語法OUTPUT INSERTED.* 中的* 為全部的資料(* = 全部)
<br>

> ### OUTPUT DELETED
```
UPDATE 圖書室借用記錄 
SET 書名 = 'C# 程式設計'
OUTPUT DELETED.編號, DELETED.書名 舊書名, INSERTED.書名 新書名
WHERE 編號 = 5

```
* OUTPUT ==> 可帶回兩種資訊(另外一種在上面)
  DELETED.欄位：取回UPDATE、DELETE、MERGE完成前 ，被影響資料的資訊
* 上一段語法所新增的資料皆在此段語法中被刪除，故OUTPUT出來的資料只有"C# 程式設計"該本書籍
### **注意!!!**
OUTPUT DELETED並非真正刪除該筆資料，只是在OUTPUT中被刪除而未顯示出來，利用SELECT還可是可以看到一開始新增的資料
* 口語化說法：資料只是假的被刪除，其實還在
<br>

> ### DELETE、OUTPUT DELETED
```
DELETE 圖書室借用記錄
OUTPUT DELETED.*
WHERE 編號 = 3

```
### **注意!!!**
此處的語法 DELETE 圖書室借用記錄 WHERE 編號 = 3，故編號3的資料真正被刪除，OUTPUT DELETED.* 為所有資料，最後顯示出來的資料都是OUTPUT所影響到的資料列
* 口語化說法：真正被刪除的資料為編號3，其他的資料皆是被假刪除
<br>

> ### CREATE TABLE #OUTPUT_TB
```
CREATE TABLE #OUTPUT_TB
   ( 編號  int IDENTITY,
     員工編號 int, 
     書名  nvarchar(16)
   )
```
### **說明**
新增一個區域性的暫存資料表
<br>

> ### UPDATE #OUTPUT_TB
```
UPDATE 圖書室借用記錄
SET 員工編號 = '57'
OUTPUT INSERTED.編號, INSERTED.員工編號, INSERTED.書名 
   INTO #OUTPUT_TB (編號, 員工編號, 書名)
WHERE 員工編號 = '12'

SELECT * FROM #OUTPUT_TB
```
### **說明**
修改區域性的站存資料表，修改內容是前面OUTPUT所新增的員工編號12。
* 口語化說法：把員工編號12的資料改成57，因為只會有兩筆異動到，故OUTPUT只有兩筆資料。
