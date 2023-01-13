<p align="center">
	<img src="https://github.com/emaworkdev/hasura/blob/main/hasura.png" alt="Hasura-logo" width="100" />	
	<p align="center">Hasura-engine is a Open-source.</p>
</p>

# Hasura

<hr>

<details>
  <summary>Installing docker and docker-compose ubuntu 20.04</summary>

  ```bash
sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl status docker

sudo usermod -aG docker ${USER}
su - ${USER}
groups
sudo usermod -aG docker username


sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version


  ```


</details>

<details>
  <summary>Installing certbot</summary>

  ```bash
[sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl status docker

sudo usermod -aG docker ${USER}
su - ${USER}
groups
sudo usermod -aG docker username


sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version](https://certbot.eff.org/instructions?ws=nginx&os=ubuntubionic

sudo snap install core; sudo snap refresh core

sudo snap install --classic certbot

sudo certbot certonly --nginx

or

sudo certbot --nginx -d <dominio>

Renovando e Deletando certificados com o Certbot/Let’s Encrypt
Para renovar todos os seus certificados automaticamente:

sudo certbot renew --quiet
É importante colocar esse comando no seu crontab para ser executado diariamente:

1 1 * * * /usr/bin/certbot renew --quiet

To manually delete a certificate:

sudo certbot delete --cert-name nomedodominio.com)


  ```


</details>


### Cloning the project

```bash


git clone https://github.com/emaworkdev/hasura.git

cd hasura

docker-compose -d up

```
<hr/>
	
<details>
  <summary>Creating a nginx service for hasura</summary>

  ```bash

  sudo vim /etc/nginx/sites-available/<nome do serviço>
	
dentro do editor vim para entrar no modo de insert: pressionar a tecla shift+i

colar esse scrypt abaixo	
	
	server {
  listen 80;
  listen 443 ssl;
  server_name hasura.<my-domain.com>;

  location / {
    proxy_pass http://localhost:8080/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
  }
}

para sair do modo insert
pressionar a tecla ESC

entrada de comando
pressionar a tecla :

para salvar e sair
digitar wq + ENTER	
	
sudo service nginx restart && sudo service nginx reload && sudo nginx -t

sudo ln -s /etc/nginx/sites-available/<nome servico> /etc/nginx/sites-enabled/

sudo systemctl restart nginx.service

remover o serviço default que vem como default
sudo rm /etc/nginx/sites-enabled/default
sudo service nginx reload

comandos:
sudo systemctl enable nginx.service
sudo systemctl stop nginx.service
sudo systemctl start nginx.service
sudo systemctl restart nginx.service
	
	

  ```


</details>	


### License

[![License GNU AGPL v3.0](https://img.shields.io/badge/License-AGPL%203.0-lightgrey.svg)](https://github.com/sufficit/sufficit-quepasa-fork/blob/master/LICENSE.md)


