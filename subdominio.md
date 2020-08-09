Creación de Dominios, Subdominios y Certificados Digitales SSL
======

### Crear Proyecto en carpeta publicadora

Necesita acceder a la carpeta publicadora de Apache
```bash
cd /var/www/
```

Luego dentro de la carpeta publicadora crear una carpeta con el nombre del dominio o subdominio al cual se referenciará el proyecto
```bash
mkdir minraura.overprimegroup.net
```

Luego necesita dar permisos de lectura y escritura a la carpeta creada
```bash
sudo chown -R www-data:www-data  minraura.overprimegroup.net
sudo chmod 755 -R   minraura.overprimegroup.net
```

### Crear Virtual Host(Dominio / Subdominio)

Necesita acceder a la carpeta donde se almacenan  los virtual host
```bash
cd /etc/apache2/sites-available
```
Ahora debe crear el host virtual para su proyecto, copiando un archivo de nombre template que se encuentra en la carpeta
```bash
cp template minraura.overprimegroup.net.conf 
```
Luego debe ingresar al archivo creado
```bash
nano minraura.overprimegroup.net.conf
```
Contenido del Archivo
```bash
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        <Directory /var/www/html/>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        <IfModule mod_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

</VirtualHost>
```
Ahora debe reemplazar con el siguiente contenido:
```bash
<VirtualHost *:80>
        ServerName  minraura.overprimegroup.net
        ServerAlias www.minraura.overprimegroup.net
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/minraura.overprimegroup.net

        <Directory /var/www/minraura.overprimegroup.net/>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        <IfModule mod_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>
</VirtualHost>
```
Nota:
 * Los cambios son los atributos ServerName, ServerAlias.
 * Se referencia la carpeta del proyecto en el atributo DocumentRoot
 * Para poder navegar en el archivo utilice las teclas de dirección.
 * Para guardar el archivo presione control + o (letra o).
 * Para salir del archivo presione control + x.
 
Ahora debe registrar el Virtual Host Creado
```bash
sudo a2ensite  minraura.overprimegroup.net.conf
```

Luego le pedira reiniciar el servicio de apache, utilice el siguiente comando:
```bash
service apache2 restart
```

## Crear Certificado Digital

Registrar Certficado Digital de Dominio y Subdominio
```bash
sudo certbot --apache -d minraura.overprimegroup.net.conf
```
Luego  de ejecutar ese comando se generará el certificado y le pedirá que confirme como desea que cargue el certificado:
* 
*


