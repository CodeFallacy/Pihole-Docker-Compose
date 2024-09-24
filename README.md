# Pihole-Docker-Compose

This is a `docker-compose.yml` file for Pi-hole using `macvlan` network driver to manually assign an LAN ip to our Pi-hole docker container and free up the host's port 80. You can follow the video tutorial here: https://youtu.be/qMNMQkGUQkk

> [!IMPORTANT]  
> This will only work with a wired ethernet connection!

```yml
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    networks:
      pihole_network:
        ipv4_address: '192.168.1.149' #update, assign open ip manually
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "80:80/tcp"
    environment:
      TZ: 'America/New_York' # update according to your timezone
      WEBPASSWORD: 'choose secured password' #update password
    volumes:
      - '/home/username/pihole/etc-pihole:/etc/pihole' #update
      - '/home/username/pihole/etc-dnsmasq.d:/etc/dnsmasq.d' #update
    cap_add:
      - NET_ADMIN
    restart: unless-stopped

networks:
  pihole_network:
    driver: macvlan
    driver_opts:
      parent: eth0 # eth0 will need to be replaced by name of your ethernet network interface
    ipam:
      config:
        - subnet: 192.168.1.0/24 #update if needed
          gateway: 192.168.1.1 #update if needed

```
