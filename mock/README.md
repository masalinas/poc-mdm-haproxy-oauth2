# build docker image
docker build -t consum-mock .

# execute docker container
docker run --name consum-mock --rm --net consum -p 8088:8088 consum-mock