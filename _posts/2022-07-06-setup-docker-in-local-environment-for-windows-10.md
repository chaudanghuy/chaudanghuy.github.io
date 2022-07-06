## Setup Docker with PHP and MYSQL for web development in Windows 10

The guideline requires the basic knowledge of Docker so you need research what is Docker first.

Software:
[Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

---

### Steps:

#### Install Docker

![image](https://user-images.githubusercontent.com/1155091/177482681-1a709765-ece0-4132-b1e7-eb36fd29e647.png)

#### Configure Docker for PHP and MySQL environment

**Folder structure**

![image](https://user-images.githubusercontent.com/1155091/177482784-f587cc83-8d39-47db-8021-49761af9b75e.png)



- app\public-html: Main source code
- docker-compose.yml :  Configure running service
- Dockerfile : Command file to run the Docker

#### Configure Docker for PHP and MySQL environment

**docker-compose.yml**

```yml
version: '3.8'
services:
    www:
        build: .        
        depends_on:
            - db
        volumes:
            - ./app/public-html:/var/www/html/
        ports:
            - 8002:80
    db:        
        image: mysql        
        environment:
            MYSQL_ROOT_PASSWORD: test
            MYSQL_DATABASE: myDB
            MYSQL_USER: test
            MYSQL_PASSWORD: test
        ports:
            - "9906:3306"       
    phpmyadmin: 
        image: phpmyadmin/phpmyadmin
        ports: 
            - "8080:80"           
        environment:
            MYSQL_ROOT_PASSWORD: test            
            MYSQL_USER: test
            MYSQL_PASSWORD: test
        links: 
            - db:db            
```

**Dockerfile**

```yml
FROM php:8.0-apache
RUN docker-php-ext-install mysqli pdo_mysql
RUN apt-get update \
    && apt-get install -y libzip-dev \
    && apt-get install -y zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip
```

#### Create sample PHP script to test

**/public_html/index.php**

```php
<?php
// The MySQL service named in the docker-compose.yml.
$host = 'db';

// Database use name
$user = 'test';

//database user password
$pass = 'test';

// check the MySQL connection status
$conn = new mysqli($host, $user, $pass);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
} else {
    echo "Connected to MySQL server successfully!";
}
?>
```

#### Run Docker Compose

```command
docker-compose up --build
```

![image](https://user-images.githubusercontent.com/1155091/177483620-2b60b99c-65ff-4d87-a5b3-a9c8367a25eb.png)

#### Result

Open the browser and enter URL to see the result

![image](https://user-images.githubusercontent.com/1155091/177483637-d197ceae-9981-4e4a-a4ea-1ba7176e394c.png)
