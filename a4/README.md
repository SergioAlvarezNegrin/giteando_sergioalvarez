# UT1-A4 Práctica guiada de Git y GitHub II (Fork y Pull Request)

### 1. Objetivos

Al terminar la práctica podrás:

- Diferenciar un repositorio original, un fork y un clon local.
- Colaborar en un repositorio sin tener permisos de escritura sobre el
  original.
- Configurar los remotos `origin` y `upstream`.
- Crear ramas para desarrollar cambios aislados.
- Registrar cambios mediante commits.
- Publicar una rama en GitHub.
- Crear una Pull Request (PR).
- Revisar el trabajo de otra persona, solicitar cambios y aprobarlos.
- Fusionar cambios y sincronizar un fork.
- Documentar el trabajo con evidencias.

### 2. Organización deL GRUPO DE TRABAJO

Se trabajará en una única ronda pero es importante que ambos miembros del grupo entiendan lo que se está haciendo:.

- **Ronda:** alumno_A es propietario del repositorio y alumno_B
  contribuye mediante un fork.

El propietario revisa y fusiona la Pull Request. El
colaborador trabaja desde su propio fork.

# El alumno_B contribuye al repositorio de alumno_A

### 4. Paso 1. El alumno_A prepara el repositorio original

El alumno_A utilizará el repositorio creado en la práctica anterior.
Debe comprobar que es público y que contiene al menos un `README.md` o
algún archivo de documentación.

A abre una terminal en el clon local de su repositorio y ejecuta:

```bash
git status
```

Este comando indica si hay archivos modificados o pendientes de
registrar. Si aparece `working tree clean`, no hay cambios locales
pendientes.

Comprueba el remoto:

```bash
git remote -v
```

Lo normal es que aparezca `origin` apuntando al repositorio de A en
GitHub.

Comprueba la rama actual:

```bash
git branch --show-current
```

En esta práctica utilizaremos `main` como rama principal. Si el
repositorio utiliza otro nombre, anótalo y adapta los comandos que hagan
referencia a `main`.

Comprueba que el repositorio está actualizado:

```bash
git pull origin main
```

Este comando descarga e integra los cambios de la rama remota `main`. Si
no hay cambios nuevos, Git indicará que ya está actualizado.

### Evidencia 2 --- alumno_A

```text
origin	git@github.com:SergioAlvarezNegrin/giteando_sergioalvarez.git (fetch)
origin	git@github.com:SergioAlvarezNegrin/giteando_sergioalvarez.git (push)
main
```

### Si el repositorio no tiene contenido suficiente

alumno_A debe crear un archivo `documentación.md` en la que debe poner un texto breve en el que se explique los objetivos de esta práctica (los tienes al inicio de la misma).
Después:

```bash
git add documentación.md
git commit -m "Documentar el repositorio"
git push origin main
```

### 5. Paso 2. El alumno_B crea un Fork

Un **fork** es una copia de un repositorio alojada en la cuenta de otro
usuario de GitHub. No es lo mismo que clonar:

- **Fork:** crea una copia del repositorio en GitHub, dentro de tu
  cuenta.
- **Clone:** descarga un repositorio de GitHub a tu ordenador.

El alumno_B debe realizar estos pasos en la web:

1.  Abrir el repositorio de alumno_A.
2.  Pulsar **Fork**.
3.  Elegir su propia cuenta como propietario.
4.  Crear el fork.
5.  Comprobar que GitHub indica que el repositorio procede del original
    de A.

El resultado será parecido a este:

```text
GitHub
├── Alumno A
│   └── repositorio-original
│
└── Alumno B
    └── repositorio-original  ← fork de A
```

Aunque tengan el mismo nombre, son dos repositorios distintos, alojados
en cuentas distintas.

### Evidencia 3 --- alumno_B

Inserta capturas donde se vea tu fork y la referencia al repositorio
original.

### 6. Paso 3. El alumno_B clona su Fork

El alumno_B debe clonar **su fork**, no el repositorio original de alumno_A. En GitHub,
dentro del fork que ha creado alumno_B, pulsa **Code**, selecciona **SSH** y copia la URL.

En la terminal:

```bash
mkdir -p ~/practicas-fork
cd ~/practicas-fork
```

Clona el fork usando la URL SSH real de B:

```bash
git clone git@github.com:USUARIO_B/NOMBRE_REPOSITORIO.git
```

Sustituye `USUARIO_B` y `NOMBRE_REPOSITORIO` por los valores reales. No
copies literalmente los marcadores de ejemplo.

Entra en el directorio creado. Por ejemplo:

```bash
cd practicas-fork
```

Comprueba dónde estás:

```bash
pwd
```

Comprueba el estado:

```bash
git status
```

Comprueba el remoto:

```bash
git remote -v
```

Deberías ver el fork de alumno_B como `origin`. Ese será el repositorio al que
alumno_B podrá subir sus ramas.

### Evidencia 4 --- alumno_B

```text
PEGA AQUÍ LA SALIDA DE git remote -v
```

## 7. Paso 4. Añadir el repositorio original como upstream

El alumno_B necesita distinguir su fork del repositorio original del alumno_A.

- `origin`: el fork del alumno_B.
- `upstream`: el repositorio original de alumno_A.

Añade el remoto original utilizando la URL SSH del alumno_A:

```bash
git remote add upstream git@github.com:USUARIO_A/NOMBRE_REPOSITORIO.git
```

Comprueba los dos remotos:

```bash
git remote -v
```

El resultado será similar a este:

```text
origin    git@github.com:USUARIO_B/NOMBRE_REPOSITORIO.git (fetch)
origin    git@github.com:USUARIO_B/NOMBRE_REPOSITORIO.git (push)
upstream  git@github.com:USUARIO_A/NOMBRE_REPOSITORIO.git (fetch)
upstream  git@github.com:USUARIO_A/NOMBRE_REPOSITORIO.git (push)
```

Las URL serán diferentes en tu caso. Lo importante es que `origin`
apunte a alumno_B y `upstream` a alumno_A.

### Evidencia 5 --- alumno_B

```text
PEGA AQUÍ LA SALIDA COMPLETA DE git remote -v
```

### Comprueba que lo has entendido

1.  ¿Qué remoto corresponde a tu fork?

```text
RESPONDE AQUÍ
```

2.  ¿Qué remoto corresponde al repositorio original?

```text
RESPONDE AQUÍ
```

3.  ¿A qué remoto subirás tu rama de trabajo?

```text
RESPONDE AQUÍ
```

### 8. Paso 5. Actualizar las referencias remotas

Antes de empezar a trabajar, alumno_B descarga la información del repositorio
original:

```bash
git fetch upstream
```

`fetch` obtiene información de los cambios y ramas remotas, pero no
modifica automáticamente los archivos de tu rama actual.

Comprueba las ramas:

```bash
git branch -a
```

Puedes ver ramas locales y referencias remotas, como
`remotes/upstream/main`.

Comprueba el estado:

```bash
git status
```

### Evidencia 6

```text
PEGA AQUÍ LA SALIDA DE git branch -a
```

### 9. Paso 6. Crear una rama de trabajo

El alumno_B no debe desarrollar directamente sobre la rama `main`. Creará una rama
específica para su contribución:

```bash
git switch -c feature/guia-comandos-git
```

Una rama permite desarrollar cambios sin mezclarlos inmediatamente con
la rama principal.

Comprueba la rama actual:

```bash
git branch --show-current
```

Y consulta el estado:

```bash
git status
```

Deberías estar en `feature/guia-comandos-git`.

### Evidencia 7

```text
PEGA AQUÍ LA SALIDA DE git branch --show-current
```

## 10. Paso 7. Crear la contribución

El alumno_B creará un archivo llamado `guia-comandos.md` en el repositorio.

Puedes utilizar el editor de texto o crear el archivo desde la terminal.
Su contenido debe ser el siguiente:

```markdown
# Guía de comandos Git
```

## Consultar el estado

```bash
git status
```

Muestra el estado del repositorio y los cambios pendientes.

## Consultar el historial

```bash
git log --oneline
```

Muestra los commits de forma resumida.

## Consultar las ramas

```bash
git branch
```

Muestra las ramas locales.

Guarda el archivo dentro del directorio del repositorio.

Comprueba qué detecta Git:

```bash
   git status
```

Como el archivo es nuevo, puede aparecer como archivo sin seguimiento
(_untracked_).

Comprueba los cambios:

```bash
git diff
```

> **Observación:** `git diff` no muestra el contenido de un archivo nuevo
> que todavía no está bajo seguimiento. En el siguiente paso lo añadiremos
> al staging y utilizaremos `git diff --staged` para revisar el contenido
> preparado.

### Evidencia 8

```text
PEGA AQUÍ LA SALIDA DE git status
```

### 11. Paso 8. Añadir el archivo al staging y revisar

Añade únicamente el archivo que quieres incluir en el commit:

```bash
git add guia-comandos.md
```

Comprueba el estado:

```bash
git status
```

Ahora revisa exactamente lo que se incluirá en el commit:

```bash
git diff --staged
```

El área _staging_ permite seleccionar qué cambios formarán parte del
siguiente commit. No es obligatorio añadir todos los archivos
modificados a la vez.

Si detectas un error, corrige el archivo antes de continuar.

### Evidencia 9

```text
PEGA AQUÍ LA SALIDA DE git diff --staged
```

### 12. Paso 9. Crear el commit

Un **commit** guarda una versión de los cambios preparados en el
repositorio local.

Ejecuta:

```bash
git commit -m "Añadir guía de comandos Git"
```

Comprueba el historial:

```bash
git log --oneline -3
```

Comprueba el estado:

```bash
git status
```

El commit todavía está en el repositorio local del alumno_B. Aún no se ha
enviado a GitHub.

### Evidencia 10

```text
PEGA AQUÍ LA SALIDA DE git log --oneline -3
```

### 13. Paso 10. Publicar la rama en el Fork

Para que el alumno_A pueda revisar los cambios, el alumno_B debe subir la rama a **su
fork**, que es `origin`:

```bash
git push -u origin feature/guia-comandos-git
```

El parámetro `-u` establece la relación entre la rama local y la rama
remota. En futuras ocasiones, desde esa misma rama normalmente bastará
con ejecutar `git push`.

Abre el fork del alumno_B en GitHub y comprueba que aparece la rama
`feature/guia-comandos-git` y el commit realizado.

### Evidencia 11

Peha aquí una captura de la rama y del commit en GitHub:

## 14. Paso 11. Crear la Pull Request

Una **Pull Request (PR)** es una solicitud para que los cambios de una
rama se revisen y, si procede, se integren en otra rama.

El alumno_B debe abrir su fork en GitHub y seleccionar la opción para crear una
Pull Request. Comprueba cuidadosamente estos valores:

| CAMPO           | VALOR ESPERADO                   |
| --------------- | -------------------------------- |
| Base repository | Repositorio original de A        |
| Base branch     | `main` del repositorio de A      |
| Head repository | Fork de B                        |
| Compare branch  | `feature/guia-comandos-git` de B |

**Si GitHub selecciona por defecto el repositorio de B como destino,
cambia el repositorio base al original de A.**

Utiliza un título descriptivo:

```text
Añadir guía de comandos Git
```

Y una descripción similar a esta:

```markdown
## Cambios realizados

- Añadido el archivo guia-comandos.md.
- Documentados los comandos git status, git log y git branch.
- Añadidas explicaciones breves.

## Comprobaciones

- He revisado el contenido.
- He comprobado los comandos y el formato Markdown.
```

Crea la Pull Request.

### Evidencia 12

Incluye una captura donde se vean el título, el destino y la rama de
origen de la Pull Request.

### 15. Paso 12. El alumno_A revisa la Pull Request

El alumno_A abre la Pull Request en el repositorio original y entra en **Files
changed**.

Debe revisar:

- Que el archivo existe y tiene un nombre adecuado.
- Que los comandos están escritos correctamente.
- Que las explicaciones son comprensibles.
- Que el Markdown está bien formado.
- Que no se han añadido archivos innecesarios.
- Que el cambio aporta valor al repositorio.

El alumno_A debe dejar al menos un comentario útil y específico. Por ejemplo: **_Añade una explicación sobre la diferencia entre consultar el estado del repositorio y consultar su historial_**

El comentario debe ayudar a mejorar la contribución, no limitarse a
decir «está mal» o «corrígelo».

### Evidencia 13

Pega aquí una captura del comentario de revisión:

### 16. Paso 13. El alumno_B responde a la revisión

Si el alumno_A solicita una mejora, el alumno_B debe corregir el archivo en su rama local.

Comprueba en qué rama estás:

```bash
git branch --show-current
```

Debe ser:

```text
feature/guia-comandos-git
```

Edita `guia-comandos.md` y realiza la corrección solicitada.

Comprueba los cambios:

```bash
git status
git diff
```

Añade el archivo:

```bash
git add guia-comandos.md
```

Revisa lo preparado:

```bash
git diff --staged
```

Crea un nuevo commit:

```bash
git commit -m "Mejorar explicaciones de la guía Git"
```

Sube la actualización:

```bash
git push
```

**_No necesitas crear otra Pull Request: GitHub incorporará los nuevos
commits a la PR existente porque pertenecen a la misma rama._**

Si el alumno_A no solicita cambios, el alumno_B puede realizar una mejora adicional que el
revisor haya sugerido como opcional, o documentar por qué la
contribución ya cumple los requisitos.

### Evidencia 14

```text
PEGA AQUÍ LA SALIDA DEL NUEVO COMMIT
Y UNA CAPTURA DE LA PR ACTUALIZADA
```

### 17. Paso 14. Aprobar y fusionar la Pull Request

El alumno_A vuelve a revisar los cambios actualizados.

Si todo es correcto, puede aprobar la PR mediante **Approve**. Después,
el propietario del repositorio original puede pulsar **Merge pull
request** y confirmar la fusión.

La aprobación y la fusión son acciones diferentes:

- **Approve:** indica que la revisión es favorable.
- **Merge:** integra los cambios en la rama destino.

Si GitHub muestra conflictos, no fuerces la fusión.

### Evidencia 15

Pega aquí una captura de la Pull Request fusionada:

### 18. Paso 15. El alumno_B actualiza su Fork

Una vez que el alumno_A ha fusionado la PR, el repositorio original contiene los
cambios, pero el `main` del fork del alumno_B puede seguir desactualizado.

El alumno_B descarga la información del alumno_A:

```bash
git fetch upstream
```

Cambia a la rama principal local:

```bash
git switch main
```

Integra los cambios del repositorio original:

```bash
git merge upstream/main
```

Por último, publica la actualización en su fork:

```bash
git push origin main
```

Comprueba el historial:

```bash
git log --oneline --graph --decorate --all
```

### Evidencia 16

```text
PEGA AQUÍ LA SALIDA DE git log --oneline --graph --decorate --all
```
