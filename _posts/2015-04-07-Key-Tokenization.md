---
layout: post
title: Key Tokenization
---

I mentioned in my previous [post](http://stevendanna.com/2015/03/28/Moving-Data-with-ODI-Studio/) about having to move data from an Oracle 12g database into a SQL Server database.  The SQL Server still had several applications that needed to read from it but the Oracle database was the source of truth.  In our case, the applications that sat on top of the SQL Server database were readonly type applications.

At the beginning of the project, I was told that id's in the Oracle database were generated using a standard sequence that generated numbers for new records that were higher than the the top value of the SQL Server.  Not only was this not true I found out, but we had the extra caveat that simply dropping the alpha part of the number would result in number collisions across the two databases.

For this situation we tokenized the alpha parts of the generated id's into a number.  So essentially we had id's that took the form similar to 'prod123456' that when tokenized would look like '10123456'.  We then used this tokenized id to match against the SQL Server database for the purposes of uniqueness in the ODI packages.  Legacy id's that were imported into Oracle retained their numeric id's.  This solution worked for this particular project due to the data growth projection.

Due to how I broke the task up, I used the Oracle REGEXP_LIKE and REGEXP_REPLACE functions in a CASE statement to pull the working set how I needed it.

```SQL
CASE
	WHEN REGEXP_LIKE(TABLE.FIELD_NAME, '^\\d{0,10}$') THEN TO_NUMBER(TABLE.FIELD_NAME)
	WHEN REGEXP_LIKE(TABLE.FIELD_NAME, '^[A-Za-z]{1,10}\\d{0,10}$') THEN TO_NUMBER(REGEXP_REPLACE(TABLE.FIELD_NAME, '[A-Za-Z]{1,10}', '10'))
	ELSE 0
END
```

This allowed me to leave my existing id's that were numeric alone and then tokenize the alpha parts of the id with a number.  The same concept could be used with a different number; 10 just fit my requirements.  I had to make sure I wasn't going to overflow the integer column in the SQL Server database.

NOTE: You'll notice the double leading slashes on the regular expression.  This was needed due to how ODI Studio sends the values you enter to generate a script.  I need the double slashes so that the generated SQL had a single slash and the regular expression would work correctly.  I used regular expressions in other parts of the query, but only the SELECT portion of the script had this anomoly.  Some things you don't want to dig too deeply into.  This is one of them.
