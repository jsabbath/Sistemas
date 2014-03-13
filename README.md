# Examen Evaluación Sistemas Operativos.

### Día 06/03/2014		Tiempo: 2 horas


* Nota: Cada pregunta se valorará como bien o como mal (valoraciones intermedias serán excepcionales).
* Nota2: En cada pregunta se indica su valoración. Para aprobar hay que obtener una puntuación mínima de 5 puntos.
* Nota3: Organice su tiempo. Si no consigue resolver un apartado pase al siguiente.
* Nota4: Lea completamente el examen antes de empezar.
* Nota5: Haga únicamente las comprobaciones que se le piden. Ejemplo: Si se le dice que un script recibe un archivo, asuma que realmente lo va a recibir, que existe, que no es oculto, ni tiene espacios en su nombre, etc y que tiene permisos sobre él.
* Nota6: El examen se entrega incluyendo las contestaciones en este fichero.

Pasos previos antes de empezar
------------------------------

* Configure su usuario de Git (es único para todos)

```bash
	git config --global user.name "user-daw-zayas"
	git config --global user.email "javier.perezarteaga@educa.madrid.org"
```

* Clone el repositorio del enunciado

```bash
	git clone https://user-daw-zayas@bitbucket.org/surtich/sistemas-20140306.git
```

* Vaya al directorio del repositorio

```bash
	cd sistemas-20140306
```

* Cree un *branch* con su nombre y apellidos separados con guiones (no incluya mayúsculas, acentos o caracteres no alfabéticos, excepción hecha de los guiones). Ejemplo:

```bash
	git checkout -b fulanito-perez-gomez
```

* Compruebe que está en la rama correcta:

```bash
	git status
```

* Suba la rama al repositorio remoto:

```bash
	git push origin nombre-de-la-rama-dado-anteriormente
```

* Dígale al profesor que ya ha terminado para que compruebe que todo es correcto y desconecte la red.


Enunciado
---------

### 1.- (1 punto) Cree un script que reciba el *path* a un archivo y muestre **únicamente** su tamaño.

```bash
ls -l $1 | cut -d' ' -f5
```

### 2.- (1 punto) Modifique el script anterior para que si no se ha pasado parámetro o el archivo no existe lo indique.

```bash
if [ -z $1 ] ; then
	echo "No param"
elif [ ! -e $1 ] ; then
	echo "No exists"
else
	ls -l $1 | cut -d' ' -f5
fi
```


### 3.- (1 punto) Cree un script que reciba una serie de archivos y muestre cuál de ellos tiene menor tamaño.

```bash
min=$(ls -l $* | tr -s ' ' | cut -f5 -d' ' | sort -n | head -1)
for file in $*
do
	size=$(ls -l $file | cut -f5 -d' ')
	if [ $size -eq $min ] ; then
		echo $file
	fi
done
```

### 4.- (3 puntos) Cree un script que reciba una serie de archivos y muestre cuáles de ellos tienen mayor tamaño que el tamaño medio de todos ellos.
Nota: Para calcular el tamaño medio utilice cantidades enteras.

```bash
avg=
sum=0
for file in $*
do
	size=$(ls -l $file | cut -f5 -d' ')
	(( sum+=size ))
done

(( avg=sum/$# ))

for file in $*
do
	size=$(ls -l $file | cut -f5 -d' ')
	if [ $size -ge $avg ] ; then
		echo $file
	fi
done
```

### 5.- (1 punto) Cree un script que reciba un número de proceso y muestre cuántos procesos hijos tiene ese proceso.

```bash
ps -e -o ppid | grep "^$1$" | wc -l
```

### 6.- (3 puntos) Cree un script que muestre cuál es el proceso que más procesos hijos tiene.
Nota: Si varios procesos tienen el mismo número de hijos, se mostrarán todos ellos.

```bash
childs=$(ps -e -o ppid | sed -e 1d | sort | uniq -c | tr -s ' ' ':')
max=$(echo -e "$childs" | sort -k2 -n -t':' | tail -1 | cut -f2 -d':')

for process in $childs
do
	if [ $(echo $process | cut -f2 -d':') -eq $max ] ; then
		echo $process | cut -f3 -d':'
	fi
done
```

Para entregar
-------------

* Ejecute el siguiente comando para comprobar que está en la rama correcta y ver los ficheros que ha cambiado:

```bash
	git status
```

* Prepare los cambios para que se añadan al repositorio local:

```bash
	git add *
	git commit -m "completed exam" -a
```

* Compruebe que no tiene más cambios que incluir:

```bash
	git status
```

* Dígale al profesor que va a entregar el examen.

* Conecte la red y ejecute el siguiente comando:

```bash
	git push origin nombre-de-la-rama
```

* Abandone el aula en silencio.
