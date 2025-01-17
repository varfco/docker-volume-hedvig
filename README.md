[![logo](http://www.hedviginc.com/hs-fs/hub/448929/file-2245740934-png/Website_Pictures/hedvig_logo_260x77.png?t=1494939093745&width=390&name=hedvig_logo_260x77.png)](http://www.hedviginc.com)


# Hedvig Docker Volume Plugin

Follow these steps to create hedvig plugin from scratch
* cd plugin/install/docker-volume-hedvig  
docker build -t rootfsimage  
docker create rootfsimage true  
// Get image_id obtained from command above  
sudo docker export <image_id> | sudo tar -x -C rootfs/  
docker rm -vf <image_id>  
docker rmi rootfsimage  

* Copy over few directories expected by the plugin  
cd plugin/install  
cp -r rootfs/* docker-volume-hedvig/rootfs/  

* Validation:  
Make sure plugin/install/docker-volume-hedvig/rootfs/etc/hedvig directory exists 

* Creating plugin  
cd plugin/install/  
docker plugin create hedvig/hedvig-volume:v-1.0 docker-volume-hedvig  

* Enabling plugin:  
docker plugin enable hedvig/hedvig-volume:v-1.0 

* Test steps:  
docker volume create -d hedvig/hedvig-volume:v-1.0 --name testhedvigvolume  
docker run  -dt --volume-driver hedvig/hedvig-volume:v-1.0 --volume testhedvigvolume:/data ubuntu sh  
docker exec -it <container_id> /bin/bash  
* Make sure a mount point like this exists:  
<hedvigproxy>:/exports/testhedvigvolume        10G     0   10G   0% /data

