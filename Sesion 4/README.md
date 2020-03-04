# Sesión 4: Herramientas de Git y personalización

### **Objetivo:**

- Creación de etiquetas en commits con el objetivo de mantener un mejor orden en el historial del repositorio.
- Creación de Alias con la finalidad de adecuar Git a las necesidades diarias del usuario
- Reordenamiento de commits con el comando git rebase y uso de la herramienta interactiva

### Instrucciones:

Leer y analizar los recursos listados a continuación.Se recomienda ver el video indicado por cada enlace.


### Etiquetado

Como muchos VCS, Git tiene la posibilidad de [etiquetar puntos específicos del historial  (video tutorial)](https://www.youtube.com/watch?v=5DkX3HFgklM) como importantes. Esta funcionalidad se usa típicamente para marcar versiones de lanzamiento (v1.0, por ejemplo).

#### Listar Tus Etiquetas

Listar las etiquetas disponibles en Git es sencillo. Simplemente escribe el comando `git tag`.

![](images/image43.png)

Por defecto Git no tiene ninguna etiqueta creada, por lo cual será necesario crearlas, git utiliza dos tipos principales de etiquetas: ligeras y anotadas.


Una etiqueta ligera es muy parecida a una rama que no cambia, simplemente es un puntero a un **commit** en específico.


Sin embargo, las etiquetas anotadas se guardan en la base de datos de Git como objetos enteros.

#### Etiquetas Anotadas

Para crear una etiqueta anotada es sencillo, solo debemos especificar con -a al momento de utilizar el comando [git tag (video tutorial)](https://www.youtube.com/watch?v=h145ORRg7Ao), por ejemplo:


`git tag -a v1.0 -m 'Primera Versión del Proyecto'
`
![](images/image9.png)

La opción **-m** especifica el mensaje de la etiqueta como si se tratara de un commit, en caso no que especificaremos este comando se nos abrirá el editor de vim, para solicitarnos escribir dicho mensaje.


Para ver la información de la etiqueta usaremos el comando **git show**, que nos mostrará la fecha en la que el commit fue etiquetado y el mensaje de la etiqueta.


`git show v1.0`

![](images/image25.png)

#### Etiquetas ligeras

La otra forma de etiquetas un **commit** es por medio de etiquetas ligeras. Una etiqueta ligera no es más que invocar el comando **git tag** sin argumentos -a, -s, -m; por ejemplo:

`git tag v1.1`

![](images/image6.png)

En este caso si invocamos el comando **git show ** nos mostrará únicamente el commit en el que realizamos la etiqueta sin ningún mensaje adicional.

`git show v1.1`

![](images/image10.png)

#### Etiquetado Tardío

Por defecto git tag funciona en el último commit realizado, pero en ocasiones es necesario crear etiquetas para commits que se realizaron tiempo atrás , mostraremos nuestra lista de commits con `git log`

![](images/image23.png)

Supongamos que queremos realizar una etiqueta en nuestro primer commit del historial, el cual corresponde al commit "Mi primer commit en Git", lo que debemos realizar a continuación es copiar los primeros 8 dígitos del commit, en este caso **fb369974** y agregarlos al final de nuestra etiqueta, por ejemplo:

`git tag -a v0.1 -m "Versión Alfa" fb369974`

![](images/image3.png)

Realizaremos un listado con `git tag` y mostraremos el contenido de la etiqueta v0.1 con `git show v0.1`

![](images/image17.png)
![](images/image30.png)

### Alias de Git

Una gran utilidad de Git es la [creación de Alias (video tutorial)](https://www.youtube.com/watch?v=wNk1k6tsXM4), ya que como podrás notar existen diversos comando de git que se vuelven recurrentes pero también pueden llegar a  ser algo complejos, como es el caso de **git log --oneline --decorate --graph --all**.

En caso de que no quieras teclear el nombre completo de cada cada comando se pueden crear Alias o atajos con el comando **git config --global alias.[nombre del alias] [comando]**

Por ejemplo para no escribir completo el comando branch el alias lo creariamos de la siguiente manera:

`git config --global alias.br branch`

![](images/image37.png)

Lo que nos dará como resultado la posibilidad de invocar el comando branch tecleando únicamente `git br`.


En el caso del comando **git log --oneline --decorate --graph --all**, crearemos el Alias de la siguiente manera.


`git config --global alias.graph 'log --oneline --decorate --graph --all'`, logrando invocar el comando únicamente con `git graph`.

![](images/image32.png)

### Reescribiendo la Historia

Muchas veces, al trabajar con Git, vas a querer confirmar tu historia por alguna razón. Una de las grandes cualidades de Git es que te permite tomar decisiones en el último momento.


Lo que no es recomendables es crear cambios sobre commits que ya fueron compartidos con el comando **git push **de manera distribuida, pero si aún se encuentran de manera local no genera ningún problema.


Git cuenta con una herramienta interactiva con el comando **git rebase -i**.

#### Cambiando la confirmación de múltiples mensajes


Para modificar una confirmación que está más atrás en tu historia, deberás aplicar herramientas más complejas Git no tiene una herramienta para modificar la historia, pero puedes usar la herramienta de rebase para rebasar ciertas series de confirmaciones en el HEAD en el que se basaron originalmente en lugar de moverlas a otro. Con la herramienta interactiva del rebase, puedes parar justo después de cada confirmación que quieras modificar y cambiar su mensaje, añadir archivos, o hacer cualquier cosa que quieras Puedes ejecutar el rebase interactivamente agregando el comando -i a [git rebase (video tutorial).](https://www.youtube.com/watch?v=0_QwiNnj_dA&list=PLTd5ehIj0goMCnj6V5NdzSIHBgrIXckGU&index=23)

Para realizar un ejemplo práctico, vamos a realizar 4 commits diferentes sobre el archivo **index.html**, por medio del comando **vim** vamos a generar 4 líneas de código, pero guardando y realizando un commit por cada línea creada.

![](images/image45.png)

Crearemos un pequeño error en el primer commit y segundo commit, invirtiendo los nombres de orden:

```
git add -a

git commit -am "Segundo commit"
```

![](images/image20.png)

```
git add -a

git commit -am "Primer commit"
```

![](images/image40.png)

Mantendremos el tercer y cuarto commit de forma correcta, quedando nuestro index.html de la siguiente manera:

![](images/image27.png)
![](images/image5.png)


Si utilizamos el comando `git graph`, los visualizamos de la forma actual donde existe el error generado.

![](images/image34.png)

En este caso para indicar a git que queremos trabajar con los últimos 4 commits utilizaremos el comando **git rebase -i HEAD~[numero de commits]**; por ejemplo:


`git rebase -i HEAD~4`

![](images/image7.png)

El comando nos desplegó una interfaz donde podremos seleccionar que deseamos realizar. En este caso usaremos la palabra edit para cambiar los mensajes de los commits equivocados de la siguiente manera:

```
reword 2c8b089 Primer commit
reword 1446584 Segundo commit
pick 005ad7f Tercer commit
pick 96f9a92 Cuarto commit
```

![](images/image4.png)

Guardaremos los cambios y Git realizará el **rebase**. Nos pedirá que confirmamos los cambios.

![](images/image41.png)
![](images/image28.png)

Una vez realizada las confirmaciones en **vim**, se nos desplegará el siguiente mensaje donde nos indica que los cambios fueron realizados.

![](images/image19.png)

Revisamos el historial con `git graph` y lograremos notar que los mensajes de los commits, se encuentran ahora de la forma correcta.

![](images/image46.png)

#### Reordenando Confirmaciones

Si el orden de los mensajes no fuera el equivocado y realmente quisiéramos cambiar el orden en que se realizaron los commits, podemos hacerlo modificando el orden en que nos muestra la consola de **vim** los commits que solicitamos cambiar por medio de **git rebase -i HEAD~**


`git rebase -i HEAD~4`

![](images/image18.png)
![](images/image44.png)

Cambiaremos el orden de:

```
pick d7d400b Primer commit
pick 3a883a9 Segundo commit
pick 9fe6fd1 Tercer commit
pick f06691c Cuarto commit
```

Al siguiente orden:

```
pick f06691c Cuarto commit
pick 9fe6fd1 Tercer commit
pick 3a883a9 Segundo commit
pick d7d400b Primer commit
```

Guardaremos los cambios con **esc** + **:x**

![](images/image12.png)

En este caso como los 4 commits se realizaron sobre el mismo archivo de forma secuencial nos va generar un error.

![](images/image35.png)

El cual para solucionar deberemos ingresar el comando `git add index.html` seguido por el comando `git commit`, esto nos desplegara una ventana de vim donde nos pedirá confirmar dichos cambios, tecleamos **esc + :q**

![](images/image16.png)
![](images/image2.png)

Para continuar solucionando el problema tendremos que usar el comando `git rebase --continue`

![](images/image14.png)

Debemos agregar nuevamente los comandos

```
git add index.html

git commit
```
Guardamos los cambios con **esc + :q**

![](images/image24.png)
![](images/image11.png)

Tendremos que iterar los mismos pasos;

```
git rebase --continue

git add index.html

git commit
```

Guardamos los cambios con **esc + :q**

![](images/image33.png)
![](images/image21.png)
![](images/image36.png)

Realizaremos una última vez la misma operación:

```
git rebase --continue

git add index.html

git commit
```

Guardamos los cambios con **esc + :q**


![](images/image22.png)
![](images/image29.png)
![](images/image13.png)

Una vez iterado en las cuatro ocasiones, nos desplegará un mensaje que el **rebase** fue realizado con éxito y podremos visualizar los cambios con el comando `git graph`

![](images/image31.png)
![](images/image8.png)

#### Unir confirmaciones

También es posible el tomar series de confirmaciones y unirlas todas en una sola confirmación con la herramienta interactiva de **rebase**. El script pone las instrucciones en el mensaje de rebase:

Para unificar los últimos 4 commits en uno solo usaremos el comando.

`git rebase -i HEAD~4  `

En **vim** realizaremos el siguiente cambio

```
pick 8bd4cf9 Cuarto commit
squash e4a0d4e Tercer commit
squash d357720 Segundo commit
squash d8c6d9b Primer commit
```

Guardamos los cambios con **esc + :x**

![](images/image15.png)

Nos pedirá escribamos un mensaje con el cual relacionamos el único commit.

![](images/image26.png)

Cambiaremos todo el mensaje por un único mensaje: por ejemplo:

```
Cambio General
```

Guardamos el cambio con **esc + :x**

![](images/image39.png)

Visualizamos que el **rebase** fue hecho con éxito y desplegamos los cambios con `git graph` donde notaremos que existe un único commit.

![](images/image38.png)
![](images/image1.png)
























