# Network controller and Pihole on docker containers on Raspberry PI
Setting up my home network

Setting Unifi controller and Pihole on a docker container on Raspberry PI. I have this running on a Pi 2.

1. Install PI

- Get [Imager](https://www.raspberrypi.org/blog/raspberry-pi-imager-imaging-utility/) and install Raspbian
- open ssh
- set static IP

    edit the file dhcpcd.conf:
    ``` 
    sudo nano /etc/dhcpcd.conf
    ``` 
    uncomment the eth0 interface
    ``` 
        interface eth0
        static ip_address=<PRIVATE_IP_ADDRESS_HERE>
        static routers=<ROUTER_IP_HERE>
    ``` 

2. Install docker

    ``` 
        curl -fsSL https://get.docker.com -o get-docker.sh
        sh get-docker.sh
    ``` 

    add pi to the docker group
    ``` 
    sudo usermod -aG docker pi
    ``` 

3. Install pihole

    ``` 
	docker run -d --name pihole -p 53:53/tcp -p 53:53/udp -p 80:80 -p 443:443 -e TZ="Europe/Lisbon" -v "/home/pi/pihole/etc-pihole/:/etc/pihole/" -v "/home/pi/pihole/etc-dnsmasq.d/:/etc/dnsmasq.d/" --dns=<ROUTER_IP_HERE> --dns=8.8.8.8 --restart=unless-stopped --hostname pi.hole -e VIRTUAL_HOST="pi.hole" -e PROXY_LOCATION="pi.hole" -e ServerIP="<PRIVATE_IP_ADDRESS_HERE>" -e WEBPASSWORD=<WEBPASSWORD> pihole/pihole:latest
    ``` 

    To reset the pihole password:
    -  get the container list and the container id
        ``` 
        docker ps
        ``` 
    - change the password
        ``` 
 	    docker exec <CONTAINER_ID> sudo pihole -a -p <NEW_PASSWORD>
        ``` 
 
 4. Install Unifi Controller Controller

    Create a directory to hold the data files:
    ``` 
    mkdir /home/pi/unifi/
    ``` 
    ``` 
	docker run -d --name unifi -p 5514:5514/udp -p 3478:3478/udp -p 10001:10001/udp -p 6789:6789 -p 8080:8080 -p 8880:8880 -p 8443:8443 -p 8843:8843 -e TZ="Europe/Lisbon" -v /home/pi/unifi/:/unifi --restart=unless-stopped jacobalberty/unifi:6.0.45
    ``` 

5. Restore unifi configuration from previous controller

    Ensure the controller address points to the new IP address

6. Check if it's all running:

    Pihole:
    ``` 
    http://<PRIVATE_IP_ADDRESS_HERE>
    ``` 

    Unifi:
    ``` 
    https://<PRIVATE_IP_ADDRESS_HERE>:8443
    ``` 
