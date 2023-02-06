# php-function-mysqli-query
지겹다. 지겨워.

```php
<?php
/***
 * MySQL 설정
 */
const MYSQL_HOST = 'localhost';
const MYSQL_ID = 'asdf';
const MYSQL_PASS = 'asdf';
const MYSQL_DB = 'asdf';

$_ENV['mLink'] = @mysqli_connect(MYSQL_HOST, MYSQL_ID, MYSQL_PASS, MYSQL_DB) or die(json_encode(['status'=>'false', 'reason'=>'DB Error!']));

function sql_query($query): mysqli_result|bool {
    return mysqli_query($_ENV['mLink'], $query);
}

function sql_fetch_array($result): array|bool|null {
    if($result===false || is_int($result) || is_string($result)) return false;
    return mysqli_fetch_assoc($result);
}

function sql_fetch($query): bool|array|null {
    $prepare = sql_query($query);
    if($prepare===false) return false;

    $fetch = sql_fetch_array($prepare);
    if($fetch===false) return false;

    return $fetch;
}

function sql_insert_id(): int|string {
    return mysqli_insert_id($_ENV['mLink']);
}

function sql_escape($string): string {
    return mysqli_real_escape_string($_ENV['mLink'], $string);
}

function sql_close(): bool {
    return mysqli_close($_ENV['mLink']);
}

function sql_begin(){
    mysqli_autocommit($_ENV['mLink'], 0);
    return mysqli_begin_transaction($_ENV['mLink']);
}

function sql_commit(){
    mysqli_autocommit($_ENV['mLink'], 1);
    return mysqli_commit($_ENV['mLink']);
}

function sql_rollback(){
    return mysqli_rollback($_ENV['mLink']);
}
```
