# 3. Pon tu base de datos en modo ArchiveLog y realiza con RMAN una copia de seguridad física en caliente.

El modo `ARCHIVELOG` de Oracle es un mecanismo de protección ante fallos del disco que permite realizar copias de seguridad en caliente. Para activar el modo archivelog reiniciamos la base de datos para montarla directamente en la instancia sin abrirla, si no no nos dejará:

```
shutdown immediate;
startup mount;
```

```
ALTER DATABASE ARCHIVELOG;
SELECT log_mode FROM V$DATABASE;
```
![ ](img/301.png)


Podemos comprobar el estado con el siguiente comando:
```
ARCHIVE LOG LIST;
```
![ ](img/302.png)

Ahora, podemos abrir de nuevo la base de datos.
```
ALTER DATABASE open;
```

![ ](img/303.png)

Ahora que está el modo archivelog activo, vamos a utilizar la herramienta RMAN para hacer la copia de seguridad.

```
rman target sys
```

![ ](img/304.png)

Podemos ver los parámetros por defecto para la copia de seguridad con `SHOW ALL`. Ya que no hemos tocado nada, estarán los parámetros por defecto.

![ ](img/305.png)

Para hacer un paralelismo con el ejercicio 1, vamos a activar la encriptación y limitar el tamaño.

NOTA: MAXSETSIZE debe ser al menos tan grande como el datafile más grande. Porque un único datafile no se puede colocar en más de un fichero backups. Si limito el tamaño de un conjunto de respaldo a un valor menor que mi datafile más grande, no cabrá. Se pueden ver los tablespaces disponibles con `report schema;`
```
CONFIGURE ENCRYPTION FOR DATABASE ON;
CONFIGURE MAXSETSIZE TO 930 M;
```
![ ](img/308.png)

![ ](img/306.png)

![ ](img/307.png)

Finalmente, podemos hacer la copia de seguridad con el siguiente comando:

```
BACKUP DATABASE;
```

![ ](img/309.png)

![ ](img/310.png)

![ ](img/311.png)


El comando para listar los backup sets es `list backups`

![ ](img/312.png)

