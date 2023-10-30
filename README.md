# Masking logs using logback - Spring Boot 

Mask, Hide & Replace Sensitive Data In Spring Boot Logs 1) Console Appender 2) Rolling File Appender.

The process:
-----------------------------------------------------
* Mask or replace sensitive log details printed in JSON or To-String format in the console appender or rollingFile appender using replace (aka %replace expression)
* This masking works in both console and rolling-File-Appender.


1. Add logback.xml file to your project root as shown.
2. %replace($msg){ regex expression for JSON or for toString , replacing placeholders }
3. Whenever you need to add another regex expression and placeholder value for some field follow.
   * Cut your ENTIRE CURRENT %replace on number 2 and nest it within another %replace's C brackets as below.
   * %replace(%replace(%msg){ old regex expression JSON or for toString , old replacing placeholders }){ new regex expression, new replacing placeholders }
   * Your keep cutting your ENTIRE CURRENT %replace and nest it within another %replace's C brackets as below, if you need to keep adding, as below.
   * %replace(%replace(%replace(%msg){ old regex expression JSON or for toString , old replacing placeholders }){ new regex expression, new replacing placeholders }){newest regex expression, newest replacing placeholders}
NB*. Use https://www.regex101.com to create, test and verify your regex expressions.
NB*. You may refer to this youtube video for steps https://www.youtube.com/watch?v=3YK6UZq_51E


The replace expression, Example:
-----------------------------------------------------
* Replace toString data: %replace(%msg){((?i)\bproductName\b(=)\w*), "productName=XXXXXXX"}
* Replace Json String: %replace(%msg){((?i)"\bsellerName\b(":\w*))"[^"]+", $1"XXXXXXXX"}


A Sample Input Data:
-----------------------------------------------------
1. Json Data: {"productId":1, "productName":"p1", "sellerName": "s1"}
2. toString Data: [productId=1, productName="p1", sellerName="s1"]


Output:
-----------------------------------------------------
1. Json Data: {"productId":1, "productName":"XXXXXXX", "sellerName": "XXXXXXXX"}
2. toString Data: [productId=1, productName="XXXXXXX", sellerName="XXXXXXX"]
