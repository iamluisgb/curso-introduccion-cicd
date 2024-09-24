# Tarea: Crea y ejecuta tu segunda pipeline de CI/CD en GitLab

## Prerrequisitos

Antes de comenzar, asegúrate de tener:

1. Un proyecto en GitLab para el que quieras usar CI/CD.
2. El rol de Mantenedor o Propietario para el proyecto.
3. Si no tienes un proyecto, puedes crear uno público gratis en https://gitlab.com.

## Pasos

### 1. Asegúrate de tener runners disponibles

Los runners son agentes que ejecutan tus trabajos de CI/CD.

- Si estás usando GitLab.com, puedes omitir este paso, ya que GitLab.com proporciona runners de instancia.
- Para ver los runners disponibles, ve a Configuración > CI/CD y expande la sección Runners.
- Deberías ver al menos un runner activo con un círculo verde a su lado.

Si no tienes un runner:
1. Instala GitLab Runner en tu máquina local.
2. Registra el runner para tu proyecto, eligiendo el ejecutor shell.

### 2. Crea un archivo .gitlab-ci.yml

Este archivo YAML es donde defines las instrucciones para GitLab CI/CD.

Para crear el archivo:

1. En la barra lateral izquierda, selecciona tu proyecto.
2. Ve a Código > Repositorio.
3. Selecciona la rama donde quieres hacer el commit (por defecto, master o main).
4. Haz clic en el icono "+" y selecciona "Nuevo archivo".
5. Nombra el archivo como `.gitlab-ci.yml`.
6. Copia y pega el siguiente código de ejemplo:

```yaml
build-job:
  stage: build
  script:
    - echo "¡Hola, $GITLAB_USER_LOGIN!"

test-job1:
  stage: test
  script:
    - echo "Este trabajo prueba algo"

test-job2:
  stage: test
  script:
    - echo "Este trabajo prueba algo, pero tarda más que test-job1."
    - echo "Después de los comandos echo, ejecuta el comando sleep durante 20 segundos"
    - echo "que simula una prueba que dura 20 segundos más que test-job1"
    - sleep 20

deploy-prod:
  stage: deploy
  script:
    - echo "Este trabajo despliega algo desde la rama $CI_COMMIT_BRANCH."
  environment: production
```

7. Haz clic en "Commit changes".

### 3. Ver el estado de tu pipeline y trabajos

1. Ve a Construir > Pipelines.
2. Deberías ver una pipeline con tres etapas.
3. Haz clic en el ID de la pipeline para ver una representación visual.
4. Puedes ver detalles de un trabajo haciendo clic en su nombre.

## Consejos para .gitlab-ci.yml

- Usa el editor de pipeline para editar tu archivo .gitlab-ci.yml.
- Cada trabajo contiene una sección de script y pertenece a una etapa.
- Usa la palabra clave `needs` para ejecutar trabajos fuera del orden de las etapas.
- Usa la palabra clave `rules` para especificar cuándo ejecutar o saltar trabajos.
- Mantén la información persistente entre trabajos y etapas con `cache` y `artifacts`.
- Usa la palabra clave `default` para especificar configuraciones adicionales que se aplican a todos los trabajos.