<br />
<div align="center">
  <h3 align="center">Partiel Architecture Web</h3>

  <p align="center">
  	Commands and results
  </p>
</div>



<!-- CC2 -->
## Redirections

* Générer le certificat auto-signé
  ```sh
  sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/monsite.local.key -out /etc/ssl/certs/monsite.local.crt
  ```

* Activer le module SSL
    ```sh
    sudo a2enmod ssl 
    sudo sudo a2ensite default-ssl
    ```  

* Dans le fichier de configuration (etc/apache2/sites-avalaibles/monsite.local.conf) 
  ```sh
  <VirtualHost *:80>
    # nom de domaine personnalisé
    ServerName monsite.local
    Redirect permanent / https://monsite.local/
  </VirtualHost>

  <VirtualHost *:443>
    ServerName monsite.local
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/monsite.local.crt
    SSLCertificateKeyFile /etc/ssl/private/monsite.local.key
  </VirtualHost>
  ```
* Redirection 301 et 302 (Dans le VirtualHost 443)
  ```sh
  RedirectMatch 301 ^/cours/(.*)$/ /formations/$1
  Redirect 302 /contact /nous-contacter
  ```

* Activer le site
  ```sh
  sudo a2ensite monsite.local.conf
  sudo systemctl reload apache2
  ```

* Logs séparés
  ```sh
  ErrorLog ${APACHE_LOG_DIR}/monsite.local-error.log
  CustomLog ${APACHE_LOG_DIR}/monsite.local-access.log combined
  ```


## Exercice 2

* Command
  ```sh
  mkdir -p projet/src projet/bin projet/doc
  mv src/main.c src/util.c bin/ && mv src/util.h doc/
  tree
  ```
* Output 
  ```sh
  .
  ├── bin
  │   ├── main.c
  │   └── util.c
  ├── doc
  │   └── util.h
  └── src
  ```

## Exercice 3
* Commmand
  ```sh
  echo Data Confidential > secure_data/data.txt
  chmod 700 data.txt
  sudo chown thomas data.txt
  sudo setfacl -m u:aclTest:r data.txt
  ls -l
  ```
* Output
  ```sh
  total 4
  -rwxr-----+ 1 thomas baka 18 févr. 26 10:24 data.txt
  ```
* Command
  ```sh
  getfacl data.txt
  ```
* Output
  ```sh
  file: data.txt
  # owner: thomas
  # group: baka
  user::rwx
  user:aclTest:r--
  group::---
  mask::r--
  other::---
  ```

## Exercice 4
* Command 
  ```sh 
  sudo apt install apache2
  sudo systemctl start apache2
  sudo systemctl enable apache2
  sudo systemctl status apache2
  ```
* Output 
  ```sh
  ● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2025-02-26 10:44:28 CET; 1min 58s ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 46799 (apache2)
      Tasks: 55 (limit: 4611)
     Memory: 6.2M
        CPU: 31ms
     CGroup: /system.slice/apache2.service
             ├─46799 /usr/sbin/apache2 -k start
             ├─46800 /usr/sbin/apache2 -k start
             └─46801 /usr/sbin/apache2 -k start

  févr. 26 10:44:28 baka-Standard-PC-Q35-ICH9-2009 systemd[1]: Starting The Apache HTTP Server...
  févr. 26 10:44:28 baka-Standard-PC-Q35-ICH9-2009 apachectl[46798]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
  févr. 26 10:44:28 baka-Standard-PC-Q35-ICH9-2009 systemd[1]: Started The Apache HTTP Server.
  ```

* Command 
  ```sh
  sudo systemctl restart apache2:q
  ```
![html](screeshotHtml.png)
