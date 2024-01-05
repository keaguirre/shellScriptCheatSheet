# Apuntes Scripting

# Unidad 1

- si desea ejecutar dos comando a la vez en un solo input esto se puede hacer separando los comandos con un ;
- Ejemplo: 
```shell
echo 'hello' ; echo 'world';
esto dara como resultado:
    hello
    world
```

1. Para crear un script de shellScript primero necesitaremos crear un archivo que tenga la extension .sh
2. Dentro del archivo la primera linea debe tener el siguiente contenido -> ```#!/bin/bash```
- Para los comentarios se hace con #
3. Luego se ingresan los comandos en el orden deseado
4. Se debe darle los permisos de ejecucion al script en este caso con el comando -> ```chmod 755 ejemplo1.sh```
5. Ejecutarlo con el comando -> ```sh [ruta_script]```

- Para imprimir un echo con una variable la variable debe estar concatenada entre comillas dobles para que interprete el valor de la variable, de lo contrario con comillas simples se mostrara solamente el texto sin interpretar, aqui unos ej:
    ```shell
    echo 'la variable $USER muestra al usuario actual'
        -> la variable $USER muestra al usuario actual

    echo "la variable $USER muestra al usuario actual"
        -> la variable root muestra al usuario actual

    echo 'la variable "$USER" muestra al usuario actual'
        -> la variable "$USER" muestra al usuario actual

    echo 'la variable' "$USER" 'muestra al usuario actual'
        -> la variable root muestra al usuario actual
    ```

- Asignacion de variables
```shell
nombre=Kevin #debe ir todo junto para ser asignacion
echo "Hola $nombre" -> Kevin
```

- Manejo de variables
```shell
echo $USER -> imprimira la interpretacion de la variable USER
echo $(ls | grep Desk) -> imprimira el resultado del comando dentro de ()
ej: test1=$(ls | grep Des)
    echo $test1 -> imprimira la palabra 'Desktop'
```
- Para leer la entrada por teclado se usa el comando read seguido del nombre de la variable
    -  echo 'Ingresa un nombre' ; read nombre ; echo "tu nombre es: $nombre"

- Para operaciones matematicas anteponer el let
    ```shell
    echo "Ingrese un numero: " ; read num1 ;echo "Ingrese otro numero"; read num2 ; let suma=$num1+$num2 ; echo "resultado de la suma: $suma"
    ```
- Redireccionar una salida de un comando a un archivo
    ```shell
    date > fecha.txt
    cat fecha.txt
    ```
- Agregar una salida de un comando al final de un archivo
    ```shell
    date >> fecha.txt
    cat fecha.txt
    ```

- Para ordenar un archivo tengo diversas variables dentro del comando sort, pero si quiero pasarle ese output a otro archivo basta con que haga -> sort [option] file.txt > file2.txt

- Podemos usar un Pipe o '|' para concatenar el output del primer comando a un input del segundo comando, por ejemplo: ls | grep [text]

## Comando TAR
- tar [optiones] [archivos/directorios]
- Crear un nuevo archivo tar
    - ```tar cvf archivo.tar [archivo/directorio]```
    - create file verbose
- Extraer archivos de un archivo tar
    - ```tar -xvf archivo.tar```
    - extract file verbose
- Listar contenido de un archivo tar
    - ```tar -tvf archivo.tar```
- Comprimir y descomprimir un archivo usando gzip
    - ```tar -czvf archivo.tar.gz [archivo/directorio]```
    - ```tar -xjvf arthivo.tar.gz```

## Señales del sistema
| Num señal | Nombre  | Def                                | Propósito                                                                                                                                                                                                  |
|-----------|---------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1         | Sighup  | Colgar                             | Se usa para informar la finalización de un proceso de control a un terminal. Además, se utiliza para solicitar que se reinicie el proceso (volvr a cargar la configuración) sin finalización.              |
| 2         | Signint | Interrupción de teclado            | Provoca la finalización del programa. Puede bloquearse o manupularse. Enviado al presionar una combinación de teclas. INTR (ctrl+c).                                                                       |
| 3         | Sigquit | Salida del teclado                 | Es similar a SIGINT, pero también provoca el volcado de un proceso en la finalización. Enviado al precionar una combinación de teclas QUIT (ctrl+\).                                                       |
| 9         | Sigkill | Finalización, no se puede bloquear | Provoca la finalización abrupta del programa. No se puede bloquear, ignnorar ni manipular; SIEMPRE ES GRAVE, utilizar unicamente cuando sea necesario.                                                     |
| 15        | Sigterm | Termina, Finaliza                  | Provoca la finalización del programa. A diferencia de SIGKILL, puede ignorarse, bloquearse o manipularse. Es la manera correcta de solicitar la finalización de un programa; hace posible la autolimpieza. |
| 18        | Sigcont | Continuar                          | Se envía a un proceso para que se reinicie, en caso de que esté detenido. No puede boquearse. Aún si se manipula, siempre reinicia el proceso.                                                             |
| 19        | Sigstop | Detener, no se puede bloquear      | Suspende el proceso. No puede bloquearse ni manipularse.                                                                                                                                                   |
| 20        | Tstp    | Detención del teclado              | a diferencia de SIGSTOP, puede bloquearse, ignorarse o manipularse. Enviado al presionar una combinación de teclas SUSP (ctrl+z).                                                                          |

## PS
- ps -> lista los procesos
- podemos usar ps con mas parametros para listar diferentes items -> ps -eo user,pid,ppid,ni,pri,pcpu,pmem,comm
- podemos ordenarlos por parametros igualmente usando --sort y el parametro como -pmem
- Eliminar un proceso    
    ```shell
    kill -9 [pid]
    ```
- Procesos que coincidan con los criterios de selección, como por ejemplo el nombre del proceso.
    ```script
    killall sleep 
    ```

- Pgrep Lista los procesos en ejecución según algún criterio de búsqueda, en este ejemplo para un usuario especifico.

- Pkill Envía una señal especifica como por ejemplo un SIGKILL a un proceso en ejecución basado en algún criterio, en este ejemplo a un usuario usuario especifico.

## Comando w
También es posible a través del envío de señales terminar la sesión 
de un usuario utilizando la señal SIGKILL conociendo el origen de su 
conexión.
```shell
w -h
-> sesiones
pkill -SIGKILL -t pts/1
w -h
-> sesiones
```
- El comando wc sirve para contar la cantidad de lineas arrojadas de la salida del comando.

- Comando getent sirve para listar databases dentro del sistema, mirar man getent para ver las disponibles


### Archivos de log mas importantes

|Archivo de log | Tipos de mensajes almacenados en este archivo |
-----------------------------------------------------------------
| /var/log/secure | Mensajes relacionados con la seguridad y eventos de autenticacion |

| /var/log/maillog | Mensajes relacionados con el servicio de correos|

| /var/log/cron | Mensajes relacionados con la ejecucion de tareas programadas |

| /var/log/boot.log | Mensajes relacionados con el arranque del sistema |

| /var/log/messages | La mayoria de los mensajes del sistema son almacenados aqui, excepto los nombrados anteriormente junto con los relacionados con solamente depuracion |

## Ejemplos scripts

### Menu de sentencias con if
```shell
echo "1-escribir UNO"
echo "2-escribir DOS"
read opcion
if [$opcion == 1 ]; then
    echo "UNO"
elif [$opcion == 2 ]; then
    echo "DOS"
else
    echo "Has ingresado una opcion distinta de 1 o 2"
fi
```
El siguiente script solicita ingresar un usuario para luego mostrar la 
cantidad de intentos de sesión fallidos que ha tenido en el sistema

```shell
echo -e "Ingresa nombre de usuario: \c"
read usuario

echo "Cantidad de intentos de inicio de sesion fallidos por parte del usuario: $usuario"
cat /var/log/secure | grep $usuario | grep Failed | wc -l
```
# Unidad 2

- Operadores logicos
    - ! -> Not
    - && -> And
    - || -> Or
- Operadores de comparacion
    - -eq -> (==) es igual a -> [$a -eq $b] -> [$c == "hola"]
    - -ne -> (!=) es distinto de [$a -ne $b] -> [$c != "hola"]
    - -gt -> (>) -> es mayor que -> [ $a -gt $b]
    - -ge -> (>=) -> es igual o igual a -> [ $a -ge $b]
    - -lt -> (<) -> es menor que -> [ $a -lt $b]
    - -le -> (<=) -> es menor o igual a -> [ $a -le $b] 

### While
```shell
while [condicion]; do
    tareas a realizar
done
```
### For
- Estructura1:
```shell
for item in lista; do
    tareas a realizar
done
```
- Estructura2:
```shell
for ((valor inicial; condicion; incremento));do
    tareas a realizar
done
```
```shell
Ej:
for i in 1 2 3 4 5; do
    echo $i
done
```
### Switch (case)
- Estructura
```shell
case $variable in
    opcion1) tareas a realizar
;;
    opcion2) tareas a realizar
;;
    *) Tareas a realizar si $variable no cumple ninguna coincidencia
esac
```
Ej:
```shell
case $opcion in
    1) echo "La hora actual es: $(date +%R)"
    ;;
    2) echo "La fecha actual es: $(date +%F)"
    ;;
    3) echo "Ha elegido salir al SO."
        exit 0
    *) echo "La opcion ingresada no es valida, sistema interrumpido"
        exit 1
```
### Funciones
- Estructura
```shell
function salir(){
 echo -e “Ha elegido salir del sistema"
 exit 0
}
```
Para llamar a esta función solo hace falta escribir lo siguiente:
```salir```
