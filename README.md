<p align="center">
	<img src="https://github.com/emaworkdev/hasura/blob/main/hasura.png" alt="Hasura-logo" width="100" />	
	<p align="center">Hasura-engine is a Open-source.</p>
</p>

# Hasura

<hr>

```bash
git clone https://github.com/emaworkdev/hasura.git

```
<hr/>


<details>
  <summary>Installing docker and docker-compose ubuntu 20.04</summary>

  ```bash
        sudo apt update

	sudo apt install apt-transport-https ca-certificates curl software-properties-common

	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

	sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

	apt-cache policy docker-ce

	sudo apt install docker-ce

	no servidor
	sudo systemctl status docker

	no wsl
	sudo service docker status
	sudo service docker start

	sudo usermod -aG docker ${USER}
	su - ${USER}
	groups
	sudo usermod -aG docker username


	sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	sudo chmod +x /usr/local/bin/docker-compose
	docker-compose --version


  ```



  ### **First step**

    Clone the repo 

    ```bash

  git clone https://github.com/sufficit/sufficit-quepasa-fork.git

    ```

  ### **Second step**

    Create Database and Users

  ```bash

  sudo mysql

  # create the user

  mysql> CREATE USER 'quepasa'@'%'IDENTIFIED BY 'S0me_RaNdoM_T3*T';

  # Granting Permition to the Quepasa User

  mysql> GRANT ALL ON quepasa.* TO 'quepasa'@'%';

  # Flushing the Privileges 

  mysql> FLUSH PRIVILEGES;

  # Create quepasa DataBase 

  mysql> CREATE DATABASE quepasa;

  # exit mysql 

  mysql> exit

  ```

  ### **Third step**

    Creating the Tables Required

    ```bash
  # cd into the cloned reop

  cd <git_clone_location>/src/migrations/

  #below will create the relevent tables in the quepasa database for you

  sudo mysql --database=quepasa < 1_create_tables.up.sql

    ```
  ### **Forth step**

  Creating the .env file

  ```bash
  # this file contains all the environment varibles that the system needed do the changes that matches your deployment

  #create the .env file in the below location

  nano <git_clone_location>/src/.env

  # content of the file should looklike this 

  WEBAPIHOST=0.0.0.0 
  WEBAPIPORT=31000 # web port of the API
  WEBSOCKETSSL=false # http or Https
  DBDRIVER=mysql #Databse Server
  DBHOST=localhost
  DBDATABASE=quepasa
  DBPORT=3306
  DBUSER=quepasa
  DBPASSWORD='S0me_RaNdoM_T3*T' #the string you created in the third step 
  DBSSLMODE=disable
  APP_ENV=development # this will write some extra debug messages you can change it to production if needed
  MIGRATIONS=false
  SIGNING_SECRET=5345fgdgfd54asdasdasdd #some random test this will be used for password encription 

  ```

  ### **Fifth step**

  Compiling the Packge

  ```bash
  # cd into the src directory

  <git_clone_location>/src/

  # compile using golang this may take few seconds to compile

  go run main.go

  ```
  if error occourd such as *"go not found"* please make sure to [export the path](#installing-golang) again


  ### **Final step**

  - go to http://your.ip.address:3100/setup in the web browser and register an admin user for your system
  - log in to the sysetm http://your.ip.address:3100 form previously created user and scan the qr using you whatsapp 






  ---



  ## Docker Implimentation

  ### Prerequisites

  For local development
  * docker
  * golang
  * postgresql

  ### Run using Docker

  * Add info about database migrations

  ```bash

  make docker_build
  # edit docker-compose.yml.sample to your hearts content
  docker-compose up
  ```

  ## HTTP API

  1. Use the `Accept: application/json` header
  2. `TOKEN` should be treated like a password.

  ### Get bot info

  A simple method for testing your bot's auth token. Requires no parameters. Returns basic information about the bot.

  **request**
  ```
  GET /bot/<TOKEN>/
  ```

  ***response***

  ```json
  {
      "id": "5454544554343@c.us",
      "user_id": "845ae4d0-f2c3-5342-91a2-5b45cb8db57c",
      "token": "8129c0b4-0b96-4486-84fc-c3dd7b03f846",
    "webhook" : "",
      "is_verified": true,
      "created_at": "2018-11-02T11:36:24.273Z",
      "updated_at": "2018-11-02T11:36:24.273Z"
  }

  ```

  ### Sending

  **request**
  ```
  POST /bot/<TOKEN>/send

  {
    "recipient": "+15555555552",
    "messsage": "Hello World!"
  }
  ```

  **response**
  ```json
  {
    "result": {
      "recipient": "+15555555551",
      "source": "+15555555552",
      "status": "sent",
      "timestamp": "1543420505142"
    }
  }
  ```

  ### Receive

  The "timestamp" query parameter is optional. A maximum of 40 messages per conversation will be returned.

  **request**
  ```
  GET /bot/<TOKEN>/receive?timestamp=1541265073783
  ```

  **response**
  ```json
  {
    "messages": [
      {
        "source": "+15555555551",
        "timestamp": "1541265073894",
        "message": {
          "body": "Hello World!",
          "profileKey": "XXTXQ=="
        }
      }
    ],
    "bot": {
      "id": "129f1757-e706-452e-aa1c-4994a95e1092",
      "number": "+15555555552",
      "user_id": "845ae4d0-f2c3-5342-91a2-5b45cb8db57c",
      "token": "8129c0b4-0b96-4486-84fc-c3dd7b03f846",
      "is_verified": true,
      "created_at": "2018-11-02T11:36:24.273Z",
      "updated_at": "2018-11-02T11:36:24.273Z"
    }
  }
  ```
  ### Environment Variables

  WEBAPIHOST:
  WEBAPIPORT:			"31000"				#
  WEBSOCKETSSL:
  DBDRIVER:			"mysql"
  DBHOST: 			"localhost"			#
  DBDATABASE:			"quepasa_dev"   	#
  DBPORT:				"5432"				#
  DBUSER:				"quepasa"			#
  DBPASSWORD:			"quepasa"			#
  DBSSLMODE:			"disable"			#
  APP_ENV:			"development"		#
  HTTPLOGS:			false				# Should log http requests ?
  MIGRATIONS:			false
  DEBUGREQUESTS:		true				#
  DEBUGJSONMESSAGES:	true				#
  SIGNING_SECRET:		"any secret here"	#
  TZ:					"America/Sao_Paulo"	#
</details>

### License

[![License GNU AGPL v3.0](https://img.shields.io/badge/License-AGPL%203.0-lightgrey.svg)](https://github.com/sufficit/sufficit-quepasa-fork/blob/master/LICENSE.md)

QuePasa is a free software project licensed under the GNU Affero General Public License v3.0 (GNU AGPLv3) by [Sufficit Soluções](https://www.sufficit.com.br).

[0]: https://whatsapp.com
[1]: https://github.com/tulir/whatsmeowz
