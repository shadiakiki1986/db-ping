# db-ping
db-ping verifies a database server is responding by executing a query in a timed loop.

[![Latest Stable Version](https://img.shields.io/packagist/v/ofbeaton/db-ping.svg?style=flat-square)](https://packagist.org/packages/ofbeaton/db-ping)
[![Minimum PHP Version](https://img.shields.io/badge/php-%3E%3D%205.6-8892BF.svg?style=flat-square)](https://php.net/)
[![Build Status](https://img.shields.io/travis/ofbeaton/db-ping/master.svg?style=flat-square)](https://travis-ci.org/ofbeaton/db-ping)

Optionally includes slave replication checks.

PHP 5.6+ console command that uses PDO to provide the database drivers:
- [x] MySQL

## Installing via phar

Before proceeding, you need a working PHP 5.6+ installation.

The recommended way to install db-ping is by downloading the phar. 

See [Releases](https://github.com/ofbeaton/db-ping/releases) for downloads.

Next, run the phar from the command line:


```bash
php db-ping.phar help

php db-ping.phar mysql --pass=mysecretpassword
```

## Testing

### Smoke test
If there is no mysql server running locally, pinging will give a `connection refused` error as below

```bash
$ php bin/db-ping mysql
DB-PING 127.0.0.1:3306
from 127.0.0.1:3306: connection refused. delay=2000ms, exec=0ms, since success=0s, since fail=0s
from 127.0.0.1:3306: connection refused. delay=2000ms, exec=0ms, since success=0s, since fail=2.0006s
```

### Against a real MySql server
Launch a temporary mysql server:

`docker run --name some-mysql --rm -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -p 3306:3306 mysql`

Wait a few seconds while it initializes, then open another terminal and ping it:

```bash
$ php bin/db-ping mysql -u user -p pass
DB-PING 127.0.0.1:3306
from 127.0.0.1:3306: connected. delay=2000ms, exec=0ms, since success=0s, since fail=0s
from 127.0.0.1:3306: check passed. delay=2000ms, exec=0ms, since success=0s, since fail=0s
from 127.0.0.1:3306: check passed. delay=2000ms, exec=0ms, since success=2.0022s, since fail=0s
from 127.0.0.1:3306: check passed. delay=2000ms, exec=0ms, since success=4.0028s, since fail=0s
```

To stop the dockerfile: `docker stop some-mysql`


### Against a real database server via ODBC

1. Set up your php server for [ODBC](http://php.net/manual/en/ref.pdo-odbc.php)
2. Add the server you'd like to test against to the `/etc/odbc.ini` file (on linux)
3. Ping it

```bash
$ php bin/db-ping ODBC -d MarketflowAcc -u rou -p rou
DB-PING odbc:MarketflowAcc
from odbc:MarketflowAcc: connected. delay=2000ms, exec=0ms, since success=0s, since fail=0s
from odbc:MarketflowAcc: check passed. delay=2000ms, exec=0ms, since success=0s, since fail=0s
from odbc:MarketflowAcc: check passed. delay=2000ms, exec=0ms, since success=2.0058s, since fail=0s
from odbc:MarketflowAcc: check passed. delay=2000ms, exec=0ms, since success=4.0071s, since fail=0s
from odbc:MarketflowAcc: check passed. delay=2000ms, exec=0ms, since success=6.0083s, since fail=0s
```

## Support Me

Hi, I'm Finlay Beaton ([@ofbeaton](https://github.com/ofbeaton)). This software is made possible by donations of fellow users like you, encouraging me to toil the midnight hours away and sweat into the code and documentation. 

Everyone's time should be valuable, please consider donating.

[Donate through Paypal](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=RDWQCGL5UD6DS&lc=CA&item_name=ofbeaton&item_number=dbping&currency_code=CAD&bn=PP%2dDonationsBF%3abtn_donate_LG%2egif%3aNonHosted)

## License

This software is distributed under the MIT License. Please see [License file](LICENSE) for more information.
