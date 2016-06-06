# VersionPress Skeleton
### Uses Docker workflow and Mark Jaquith's [WordPress Skeleton](https://github.com/markjaquith/WordPress-Skeleton)

Uses [this versionpress-ready Docker image](https://github.com/tatemz/docker-versionpress) based on the official WordPress image.

## 1. Clone the Repo
```
git clone git@github.com:tatemz/versionpress-skeleton.git ./wordpress --recursive
```

## 2. Start Docker

**With docker-compose:**

```
cd ./wordpress
docker-compose up
```

**Without docker-compose:**
```
cd ./wordpress
docker run -d --name="mydb" -e "MYSQL_ROOT_PASSWORD=root" mariadb
docker run -d --name="mywp" -p "8080:80" --link="mydb:mysql" -e "WORDPRESS_DB_PASSWORD=root" -v "$(pwd):/var/www/html" tatemz/versionpress
```

## 3. Setup Alias

**With docker-compose:**
```
alias docker-wp='docker-compose run --rm mywp wp'
```

**Without docker-compose:**
```
alias docker-wp='docker exec -it mywp wp'
```

## 4. Install WordPress

**First Installation**

```
docker-wp core install --url="localhost:8080/wp" --title="WordPress" --admin_user="admin" --admin_password="password" --admin_email="admin@email.com"
docker-wp plugin activate versionpress 
docker-wp vp activate 
```

**Restore Site With VersionPress**
```
docker-wp vp restore-site --siteurl="localhost:8080/wp" --require="content/plugins/versionpress/src/Cli/vp.php" --yes
```


## Notes

Not all versionpress commands and processes have been tested using this skeleton, but feel free to submit issues and pull requests to either this skeleton or the [docker image](https://github.com/tatemz/docker-versionpress).

Just like with Mark Jaquith's WordPress Skeleton, [local configuration](https://github.com/tatemz/versionpress-skeleton/blob/master/wp-config.php#L9) is stored inside of a `local-config.php` file.  Just like the official wordpress docker image, this `local-config.php` is configured and [created on launch](https://github.com/tatemz/docker-versionpress/blob/master/docker-entrypoint.sh#L66) of the versionpress docker image.

Additionally, the versionpress docker image, [downloads a specified version of a versionpress release](https://github.com/tatemz/docker-versionpress/blob/master/docker-entrypoint.sh#L27), and unzips it in the plugins directory.
