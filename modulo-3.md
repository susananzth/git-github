---
layout: default
title: Introducción GitHub
---

## Git en Visual Studio Code

La terminal es poderosa, pero VS Code ofrece una interfaz visual que hace más fácil entender lo que está pasando en el repositorio, especialmente cuando se trata de ramas y conflictos.

Para abrir VS Code directamente en la carpeta del proyecto desde la terminal:

```bash
code .
```

### Commits y ramas desde la interfaz

El ícono de control de versiones en la barra lateral muestra en tiempo real cuántos archivos han cambiado. Desde ahí puedes escribir el mensaje del commit y confirmarlo sin tocar la terminal, exactamente igual que con `git add` + `git commit -m`.

Crear una nueva rama también es directo: desde el menú de ramas en la barra inferior seleccionas *Create New Branch*, le das un nombre y VS Code te cambia a ella automáticamente. Para moverte entre ramas existentes usas ese mismo selector en la barra inferior.

Una ventaja real sobre `git log` es la vista gráfica del historial: muestra las ramas en colores diferenciados, con sus puntos de fusión, lo que hace mucho más fácil entender la historia del proyecto de un vistazo.

### Resolución de conflictos visual

Cuando intentas fusionar dos ramas con cambios incompatibles, VS Code resalta los conflictos en colores y te ofrece botones para elegir directamente: quedarte con el cambio actual, con el entrante, o con ambos. Si prefieres una vista más completa, el *Merge Editor* muestra las dos versiones lado a lado y te deja confirmar la fusión con un botón de *Complete Merge* al terminar.

### Inicializar un repositorio desde VS Code

Si abres una carpeta que todavía no es un repositorio, VS Code te muestra el botón *Initialize Repository* en el panel de control de versiones. Eso ejecuta `git init` por ti y deja todo listo para empezar a trabajar sin necesidad de abrir la terminal.

---

## GitHub: colaboración y desarrollo en la nube

Git resuelve el control de versiones en tu máquina. GitHub resuelve el problema de cómo compartir ese trabajo, colaborar con otras personas y mantener el código accesible desde cualquier lugar.

GitHub es la plataforma más usada para alojar repositorios Git, pero no es la única. Existen alternativas como GitLab, Bitbucket, Azure DevOps o CodeCommit (Amazon), y también es posible montar un servidor Git propio. La elección depende del equipo y del proyecto.

Desde sus inicios como un simple repositorio en la nube, GitHub ha evolucionado hasta convertirse en una plataforma completa que incluye herramientas de automatización, revisión de código, seguimiento de issues, integración con inteligencia artificial y funcionalidades de seguridad integradas desde el inicio del desarrollo.

Lo que hace a GitHub especialmente valioso para los desarrolladores es la posibilidad de contribuir a cualquier proyecto público del mundo, desde mejoras en librerías hasta cambios en el kernel de Linux. Esa apertura es lo que convierte a GitHub en algo más que una herramienta: es una red de colaboración global.

Para quienes quieran validar sus conocimientos formalmente, GitHub ofrece la *GitHub Foundation Certification*, una certificación de entrada que acredita el manejo de la plataforma en un contexto profesional.

---

## Crear y configurar una cuenta en GitHub

Registrarse en GitHub es directo: desde [github.com](https://github.com) haces clic en *Sign up* y completas el formulario con tu correo, una contraseña y un nombre de usuario. Al final hay un paso de verificación y GitHub envía un código a tu correo para confirmar la cuenta.

Una vez dentro, en *Settings* puedes completar tu perfil con nombre, información de contacto, redes sociales y la empresa donde trabajas si tiene presencia en la plataforma.

### Seguridad de la cuenta

Proteger tu cuenta es importante, especialmente si vas a manejar repositorios de trabajo o proyectos con otras personas. La forma más efectiva de hacerlo es activar la verificación en dos pasos desde *Settings > Password and authentication*.

El proceso es sencillo: descargas una app de autenticación (como Microsoft Authenticator o la app de GitHub Mobile), escaneas el código QR que te muestra GitHub e ingresas el código generado para completar la configuración.

> ⚠️ Evita usar SMS como método de verificación. Las apps de autenticación son significativamente más seguras. Al completar la configuración, GitHub te da códigos de recuperación: guárdalos en un lugar seguro, son tu única salida si pierdes acceso a tu app de autenticación.

---

## Flujo de trabajo con Git y GitHub

Git y GitHub son herramientas distintas que se complementan. Git vive en tu máquina y registra los cambios. GitHub vive en la nube y permite que esos cambios sean accesibles, revisables y colaborativos.

El flujo de trabajo típico en un equipo empieza en GitHub: se crea un repositorio ahí, se clona en la máquina local y desde ese punto los cambios viajan en ambas direcciones entre lo local y lo remoto.

### Por qué trabajar en ramas separadas

Nunca se trabaja directamente sobre `main` en un proyecto colaborativo. Cada tarea o funcionalidad vive en su propia rama. Cuando está lista, se sube a GitHub y se abre un proceso de revisión antes de integrarla al código principal. Esto mantiene `main` estable en todo momento.

Una buena regla práctica para organizar el trabajo es *una tarea, un objetivo*: cada rama debe tener un propósito claro y acotado. Los cambios pequeños y enfocados son más fáciles de revisar, menos propensos a conflictos y más simples de revertir si algo sale mal.

---

## Crear repositorios y colaborar en GitHub

### Crear un repositorio

Desde tu perfil en GitHub, haz clic en el botón "+" y selecciona *New repository*. El formulario te pide el nombre, una descripción opcional, y si será público o privado. También puedes inicializarlo con un archivo README, lo que es una buena práctica porque deja el repositorio listo para clonar desde el primer momento.

### Agregar colaboradores

Para invitar a alguien a trabajar en tu repositorio, ve a *Settings > Collaborators*, busca al usuario por su nombre de GitHub y envía la invitación. La persona recibirá un correo y tendrá que aceptarla antes de poder acceder al repositorio.

### Clonar el repositorio en tu máquina

Una vez creado el repositorio, puedes descargarlo a tu equipo con:

```bash
git clone https://github.com/tu-usuario/mi-primer-repo.git
```

Eso crea una carpeta local con todos los archivos del repositorio, conectada al remoto. Desde ahí puedes editar, hacer commits y subir los cambios de vuelta a GitHub.

---

## Configurar SSH en GitHub

Por defecto, GitHub usa HTTPS para autenticar las operaciones. Con SSH, en cambio, la autenticación se hace a través de un par de llaves criptográficas: una privada que vive en tu máquina y una pública que registras en GitHub. El resultado es que nunca más tienes que escribir tu contraseña para hacer `push` o `pull`.

### Generar la llave SSH

En la terminal (en Windows puedes usar WSL):

```bash
ssh-keygen -t ed25519 -C "tu@email.com"
```

El flag `-t ed25519` define el algoritmo de encriptación. El flag `-C` agrega un comentario para identificar la llave en GitHub. Cuando el comando te pida una contraseña, agrégala: protege tu llave privada en caso de que alguien acceda a tu máquina.

La llave pública quedará guardada en `~/.ssh/id_ed25519.pub`.

### Activar el agente SSH y agregar la llave

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Registrar la llave en GitHub

Abre el archivo `~/.ssh/id_ed25519.pub`, copia todo su contenido y en GitHub ve a *Settings > SSH and GPG keys > New SSH key*. Ponle un nombre que identifique el equipo desde el que estás configurando y pega la llave.

### Pasos adicionales en Mac

En Mac necesitas crear o editar el archivo `~/.ssh/config` y agregar:

```bash
Host github.com
   AddKeysToAgent yes
   UseKeychain yes
   IdentityFile ~/.ssh/id_ed25519
```

Luego agrega la llave al agente con:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

### Verificar la conexión

```bash
ssh -T git@github.com
```

Si todo está bien, GitHub responde con un mensaje que confirma tu usuario. Desde ese momento puedes clonar repositorios usando la URL SSH en lugar de HTTPS:

```bash
git clone git@github.com:tu-usuario/mi-repo.git
```

**Referencias:** [Conexión a GitHub con SSH](https://docs.github.com/es/authentication/connecting-to-github-with-ssh)

---

## Forks y estrellas en GitHub

### Forks: trabajar sobre el código de otros

Un fork es una copia de un repositorio ajeno que queda alojada en tu propia cuenta de GitHub. A diferencia de clonar directamente, el fork te da un espacio propio para modificar el proyecto sin afectar el original, y desde ahí puedes proponer cambios de vuelta al repositorio fuente.

Para crear un fork basta con abrir el repositorio y hacer clic en el botón *Fork*. GitHub copia el estado actual del proyecto a tu cuenta. Los cambios futuros que haga el autor original no se reflejan automáticamente en tu fork; si necesitas sincronizarlos, tienes que hacerlo manualmente.

Una vez forkeado, puedes clonarlo a tu máquina como cualquier otro repositorio:

```bash
git clone git@github.com:tu-usuario/repositorio-forkeado.git
```

### Estrellas: tu sistema de favoritos

Las estrellas funcionan como marcadores. Al dar una estrella a un repositorio queda guardado en la sección *Your stars* de tu perfil, accesible en cualquier momento. Es útil para guardar proyectos de referencia, librerías que usas frecuentemente o recursos que quieres revisar después.

---

## git pull, git push y git fetch

Estos tres comandos manejan la sincronización entre tu repositorio local y el remoto en GitHub.

### git push — subir cambios

Una vez que tienes commits listos en tu rama local, los subes a GitHub con:

```bash
git push origin main
```

Si es la primera vez que subes una rama nueva, usa `-u` para establecer el vínculo:

```bash
git push -u origin nombre-de-la-rama
```

### git pull — bajar y aplicar cambios

Descarga los cambios del repositorio remoto y los fusiona directamente en tu rama local:

```bash
git pull origin main
```

Es lo que haces al comenzar el día de trabajo o cuando alguien del equipo te avisa que subió cambios.

### git fetch — bajar sin aplicar

`git fetch` descarga los cambios del remoto pero no los aplica. Te deja revisar qué llegó antes de decidir si lo integras:

```bash
git fetch origin
git log main..origin/main    # ver qué cambios llegaron
git merge origin/main        # aplicarlos cuando estés listo
```

La diferencia práctica: `git pull` es conveniente cuando confías en los cambios remotos y quieres actualizarte rápido. `git fetch` es más cauteloso y útil cuando quieres evaluar antes de integrar, especialmente en proyectos donde los cambios pueden afectar lo que estás haciendo.

**Referencias:** [git-fetch](https://git-scm.com/docs/git-fetch) · [git-push](https://git-scm.com/docs/git-push) · [git-pull](https://git-scm.com/docs/git-pull)

---

## Plantillas de Issues en GitHub

Un Issue es la forma de señalar algo que necesita atención en un repositorio: un error en el código, un problema en la documentación, una mejora sugerida. Cualquier persona con acceso al repositorio puede abrirlos, lo que convierte a los Issues en el canal principal de comunicación entre colaboradores y mantenedores del proyecto.

Para crear uno, vas a la pestaña *Issues* del repositorio, haces clic en *New Issue* y completas el título y la descripción. Una buena descripción incluye qué está pasando, qué se esperaba que pasara y los pasos para reproducir el problema.

Las plantillas de Issues van un paso más allá: permiten definir una estructura predeterminada para que todos los reportes lleguen con la información necesaria. Se configuran desde los ajustes del repositorio y aparecen como opciones cuando alguien hace clic en *New Issue*.

**Referencias:** [Acerca de los Issues](https://docs.github.com/es/issues/tracking-your-work-with-issues/about-issues)

---

## Pull Requests

Un Pull Request (PR) es la forma estructurada de proponer que los cambios de una rama se integren a otra, generalmente a `main`. Es el momento donde el equipo revisa el código antes de que llegue a producción.

### El flujo completo

Primero creas una rama local y haces tus commits ahí. Cuando está lista, la subes a GitHub:

```bash
git push -u origin nombre-de-la-rama
```

GitHub detecta la rama nueva y te ofrece crear un Pull Request directamente. Ahí describes qué cambiaste y por qué, y el equipo puede comentar línea por línea, pedir ajustes o aprobar los cambios.

### Code Review

La revisión de código puede sentirse intimidante al principio, pero es una de las prácticas más valiosas en el desarrollo colaborativo. Permite detectar errores que el autor no vio, compartir conocimiento entre el equipo y mantener una calidad consistente en el código.

Cuando el PR está aprobado, GitHub verifica que no haya conflictos y habilita el botón de fusión. Si hay conflictos, tendrás que resolverlos antes de poder continuar.

### Después de fusionar

Una vez integrada la rama a `main`, el PR se cierra y la rama puede eliminarse. GitHub lo hace directamente desde la interfaz con un botón de *Delete branch*, dejando el repositorio limpio para la siguiente tarea.

**Referencias:** [About pull requests](https://docs.github.com/es/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) · [Pull requests](https://docs.github.com/es/pull-requests)
