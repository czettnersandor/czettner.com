---
layout: post
title: MySQL database engines - comparision
created: 1355348665
comments: true
categories: [mysql]
---
The design of a database engine is always centered around the data structure and user requirements. The choice of MySQL as a relational database management system is an evidence if you are using an existing system such as Drupal or Magento, because both them are using MySQL by default and the documentation, community support is much stronger for these frameworks if you are using the supported database server. The choice of MyISAM or InnoDB as a storage engine is based on the user needs as well. The newest versions of Drupal uses InnoDB, as Magento does. Let me introduce why they decided this storage engine.

The differences between InnoDB and MyISAM are "referential integrity" and support for "transactions". InnoDB supports foreign keys, MyISAM does not. InnoDB has row level lock, MyISAM has table level lock.

In the practice, it means for operating a single database row without references, only writing this single row with the same table structure:

<ul>
<li><span>SELECT</span> command is always slower in InnoDB</li>
<li><span>INSERT</span> command is always slower in InnoDB</li>
<li><span>UPDATE</span> command is always slower in InnoDB</li>
<li><span>DELETE</span> command is always slower in InnoDB</li>
</ul>

The reason is that to operate this single row MyISAM is not using transactions.

If you need to enforce foreign key constraints, or you need the database to handle the changes on other tables updated on the referred tables, then you would choose InnoDB, because these features are not implemented in MyISAM.

Those are the most important differences.

If you don't need the features provided by InnoDB, then MyISAM could be a good choice as well. So back to the performance comparison above. Such as InnoDB is slower for a single operation (that is true for 10000 INSERT, 10000 UPDATE commands as well), the point for using InnoDB is not only it's features. I mentioned the table level lock. In MyISAM, when a SELECT command is running, it queues every write command. So an INSERT command should wait until the SELECT command has finished and vice versa. If a longer UPDATE/INSERT/DELETE command is running, then a SELECT command is waiting, which can cause lags on a website frontend.

InnoDB is using a row level lock, then while a write command is working on a row, it's possible to run a SELECT command simultaneously. Only the currently written row will be skipped.

In a live example, It's a good idea for mixing the different storage engines. For example a system log table could be stored with MyISAM as no references are required, we are just writing it except those few requests when we need to analyse or check the logs, but these are the use cases when the administrator can wait a few seconds to run a query.
