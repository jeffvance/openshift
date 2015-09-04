## Setting Up and Validating Containerized MySQL:

First, OSE's Security Context Constraints (SSC) need to be defined such that the *seLinuxContext* and *runAsUser* values are set to "RunAsAny". Selinux is still enabled/enforcing on the master and worker-node hosts, as decribed in the [OSE environment readme](ENV.md).

Our mysql example uses the official mysql image found [here](https://hub.docker.com/_/mysql/). First, pull down the image on each of your OSE worker nodes:
```
docker pull mysql
```

Next, test to ensure that mysql can be run from a containers:
```
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=foopass -d mysql
```
Note: if you re-run the above you will first need to remove the archived container so that the container name, "mysql", can be reused:

```
$ docker ps -a
$ docker rm <mysql-container-ID>

# to remove all containers:
$ docker rm $(docker ps -a)
```

Shell into the mysql container and run mysql:
```
$ docker exec -it <mysql-container-ID> bash
bash# mysql -p  # -p needed since a root password was supplied above
mysql> show datbases;
mysql> quit
bash# exit
```

Delete the mysql container so we can create it from a pod or re-run docker to create it later:
```
$ docker rm <mysql-container-ID>
```