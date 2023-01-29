---
title: "Workaround for deleting Columns in SQLite"
date: 2019-08-07T13:28:18+01:00
tags: ["SQL"]
slug: 
description: "How to insert and delete columns from an SQLite Database."
---

SQLite has limited `ALTER TABLE` support that you can use to add a column to the end of a table or to change the name of a table. If you want to make more complex changes in the structure of a table, you will have to recreate the table. You can save existing data to a temporary table, drop the old table, create the new table, then copy the data back in from the temporary table.

From the docs.

> It is not possible to rename a column, remove a column, or add or remove constraints from a table.

source : [http://www.sqlite.org/lang_altertable.html][1]

While you can always create a new table and then drop the older one.

I will try to explain this [this workaround ][2] with an example.

{{< highlight html >}}
sqlite> .schema
CREATE TABLE person(
 id INTEGER PRIMARY KEY, 
 first_name TEXT,
 last_name TEXT, 
 age INTEGER, 
 height INTEGER
);
sqlite> select * from person ; 
id          first_name  last_name   age         height    
----------  ----------  ----------  ----------  ----------
0           john        doe         20          170       
1           foo         bar         25          171  
{{< /highlight >}}

Now you want to remove the column `height` from this table.


Create another table called `new_person` 

{{< highlight html >}}
sqlite> CREATE TABLE new_person(
   ...>  id INTEGER PRIMARY KEY, 
   ...>  first_name TEXT, 
   ...>  last_name TEXT, 
   ...>  age INTEGER 
   ...> ) ; 
sqlite> 
{{< /highlight >}}

Now copy the data from the old table 


{{< highlight html >}}
sqlite> INSERT INTO new_person
   ...> SELECT id, first_name, last_name, age FROM person ;
sqlite> select * from new_person ;
id          first_name  last_name   age       
----------  ----------  ----------  ----------
0           john        doe         20        
1           foo         bar         25        
sqlite>
{{< /highlight >}}


Now Drop the `person` table and rename `new_person` to `person`

{{< highlight html >}}
sqlite> DROP TABLE IF EXISTS person ; 
sqlite> ALTER TABLE new_person RENAME TO person ;
sqlite>
{{< /highlight >}}

So now if you do a `.schema`, you will see

{{< highlight html >}}
sqlite>.schema
CREATE TABLE "person"(
 id INTEGER PRIMARY KEY, 
 first_name TEXT, 
 last_name TEXT, 
 age INTEGER 
);
{{< /highlight >}}


So that's how you insert and delete columns from an SQLite Database

[1]: http://www.sqlite.org/lang_altertable.html
[2]: http://www.sqlite.org/faq.html#q11