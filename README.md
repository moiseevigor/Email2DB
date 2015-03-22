# Email2DB

[![Join the chat at https://gitter.im/moiseevigor/email2db-php](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/moiseevigor/email2db-php?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

**ATTENTION! Work in progress, no working release yet!**

Email2DB parses the email schema into the relational schema.

## Installation procedure

We assume the Ubuntu 12.04 and later here on.

### System-wide installation procedure

Install IMAP extension

```
sudo apt-get install php5-imap
```

Enable IMAP extensions (creates symlink in `/etc/php5/cli/conf.d/`)

```
sudo php5enmod imap
```

Mailparse installation

```
sudo apt-get install php5-dev
```

Compiling `mailparse`

```
sudo pecl install mailparse
```

And enabling it

```
echo "extension=mailparse.so" > /etc/php5/mods-available/mailparse.ini
```

```
sudo php5enmod mailparse
```

### Project-wide installation procedure

Install composer

```
curl -sS https://getcomposer.org/installer | php
```

### Configure database

For MySQL configuration on Ubuntu older 14.04

```
sudo apt-get install php5-mysqlnd
```

and find the `pdo_mysql` module in the running config, the essential to Doctrine

```
$ php -m| grep -i mysql
mysql
mysqli
mysqlnd
pdo_mysql
```

### Install dependences

Next install all necessary packages

```
./composer.phar install
```

No errors should be in the output, if there appear some, please check the previous step.

Now check out the configuration

```
$ ./composer.phar show -p | grep -E "imap|mail"
ext-imap            0        The imap PHP extension
ext-mailparse       2.1.6    The mailparse PHP extension
ext-mysql           1.0      The mysql PHP extension
ext-mysqli          0.1      The mysqli PHP extension
ext-mysqlnd         0        The mysqlnd PHP extension
ext-pdo_mysql       1.0.2    The pdo_mysql PHP extension
```

### Configure Doctrine

```
$ vendor/bin/doctrine orm:schema-tool:create

ATTENTION: This operation should not be executed in a production environment.

Creating database schema...
Database schema created successfully!
```

## Dependences

Here we describe our decision on dependences

```
cat composer.json

...
  "require": {
    "php": ">=5.3.0",
    "phing/phing" : "2.*",
    "pear-pear.php.net/pear": "*",
    "pear/System_Daemon": "*@dev",
    "pear/pear_exception": "*@beta",
    "pear/Log": "*",
    "exorus/php-mime-mail-parser": "dev-master",
    "doctrine/orm": "*",
    "ext-imap": "*"
  },
...
```

### ORM :: Doctrine
For the database interaction we choose [Doctrine](http://doctrine-orm.readthedocs.org/), we find it to be the most reliable and the most flexible version of open ORM. 
It can intercat with variety of RDBMS and NoSQL databases and can exploit tailable capabilities of some databases like [MongoDB](https://github.com/doctrine/doctrine-mongodb-odm-tailable-cursor-bundle).

### PEAR :: System Daemon
The [PEAR :: System Daemon](http://pear.php.net/package/System_Daemon) library is used to run Email2DB as a daemon in background.

### PECL :: Mailparse
[PECL :: Mailparse](http://pecl.php.net/package/mailparse) is an extension for parsing and working with email messages. It can deal with RFC822 and RFC2045 (MIME) compliant messages.

### PHP :: IMAP
[PHP :: IMAP](http://php.net/manual/en/book.imap.php) is the most comprehensive PHP library to operate with the IMAP protocol, as well as the NNTP, POP3 and local mailbox.

### PHP Mime Mail Parser
[PHP Mime Mail Parser](https://code.google.com/p/php-mime-mail-parser/) is used to parse the Mime messages in the version maintained by [@eXorus](https://github.com/eXorus/php-mime-mail-parser).

## Contributing

Please clone, star and pull request. Please follow the [PSR coding style](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader-examples.md).
