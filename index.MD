## Instalacion de NGINX, PHP, MySQL, PHPMyAdmin, Laravel, Composer, VSCode y Git en Arch Linux y derivados

Elaborado por Fernando Duarte Villavicencio.

__**Antes de empezar, varias imagenes aparece un texto que se esta reinstalando la aplicación todo ello se hizo con el fin de comprobar si funcionaba cada uno de los comandos mostrados en este documento**__
**Si su GNU/Linux esta basado en Debian, Ubuntu o derivados** [Click aqui](https://github.com/Du-F23/LAMPP-Composer-Laravel-en-Debian-Ubuntu-y-derivados.io)

Para empezar iniciaremos actualizando todos las aplicaciones pendientes

>`sudo pacman -Syu`
_________________________________
## Instalación de Nginx 
Despues iniciaremos con la instalación de NGINX (__**NGINX un software similar a Apache aunque es mas moderno**__).

>`sudo pacman -S nginx`

![](/img/nginx_install.png)

__**Seguido a eso habilitaremos y activaremos nginx**__

>`systemctl enable nginx.service`

>`systemctl start nginx.service`

Ahora ya tenemos la gran parte hecha para el lado de nginx

___________________________________________________________

## Instalación de php

Ahora procederemos a instalar PHP 

>`sudo pacman -S php php-fpm`

![](/img/php_install.png)

__**Seguido a eso habilitaremos y activaremos PHP**__

>`systemctl enable php-fpm`

>`systemctl start php-fpm`

__**Empezaremos a hacer la configuración para incluir PHP**__

>`sudo nano /etc/nginx/nginx.conf`

__**Los comandos que usaremos para usar nano de forma correcta son los siguientes**__
- ctrl + x (Para salir del editor)
- ctrl + o (Para guardar los cambios dentro del archivo)
- ctrl + w (Buscar alguna linea)

Ubicaremos la linea que dice lo siguiente
__**#pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000**__

Y pegaremos remplazaremos por lo siguiente

>`location ~ .php$ {`
>
>`try_files $uri = 404;`
>
>`root /usr/share/nginx/html;`
>
>`fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;`
>
>`fastcgi_index index.php;`
>
>`fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;`
>
>`include fastcgi_params;`
>
>`}`

![](/img/nginx_config.png)
Una vez terminado lo anterior ubicaremos la siguiente linea

>`index index.html index.htm`

Cuando encontremos la linea añadiremos lo siguiente

>`index index.html index.htm index.php`

![](/img/nginx_config2.png)

Ahora creamos el archivo de información de PHP, para ello ingresaremos a la siguiente ruta
>`cd /usr/share/nginx/html/`

Y despues con ayuda de nano crearemos y editaremos el archivo, __**Se puede colocar cualquier nombre, pero que termine con la extension .php**__
>`sudo nano info.php`

![](/img/php_info.png)

Una vez abierto colocaremos las siguientes lineas

> `<?php`
> 
> `phpinfo();`
> 
> `?>`

![](/img/php_info2.png)

Y reiniciamos el servicio de nginx

>`systemctl restart nginx.service`

_____________________________________

## Instalación de MySQL 

Ahora procederemos a instalar MySQL

>`sudo pacman -S mysql`

__**Nos pedira que escojamos el servicio, por lo cual 
indicaremos el numero donde este ubicado mariadb**__

![](/img/mysql_install.png)

__**Seguido a eso habilitaremos y activaremos PHP**__

>`systemctl enable mariadb.service`

>`systemctl start mariadb.service`

Ahora en la terminal digitaremos lo siguiente

>`sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql`

![](/img/mysql_config.png) **Cabe resaltar que como yo ya lo tenia configurado me aparecio lo siguiente**

**Normalmente aparece algo similar a lo siguiente**
![](/img/mysql_config2.png)
![](/img/mysql_config3.png)

_____________________

## Instalación de PHPMyAdmin

Para instalar phpmyadmin escribiremos lo siguiente

>`sudo pacman -S phpmyadmin`

![](/img/phpmyadmin.png)

Despues crearemos el enlace para acceder a phpmyadmin

>`ln -s /usr/share/webapps/phpMyAdmin /usr/share/nginx/html/phpmyadmin`

![](/img/phpmyadmin_config.png)

Modificaremos el archivo de configuración de PHP
> `sudo nano /etc/php/php.ini`

__Descomentaremos las siguientes lineas para ello borraremos el siguiente simbolo ;__

extension=mysqli
extension=mysql
extension=zip
extension=bz2
extension=iconv

![](/img/phpmyadmin_config2.png)
![](/img/phpmyadmin_config3.png)

Y al fin tendriamos NGINX, PHP, MySQL, PHPMyAdmin configurados.

_______________________

## Instalación de Composer y Laravel

Descargamos composer

>`php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

![](/img/composer.png)

Ejecutaremos el instalador de composer

![](/img/composer_install.png)

Ahora procederemos a instalar laravel

![](/img/laravel.png)

Escribimos lo siguiente

>`echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >> ~/.bashrc`

> `echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc`

Recargamos el archivo

>`source ~/.bashrc`

Para verificar si laravel esta siendo reconocida haremos lo siguiente

![](/img/laravel_init.png)

__**Para poder cargar laravel una vez cerrada la terminal lo unico que tenemos que hacer es lo siguiente**__

>`source ~/.bashrc`

Para crear aplicaciones usando laravel usamos lo siguiente

>`laravel new laravel_prueba`

![](/img/laravel_new.png)
________________________

## Instalación de Git

>`sudo pacman -S git`

![](/img/git.png)


________________________

## Instalacion de VS Code

Para instalar VS Code hay distintas formas
la mas sencilla es mediante 

>`sudo pacman -S code` Este software es la version de codigo abierto de VS Code

La segunda es la version oficial de Microsoft

__**Importante: Necesitamos tener git y makepkg**__

Primero debemos clonar el repositorio

>`git clone https://aur.archlinux.org/visual-studio-code-bin.git`
> 
>`cd visual-studio-code-bin/`

![](/img/vscode_clone.png)
 
Una vez clonado y dentro de la ubicación de visual-studio-code-bin

>`makepkg -si`

![](/img/makepkg_vscode.png)
![](/img/makepkg_vscode2.png)
________________________

## Links a utilidades usadas en este documento

Pagina utilizada para configurar LEMP [NGINX, PHP, MySQL, PHPMyAdmin ](https://securityhacklabs.net/articulo/parte-1-montando-un-servidor-lnmp-linux-nginx-mysql-php-con-proteccion-ddos-y-phpmyadmin).

Pagina utilizada para configurar Laravel [Laravel en Arch Linux](https://www.linuxandubuntu.com/home/install-laravel-on-manjaro-arch-linux).

[phpMyAdmin error: The $cfg['TempDir'] (./tmp/) is not accessible](https://www.discoverbits.in/2122/phpmyadmin-error-the-$cfg-tempdir-tmp-is-not-accessible)

Pagina de la Wiki de Arch [Nginx](https://wiki.archlinux.org/title/nginx), [MySQL](https://wiki.archlinux.org/title/MySQL), [PHP](https://wiki.archlinux.org/title/PHP), [PHPMYADMIN](https://wiki.archlinux.org/title/PhpMyAdmin), [VS Code](https://wiki.archlinux.org/title/Visual_Studio_Code).
