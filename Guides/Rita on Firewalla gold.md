# Firewalla gold 
Firewalla gold is already running  Bro/Zeek on the device. 
What we need to do to make Rita work on FIrewalla is install mongodb and Rita, 
Then point RIta to the Bro/Zeek logs using cron 

## Get Rita 

```Bash
git clone https://github.com/activecm/rita 
cd rita
sudo ./install.sh --disable-zeek --disable-mongo #This will only install Rita, zeek is already on the device and mongo will get installed using docker
```

## Get and install mongo as docker container
```Bash
sudo docker pull mongo:3.6 #Get docker image
sudo docker run --name mongo -p 27017:27017 -v /data/mongodata:/data/db  -d mongo:3.6 
```

Rita is now connected to mongo 

## Import zeek/bro logs using cron 
The task here is creating a cron job which will import logs files every hour into Rita. 
it important we import the logs rolling to get progressively

```Bash
crontab -e
#Add the following code to cron to import every hour
0 * * * * /usr/local/bin/rita import --rolling /log/blog/current$(date --date='-1 hour' +\%Y-\%m-\%d)/ Firewalla_logs

```