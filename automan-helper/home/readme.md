Following these instructions



Run this stuff in a single docker container running on a pi

homeassistant
mosquitto (port: 1883, user:1000)
node-red (port: 1880) 192.168.99.192:3000

mqtt-bridge (port: 8080) (remove this one?)

influxdb (port: 8086)
graphana (port: 3000) 192.168.99.192:3000


organizr (port: 80, 443)
dockermon (port: 8126)

portainer (port: 9000)


see /opt for persistant location of docker files

Built out using a docker-compose
Fixups needed in the docker compose file
1) change the ver



https://www.reddit.com/r/homeassistant/comments/895iw6/my_home_assistant_setup_rpi_3b_docker_compose/




How to connect Portainer to a remote docker is a mystery
- is the port not open on the pi?
- is the port not exposed on the docker container?
- Is there a path from the pi into the docker container?



You'll need this tip
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

Launching one at a time...

docker-compose up -d influxdb
docker-compose up -d mosquitto
docker-compose up -d homeassistant
docker-compose up -d portainer
docker-compose up -d node-red
touch /opt/grafana/grafana.ini (first time only)
docker-compose up -d grafana

-- Connect Home Assistant to write to InfluxDB
-- Connect Grafana read from InfluxDB

-- Connect Home Assistant up to listen to MQTT

-- Connect Node-RED to Home Assistant


Hacking to fix stuff

InfluxDB

create a docker volume
docker volume create --name influxdb

/var/lib/influxdb

Create a default config file
'''
docker run --rm influxdb influxd config > influxdb.conf
'''

modify to use it

'''
docker run -p 8086:8086 \
      -v $PWD/influxdb.conf:/etc/influxdb/influxdb.conf:ro \
      influxdb -config /etc/influxdb/influxdb.conf
'''

docker run --rm influxdb influxd config > ~/Documents/Kitematic/influxdb/var/lib/influxdb/influxdb.conf

Create the homeassistant database 

docker run --rm -v ~/Documents/Kitematic/influxdb/var/lib/influxdb/influxdb.conf:/var/lib/influxdb/influxdb.conf -v ~/Documents/Kitematic/influxdb/var/lib/influxdb:/var/lib/influxdb -e INFLUXDB_DB=homeassistant influxdb -config /var/lib/influxdb/influxdb.conf /init-influxdb.sh

Docker volume to influxdb
/var/lib/influxdb/
    / 
    /data
    /meta
    /wal


my local volume LOCAL_VOLUME
/opt
LOCAL_VOLUME = /Users/dklaussen/Documents/Kitematic/

"remote" volume in the docker container
DOCKER_VOLUME == /var/lib


/influxdb/influxdb.conf

-v == volume
-e == set environment variable

$ docker run --rm 
-v /Users/dklaussen/Documents/Kitematic/influxdb/var/lib/influxdb/influxdb.conf:/var/lib/influxdb/influxdb.conf 
-v /Users/dklaussen/Documents/Kitematic/influxdb/var/lib/influxdb:/var/lib/influxdb 
-e INFLUXDB_DB=homeassistant 
influxdb -config /var/lib/influxdb/influxdb.conf /init-influxdb.sh



docker run --rm \
-v /Users/dklaussen/Documents/Kitematic/influxdb/var/lib/influxdb/influxdb.conf:/var/lib/influxdb/influxdb.conf \
-v /Users/dklaussen/Documents/Kitematic/influxdb/var/lib/influxdb:/var/lib/influxdb \
-e INFLUXDB_DB=homeassistant \
influxdb -config /var/lib/influxdb/influxdb.conf /init-influxdb.sh


docker run --rm 
-v /Users/dklaussen/Documents/Kitematic/influxdb/var/lib/influxdb/influxdb.conf:/var/lib/influxdb/influxdb.conf 
-v /Users/dklaussen/Documents/Kitematic/influxdb/var/lib/influxdb:/var/lib/influxdb 
-e INFLUXDB_DB=homeassistant 
-e INFLUXDB_ADMIN_USER=admin 
-e INFLUXDB_ADMIN_PASSWORD=SuperStrongPasswordGoesHere 

influxdb -config /var/lib/influxdb/influxdb.conf /init-influxdb.sh
