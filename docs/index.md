# Informe Práctica 1. Configuración de máquina virtual en el IaaS
## 1. Introducción

Informe de la primera práctica de la asignatura de **Desarrollo de Sistemas Informáticos**. En la práctica propuesta se ha realizado la configuración inicial de la **máquina virtual** otorgada mediante el **IaaS**, así cómo la posterior instalación de **git** y **Node.js**, además de configurar la **máquina local** para agilizar la conexión remota entre ambas máquinas, y con el GitHub asociado al correo institucional. A su vez, aprender a utilizar el lenguaje de marcas **Markdown** y **GitHub Pages**.

## 2. Objetivos

El objetivo principal de la práctica es la preparación del entorno de trabajo de la asignatura. Para conseguirlo se tienen los siguientes objetivos:

* Aprender a conectarse a la **VPN de la ULL**, así como acceder al Iaas de la ULL.
* Saber realizar una conexión remota mediante **SSH**.
* Configuración inicial de la máquina virtual.
* Configuración inicial de la máquina local.
* Manejo de **claves SSH**.
* Configuración de del **prompt** del la máquina virtual.
* Instalación de **git** y **Node** en la máquina virtual.
* Utilización del **lenguaje de marcas Markdown**.
* Utilización de la herramienta **GitHub Pages**.

## 3. Desarrollo

### 3.1 Tareas Previas

Para el desarrollo de la práctica cómo tal primero hay que hacer unas preparaciones previas, cumplimentar las encuestas de elección de trabajo y la de expectativas y conocimientos, darse de alta en el **Google Classroom**, así como solicitar los beneficios de estudiantes de **GitHub Education** con nuestro **GitHub** asociado del correo institucional, darnos de alta en el **GitHub Classroom** así como la asignación para obtener el **repositorio** para la **práctica 1**.

Cuando ya tengamos el repositorio de la **práctica 1**, necesitaremos saber manejarnos con el **lenguaje de marcas Markdown** y aprender a crear un **GitHub Pages**, Las siguientes guías son muy útiles, [Introducción a Markdown](https://guides.github.com/features/mastering-markdown/) y [GitHub Pages](https://docs.github.com/en/github/working-with-github-pages). Igualmente, para crear una **Page de GitHub** antes necesitamos tener archivos en nuestro repositorio, empezaremos creando un *README.md* para explicar la función del repositorio y un *index.md* dentro de una carpeta llamada *docs* en donde pondremos todo el contenido que cogerá **GitHub Pages** para la presentación del informe. Con esto, nos dirigmos a **Settings** y nos vamos a al apartado de **GitHub Pages**, aquí escogemos cómo **Branch** nuestro *main*, le damos a guardar y escogemos como carpeta ```/docs```, con esto ya habremos creado la **Page de GitHub**, lo siguiente es escoger un tema en el selector de temas, para este informe se escogió Cayman. El resultado final quedaría de la siguiente manera.

![Image of Settings](img/Setting%20GitHub%20Page.jpg)

![Image of GitHub Pages](img/github%20page.jpg)

### 3.2 Configuración de la máquina virtual en el Iaas

####   3.2.1 Conectarse a la VPN de la ULL

Lo primero será la configuración inicial para conectarse a la **VPN** de la ULL, en mi caso ya tenía acceso directo a la **VPN** gracias a asignaturas anteriores, pero para configurarla según el sistema operativo se puede seguir la siguiente guía [Configuración de VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/). En mi caso yo cuento con **Ubuntu** por lo que se vería de la siguiente forma una vez conectados a la **VPN**.

![Image of VPN](img/vpn.png)

#### 3.2.2 Acceso al servicio IaaS de la ULL

Conectados a la **VPN** accedemos al [IaaS de la ULL](https://iaas.ull.es) con nuestro correo institucional, elegimos la **máquina virtual** de DSI y la iniciamos, una vez que la iniciamos se nos asignara una para nuestro uso, la mía en concreto es la DSI-13. Dentro del menú de la máquina buscamos la **IP externa** para así poder **acceder remotamente**.

![Image of Ip máquina virtual](img/Ip%20maquina%20virtual.png)


#### 3.2.3 Conexión remota y primeras configuraciones

Conectados a la **VPN** de la ULL y con la máquina virtual encendida procedemos a abrir un **terminal** y a conectarnos de **manera remota** mediante el comando ```ssh```.

```bash
bruno@bruno-X550VX:~$ ssh usuario@10.6.XXX.XXX
```

Cuando nos hayamos conectado rémotamente nos preguntará lo siguiente:

```bash
The authenticity of host '10.6.XXX.XXX (10.6.XXX.XXX)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

por lo que tendremos que introducir ```yes```. Una vez hecho esto nos pedirá introducir la contraseña (por defecto las credenciales son ```usuario``` y ```usuario``` para el usuario y la contraseña, respectivamente). Cuando la hayamos introducido el sistema nos pedirá modificarla. Para ellos, introduciremos la contraseña actual que es ```usuario``` y, luego la nueva contraseña por duplicado.

![Image of SSH](img/ssh.png)


Lo siguiente será cambiar el nombre de nuestra máquina, para eso abriremos el fichero ```/etc/hostname``` con ```Vi``` o ```Vim``` y el nombre de *ubuntu* lo cambiamos por el de nuestra máquina virtual, en nuestro caso es *iaas_dsi13*.

![Image of cambiar nombre](img/cambiar%20nombre.png)

A su vez, cambiaremos donde pone *ubuntu* en el siguiente fichero ```/etc/hosts``` también por el nombre de la **máquina virtual**, con esto cambiamos el nombre del host.

![Image of host local](img/host%20local.png)


Ahora tendriamos que reiniciar nuestra máquina virtual para que todo los cambios tengan efecto, pero antes actualizaremos el software de la misma. Para esto realizamos los siguientes comandos:

```bash
usuario@ubuntu:~$ sudo apt update
...
usuario@ubuntu:~$ sudo apt upgrade
...
usuario@ubuntu:~$ sudo reboot
...
```

Aprovechando que la **máquina virtual** se encuentra reiniciándose procedemos a configurar nuestra **máquina local** para que cuando accedamos de manera remota no tendremos que recordar la IP. el fichero que hay que modificar es el ```etc/hosts``` añadiendo una línea al fichero que sea la la **IP** y el host de la **máquina virtual**.

![Image of cambiar hosts](img/cambiar%20hosts.png)

#### 3.2.4 Claves públicas-privadas

Ahora realizaremos la configuración de las **claves ssh** con lo que conseguiremos que se pueda acceder a nuestra **máquina virtual** sin necesidad de introducir la contraseña. Para empezar revisaremos que tenemos una **clave ssh** creada con el siguiente comando:

```bash
bruno@bruno-X550VX:~$ cat .ssh/id_rsa.pub
```

![Image of clave ssh](img/clave%20ssh.png)

Podemos observar que ya tenía creada una clave, pero en el caso de no tenerla creada la creamos introduciendo el siguiente comando:

```bash
bruno@bruno-X550VX:~$ ssh-keygen
```

Es importante el no introducir ninguna *passphrase** asociada al par de claves pública-privada. Una vez que tengamos las claves ejecutamos el siguiente comando, lo cual nos permite copiar la clave desde la máquina local a la máquina virtual:

```bash
bruno@bruno-X550VX:~$ ssh-copy-id usuario@iaas-dsi13
```

Ahora, cuando realizamos una **conexión remota** a la **máquina virtual** podemos acceder sin la necesidad de introducir ninguna contraseña.

![Image of ssh sin contraseña](img/ssh%20sin%20contrase%C3%B1a.png)

Por otro lado, si no queremos utilizar el nombre de usuario ```usuario```  de la **máquina virtual** a la hora de conectarnos vía SSH, podemos configurar el fichero ```/.ssh/config```, es posible que a lo mejor no lo tengamos creado por lo que podríamos crear con el comando ```touch```.

```bash
bruno@bruno-X550VX:~$ touch ~/.ssh/config
bruno@bruno-X550VX:~$ vim ~/.ssh/config
bruno@bruno-X550VX:~$ cat ~/.ssh/config
Host iaas-dsi13
  Hostname iaas-dsi13
  User usuario
```

Con esto podemos iniciar una conexión SSH simplemente indicando el nombre de la **máquina virtual**.

![Image of ssh solo nombre](img/ssh%20solo%20nombre.png)

Para terminar este apartado, vamos a generar las **claves pública-privada** en la **máquina virtual** también, siguiendo los pasos que seguimos con anterioridad en la **máquina local.**

```bash
usuario@iaas-dsi13:~$ ssh-keygen
...
usuario@iaas-dsi13:~$ cat ./ssh/id_rsa.pub
```

### 3.3 Instalación de git y Node.js en la máquina virtual del IaaS

#### 3.3.1 Instalación y configuración de git

Continuamos con la instalación de **git** en nuestra **máquina virtual**, para eso ejecutamos:

```bash
usuario@iaas-dsi13:~$ sudo apt install git
...

```

Una vez instalado es necesario realizar las configuraciones iniciales que se reduciría a los siguientes comandos a ejecutar:

```bash
usuario@iaas-dsi13:~$ git config --global user.name "Bruno Arroyo"
usuario@iaas-dsi13:~$ git config --global user.email alu0101123677@ull.edu.es
usuario@iaas-dsi13:~$ git config --list
...
```

![Image of git config](img/git%20config.png)

Si se quiere profundizar en la configuración inicial del **git** se puede acceder al siguiente enlace [Configurar Git por primera vez](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez).

#### 3.3.2 Configuración del prompt

Ahora, configuraremos el prompt de la terminal para que aparezca la rama actual en la que nos encontramos cuando accedemos a algún directorio que resulta estar asociado a un **repositorio git**. Para ello descargaremos el script [git prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh), también podemos crear un fichero con ```touch``` al que llamaremos *git-prompt.sh* y en el que copiaremos el contenido del script [git prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) usando ```Vi``` o ```Vim```. Una vez con nuestro fichero en nuestra **máquina virtual** procedemos acambiarle el nombre con el comando ```mv```.

```bash
usuario@iaas-dsi13:~$ mv git-prompt.sh .git-prompt.sh
```

Continuamos con la modificación del fichero ```~/.bashrc```, incluyendo al final del mismo las dos líneas de código siguiente.

```bash
usuario@iaas-dsi13:~$ vim .bashrc
usuario@iaas-dsi13:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'
```

![Image of cambiar prompt](img/cambiar%20prompt.png)

Terminando con este apartado, reiniciaremos la terminal, con lo que se actualizaran los cambios que hemos realizado y el prompt debería cambiar.

```bash
usuario@iaas-dsi13:~$ exec bash -l
[~()]$
```

#### 3.3.3 Conexión con GitHub

Para comprobar si realmente el prompt muestra lo que deseamos, que no es otra cosa que la rama actual de trabajo cuando accedemos a un directorio asociado a un repositorio, tendremos que añadir la **clave pública** de la **máquina virtual** en la configuración de las claves de nuestra cuenta de **GitHub**, de modo que nos sea mucho más fácil trabajar con **repositorios remotos**, y así poder también clonar alguno de los repositorios para hacer la prueba. En primer lugar, copiaremos la **clave pública** de nuestra **máquina virtual**:

```bash
[~()]$cat ~/.ssh/id_rsa.pub
```

Una vez copiada, accedemos a la configuración de la cuenta de **GitHub** (account settings), y en la sección *SSH* and *GPG keys*, pulsamos sobre el botón New *SSH key*. En el formulario añadimos un título para la clave y pegamos la **clave pública** de nuestra **máquina virtual** en el campo de texto key. Por último, pulsamos sobre el botón Add *SSH key*. Si todo ha ido bien, ahora podríamos ejecutar el siguiente comando desde la **máquina virtual** para clonar un repositorio:


```bash
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2021/prct01-iaas-vscode.git
...
[~()]$ls
prct01-iaas-vscode
[~()]$cd prct01-iaas-vscode/
[~/prct01-iaas-vscode(main)]$
```

![Image of clone git](img/clone%20git.png)

#### 3.3.4 Instalación de Node

Ahora vamos a proceder a instalar Node Version Manager (nvm), el gestor de versiones de **Node.js**. **Node.js** es un entorno que permite la ejecución de código desarrollado en **JavaScript** y variantes, como por ejemplo, **TypeScript**.

```bash
[~()]$wget -q0- https://raw.githubusercontent.com/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
[~()]$nvm --verion
0.37.2
```

![Image of instalación node](img/instalaci%C3%B3n%20node.png)


Ahora que hemos instalado nvm satisfactoriamente procedemos a instalar la versión más reciente de **Node.js**.

```bash
[~()]$nvm install node
...
[~()]$node --version
v15.8.0
[~()]$npm --version
7.5.1
```

![Image of instalación node2](img/instalaci%C3%B3n%20node%202.png)

Al instalar **Node.js** podemos observar como se ha instalado la última versión (15.8.0), además de la última versión de **Node Package Manager (npm)**, el gestor de paquetes de **Node.js**. Para instalar una versión concreta de **Node.js**, podemos hacer lo siguiente:

```bash
[~()]$nvm install 12.0.0
...
[~()]$node --version
v12.0.0
[~()]$npm --version
6.9.0
```

Por último, para cambiar entre versiones, podemos ejecutar los siguientes comandos:

```nvm list
...
[~()]$nvm use v15.8.0
Now using node v15.8.0 (npm v7.5.1)
[~()]$node --version
v15.8.0
[~()]$npm --version
7.5.1
```

![Image of instalación node2](img/cambio%20de%20versi%C3%B3n.png)

## 4. Conclusiones

En conclusión, me parece que este tipo de prácticas introductorias son muy importantes debido a qué si no se han realizado con anterioridad **conexiones remotas**, conectarse a una **VPN** o configurar un **entorno de trabajo**, son cosas que se tienen que tener en cuenta como básicas a nivel profesional en el mundo de la **ingeniería informática**. Por otro lado, también es importante saber cómo manejarse con **GitHub**, sus repositorios y control de versiones con **git**, así como tener soltura con un **lenguaje de marcas** como **Markdown** o herramientas como **GitHub Pages**.

Ya para terminar, recalcar la importancia de la preparación del entorno de trabajo para así llegar a aportar soluciones de la manera más óptima posible, en este caso preparando para utilizar **JavaScript** y **TypeScript**.

## 5. Bibliografía

**[Guión Práctica 1](https://ull-esit-inf-dsi-2021.github.io/prct01-iaas/):** Guión de la práctica 1 de la asignatura de Desarrollo de Sistemas Informáticos.

**[Introducción a Markdown](https://guides.github.com/features/mastering-markdown/):** Guía sobre la utilización de Markdown.

**[GitHub Pages](https://docs.github.com/en/github/working-with-github-pages):** Guía para la creación y utilización de GitHub Pages.

**[Configuración de VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/):** Guía para la configuración y conexión a la VPN de la ULL.

**[Web de Jekyll](https://jekyllrb.com):** Sitio Web de Jekyll.

**[Configurar Git por primera vez](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez):** Guía para configurar Git por primera vez.


