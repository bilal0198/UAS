# Docker:
     Installation: Ensure Docker and Docker Compose are installed on your system. If not, download and install them from the official Docker website.
# OpenSSL:
     Installation: Verify that OpenSSL is installed on your system. If not, install it using your system's package manager (e.g., apt, yum, brew).
## Project Structure:
     Create a Project Directory:
    Set up a new project folder on your local machine.
    Create an Nginx Configuration Directory:
    Within your project directory, create a folder named nginx-config.
## Create the Nginx Configuration File:
    Inside the nginx-config folder, create a file named default.conf. This file will hold the Nginx configuration settings
# CODE
    server {
    #HTTP/1.1 listen on port 80
    listen 80;
    #HTTP/2 listen on port 443 for SSL
    listen 443 ssl;
    http2 on;
     #OpenSSL
    ssl_certificate /etc/nginx/certs/server.crt;
    ssl_certificate_key /etc/nginx/certs/server.key;
    ssl_protocols TLSv1.3;
    ssl_prefer_server_ciphers off;
    #Security headers
    add_header X-Content-Type-Options nosniff;
    #Root directory of website on linux os
    root /var/www/;
    index index.html;
    }
## Save
# Generating SSL Certificates
    Navigate to the Project Root:
    Open your terminal or command prompt and navigate to the root directory of your project.
## Run the command
  ## CODE
    openssl req -x509 -newkey rsa:2048 -keyout ssl/server.key -out ssl/server.crt -days 365 -nodes
    Fill in the answers to the questions as desired
# Landing Page
## Create a fileindex.html
### Enter the following code
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTTP1 dan HTTP2</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@300..700&family=Roboto+Slab:wght@100..900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Fira Code', 'Arial Narrow', Arial, sans-serif, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>HTTP1 dan HTTP2</h1>
    <p>HTTP1 dan HTTP2 adalah protokol yang digunakan untuk mengakses data dari server.</p>
    <br>
    <h2>Pembuktian menggunakan Protokol HTTP/1 dan HTTP2</h2>
    <ol>
        <li>Gunakan browser Chrome</li>
        <li>Klik Kanan pada sembarang tempat -> Pilih <strong>Inspect</strong></li>
        <li>Pilih Tab <strong>Network</strong></li>
        <li>Refresh halaman ini</li>
        <li>Lihat baris request halaman ini (localhost)</li>
        <li>Lihat bagian kolom <strong>Protocol</strong></li>
        <li>Jika tidak ada kolom <strong>Protocol</strong>, klik kanan pada salah satu kolom -> <strong>Check Protocol</strong></li>
        <li>HTTP1 -> Protokol:HTTP1</li>
        <li>HTTP2 -> Protokol:h2</li>
    </ol>
    </body>
    </html>
![](https://github.com/bilal0198/UAS/blob/8141f68be65266483ed61ee650c643abdbc6c4a5/README/http1.png)
# Docker Compose
Create a filedocker-compose.yml
## Enter the following code
    version: '3.8'
    services:
    web:
    image: nginx:alpine
    ports:
      - "8001:80" # HTTP/1.1
      - "443:443" # HTTP/2
    volumes:
      - ./nginx-config:/etc/nginx/conf.d
      - ./ssl:/etc/nginx/certs
      - ./index.html:/var/www/index.html
![http2.png](http2.png)
### Save
# Test
## Enter the directory{root_project}/
### Open terminal
     Run the commanddocker-compose up
# Open Chrome browser, access localhost on port 8001 localhost:8001
    Follow the instructions from the website
    Protokol HTTP/1
![](wireshsrk1.png)
![](wiresharkhttp2.png)
