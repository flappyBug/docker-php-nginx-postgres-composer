# docker-php-nginx-postgres-composer

PHP运行环境的Docker Compose配置。包含以下组件：
- web （最新的Nginx）
- php （PHP7.x with PHP-FPM）
- db （最新的PostgreSQL）
- composer
可以很容易地对以上组件的版本进行限定，其中php在`.docker/Dockerfile`中定制，其它组件在`docker-compose.yml`中。

已针对国内环境替换成国内源，加速构建容器。

# 如何使用

1. clone本仓库后，复制`.env.example`为`.env`，在其中填入相关的数据库参数
2. 运行`docker-compose up`，完毕。

Nginx侦听80端口，Postgres侦听5432端口。请确保这些端口未被占用。

php工程放在`./app`中，入口文件`./app/index.php`。

# 连接组件终端

`docker-compose exec [php|web|db] /bin/bash`

# 连接psql

`docker-compose exec db psql -U postgres`



## Change configuration

### Configuring PHP

To change PHP's configuration edit `.docker/conf/php/php.ini`.
Same goes for `.docker/conf/php/xdebug.ini`.

You can add any .ini file in this directory, don't forget to map them by adding a new line in the php's `volume` section of the `docker-compose.yml` file.

### Configuring PostgreSQL

Any .sh or .sql file you add in `./.docker/conf/postgres` will be automatically loaded at boot time.

If you want to change the db name, db user and db password simply edit the `.env` file at the project's root.

If you connect to PostgreSQL from localhost a password is not required however from another container you will have to supply it.

## Adding aliases

To avoid typing over and over again the same commands you can add two useful aliases in your shell's configuration (`.bashrc` or `.zshrc` for instance):

```
alias dcu="docker-compose up"
alias dcr="docker-compose run"
alias dce="docker-compose exec"
```

It then becomes way faster to execute a composer command for instance:

`dcr composer require --dev phpunit/phpunit`
