
Develop Guides
==============

Good Practices
--------------
TDD
Security first
No secrets

Dev Approaches
--------------
Seguridad y Vulnerability-less
Docker as Dev Environment
Micro-frontends
Backend for frontends
 
Code control
------------
### Concepto
Una metodología que proporciona un mejor control y administración del código de un proyecto de software, es aquella que adopta los conceptos de Gitflow. Con esta implementación es posible ordenar las tareas de los equipo de desarrollo, asi como las historias asociadas en un proyecto ágil.

### Alcance
Permite gestionar el código relacionado a las tareas de los equipos de desarrollo y tod@s aquellos que trabajan sobre el proyecto, ya sea en teams o de forma individual.

### Uso de Ramas
Siguiendo las definiciones de Git, la historia de cambios puede ser separado en ramas de trabajo. Según Gitflow, las ramas a utilizar son:
**Principales:** Aquellas que son permanentes en la historia del proyecto.
    - **develop:** Rama con historia principal de desarrollo.
    - **master:** Rama con historia de producción. *Bloqueada para ser modificada por desarrolladores.*

**Auxiliares:** Aquellas de carácter temporal utilizadas para ordenar y separar los cambios previos a una liberación a las historias principales de la cual se origina. 
    - **feature/:** Rama para el desarrollo de cambios y nuevas funcionalidades. Auxiliar, temporal.
    - **bugfix/:** Rama para la solución de issues menores en desarrollo.
    - **support/:** Rama para la solución de issues técnicos en desarrollo
    - **release/:** Rama para la entrega de versiones a producción. Auxiliar, temporal.
    - **hotfix/:** Rama para la solución de issues críticos pero no funcionales  en producción. Auxiliar, temporal.
​

![](img/ramas.png)

**Tags:** Hitos de control. Definen un punto en la historia del proyecto.


![](img/tags.png)

### Tag Formulae
#### SemVer (semantic versioning, https://semver.org/lang/es/)
+ **MAJOR:** version when you make incompatible API changes,
+ **MINOR:** version when you add functionality in a backwards compatible manner, and
+ **PATCH:** version when you make backwards compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the *MAJOR.MINOR.PATCH* format.

![](img/9b954050-4dea-4863-844c-b3f73a3ba25a.png)

### Roles de gestión Git
**Dueño:**
    - Superadministrador de TODOS los grupos.
    - Crea y configura sub-grupos, ramas e integraciones.

**Mantenedor:**
    - Administrador de sub-grupos
    - Puede:
        - Crear sub-grupos.
        - Crear Proyectos.
        - Habilitar integraciones, tokens y group-runners.
        - Trabajar en ramas protegidas (commit y merge request)

**Desarrollador:**
    - Puede:
        - Crear Proyectos
        - Hacer clone, pull and push
        - Crear ramas NO protegidas.
        - Crear y aprobar commit y merge request a ramas NO protegidas.

**Visor (Reporter):**
    - Puede:
        - Ver código, ramas, tags y merge request.
        - Clonar proyecto pero NO editar

### Proceso
#### Iniciar proyecto/repositorio
- **De manera local.**
    - Posicionarse en la carpeta que contendrá el proyecto: git init
    - Crear rama de desarrollo
        - Si tiene git flow instalado*:
            ```git flow init```
        - Si NO tiene git flow instalado:
            ```git checkout -b develop```
    - Crear repositorio en servidor
    - Agregar url de repositorio en servidor
        ```git remote add origin git@urlrepositorio.git```
    - Subir primer código (o agregar README.md)
        ```git add .```
        ```git commit -m "primer commit"```
        ```git push -u origin master```

- **En el servidor de git:**
    - Crear repositorio
    - Crear rama develop
    - Activar plugin de ChatOps / gitbot correspondiente.
    - Clonar repositorio en local
        ```git clone URL_REPOSITORIO```
 
#### Trabajar con feature o historia de cambio
- Clonar repositorio:
    ```git clone```
- Posicionarse en develop
    ```git checkout develop```
- Crear rama de cambio
    ```git checkout -b feature/myfeature```
- Una vez hecho algunos cambios en el código hacer punto de control (commit de estos cambios)
    git add .
    git commit -m “descripción commit”
- Luego de varios commit de control y para precaver pérdida de trabajo es necesario subir la nueva rama de trabajo y los cambios realizados *(cabe recordar que en git los commit son locales mientras no se haga "push" al repositorio remoto)*.
    - Si la rama de trabajo no existe se debe enviar al repositorio remoto
        ```git push --set-upstream origin feature/myfeature```
    - Si la rama ya existe en lo sucesivo basta con enviar el código
        ```git push (en las publicaciones sucesivas)```

![](img/e75b2493-5fc4-4dbb-8ea5-d4f14aeea14d.png)

- Una vez el cambio o la historia puede ser cerrado se debe integrar en la rama de desarrollo.
    - Esto requiere previamente:
        - que la rama feature contenga cambios que otras historias han integrado en "develop"
        - que la rama feature esté sin cambios "por commitear"
        - que la rama feature integre los cambios de la rama develop que hayan sido integrados por otro feature.
        - que la rama feature integre cambios que otr@(s) desarrollador(es) haya subido a la rama remota.
            ```git pull origin remote```
            ```git checkout develop```
            ```git pull origin remote```
            ```git checkout feature/myfeature```
            ```git merge develop```
            ```git checkout develop```
            ```git merge feature/myfeature```
- Una vez integrado la historia de cambios en desarrollo se requiere eliminar rama en el repositorio local y remoto
    ```git branch -d feature/myfeature```
    ```git push origin :feature/myfeature```

#### Liberar releases
```git checkout develop```
```git checkout -b release/vX.X.X```
```git add .```
```git commit -m “descripción commit”```
```git push --set-upstream origin release/vX.X.X```
```git push (en las publicaciones sucesivas)```
```git checkout master```
```git merge release/vX.X.X```
```git branch -d release/vX.X.X```
```git push origin :release/vX.X.X```
```git pull```
```git checkout develop```
```git merge release/vX.X.X```
```git checkout master```
```git merge release/vX.X.X```
```git branch -d release/vX.X.X```
```git push origin :release/vX.X.X```
```git tag -a vX.X.X -m “new version vX.X.X"```
```git tag vX.X.X```
```git tag -a vX.X.X 1a2b3c```
```git push origin vX.X.X```
```git push origin --tag```

#### Trabajar con Hotfixes
```git checkout master```
```git checkout -b hotfix/myfix```
```git add .```
```git commit -m “descripción commit”```
```git push --set-upstream origin hotfix/myfix```
```git push```
```git checkout develop```
```git merge hotfix/myfix```
```git checkout master```
```git merge hotfix/myfix```
```git branch -d hotfix/myfix```
```git push origin :hotfix/myfix```
```git tag -a vX.X.X -m “new version vX.X.X-myfix"```
```git push origin vX.X.X-myfix```

### Flujo

 
Security Test
-------------
### Clair: 
Herramientas SAST que analiza contenedores en busca de vulnerabilidades.

### OWASP Depedency-check: 
analiza vulnerabilidades en los componentes de aplicaciones basadas en java, .NET, php, ruby y python.

### Detect-secrets: 
detecta y previene el almacenamiento de contraseñas y secretos en el código fuente.

### Sonarqube: 
Analiza el código fuente en busca de vulnerabilidades, malas prácticas y errores de codificación. Permite general estadísticas de coverage, calidad y seguridad del código.

### Veracode: 
Es una aplicación/servicio tanto SAST como DAST, que analiza aplicaciones en busca de vulnerabilidades y otros riesgos de seguridad.

### Gitlab AutoDevOps: 
herramienta integrada a gitlab que permite realizar validaciones de seguridad, como check de dependencias, almacenamiento de secretos y análisis de vulnerabilidades.

### NPM Audit: 
Plugin de NPM que permite analizar estado de las dependencias/módulos de una aplicación basada en NodeJS y entregar un reporte del estado y tipo de vulnerabilidades.

### OWASP Depedency-check Maven Plugin: 
plugin POM que añade análisis de dependencias durante la compilación de una aplicación basada en Maven/Springboot (existe un símil como módulo de NodeJS).

### Docker Bench for Security