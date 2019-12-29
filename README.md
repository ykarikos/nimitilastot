# Nimitilastot

Search data from Finnish names.
Source data: [Name data](https://www.avoindata.fi/data/fi/dataset/none) from [Population Information System](https://vrk.fi/en/population-information-system) (Väestötietojärjestelmä).
Source data license: [CC-BY](https://creativecommons.org/licenses/by/4.0/).

# Initialize local database

1. Create database
	```
	docker pull mariadb
	docker network create mariadb-net
	docker run --network mariadb-net --name nimitilastot-mariadb -v $PWD/mariadb-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=nimiadmin -d mariadb:latest
	docker exec -i nimitilastot-mariadb mysql -hlocalhost -u root -pnimiadmin <create-tables.sql
	```
1. Import data
	```
	for f in `ls -1 etunimitilasto-2019-08-07-vrk`; do docker run -it -v "$PWD/etunimitilasto-2019-08-07-vrk:/nimitilasto" -w "/nimitilasto" --network mariadb-net --rm mariadb mysqlimport --ignore-lines=1 -hnimitilastot-mariadb --fields-terminated-by=\; --local -u root -pnimiadmin nimitilastot $f; done
	```

## Run Mysql command line

```
docker run -it --network mariadb-net --rm mariadb mysql -hnimitilastot-mariadb -u root -pnimiadmin nimitilastot
```

or

```
docker exec -it nimitilastot-mariadb mysql -hlocalhost -u root -pnimiadmin nimitilastot
```
