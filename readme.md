#### Practica 9 - Johan Erazo

### 1 Engade un DNS ó docker-compose (ou o que xa tes)
### Usa docker-compose para configura-las IP fixas ós dous contenedores

Configura o docker con todo o requerido. Coas súas imaxes, as ips, nomes do contedor e do cliente, coas suas volumes.

'''

services:
  web:
    container_name: Practica_F
    image: php:7.4-apache
    ports:
      - "80:80"
    volumes:
      - ./www:/var/www
      - ./confApache:/etc/apache2 
    networks:
      mi_red:
        ipv4_address: 198.172.1.23
  bind9:
    container_name: practi
    image: ubuntu/bind9
    platform: linux/amd64
    ports:
      - 55:53
    volumes:
      - ./confDNS/conf:/etc/bind
      - ./confDNS/zonas:/var/lib/bind
    networks:
      mi_red:
        ipv4_address: 198.172.1.24
      
networks:
  mi_red:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 198.172.0.0/16

'''

### 2. O DNS ten que resolver dous dominios á la ip do apache.

Para isto executa na terminal o comando (sudo nano /etc/systemd/resolved.conf) isto para poder abrir o ficheiro e editalo. Ubicamos onde pon '#DNS', eliminamos o '#' para activalo e colocamos a ip do contedor co porto, quedando da seguinte forma: DNS=192.172.1.24#55.

Isto para que ao correr o contedor o sistema poida resolver as ips que queremos crear.

Tamén é importante engadir correctamente na carpeta (ConfApache/sites-available) e (ConfApache/sites-enabled) coas páxinas que queremos cos seus portos.

Todas as configuracións feitas están adxuntas a este git.