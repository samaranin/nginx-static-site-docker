# Serve static sites with Docker and Nginx  
## How to use  
Clone this repository, place your website to `/code` folder inside cloned repo and run 
```
$: docker-compose up -d
```  


## Files description   
### docker-compose.yaml   

```yaml
services:
    web:
        image: nginx:latest
        restart: always
        container_name: default.container
        # ports:
        #   - "80:80"
        volumes:
            - ./code:/code
            - ./nginx.conf:/etc/nginx/conf.d/default.conf
        environment:
            VIRTUAL_HOST: site.com,www.site.com
            LETSENCRYPT_HOST: site.com
            LETSENCRYPT_EMAIL: admin@site.com
        expose:
            - 80
            - 443

networks:
  default:
    external:
      name: nginx-proxy
```  
- `ports: - "80:80"` - uncomment this section to use port forwarding (not recomended when using with [nginx-proxy](https://github.com/samaranin/nginx-proxy) service)   

- `VIRTUAL_HOST, LETSENCRYPT_HOST, LETSENCRYPT_EMAIL` - environments parameters to use with [nginx-proxy](https://github.com/samaranin/nginx-proxy) service (more information find there).  

- `networks: default: external: name: nginx-proxy` - external network to user with  [nginx-proxy](https://github.com/samaranin/nginx-proxy) service (more information find there).   

### nginx.conf   

```nginx
server {
    index index.php index.html;
    server_name _;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /code;
}
```
- `root /code;` - change root folder for site hosting (also needs to be change in `volume` section of docker-compose)