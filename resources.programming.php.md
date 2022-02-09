---
id: e4XfQNS2Ax3lUF61vQCrt
title: PHP
desc: ''
updated: 1638527990019
created: 1638527865466
---

## Łączenie z bazą danych

Config:

```php
<?php 

$DB_SERVER_NAME = "localhost";
$DB_USERNAME = "root";
$DB_PASSWORD = "";
$DB_NAME = "php_playground";

$conn = mysqli_connect($DB_SERVER_NAME, $DB_USERNAME, $DB_PASSWORD, $DB_NAME);
```

Następnie:
```php
<?php
	$sql = "SELECT * FROM users;";
	$result = mysqli_query($conn, $sql);
	$resultCheck = mysqli_num_rows($result);

	if ($resultCheck > 0) {
		echo "Numbers of rows in db: $resultCheck";
	}
?>
```
