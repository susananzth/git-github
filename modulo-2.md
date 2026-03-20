---
layout: default
title: Git y GitHub
---

## Comandos básicos y flujo de trabajo

El flujo de trabajo en Git siempre sigue la misma lógica: modificas archivos, los preparas para guardar y luego confirmas los cambios. Una vez que internalizas ese ciclo, todo lo demás encaja.

Todo empieza con `git init`, que convierte una carpeta normal en un repositorio. Lo que hace internamente es crear una carpeta oculta llamada `.git`, que actúa como la bitácora del proyecto: ahí Git registra cada cambio, cada versión, cada movimiento.

### El área de staging

Antes de guardar un cambio, Git te pide que lo declares explícitamente. Esa zona de espera se llama *staging area* o área de preparación. Puedes pensarla como un borrador: los archivos están ahí listos para el commit, pero todavía no forman parte del historial.

Para mover un archivo al staging usas `git add nombre_del_archivo.txt`, o `git add .` si quieres agregar todos los archivos modificados de una vez. En cualquier momento puedes revisar el estado de tus archivos con `git status`.

Si agregaste algo por error y quieres sacarlo del staging sin perder los cambios, lo más seguro es:

```bash
git restore --staged nombre_del_archivo.txt
```

> ⚠️ Evita usar `git rm --cached` para esto. Aunque funciona, `git rm` está pensado para eliminar archivos del repositorio y puede tener efectos no deseados. `git restore --staged` es más preciso y seguro para simplemente sacar un archivo del staging.

### Confirmar cambios con commit

Una vez que tienes los archivos que quieres en staging, los guardas en el historial con:

```bash
git commit -m "descripción clara de lo que hiciste"
```

El mensaje es importante: es lo que verás en el historial cuando necesites entender qué cambió y por qué. Un buen mensaje dice *qué* se hizo, no solo "cambios" o "fix".

Para revisar el historial de commits acumulado:

```bash
git log
```

Git trata cualquier tipo de archivo de la misma manera, sin importar si es código, texto o una imagen. El flujo siempre es el mismo: `git add` para preparar, `git commit` para guardar.

**Referencias:** [git-add](https://git-scm.com/docs/git-add) · [git-log](https://git-scm.com/docs/git-log) · [Docs de GitHub](https://docs.github.com/es/get-started/start-your-journey)

## Gestión de ramas: creación, fusión y eliminación

Las ramas son uno de los conceptos más poderosos de Git. Te permiten trabajar en una funcionalidad o corrección de forma aislada, sin tocar el código que ya funciona en la rama principal. Si algo sale mal, simplemente descartas la rama y no pasa nada.

### Crear y navegar entre ramas

Para ver en qué rama estás y listar todas las disponibles:

```bash
git branch
```

La rama activa aparece marcada con un asterisco.

Para crear una nueva rama y moverte a ella en un solo paso:

```bash
git checkout -b nombre-de-la-rama
```

También puedes usar la sintaxis moderna equivalente:

```bash
git switch -c nombre-de-la-rama
```

Para moverte entre ramas existentes, `git switch nombre-de-la-rama` es la opción más clara hoy en día, aunque `git checkout nombre-de-la-rama` también funciona.

### Fusionar cambios con merge

Cuando terminas el trabajo en una rama y quieres integrarlo en `main`, primero vuelves a la rama principal y luego traes los cambios:

```bash
git switch main
git merge nombre-de-la-rama
```

### Eliminar ramas ya fusionadas

Una vez que una rama cumplió su propósito, lo mejor es eliminarla para mantener el repositorio limpio:

```bash
git branch -d nombre-de-la-rama
```

Si la rama no fue fusionada y aun así quieres forzar la eliminación, usa `-D` en lugar de `-d`. Úsalo con cuidado, porque los cambios que no hayas fusionado se perderán.

**Referencias:** [git-branch](https://git-scm.com/docs/git-branch) · [git-switch](https://git-scm.com/docs/git-switch) · [git-checkout](https://git-scm.com/docs/git-checkout)

---

## Deshacer cambios: reset y revert

Saber deshacer cosas es tan importante como saber hacerlas. Git ofrece dos herramientas principales para esto, y elegir la correcta depende del contexto.

### git revert — deshacer sin borrar historial

`git revert` crea un nuevo commit que invierte los cambios de un commit anterior. El historial queda intacto: se puede ver que algo se revirtió y por qué. Es la opción recomendada cuando trabajas con otras personas, porque no reescribe el historial compartido.

```bash
git revert hash-del-commit

```

Git abrirá un editor para que escribas el mensaje del nuevo commit de reversión.

### git reset — mover el puntero hacia atrás

`git reset` mueve el puntero `HEAD` a un commit anterior, como si los commits posteriores no hubieran existido. Tiene tres modos:

| Modo | Qué hace |
| --- | --- |
| `--soft` | Vuelve al commit indicado, pero mantiene los cambios en staging |
| `--mixed` | (por defecto) Vuelve al commit y saca los cambios del staging, pero los conserva en tus archivos |
| `--hard` | Vuelve al commit y **borra** todos los cambios posteriores de forma irreversible |

```bash
git reset --hard hash-del-commit
```

> ⚠️ `--hard` es destructivo. Úsalo solo cuando estés seguro de que quieres descartar esos cambios para siempre, y nunca sobre commits que ya subiste y que otros puedan tener.

### ¿Cuándo usar cada uno?

Usa `git revert` cuando el commit ya está en un repositorio compartido o cuando quieres conservar la trazabilidad de lo que pasó. Usa `git reset` para limpiar tu historial local antes de compartir cambios, o para deshacer algo que todavía no salió de tu máquina.

**Referencias:** [git-reset](https://git-scm.com/docs/git-reset) · [git-revert](https://git-scm.com/docs/git-revert)

---

## git tag y git checkout para gestión de versiones

### Etiquetar versiones con git tag

Los tags son marcadores que puedes colocar sobre commits específicos para señalar hitos importantes: una versión estable, un release, un punto de no retorno. A diferencia de los nombres de rama, los tags no se mueven.

Para crear un tag anotado (el tipo recomendado, porque incluye metadatos):

```bash
git tag -a v1.0 -m "primera versión estable"
```

Si quieres etiquetar un commit anterior en lugar del más reciente, agrega su hash al final:

```bash
git tag -a v1.0 hash-del-commit -m "primera versión estable"
```

Para ver todos los tags creados: `git tag`. Para ver los detalles completos de uno en particular:

```bash
git show v1.0
```

Para eliminar un tag localmente:

```bash
git tag -d v1.0
```

Si ya lo subiste a GitHub y necesitas eliminarlo también del remoto:

```bash
git push origin --delete v1.0
```

Y para subir un tag al repositorio remoto:

```bash
git push origin v1.0
```

### Explorar commits anteriores con git checkout

`git checkout` no solo sirve para cambiar de rama. También puedes usarlo para navegar a un commit específico y revisar el estado del proyecto en ese momento:

```bash
git checkout hash-del-commit
```

Cuando haces esto, Git te avisa que estás en modo *detached HEAD*: estás viendo el proyecto en ese punto del tiempo, pero no estás en ninguna rama. Puedes explorar y hacer pruebas, pero nada de lo que hagas ahí afectará tu historial principal.

Para volver al estado actual:

```bash
git checkout main
```

**Referencias:** [git-tag](https://git-scm.com/docs/git-tag) · [git-checkout](https://git-scm.com/docs/git-checkout)

## Resolución de conflictos de ramas

Los conflictos ocurren cuando dos ramas modifican las mismas líneas de un archivo y Git no puede decidir solo cuál versión conservar. No son errores: son Git pidiéndote que tomes una decisión.

### Cómo se genera un conflicto

El escenario típico es este: creas una rama, modificas un archivo y haces commit. Mientras tanto, alguien (o tú mismo desde `main`) modifica las mismas líneas de ese archivo y también hace commit. Cuando intentas fusionar las dos ramas, Git detecta la contradicción y detiene el merge.

### Cómo se ve un conflicto en el archivo

Git marca directamente en el archivo las dos versiones en disputa:

```bash
<<<<<<< HEAD
cambios desde main
=======
cambios desde la otra rama
>>>>>>> nombre-de-la-rama
```

Todo lo que está entre `<<<<<<< HEAD` y `=======` es lo que tiene `main`. Lo que está entre `=======` y `>>>>>>>` es lo que viene de la otra rama.

### Cómo resolverlo

Abre el archivo, lee ambas versiones y edítalo hasta dejarlo como quieres que quede: puedes conservar una versión, la otra, combinarlas o reescribir esa sección completamente. Lo único importante es eliminar las líneas de marcación (`<<<<<<<`, `=======`, `>>>>>>>`) antes de guardar.

Una vez resuelto, confirmas la resolución:

```bash
git add conflict.txt
git commit -m "Conflicto resuelto"
```

Después de resolver y fusionar, elimina la rama que ya no necesitas para mantener el repositorio limpio:

```bash
git branch -d nombre-de-la-rama
```

**Referencias:** [git-branch](https://git-scm.com/docs/git-branch) · [git-merge](https://git-scm.com/docs/git-merge)
