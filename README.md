<br />
<div align="center">
  <h3 align="center">Partiel Architecture Web</h3>

  <p align="center">
  	Commands and results
  </p>
</div>


## Redirections

* Générer le certificat auto-signé
  ```sh
  sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/monsite.local.key -out /etc/ssl/certs/monsite.local.crt
  ```

* Activer le module SSL
    ```sh
    sudo a2enmod ssl 
    sudo a2ensite default-ssl
    ```  
    [ssl](ssl.png)

* Dans le fichier de configuration (etc/apache2/sites-avalaibles/monsite.local.conf) 
  ```sh
    <VirtualHost *:80>
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
  [Redirect 301](redirect301.png)
  [Redirect 302](redirect302.png)

* Activer le site (Dans le VirtualHost 443)
  ```sh
  sudo a2ensite monsite.local.conf
  sudo systemctl reload apache2
  ```

* Logs séparés (Dans le VirtualHost 443)
  ```sh
  ErrorLog ${APACHE_LOG_DIR}/monsite.local-error.log
  CustomLog ${APACHE_LOG_DIR}/monsite.local-access.log combined
  ```

* Page 404 custom (Toujours dans le VirtualHost 443)
  ```sh
  ErrorDocument 404 /404.html
  ```
![curl 404](404.png)

