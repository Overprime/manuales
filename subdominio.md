Creación de dominios y Subdominios
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


### Install with git

From the command line, switch to the directory where dompdf will
reside and run the following commands:

```sh
git clone https://github.com/dompdf/dompdf.git
cd dompdf/lib

git clone https://github.com/PhenX/php-font-lib.git php-font-lib
cd php-font-lib
git checkout 0.5.1
cd ..

git clone https://github.com/PhenX/php-svg-lib.git php-svg-lib
cd php-svg-lib
git checkout v0.3.2
cd ..

git clone https://github.com/sabberworm/PHP-CSS-Parser.git php-css-parser
cd php-css-parser
git checkout 8.1.0
```

Require dompdf and it's dependencies in your PHP.
For details see the [autoloader in the utils project](https://github.com/dompdf/utils/blob/master/autoload.inc.php).

## Quick Start

Just pass your HTML in to dompdf and stream the output:

```php
// reference the Dompdf namespace
use Dompdf\Dompdf;

// instantiate and use the dompdf class
$dompdf = new Dompdf();
$dompdf->loadHtml('hello world');

// (Optional) Setup the paper size and orientation
$dompdf->setPaper('A4', 'landscape');

// Render the HTML as PDF
$dompdf->render();

// Output the generated PDF to Browser
$dompdf->stream();
```

### Setting Options

Set options during dompdf instantiation:

```php
use Dompdf\Dompdf;
use Dompdf\Options;

$options = new Options();
$options->set('defaultFont', 'Courier');
$dompdf = new Dompdf($options);
```

or at run time

```php
use Dompdf\Dompdf;

$dompdf = new Dompdf();
$dompdf->set_option('defaultFont', 'Courier');
```

See [Dompdf\Options](src/Options.php) for a list of available options.


## Limitations (Known Issues)

 * Dompdf is not particularly tolerant to poorly-formed HTML input. To avoid
   any unexpected rendering issues you should either enable the built-in HTML5
   parser at runtime (`$dompdf->set_option('isHtml5ParserEnabled', true);`) 
   or run your HTML through a HTML validator/cleaner (such as
   [Tidy](http://tidy.sourceforge.net) or the
   [W3C Markup Validation Service](http://validator.w3.org)).
 * Table cells are not pageable, meaning a table row must fit on a single page.
 * Elements are rendered on the active page when they are parsed.

---

[![Donate button](https://www.paypal.com/en_US/i/btn/btn_donate_SM.gif)](http://goo.gl/DSvWf)

*If you find this project useful, please consider making a donation. Any funds donated will be used to help further development on this project.)*
