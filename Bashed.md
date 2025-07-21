
![[Pasted image 20250620132241.png]]

Puertos abiertos:

![[Pasted image 20250620132655.png]]

Al ingresar dentro del servidor web, se encuentra la siguiente pagina:

![[Pasted image 20250620132820.png]]

Al realizar fuzzing de directorios con gobuster, se encuentra lo siguiente:

![[Pasted image 20250620133120.png]]

Dentro del directorio dev, se encuentra un archivo llamado phpbash.php, el cual genera una consola:

![[Pasted image 20250620133130.png]]

![[Pasted image 20250620133202.png]]

Se descarga en el directorio de uploads, desde la consola interactiva, una reverse shell para poder ejecutarla desde el navegador e ingresar dentro del sistema:

![[Pasted image 20250620133646.png]]

![[Pasted image 20250620133722.png]]

Al ingresar sudo -l como www-data, gracias a una mala configuracion en el sistema, se puede ejecutar todo como usuario scriptmanager:

![[Pasted image 20250620133907.png]]

Se ingresa:

```bash
sudo -u scriptmanager /bin/bash
```

Para ingresar como el usuario scriptmanager:

![[vmware_NEpRJCGoDd.png]]

Al buscar los vectores para escalada de privilegios, con la herramienta de pspy64, se mira como en el sistema, se ejecuta como root un script llamado test.py:

![[vmware_M9DFJpqLW3.png]]

Buscando el archivo, se encuentra que esta en un directorio llamado scripts que se encuentra en la raiz, ademas que scriptmanager es dueño del archivo test.py

![[Pasted image 20250620140050.png]]

Dentro del script test.py, se coloca codigo python para que ejecute un comando de sistema y coloque el bit SUID en el binario bash:

![[Pasted image 20250620140417.png]]

![[Pasted image 20250620140247.png]]

![[Pasted image 20250620140444.png]]

Para ingresar como root se ejecuta bash -p:

![[Pasted image 20250620140544.png]]

![[Pasted image 20250620140618.png]]