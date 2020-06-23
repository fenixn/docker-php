# docker-php
PHP Development Environment with Docker
=======
# Docker-Compose - PHP, NGINX, MySQL, PHPMyAdmin

This is a docker server setup for a PHP development environment with NGINX, MySQL and PHPMyAdmin. 

## Setup
1. Rename .env.sample to .env, and change the settings accordingly.
2. Make sure you set the USER_ID and GROUP_ID correctly. You want to set these values to match your current host ids so that you can edit files on the host computer and have the container be able to access them as well. To see your host id, open your terminal and type:
    ```
    id
    ```
    Look for the uid and gid number, and enter those numbers into your env file. 
    Warning! A mistake here will cause a lot of file permission errors.
3. Create the server containers with docker compose:
    ```
    docker-compose up
    ```
4. You should now be able to reach the site at http://localhost. However, the www folder is empty and you must drop your site files in there. You can create a test index.html or index.php in the www folder to test the server before moving on.
