###### tags: `SQL`、`SELECT進階語法`、`CH10`
# SELECT進階語法

> ### UNION
```
SELECT 聯絡人 AS 邀請名單, 地址
FROM 合作廠商
UNION
SELECT 聯絡人, 地址
FROM 客戶
ORDER BY 聯絡人
```
*  UNION
    1. 將兩個(以上) SQL 查詢的結果合併起來，而由 UNION 查詢中各別 SQL 語句所產生的欄位需要是相同的資料型別及順序。
    2. 查詢只會返回不同值的資料列，有如 SELECT DISTINCT(重複的值會自動濾掉)。
    3. 像是 OR (聯集)，如果紀錄存在於第一個查詢結果集或第二個查詢結果集中，就會被取出。
    4. UNION 與 JOIN 不同的地方在於，JOIN 是作橫向結合 (合併多個資料表的各欄位)；而 UNION 則是作垂直結合 (合併多個資料表中的紀錄)。
*  口語化說法：查詢的資料(邀請名單，地址)會從合作廠商及客戶中UNION出來，所UNION的資料皆不重複。
![](https://i.imgur.com/PLG1x9Z.jpg)
<br>

> ### UNION ALL
```
SELECT 聯絡人 AS 邀請名單, 地址
FROM 合作廠商
UNION ALL
SELECT 聯絡人, 地址
FROM 客戶
ORDER BY 聯絡人
```
*  UNION ALL ==> 查詢會返回相同值的資料列。
![](https://i.imgur.com/pK2IrSU.jpg)
與上個語法不同之處，此語法連同重複的也一併UNION出來(如：方永正)。
<br>

> ### UNION進階用法
```
SELECT 聯絡人 AS 邀請名單, 地址
FROM 合作廠商
UNION
SELECT 聯絡人, 地址
FROM 客戶
UNION 
SELECT '王大砲', '台北市南京東路三段34號5樓'
ORDER BY 聯絡人
```
### **說明**
```
UNION 
SELECT '王大砲', '台北市南京東路三段34號5樓
```
此語法的SELECT的資料，未存在在客戶及合作廠商的資料中，所以利用UNION來將其資料合併到前面的UNION的資料列中。
<br>

> ### MAX
```
SELECT 產品名稱, 價格 
FROM    旗旗公司 
WHERE 價格 > ( SELECT MAX(價格) FROM  標標公司 ) 
```
*  MAX ==> 取得特定欄位中的最大紀錄值
<br>

> ### IN
```
SELECT * 
FROM 標標公司 
WHERE 產品名稱 IN ( SELECT 產品名稱 
                    FROM 旗旗公司 )
```
### **說明**
搜尋出來的資料，會是存在標標公司與旗旗公司的產品名稱。
<br>

> ### <= ALL
```
SELECT 價格 
FROM 標標公司 
WHERE 價格 <= ALL ( SELECT 價格   
                    FROM  旗旗公司 
                    WHERE 價格 > 410)
```
### **說明**
會先搜尋旗旗公司中價格大於410元的，此語法搜尋出來的是420元與500元。
再來搜尋標標公司的價格，是否皆比420與500小，如果以上兩個條件成立，便會被搜尋出來。
<br>

> ### <= ANY
```
SELECT 價格 
FROM 標標公司 
WHERE 價格 <= ANY ( SELECT 價格
                    FROM 旗旗公司 
                    WHERE 價格 > 410 ) 
```
### **說明**
會先搜尋旗旗公司中價格大於410元的，此語法搜尋出來的是420元與500元。
再來搜尋標標公司的價格，只要符合其中一項條件，便會被搜尋出來。
此說明的條件，為搜尋出來的價格(420元與500元)。
<br>

> ### EXISTS
```
SELECT * 
FROM 標標公司 
WHERE EXISTS ( SELECT * 
               FROM 旗旗公司 
               WHERE 產品名稱 = 標標公司.產品名稱
                     AND 價格 > 495) 
```
* EXISTS ==> 用來判斷子查詢是否有返回的結果，如果有結果返回則為真、否則為假。若 EXISTS 為真，就會繼續執行外查詢中的 SQL；若 EXISTS 為假，則整個 SQL 查詢就不會返回任何結果。
* 此語法有符合EXISTS，固有傳值回來。
<br>

> ### DECLARE
```
DECLARE @number int
DECLARE @string char(20) 
SET @number = 100
SET @string = '天天書局'
SELECT @number AS 數字, @string AS 字串
```
* DECLARE ==> 要宣告變數，只要使用 DECLARE 陳述式就可以了，要注意的是，變數名稱的開頭必須有 @ 符號。
### **說明**
在此與發宣告兩個變數，@number與@string，接著在裡面SET值，最後SELECT出來。
<br>

> ### AND、OR
```
SELECT * 
FROM 旗旗公司 
WHERE (價格 > 450 AND 價格 < 500) OR 價格 < 430
```
* 一則符合(價格 > 450 AND 價格 < 500)或符合(價格 < 430)。
<br>

> ### BETWEEN
```
SELECT * 
FROM 旗旗公司 
WHERE 價格 BETWEEN 420 AND 510
```
BETWEEN ==> 列出一個範圍找資料
### **說明**
大於等於420及小於等於510，若符合便可以SELECT出來。
<br>

> ### IN
```
SELECT * 
FROM 標標公司 
WHERE 產品名稱 IN ( 'SQL 指令寶典', 'AutoCAD 教學', 'Linux 手冊' ) 
```
* IN ==> 要完全地知道我們需要的條件才可找出資料
### **說明**
標標公司的產品名稱倘若有與IN條件裡的資料相符，便可被SELECT出來。
<br>

> ### LIKE
```
SELECT * 
FROM 標標公司 
WHERE 產品名稱 LIKE '_SQL%'
```
* LIKE ==> 依據一個模式來找出我們要的資料
* 在不確定字元數時使用 EX: ‘SQL%’

```
WHERE 產品名稱 LIKE {模式}
```
模式網址(1)：https://www.1keydata.com/tw/sql/sql-wildcard.html
模式網址(2)：http://fecbob.pixnet.net/blog/post/38204901-sql%E6%A8%A1%E7%B3%8A%E6%9F%A5%E8%A9%A2%E8%AA%9E%E6%B3%95%E8%A9%B3%E8%A7%A3
<br>

> ### NOT EXISTS
```
SELECT  * 
FROM  標標公司
WHERE  NOT EXISTS ( SELECT  *  
                    FROM  旗旗公司 
                    WHERE  產品名稱 = 標標公司.產品名稱)
```
### **說明**
EXISTS在前面有說明過。
此語法是將不符合條件的資料SELECT出來，列出來的資料為"產品名稱 ≠ 標標公司.產品名稱"。
此語法的條件如下
```
SELECT  *  
FROM  旗旗公司 
WHERE  產品名稱 = 標標公司.產品名稱
```
<br>

> ### &、|、^
```
PRINT 59 & 12 
PRINT 59 | 12 
PRINT 59 ^ 12 
```
結果顯示
```
8
63
55
```
![](https://i.imgur.com/T2qpMBC.jpg)
![](https://i.imgur.com/eOPpMW8.jpg)
<br>

> ### CONVERT
```
SELECT 'Linux 架站實務的價格是 ' + CONVERT(varchar, 價格) + ' 元' 
FROM 標標公司 
WHERE 產品名稱 = 'Linux 架站實務' 
```
### **說明**
為了其他資料型別要與字串相連，"價格"則要先轉換為字串型別。
<br>

> ### ~
```
PRINT ~ CAST(1 AS tinyint) 
```
結果顯示
```
254
```
### **說明**
位元補數的轉換。
![](https://i.imgur.com/AkQ5acn.jpg)
<br>

> ### +=
```
UPDATE 書籍
SET 價格 += 100 
WHERE 書籍名稱 = 'Windows Server 系統實務'
```
### **說明**
書籍名稱=Windows Server 系統實務的，價格加100。
<br>

> ### SET ANSI_NULLS OFF
```
SET ANSI_NULLS OFF
SELECT * 
FROM 員工 
WHERE 主管編號 = NULL
```
### **說明(不一定正確)**
SET ANSI_NULLS ON時，NULL = NULL ==>假 、NULL <> NULL ==> 假
但SET ANSI_NULLS OFF，（NULL = NULL） ==> 真。
*  口語化說法：最後搜尋出來的結果會是符合(主管編號 = NULL)的資料。
<br>

> ### ISNULL
```
SELECT 姓名,
       ISNULL(CAST(主管編號 AS VARCHAR), '無') AS 主管
FROM 員工
```
### **說明**
如果主管編號為NULL的話，則會顯示無
![](https://i.imgur.com/73ck6OU.jpg)
<br>

> ### RETURNS
```
CREATE FUNCTION 優惠計算(@書籍編號 AS int)
RETURNS TABLE AS
RETURN
 SELECT (價格 * 0.8) AS 優惠價
 FROM 書籍
 WHERE 書籍編號 = @書籍編號
```
* RETURN ==> 無條件地結束查詢或程序。
<br>

> ### CROSS APPLY
```
SELECT 書籍編號, 書籍名稱, 價格, 優惠價, 出版公司
FROM 書籍
CROSS APPLY 優惠計算(書籍編號)
```
* CROSS APPLY ==> 等同於 INNER JOIN 與 LEFT OUTER JOIN。
* 此語法的優惠價從"優惠計算"裡算出來的。
![](https://i.imgur.com/ZdqOQZj.jpg)
<br>

> ### IIF
```
SELECT IIF(性別=0, '女生', '男生') AS 性別, 滿意度, COUNT(*) AS 人數
FROM 問卷
GROUP BY 性別, 滿意度  -- 依性別及滿意度分組計數
ORDER BY 性別, 滿意度
```
### **說明**
1. 性別=0為女生
![](https://i.imgur.com/UnLKKIg.jpg)
<br>

> ### IIF進階用法
```
SELECT IIF(滿意度=3, '滿意', IIF(滿意度=2, '尚可', '差勁')) 評價, COUNT(*) 人數
FROM 問卷
GROUP BY 滿意度  -- 依滿意度分組計數
ORDER BY 滿意度 DESC
```
![](https://i.imgur.com/rpBY1Eb.jpg)
<br>

> ### ROW_NUMBER()
```
SELECT 書籍編號, 書籍名稱, 價格, 出版公司,
       ROW_NUMBER() OVER(ORDER BY 價格) AS 價格排名
FROM 書籍
```
* ROW_NUMBER()
1. 為結果集的輸出編號。
2. 會依序為所有資料列編號 (例如 1、2、3、4、5)。![](https://i.imgur.com/vUEbUbp.jpg)
<br>

> ### RANK()
```
SELECT 書籍編號, 書籍名稱, 價格, 出版公司,
   RANK() OVER(ORDER BY 價格) AS 價格排名
FROM 書籍
```
* RANK()
1.  資料列的次序等於一加上前述資料列之前的次序數目。
2.  為繫結提供相同的數值 (例如 1、2、2、4、5)。
![](https://i.imgur.com/bgoXHQj.jpg)
<br>

> ### DENSE_RANK() 
```
SELECT 書籍編號, 書籍名稱, 價格, 出版公司,
  DENSE_RANK() OVER(ORDER BY 價格) AS 價格排名
FROM 書籍
```
* DENSE_RANK()
1. 次序值中沒有任何間距。
2. 特定資料列的次序是一加上該特定資料列前面之相異次序值的數目。
![](https://i.imgur.com/9LNODEH.jpg)
<br>

> ### ROW_NUMBER()進階用法 
```
SELECT 書籍編號, 書籍名稱, 價格, 出版公司,
ROW_NUMBER() OVER(ORDER BY 價格) AS 價格排名
FROM 書籍
ORDER BY 價格排名
OFFSET 4 ROWS FETCH NEXT 4 ROWS ONLY
```
### **說明**
OFFSET 4 ROWS FETCH NEXT 4 ROWS ONLY ==> 只列出第5筆到第8筆的資料。
<br>

> ### PARTITION BY 
```
SELECT 書籍編號, 書籍名稱, 價格, 出版公司,
ROW_NUMBER() OVER(PARTITION BY 出版公司 ORDER BY 價格) AS 價格排名
FROM 書籍
```
* PARTITION BY ==> 將查詢結果集分成幾個資料分割。
![](https://i.imgur.com/Zs6HDlH.jpg)

### **說明**
分割後的結果進行排名。
<br>

> ### SUM、AVG 
```
SELECT 客戶名稱,書名,數量,
       SUM(數量) OVER(PARTITION BY 客戶名稱) AS 總數量,
	   AVG(數量) OVER(PARTITION BY 客戶名稱) AS 平均量
FROM 出貨記錄
```
![](https://i.imgur.com/JrBZpRi.jpg)
<br>

> ### SUM、AVG 進階用法
```
SELECT 客戶名稱,月份,數量,
   AVG(數量) OVER(PARTITION BY 客戶名稱 ORDER BY 月份) 移動平均值,
   SUM(數量) OVER(PARTITION BY 客戶名稱 ORDER BY 月份) 累計總和
FROM 月份銷售統計 
```
![](https://i.imgur.com/chtNkU6.jpg)
<br>

> ### ROWS BETWEEN 1 PRECEDING AND 1 (有點難懂)
```
SELECT 客戶名稱,月份,數量,
   SUM(數量) OVER (PARTITION BY 客戶名稱 
                   ORDER BY 月份 
                   ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) 累計總和
FROM 月份銷售統計
```
![](https://i.imgur.com/asP4zJf.jpg)
<br>

> ### RANGE UNBOUNDED PRECEDING (有點難懂)
```
SELECT 客戶名稱,月份,數量,
   SUM(數量) OVER (ORDER BY 月份 
                   RANGE UNBOUNDED PRECEDING) 累計總和
FROM 月份銷售統計
```
![](https://i.imgur.com/EQpSWlX.jpg)
<br>

> ### ROWS UNBOUNDED PRECEDING 
```
SELECT 客戶名稱,月份,數量,
   SUM(數量) OVER (ORDER BY 月份 
                   ROWS UNBOUNDED PRECEDING) 累計總和
FROM 月份銷售統計
```
![](https://i.imgur.com/SrYERBr.jpg)
<br>

> ### PIVOT 
```
SELECT 書名, 天天書局, 大雄書局
FROM 
   (SELECT 客戶名稱, 書名, 數量
    FROM 出貨記錄) as Src 
    PIVOT (SUM(數量) 
           FOR 客戶名稱 
           IN (天天書局, 大雄書局)) as Piv
```
* PIVOT ==> 扭轉資料，由直列轉為橫向資料。
![](https://i.imgur.com/vQjNpBp.jpg)
<br>

> ### PIVOT 進階用法
```
CREATE TABLE My_Table
(書名 VARCHAR(50),
 天天書局 VARCHAR(20),
 大雄書局 VARCHAR(20)
)
INSERT My_Table
SELECT 書名, 天天書局, 大雄書局
FROM 
   (SELECT 客戶名稱, 書名, 數量
    FROM 出貨記錄) as Src
  PIVOT 
   (SUM(數量) 
    FOR 客戶名稱 
    IN (天天書局, 大雄書局)) as Piv

SELECT 客戶名稱, 書名, 數量 
FROM My_Table
  UNPIVOT 
    (數量
     FOR 客戶名稱
     IN (天天書局, 大雄書局)) as unPiv
```
![](https://i.imgur.com/Mg3klVS.jpg)