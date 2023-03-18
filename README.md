# xteve, guide2go in one docker with cron 

Thanks to the work of alturismo I just took what he did and rebuilt the binaries for the Raspberry Pi

This latest image, available on Docker Hub is built for 64bit ARM, so I guess thats RPi3, 4, and Zero2 - I've only tried it on an RPi 4

The rest of this readme is his documentation:

docker runs in host mode \
access xteve webui ip:34400/web/

after docker start check your config folder and do your setups, setup is persistent, start from scratch by delete them

cron and xteve start options are updated on docker restart.

mounts to use as sample \
Container Path: /root/.xteve <> /mnt/user/appdata/xteve/ \
Container Path: /config <> /mnt/user/appdata/xteve/_config/ \
Container Path: /guide2go <> /mnt/user/appdata/xteve/_guide2go/ \
Container Path: /tmp/xteve <> /tmp/xteve/ \
Container Path: /TVH <> /mnt/user/appdata/tvheadend/data/ << not needed if no TVHeadend is used \
while /mnt/user/appdata/ should fit to your system path ...

```
docker run -d \
  --name=xteve_guide2go \
  --net=host \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  -e TZ="Europe/Berlin" \
  -v /mnt/user/appdata/xteve/:/root/.xteve:rw \
  -v /mnt/user/appdata/xteve/_config/:/config:rw \
  -v /mnt/user/appdata/xteve/_guide2go/:/guide2go:rw \
  -v /tmp/xteve/:/tmp/xteve:rw \
  -v /mnt/user/appdata/tvheadend/data/:/TVH \
  alturismo/xteve_guide2go
```

setup guide2go SD subscrition as follows or copy your existing .yaml files into your mounted /guide2go folder \
docker exec -it "dockername" guide2go -configure /guide2go/"your_epg_name".yaml

to test the cronjob functions \
docker exec -it "dockername" ./config/cronjob.sh

included functions are (all can be individual turned on / off)

xteve - iptv and epg proxy server for plex, emby, etc ... thanks to @marmei \
website: http://xteve.de \
Discord: https://discordapp.com/channels/465222357754314767/465222357754314773

guide2go - xmltv epg grabber for schedules direct, thanks to @marmei \
github: https://github.com/mar-mei/guide2go \
Schedules Direct web: http://www.schedulesdirect.org/

some small script lines cause i personally use tvheadend and get playlist for xteve and cp xml data to tvheadend

plex and/or emby direct epg update after guide2go update (no wait for scheduled tasks, trigger more often, ...)

so, credits to the programmers, i just putted this together in a docker to fit my needs
