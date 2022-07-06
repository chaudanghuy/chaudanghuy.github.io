## How to fix error row size too large from SQL when changing column

Due to MYSQL, it is pretty clear about its maximum row size: 
```note
Every table (regardless of storage engine) has a maximum row size of 65,535 bytes. Storage engines may place additional constraints on this limit, reducing the effective maximum row size.
```

The error message indicate that the table's definition allows rows that the table's InnoDB row format can't actually store

![image](https://user-images.githubusercontent.com/1155091/177480418-6a5cceae-dba6-4d9f-b80e-4fc70f2157c1.png)

---

### Solution

#### Disabled Innodb strict mode 

```tsql
SET GLOBAL innodb_strict_mode = 0;
```

#### Update config MYSQL

```powershell
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

innodb_strict_mode = 0
```
