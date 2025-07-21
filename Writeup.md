
![[Pasted image 20250620173252.png]]

Puertos abiertos:

![[Pasted image 20250620173605.png]]

Al ingresar dentro del servidor web, se encuentra lo siguiente:

![[Pasted image 20250620173834.png]]

De la pagina, llama la atencion esta nota:

![[Pasted image 20250620173913.png]]

Vi es un editor de texto basado en sistemas Unix.

Se encuentra nombre de usuario, jkr:

![[Pasted image 20250620174034.png]]

Durante el escaneo de nmap, se detecto un archivo robots.txt de la pagina web. Dentro del archivo se encuentra lo siguiente:

![[Pasted image 20250620174521.png]]

![[Pasted image 20250620174634.png]]

Gracias a wappalyzer, se encuentra que la pagina writeup esta corriendo por detras un CMS Simple:

![[Pasted image 20250620181020.png]]


Investigando la jerarquia de archivos y directorios que contiene el CMS Made simple gracias a Github, se prueba una por una para verificar en cuales son accesibles:

![[Pasted image 20250621181058.png]]

![[Pasted image 20250621181316.png]]

Investigando en el repositorio de GitHub, se encuentra que en el directorio doc, hay un archivo robots.txt

![[Pasted image 20250621181653.png]]

Realizando la busqueda de los archivos accesibles del CMS, gracias a la estructura del CMS encontrada en el GitHub oficial de CMS Made Simple, se encuentra la version que esta instalada en la maquina victima:

![[Pasted image 20250623122409.png]]

Buscando exploits para la version de este CMS, se encuentra que existe una de SQL Injection:

![[Pasted image 20250623122456.png]]

Clonado el repositorio de github a la maquina atacante, se ejecuta el exploit:

```bash
python3 exploit.py -u http://writeup.htb/writeup -c -w /usr/share/wordlists/rockyou.txt
```

![[Pasted image 20250623125037.png]]

Como se otorga el salt del hash de la contraseña, ambos se agregan en un archivo llamado hash.txt y se rompe con hashcat:

```bash
hashcat -m 20 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

![[vmware_WYbtl0UrqY.png]]

Credenciales de jkr:
- jkr:raykayjay9

Ingresando las credenciales dentro del servicio ssh, se ingresa exitosamente:

![[Pasted image 20250623135836.png]]

Revisando las posibles escaladas de privilegios a root, se encuentra que el usuario jkr pertenece al grupo de staff, el cual gracias a la pagina de hacktricks se realiza lo siguiente:

![[Pasted image 20250623141405.png]]

Este grupo puede acceder al directorio de /usr/local/bin y crear archivos dentro de el que contenga codigo malicioso, gracias al archivo run-parts, que se ejecuta cada vez que se inicia sesion por ssh, por tanto la escalada se conforma de:

- Crear un archivo en /usr/local/bin llamado run-parts
- Colocar codigo malicioso dentro de el
- Darle permisos de ejecucion a ese archivo
- Iniciar sesion por ssh

![[Pasted image 20250623141829.png]]

![[Pasted image 20250623141900.png]]

![[Pasted image 20250623141946.png]]

Exitosamente se coloco el bit SUID en la bin bash, unicamente se ingresa bash -p para ingresar como root:

![[Pasted image 20250623142033.png]]

![[Pasted image 20250623142140.png]]