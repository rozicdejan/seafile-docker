# seafile-docker
Seafile is an open source file sync and share platform, focusing on reliability and performance. Seafile's built-in collaborative document SeaDoc, make it easy for collaborative writing and publishing documents.
# check for user ID and group ID
Since Syncthing is downloading and transferring files from your system, it must run under the user who will own the files you are accessing.
You can get your current user’s ID using the “id” command, as shown below.


     id -u
     id -g
The result  shows our group ID and  User ID
# docker-comit.yml
 
    services:
    db:
      image: mariadb:10.11
      container_name: seafile-mysql
      environment:
        - MYSQL_ROOT_PASSWORD=biker111
        - MYSQL_LOG_CONSOLE=true
        - MARIADB_AUTO_UPGRADE=1
      volumes:
        - ./mysql-data/db:/var/lib/mysql
      networks:
        - seafile-net
  
    memcached:
      image: memcached:1.6.18
      container_name: seafile-memcached
      entrypoint: memcached -m 256
      networks:
        - seafile-net
  
    seafile:
      image: seafileltd/seafile-mc:11.0-latest
      container_name: seafile
      ports:
        - 8900:80
        - "443:443" #Uncomment if you are using HTTPS
      volumes:
        - ./seafile-data:/shared
      environment:
        - DB_HOST=db
        - DB_ROOT_PASSWD=biker111
        - TIME_ZONE=Australia/Hobart
        - SEAFILE_ADMIN_EMAIL=drozic1989@gmail.com
        - SEAFILE_ADMIN_PASSWORD=biker111
        - SEAFILE_SERVER_LETSENCRYPT=false
        #- SEAFILE_SERVER_HOSTNAME=<HOSTNAME>
      depends_on:
        - db
        - memcached
      networks:
        - seafile-net
  
    networks:
      seafile-net:
