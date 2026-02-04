# Seguridad Informátcia - Práctica 4: Almacenamiento Seguro
## Disponibilidad
La disponibilidad es uno de los principios fundamentales de la seguridad informática, junto con la confidencialidad, la integridad, la autenticidad y la no repudio. Es la capacidad de un sistema, servicio o datos para estar operativo y accesible cuando se necesita. 

Un sistema puede ser muy seguro, pero si no está disponible cuando los usuarios autorizados lo requieren, no es útil. 

La disponbilidad depende de varios factores, entre ellos:
- **Redundancia**: Tener componentes duplicados o sistemas de respaldo para evitar puntos únicos de falla.
- **Tolerancia a fallos**: La capacidad de un sistema para continuar operando
- **Copias de seguridad**: Realizar copias regulares de datos para recuperarlos en caso de pérdida o corrupción.

## Tolerancia a Fallos
La tolerancia a fallos es la capacidad de un sistema para continuar funcionando correctamente incluso cuando uno o más de sus componentes fallan. Esto se logra mediante el diseño de sistemas que pueden detectar fallos y recuperarse automáticamente sin interrupciones significativas en el servicio.

Algunas técnicas comunes para lograr la tolerancia a fallos incluyen:
- **Clustering**: Agrupar múltiples servidores para que trabajen juntos y  de seguridad (Información Copiada)
Podemos clasificar las copias de seguridad según la cantidad de información que se copia en cada sesión. Los tipos más comunes son:

- **Copia de seguridad Completa**: Se copia toda la información seleccionada, independientemente de si ha cambiado o no.
- **Copia de seguridad Incremental**: Solo se copian los datos que han cambiado desde la última copia de seguridad, ya sea completa o incremental.
- **Copia de seguridad Diferencial**: Se copian todos los datos que han cambiado desde la última copia de seguridad completa.

## RAID
RAID (Redundant Array of Independent Disks) es una tecnología que combina múltiples discos duros en una sola unidad lógica para mejorar el rendimiento, la redundancia o la disponibilidad de los datos. Existen varios niveles de RAID, cada uno con sus propias características y beneficios. Los mas importantes son:

- **RAID 0 (Striping)**: Distribuye los datos entre múltiples discos para mejorar la velocidad de lectura y escritura, pero no ofrece redundancia.
- **RAID 1 (Mirroring)**: Duplica los datos en dos o más discos, proporcionando redundancia y protección contra fallos de disco.
- **RAID 5 (Striping con Paridad)**: Distribuye los datos y la información de **paridad** entre tres o más discos, ofreciendo un equilibrio entre rendimiento y redundancia.
- **RAID 6 (Striping con Doble Paridad)**: Similar a RAID 5, pero con doble **paridad**, lo que permite la falla de hasta dos discos sin pérdida de datos.
- **RAID 10 (1+0)**: Combina las características de RAID 1 y RAID 0, proporcionando tanto redundancia como rendimiento al agrupar discos en espejos y luego distribuir los datos entre esos espejos.

### Paridad
La paridad es un método utilizado en algunos niveles de RAID (como RAID 5 y RAID 6) para proporcionar redundancia y protección contra la pérdida de datos. Consiste en calcular un valor adicional (bit de paridad) basado en los datos almacenados en los discos. Este valor se utiliza para reconstruir los datos en caso de fallo de uno o más discos.

### Función XOR
La función XOR (o "exclusive or") es una operación lógica que se utiliza comúnmente en la implementación de la paridad en sistemas RAID. La operación XOR compara dos bits y devuelve un resultado de 1 si los bits son diferentes (uno es 0 y el otro es 1) y devuelve 0 si los bits son iguales (ambos son 0 o ambos son 1).

La idea es que al aplicar la función XOR a los datos almacenados en varios discos, se puede generar un bit de paridad que puede ser utilizado para recuperar los datos en caso de fallo de uno de los discos.

Siendo A y B los datos de dos discos, y P la paridad calculada, se cumple que:

$$A \oplus B = P$$
$$A \oplus P = B$$
$$B \oplus P = A$$

De esta forma , si uno de los discos falla, se puede recuperar su contenido utilizando los datos del otro disco y la paridad calculada.

### RAID por Software vs Hardware
Es posible implementar RAID tanto por software como por hardware. Basicmanete, podemos tener un programa en equipo que gestione el RAID (RAID por Software) o podemos tener un aparato independienteo con un programa dedicado a gestionar el RAID (RAID por Hardware).


## Encriptación
Habitualmente, los datos almacenados en discos duros no están encriptados, lo que significa que si alguien obtiene acceso físico al disco, puede leer fácilmente la información almacenada en él. Para proteger los datos en caso de pérdida o robo del disco, es recomendable utilizar técnicas de encriptación. La encriptación de discos puede realizarse a nivel de archivo, partición o disco completo. Esto significa que es necesario desencriptar los datos antes de poder acceder a ellos, lo que añade una capa adicional de seguridad, pero también reduce la velocidad de acceso a los datos debido al proceso de encriptación y desencriptación, ademas de consumir recursos del sistema.

## Compresión
La compresión de datos es una técnica utilizada para reducir el tamaño de los archivos y optimizar el uso del espacio de almacenamiento. Existen dos tipos principales de compresión:

- **Compresión sin pérdida (Lossless)**: Permite recuperar los datos originales exactamente como eran antes de la compresión. Ejemplos de algoritmos de compresión sin pérdida incluyen ZIP y GZIP. Este tipo de compresión es ideal para archivos de texto, código fuente y otros datos donde la integridad es crucial.
- **Compresión con pérdida (Lossy)**: Elimina algunos datos para lograr una mayor reducción de tamaño, lo que puede resultar en una pérdida de calidad. Este tipo de compresión es comúnmente utilizado en archivos multimedia, como imágenes, audio y video. Ejemplos de algoritmos

La compresión se utiliza principalmente para transporte, almacenamiento a largo plazo y copia de seguridad de datos.

## Empaqutado
El empaquetado es el proceso de combinar múltiples archivos en un solo archivo para facilitar su almacenamiento, transporte o distribución. Los archivos empaquetados pueden ser comprimidos para reducir su tamaño y optimizar el uso del espacio de almacenamiento.
El empaquetado es comúnmente utilizado para distribuir software, crear copias de seguridad y organizar conjuntos de archivos relacionados. Algunos formatos de empaquetado populares incluyen TAR, ZIP y RAR.


## Parte Práctica
En esta práctica utilizaremos `mdadm`, que es la herramienta de administración de RAID por software en Linux.

En una máquina virtual de Debian realizaremos varias configuraciones de RAID utilizando discos virtuales adicionales. 

**Para cada primero crearemos nuevos discos virtuales, los formatearemos, crearemos los arreglos RAID correspondientes y montaremos los sistemas de archivos resultantes para verificar su funcionamiento.**

### Comandos Utiles
- `mdadm`: Herramienta para gestionar arreglos RAID por software en Linux.
- `mkfs`: Comando para crear sistemas de archivos en discos o particiones.
- `mount`: Comando para montar sistemas de archivos en puntos de montaje.
- `umount`: Comando para desmontar sistemas de archivos montados.
- `lsblk -f`: Comando para listar los dispositivos de bloques (discos) y sus sistemas de archivos.
- `df -h`: Comando para mostrar el uso del espacio en disco de los sistemas de archivos montados.
- `cat /proc/mdstat`: Comando para mostrar el estado actual de los arreglos RAID gestionados por `mdadm`.

### Creación de Discos Virtuales
Para crear discos virtuales en una máquina virtual de VirtualBox, podemos seguir estos pasos:

1. Apagar la máquina virtual si está en ejecución.
2. Abrir la configuración de la máquina virtual en VirtualBox.
3. Ir a la sección "Almacenamiento".
4. Hacer clic en el icono de disco con un signo "+" para agregar un nuevo disco duro.
5. Seleccionar "Crear un disco duro virtual ahora" y seguir las instrucciones para crear un disco duro de tamaño adecuado (por ejemplo, 1 GB).

### Montaje de Discos
En Linux, los discos duros deben ser montados para poder acceder a su contenido. Podemos utilizar el comando `mount` para montar un disco en un punto de montaje específico.

Montar un disco duro, basicamente consiste en asignarle una carpeta del sistema de archivos donde se podrá acceder a su contenido.

Para montar un disco, primero debemos crear un punto de montaje (una carpeta vacía) y luego utilizar el comando `mount` para montar el disco en esa carpeta. Por ejemplo:

```bash
sudo mkdir /mnt/disco1
sudo mount /dev/sdb1 /mnt/disco1 # Montar el disco /dev/sdb1 en /mnt/disco1
```

Algunos discos se montan automáticamente al encender el sistema operativo, esto se configura en el archivo `/etc/fstab`, pero en esta práctica montaremos los discos manualmente.

### Formateo de Discos
Antes de utilizar un disco duro o partición, es recomendable formatearlo con un sistema de archivos adecuado. En el caso de Linux el formato más común es ext4. Podemos utilizar el comando `mkfs` para formatear un disco o partición. Por ejemplo, para formatear un disco como ext4:

```bash
sudo mkfs.ext4 /dev/sdb1
```

### RAID 0
Esta es la forma más sencilla de RAID, que distribuye los datos entre múltiples discos para mejorar el rendimiento, pero no ofrece redundancia. Si un disco falla, se pierde toda la información. 

Primero crearemos dos discos virtuales adicionales en la máquina virtual de Debian, por ejemplo `/dev/sdb` y `/dev/sdc`.

Para crear un RAID 0 con `mdadm`, podemos utilizar el siguiente comando:

```bash
sudo mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc
```

Después de crear el RAID 0, lo podremos ver con el comando:

```bash
sudo lsblk -f
```

Vereis que aparece un nuevo dispositivo llamado `/dev/md0` y que este tiene aproximadamente la suma del tamaño de los dos discos. Lo que escribamos en este nuevo dispositivo se distribuirá entre los dos discos originales.

Si uno de los discos falla, se perderán todos los datos del RAID 0, ya que no hay redundancia.


### RAID 1
Para crear un RAID 1 (mirroring) con `mdadm`, podemos utilizar el siguiente comando:

```bash
sudo mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
```

Después de crear el RAID 1, lo podremos ver con el comando:

```bash
sudo lsblk -f
```

Vereis que aparece un nuevo dispositivo llamado `/dev/md1` y que este tiene aproximadamente el tamaño de uno de los discos originales. Lo que escribamos en este nuevo dispositivo se duplicará en ambos discos originales, proporcionando redundancia en caso de fallo de uno de ellos.

Si uno de los discos falla, los datos seguirán estando disponibles en el otro disco. Podremos reemplazar el disco fallido y reconstruir el RAID utilizando `mdadm`. Pero esto no lo veremos en esta práctica.


### RAID 5

Para crear un RAID 5 con `mdadm`, necesitaremos al menos tres discos. Supongamos que tenemos `/dev/sdb`, `/dev/sdc` y `/dev/sdd`. Para crear el RAID 5, podemos utilizar el siguiente comando:

```bash
sudo mdadm --create --verbose /dev/md2 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd
```

Después de crear el RAID 5, lo podremos ver con el comando:

```bash
sudo lsblk -f
```

Vereis que aparece un nuevo dispositivo llamado `/dev/md2` y que este tiene aproximadamente el tamaño de la suma de los discos menos uno. Lo que escribamos en este nuevo dispositivo se distribuirá entre los tres discos originales, con paridad para permitir la recuperación en caso de fallo de uno de ellos.

Si uno de los discos falla, los datos seguirán estando disponibles gracias a la paridad almacenada en los otros discos. Podremos reemplazar el disco fallido y reconstruir el RAID utilizando `mdadm`. Pero esto no lo veremos en esta práctica.

### RAID 6

Para crear un RAID 6 con `mdadm`, necesitaremos al menos cuatro discos. Supongamos que tenemos `/dev/sdb`, `/dev/sdc`, `/dev/sdd` y `/dev/sde`. Para crear el RAID 6, podemos utilizar el siguiente comando:

```bash
sudo mdadm --create --verbose /dev/md3 --level=6 --raid-devices=4 /dev/sdb /dev/sdc /dev/sdd /dev/sde
``

Después de crear el RAID 6, lo podremos ver con el comando:

```bash
sudo lsblk -f
```

Vereis que aparece un nuevo dispositivo llamado `/dev/md3` y que este tiene aproximadamente el tamaño de la suma de los discos menos dos. Lo que escribamos en este nuevo dispositivo se distribuirá entre los cuatro discos originales, con doble paridad para permitir la recuperación en caso de fallo de hasta dos de ellos.

Si uno o dos de los discos fallan, los datos seguirán estando disponibles gracias a la doble paridad almacenada en los otros discos. Podremos reemplazar los discos fallidos y reconstruir el RAID utilizando `mdadm`. Pero esto no lo veremos en esta práctica.


### Entrega
Debereis entregar un documento PDF con capturas de pantalla con las salidas de los siguientes comandos:

- `proc/mdstat` después de crear cada RAID.