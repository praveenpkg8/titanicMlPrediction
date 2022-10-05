# Pyspark boilerplate 

I wanted to practice data analysation and ETL stuffy using PySpark. But setting up PySpark was a troublesome one. I thought of getting it to a docker container and exposing the port and trying to run it in my local.
I faced much endurance during the process of bringing up the boilerplate setup. I thought of recording the problems that I faced as a blog that can be revisited at any point in time. 

Problems I faced:
1. What would be the docker image I have to use? Do I have to install all spark-related stuff myself manually?
2. How would I use the Jupyter notebook environment outside of docker for my use case?
3. What are the ways to reflect the changes that I make inside the container to get reflected in the local notebook?
4. So many steps to run the notebook. Could it be much simpler?

All the answers to the above 
questions were achieved through googling but representing the thought process while I was trying to solve the questions

## 1. What would be the docker image I have to use? Do I have to install all spark-related stuff myself manually?
I googled how to run a PySpark Jupiter notebook from the docker container. This step suggested using `jupyter/pyspark-notebook`
Docker image with a few extra flags to run the notebook inside and expose the port outside.
```
docker run -p 8888:8888 jupyter/pyspark-notebook
```

## 2. How would I use the Jupyter notebook environment outside of docker for my use case?
There is a concept called port forwarding which would expose a particular port to be accessed from outside the container. Using this functionality of docker we can expose the port of the jupyter notebook running outside of the container.

```
docker run -p 8888:8888 jupyter/pyspark-notebook

In this -p 8888:8888 would expose the 8888 port of the docker container to host machine 8888 port
```

## 3. What are the ways to reflect the changes that I make inside the container to get reflected in the local notebook?
The changes implemented inside of the docker container didn't reflect outside the local machine directory. Googling this resulted in the volume mount concept of docker. Which can reflect the changes made inside of the docker container to the local machine repository.

```
docker run -p 8888:8888 -v $(pwd):/home/jovyan jupyter/pyspark-notebook
```
-v flag would help to mount your local directory to the container directory. Running using this flag would help to reflect the changes made to the jupyter notebook.

## 4. So many steps to run the notebook. Could it be much simpler?
Achieving the above three results involved many steps I didn't want to repeat this step whenever I wanted to work with the PySpark environment. Docker compose would help make the nested docker commands into a single command.
docker-compose.yml
```
version: '3'
services:
    pyspark-env:
        image: jupyter/pyspark-notebook
        ports:
            - 8888:8888
        volumes:
            - ./:/home/jovyan
```


These are the few questions that were raised when I wanted to bring the PySpark env using docker.

Thanks for reading feel free to share your feedback