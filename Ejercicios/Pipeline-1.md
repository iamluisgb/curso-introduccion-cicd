# Tarea: Desarrollo de tu primera pipeline CI/CD en GitLab

## Objetivo
Crear y entender una pipeline básica de CI/CD en GitLab que simule la construcción y prueba de un laptop.

## Prerrequisitos
- Cuenta en GitLab
- Conocimientos básicos de Git
- Familiaridad con la línea de comandos

## Pasos a seguir

### 1. Crear un nuevo proyecto en GitLab
- Inicia sesión en tu cuenta de GitLab
- Crea un nuevo proyecto llamado "mi-primera-pipeline"

### 2. Crear el archivo .gitlab-ci.yml
- En la raíz del repositorio, crea un nuevo archivo llamado `.gitlab-ci.yml`
- Copia el siguiente contenido en el archivo:

```yaml
stages:
    - build
    - test

variables:
    BUILD_FILE_NAME: laptop.txt 

build laptop:
    image: alpine
    stage: build
    script:
        - echo "Building a laptop"
        - mkdir build
        - touch build/computer.txt 
        - echo "Mainboard" >> build/$BUILD_FILE_NAME
        - cat build/$BUILD_FILE_NAME
        - echo "Keyboard" >> build/$BUILD_FILE_NAME
        - cat build/$BUILD_FILE_NAME
    artifacts:
        paths:
            - build

test laptop:
    image: alpine
    stage: test
    script:
        - test -f build/$BUILD_FILE_NAME
        - grep "Mainboard" build/$BUILD_FILE_NAME
        - grep "Keyboard" build/$BUILD_FILE_NAME
```

### 3. Entender la estructura del archivo .gitlab-ci.yml
Analiza cada sección del archivo y responde a las siguientes preguntas:

a) ¿Qué significan las secciones `stages` y `variables`?
b) ¿Cuál es el propósito de usar `image: alpine` en los jobs?
c) ¿Qué hace cada comando en la sección `script` del job `build laptop`?
d) ¿Para qué sirve la sección `artifacts`?
e) ¿Qué está haciendo el job `test laptop`?

### 4. Hacer commit y push del archivo .gitlab-ci.yml
- Realiza un commit del archivo .gitlab-ci.yml a tu repositorio
- Haz push de los cambios a GitLab

### 5. Observar la ejecución de la pipeline
- Ve a la sección "CI/CD > Pipelines" en tu proyecto de GitLab
- Observa cómo se ejecuta tu pipeline
- Revisa los logs de cada job para ver qué está sucediendo

### 6. Modificar la pipeline
Ahora que has visto cómo funciona la pipeline básica, realiza las siguientes modificaciones:

a) Agrega un nuevo componente al laptop (por ejemplo, "Screen") en el job `build laptop`
b) Crea un nuevo job en la etapa de test que verifique la existencia de este nuevo componente
c) Modifica la variable `BUILD_FILE_NAME` para que use un nombre diferente

### 7. Documentación y reflexión
Escribe un breve documento (máximo una página) que incluya:
- Una explicación de qué hace esta pipeline
- Los desafíos que encontraste al crear y modificar la pipeline
- Cómo crees que esta pipeline podría mejorarse o expandirse en un proyecto real

## Entrega
- El enlace a tu repositorio de GitLab con el archivo .gitlab-ci.yml final
- El documento de reflexión
