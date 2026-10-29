# Operación MONCLOA.GIT — El Ministerio de las Promesas Imposibles

## Introducción
El Gobierno de la República de Gitlandia acaba de crear el Ministerio de las Promesas Imposibles, una institución encargada de desarrollar un programa electoral que cambiará constantemente. El problema es que cada cambio que hace un político se sobreescribe cada cambio anterior. Nuestro equipo ha sido contratado para desarrollar un sistema Git para seguir los cambios y implementar un control de versiones que guarda la historia de cambios.

## Objetivos
Instalar y configurar Git desde la terminal.
Crear e inicializar un repositorio local.
Comprender el directorio de trabajo, el área de preparación (staging) y los commits.
Añadir, modificar, eliminar y recuperar archivos.
Consultar el historial y comparar versiones.
Utilizar .gitignore para excluir determinados archivos.
Crear y utilizar ramas individualmente.
Trabajar con un repositorio remoto en GitHub.
Subir, descargar y sincronizar cambios.
Clonar un repositorio y continuar trabajando sobre él.

## Entorno de trabajo
Ubuntu 26 con VSCode como editor de archivos

---

## Fase 1. Preparación del entorno

### Paso 1. Comprobar si Git está instalado
```sh
git --version
```
```
 git version 2.53.0
```

### Paso 2. Configurar nuestra identidad
```sh
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@example.com"
```
- Eso determina el autor que se va a usar para cada commit 

```sh
git config --global --list
```
- para ver el usuario que se va a usar
```
credential.helper=store
user.email= APARECERÁ CORREO
user.name= APARECERÁ USUARIO
...skipping...
credential.helper=store
user.email= APARECERÁ CORREO
user.name= APARECERÁ USUARIO
```

```sh
git config --global init.defaultBranch main
```
- Crea la rama inciial predeterminada


### Paso 3. Crear nuestro espacio de trabajo

```sh
mkdir -p ~/proyectos-git
cd ~/proyectos-git

mkdir ministerio-promesas
cd ministerio-promesas
```

- Comprobar el contenido del directorio
```sh
ls -la
```
```
total 16
drwxrwxr-x 2 alex alex 4096 Oct 29 08:50 .
drwxrwxr-x 3 alex alex 4096 Oct 29 09:03 ..
-rw-rw-r-- 1 alex alex 4808 Oct 29 09:02 README.md
```



### Paso 4. Inicializar un repositorio
```sh
git init -b main
```
- Se convierte un directorio normal en uno de Git

```
Initialized empty Git repository in DIRECTORIO/.git/
```
Y haciendo un `ls -la`
total 20
drwxrwxr-x 3 alex alex 4096 Oct 29 09:08 .
drwxrwxr-x 3 alex alex 4096 Oct 29 09:03 ..
drwxrwxr-x 6 alex alex 4096 Oct 29 09:08 .git
-rw-rw-r-- 1 alex alex 4808 Oct 29 09:02 README.md

- Se ha creado una carpeta oculta que gestiona el repositorio que hemos creado

```sh
git status
```
```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```
- Dice que no se ha hecho ningún commit 

### Analisis
Git funciona completamente en local. Sigue todos los cambios desde la carpeta que hemos creado de .git. Solo se necesitaría internet si quisieramos conectarlo a un servicio remoto cómo GitHub. Borrando .git quitaría el seguimiento del directorio. 


---

## Fase 2. Primer programa electoral

### Paso 5. Crear el documento oficial
```sh
nano programa.md
```
```
# Programa Electoral de Gitlandia

## Elecciones Generales

Presidente: Don Commit Permanente

## Promesas oficiales

1. WiFi gratis en todo el país.
2. Los lunes serán festivos.
3. Cada ciudadano tendrá un robot asistente.
4. Los exámenes serán voluntarios.
5. Las impresoras funcionarán siempre.

## Lema

"Prometemos hasta que se llene el disco duro".
```

### Paso 6. Consultar el estado del repositorio
```sh
git status
```
```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
        programa.md

nothing added to commit but untracked files present (use "git add" to track)
```
- Vemos que se ha detectado el nuevo archivo, pero por ahora no hemos hecho un add para empezar el seguimiento. 



### Paso 7. Registrar el primer commit

```sh 
git add programa.md
git status
```
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   programa.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```
- Ahora vemos que hemos dicho a Git que empiece a seguir el nuevo documento. 

```sh
git commit -m "Primer programa electoral de Gitlandia"
```

```
[main (root-commit) 799ade1] Primer programa electoral de Gitlandia
 1 file changed, 17 insertions(+)
 create mode 100644 programa.md
 ```
 ```sh
 git log --oneline
```
```
commit 799ade11fdaef8aea03278b987dba33e9489c94e (HEAD -> main)
Author: AUTOR CON CORREO
Date:   Thu Oct 29 09:22:48 2026 +0100

    Primer programa electoral de Gitlandia
```
- Se ha registrado el commmit

---

## Fase 3. Primer escándalo: las promesas cambian

### Paso 8. Modificar una promesa
- El Ministerio ha cambiado la línea de lunes de festivos a "Los lunes serán festivos únicamente si no hay trabajo pendiente." Cambia la línea, guardarla y usar 
```sh
git diff
``` 
para ver la diferencia.

```
-2. Los lunes serán festivos.
+2. Los lunes serán festivos únicamente si no hay trabajo pendiente.
```
- Se ve que se ha quitado una línea y se ha añadido otra. 

```sh
git add programa.md

git commit -m "Economía limita los lunes festivos"
```
```sh
git log --oneline
```
```
commit 03499f8cebfc17010cc1707e55216c40c7a7ea7a (HEAD -> main)
Author: AUTOR Y CORREO
Date:   Thu Oct 29 09:30:28 2026 +0100

    Economía limita los lunes festivos

commit 799ade11fdaef8aea03278b987dba33e9489c94e
Author: AUTOR Y CORREO
Date:   Thu Oct 29 09:22:48 2026 +0100

    Primer programa electoral de Gitlandia
```

### Paso 9. Segunda modificación: llega la oposición
Añade al final:
```
## Nuevas medidas

6. Un unicornio eléctrico para cada familia.
7. Internet gratuito incluso dentro de los túneles.
8. Los atascos serán declarados ilegales.
9. Se eliminarán los errores 404 por decreto.
```
Ejecuta

```sh
git status
git diff
```
```
diff --git a/programa.md b/programa.md
index f6d5272..abd77b7 100644
--- a/programa.md
+++ b/programa.md
@@ -15,3 +15,10 @@ Presidente: Don Commit Permanente
 ## Lema
 
 "Prometemos hasta que se llene el disco duro".
+
+## Nuevas medidas
+
+6. Un unicornio eléctrico para cada familia.
+7. Internet gratuito incluso dentro de los túneles.
+8. Los atascos serán declarados ilegales.
+9. Se eliminarán los errores 404 por decreto.
```
- Ha visto el cambio que hemos hecho

Hacer el stage
```sh
git add programa.md
```
y observar qué se va a registrar
```sh
git diff --staged
```
```
diff --git a/programa.md b/programa.md
index f6d5272..abd77b7 100644
--- a/programa.md
+++ b/programa.md
@@ -15,3 +15,10 @@ Presidente: Don Commit Permanente
 ## Lema
 
 "Prometemos hasta que se llene el disco duro".
+
+## Nuevas medidas
+
+6. Un unicornio eléctrico para cada familia.
+7. Internet gratuito incluso dentro de los túneles.
+8. Los atascos serán declarados ilegales.
+9. Se eliminarán los errores 404 por decreto.
```

y finalmente
```sh
git commit -m "Ampliación del programa con medidas revolucionarias"
```


### Preguntas
Git diff muestra todos los cambios que no tienen el staging, es decir que no hemos hecho `git add`.  La opción `--staged` muestra los cambios que tienen staging, es decir los que están listos para un commit. Nos da la oportunidad de ver los cambios propuestos antes de hacer un commit por si hemos cometido un error o hay algo que no queremos comitear. 


---

## Fase 4. El presupuesto secreto
Crear varias otras carpetas para departamentos
```sh
mkdir -p economia comunicacion secretos
```





### Paso 10. Crear varios documentos
Crear un archivo de presupuesto
```sh
cat > economia/presupuesto.txt <<'EOF'
PRESUPUESTO GENERAL DE GITLANDIA

WiFi nacional: 500 millones
Unicornios eléctricos: 800 millones
Robot por ciudadano: 2000 millones
Publicidad institucional: 9000 millones

Total: pendiente de calcular
EO
```

Y un comunicado oficial
```sh
cat > comunicacion/nota.txt <<'EOF'
COMUNICADO DEL GOBIERNO

El presupuesto está perfectamente controlado.
Las calculadoras de la oposición están manipuladas.
EOF
```
Y un documento secreto
```sh
echo "Clave ficticia de demostración: VOTO123" > secretos/claves.txt
```

### Paso 11. Evitar archivos innecesarios con .gitignore
```sh
nano .gitignore
```
Es una lista de archivos que serán excluidos cuando hacemos un commit
Añade a la lista:
```
# Documentos internos ficticios
secretos/

# Archivos temporales
*.tmp

# Copias de seguridad
*.bak

# Configuraciones locales
.env
```
```sh
git status
```
```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        README.md
        comunicacion/
        economia/

nothing added to commit but untracked files present (use "git add" to track)
```
- Vemos que el directorio secretos no se va a publicar porque lo hemos puesto en gitignore.

Se puede verificar la regla:
```sh
git check-ignore -v secretos/claves.txt
```

```
.gitignore:2:secretos/  secretos/claves.txt
```
está incluido en el gitignore.


---

## Fase 5. Crisis institucional: recuperar los errores

### Paso 12. Recuperar un archivo modificado

### Paso 13. Recuperar un archivo eliminado

### Paso 14. Quitar un cambio del staging

### Explicación de los incidentes

### Capturas

---

## Fase 6. La hemeroteca: Git nunca olvida

### Paso 15. Investigar el historial de promesas

### Paso 16. Comparar dos momentos de la historia

### Investigación periodística
- ¿Cuál fue la promesa modificada?
- ¿En qué commit ocurrió?
- ¿Qué mensaje acompañaba al cambio?
- ¿Cómo recuperar la versión original sin modificar el proyecto actual?

### Capturas

---

## Fase 7. El Gobierno comete un error histórico

### Paso 17. Registrar una medida equivocada

### Explicación del incidente

### Capturas

---

## Fase 8. Crear una rama: el programa electoral alternativo

### Paso 18. Crear el programa de la oposición

### Paso 19. Volver al Gobierno

### Paso 20. Fusionar la oposición con el Gobierno

### Capturas

---

## Fase 9. Llega la revolución: publicar en GitHub

### Paso 21. Crear una cuenta en GitHub

### Paso 22. Vincular Git local con GitHub

### Paso 23. Publicar el primer repositorio

### Paso 24. Comprobar la publicación

### Paso 25. Crear un README para los visitantes

### Capturas

---

## Fase 10. Sincronización y clonación

### Paso 26. Simular una modificación desde otro ordenador

### Paso 27. Descargar los cambios

### Paso 28. Clonar el proyecto en otro directorio

### Última misión

### Preguntas
- ¿Qué diferencia existe entre `git fetch`, `git pull` y `git push`?

### Capturas

---

## Fase 11. El examen del ministro

### Misiones finales

- [ ] Crear `ministros.md` y registrar un commit.
- [ ] Añadir una promesa, comprobar los cambios y registrarlos.
- [ ] Recuperar el presupuesto después de una modificación accidental.
- [ ] Comprobar que Git ignora `discurso.tmp`.
- [ ] Crear la rama `reforma` y fusionarla con `main`.
- [ ] Publicar los cambios y comprobar la sincronización.
- [ ] Mostrar el historial completo con un gráfico de commits.

### Capturas

---

## Conclusión

## Repositorio de GitHub

## Referencias