# spark-app-example

# Main Goal

Create a local but iso to production environnement to be as autonomous as possible while working on spark projects.

# How ?

this project contain all needed configuration files to create : 

- [X] Dockerized environnement
- [X] Local but a real distributed environnement
    - 1 Namenode
    - 1 Datanode (to increase as you wish)
    - Yarn resource manager
    - 3 Yarn node managers
    - Yarn hitory server
    - Spark history
    - Spark shell
- [X] Line up with exact Hadoop components version on production
- [X] Deployment to dockerized cluster via sbt command line
- [X] Mount data to hdfs via docker volumes from withing project folder
- [X] Access spark history webui for inspection :)
- [X] Access Yarn logs for debugging :)
- [X] Access to Spark shell for fiddling :)

# prerequisite

## add these localhost aliases to /etc/hosts
```bash
echo "127.0.0.1       namenode datanode resourcemanager nodemanager nodemanager-1 nodemanager-2 nodemanager-3 historyserver spark-master spark-worker spark-history" >> /etc/hosts
```

# how to run

```bash
# start up the cluster if already has been built
docker-compose up -d
```
![cluster docker containers](./images/docker-compose-up.png "cluster docker containers")

# Load data into hdfs

```bash
# Load dev data placed in the data directory into hdfs
docker exec -it namenode bash /scripts/hdfs-loader.sh
```
![Hdfs data load](./images/hdfs-data-load.png "Hdfs data load")

# Run spark job in the cluster via sbt

```bash
sbt
;clean;reload;compile;docker;dockerComposeUp
```
![Sbt run](./images/sbt-run.png "Sbt run")

# Run Spark shell connected to yarn cluster

```bash
docker exec -it spark-shell /spark/bin/spark-shell
```
![Spark shell](./images/spark-shell.png "Spark shell")

# Check Yarn history

```bash
chrome|firefox http://localhost:8188
```
![Yarn hisotry](./images/yarn-application-log.png "Yarn hisotry")

# Check Spark history

```bash
chrome|firefox http://localhost:18080
```
![Spark history](./images/spark-history.png "Spark history")

# Check hadoop hdfs namenode

```bash
chrome|firefox http://localhost:9870
```
![hdfs-ui](./images/hdfs-ui.png "hdfs-ui")

# stop, remove, clean volumes and network of all cluster

```bash
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q) && docker volume prune -f && docker network prune -f
```