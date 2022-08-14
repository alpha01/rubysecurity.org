---
categories:
  - awesome-applications
  - mysql
tags: mysql
layout: post
title: Exporting data in MySQL to an XML file
created: 1376363615
---

So I just started reading a new MySQL administration book and starting to learn cool things that I didn't even know. One cool feature MySQL supports that I wasn't aware of is the ability to export and import data to and from XML files.

For example the following will export the table City from world database to an XML file.

```bash
root@mysql:~# mysql --xml -e 'SELECT * from world.City' > city.xml
```

To import data from the XML itself can be accomplished using the LOAD XML statement.

```sql
(root@localhost) [world] LOAD XML INFILE 'city.xml' INTO TABLE City;
Query OK, 4079 rows affected, 14 warnings (0.81 sec)
Records: 4079  Deleted: 0  Skipped: 0  Warnings: 14
```

### Resources

* <a href="http://dev.mysql.com/doc/refman/5.5/en/load-xml.html" target="_blank">http://dev.mysql.com/doc/refman/5.5/en/load-xml.html</a>
