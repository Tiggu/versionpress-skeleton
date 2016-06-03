# VersionPress Skeleton
### Uses Docker workflow and Mark Jaquith's [WordPress Skeleton](https://github.com/markjaquith/WordPress-Skeleton)

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
