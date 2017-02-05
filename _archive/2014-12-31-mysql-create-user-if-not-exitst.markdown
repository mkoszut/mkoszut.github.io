---
title:  "MySql 'CREATE USER IF NOT EXISTS'"
date:   2014-12-31 12:13:52
description: "MySql 'CREATE USER IF NOT EXISTS'"
---

Ciekawostka przyrodnicza. MySql nie ma CREATE USER IF NOT EXISTS dlatego trzeba użyć:

```
GRANT ALL PRIVILEGES ON  database_name . * TO 'user_name'@'localhost';
```