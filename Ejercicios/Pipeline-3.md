# Tarea: Crea y Ejecuta una Pipeline CI/CD Completa

## Descripción General
Implementarás una pipeline CI/CD para una aplicación Python simple. La pipeline constará de cuatro etapas principales: linting, pruebas, construcción de la imagen Docker y despliegue en un servidor remoto.

## Preparación del Entorno

Antes de comenzar con la implementación de la pipeline, configura las siguientes cuentas y servicios:

1. **Crear una cuenta en Docker Hub**:
   - Visita [Docker Hub](https://hub.docker.com/) y crea una cuenta gratuita.
   - Esta cuenta te permitirá almacenar y compartir tus imágenes Docker.
   - Crea un repositorio

2. **Crear una cuenta en DigitalOcean**:
   - Regístrate en [DigitalOcean](https://www.digitalocean.com/).
   - Puedes aprovechar el período de prueba gratuito.

3. **Crear un Droplet en DigitalOcean**:
   - En tu panel de DigitalOcean, crea un nuevo Droplet.
   - Elige una imagen de Ubuntu (preferiblemente la versión LTS más reciente).
   - Selecciona el plan más básico para este proyecto.
   - Elige una región cercana a tu ubicación.
   - En la sección de autenticación, selecciona "SSH key".

4. **Generar una clave SSH**:
   - En tu máquina local, abre una terminal.
   - Ejecuta el comando: `ssh-keygen`
   - Sigue las instrucciones para guardar la clave (puedes dejar la passphrase en blanco para este proyecto).
   - Añade la clave pública generada a tu Droplet de DigitalOcean.

5. **Configurar variables en GitLab CI/CD**:
   - En tu proyecto de GitLab, ve a Settings > CI/CD > Variables.
   - Añade las siguientes variables:
     - `REGISTRY_USER`: Tu nombre de usuario de Docker Hub
     - `REGISTRY_PASS`: Tu contraseña de Docker Hub
     - `SSH_KEY`: El contenido de tu clave privada SSH (la que generaste en el paso anterior)
     - `DEPLOY_SERVER_IP`: La dirección IP de tu Droplet de DigitalOcean

6. **Instalar Docker en tu Droplet**:
   - Conéctate a tu Droplet vía SSH: `ssh root@tu_ip_droplet`
   - Sigue las [instrucciones oficiales de Docker](https://docs.docker.com/engine/install/ubuntu/) para instalar Docker en Ubuntu.

## Conceptos Clave

Antes de implementar la pipeline, familiarízate con estos conceptos clave:

### Make y Makefile
Make es una herramienta de automatización que lee instrucciones de un archivo llamado Makefile. En esta pipeline, usaremos `make lint` y `make test`. Un Makefile típico podría ser:

```makefile
lint:
    flake8 .

test:
    pytest

.PHONY: lint test
```

### Docker-in-Docker (DinD)
Permite ejecutar Docker dentro de un contenedor Docker. Lo usamos en la etapa de build para construir imágenes Docker dentro del entorno de CI de GitLab.

### Services en GitLab CI
Son contenedores adicionales que se ejecutan junto con los jobs. Usamos el servicio Docker-in-Docker para proporcionar un demonio Docker.

### Certificados de Docker
Son importantes para la comunicación segura entre el cliente Docker y el demonio Docker. GitLab CI/CD genera automáticamente estos certificados cuando se usa Docker-in-Docker.

### Docker Hub
Es un servicio de registro de Docker que utilizamos para almacenar nuestras imágenes Docker.

## Pasos de la Tarea

### 1. Configuración del Proyecto
a) Crea un nuevo proyecto en GitLab.

b) Incluye este [código](https://gitlab.com/iamluisgbfamytec/gitlab-cicd-hackaboss).


### 2. Creación del Dockerfile
Crea un Dockerfile en la raíz de tu proyecto para construir la imagen de tu aplicación:

```dockerfile
FROM python:3.9-slim-buster

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

### 3. Creación del Makefile
Crea un Makefile en la raíz de tu proyecto con los siguientes comandos:

```makefile
lint:
    flake8 .

test:
    pytest

.PHONY: lint test
```

### 4. Implementación de la Pipeline
Crea un archivo `.gitlab-ci.yml` en la raíz de tu proyecto con la siguiente estructura:

```yaml
variables:
  IMAGE_NAME: $REGISTRY_USER/demo-app
  IMAGE_TAG: $CI_COMMIT_SHORT_SHA

stages:
  - lint
  - test
  - build
  - deploy

run_lint:
  stage: lint
  image: python:3.9-slim-buster
  before_script:
    - pip install flake8
  script:
    - flake8 .

run_tests:
  stage: test
  image: python:3.9-slim-buster
  before_script:
    - pip install -r requirements.txt
    - pip install pytest
  script:
    - pytest

build_image:
  stage: build
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - docker login -u $REGISTRY_USER -p $REGISTRY_PASS
  script:
    - docker build -t $IMAGE_NAME:$IMAGE_TAG .
    - docker push $IMAGE_NAME:$IMAGE_TAG

deploy:
  stage: deploy
  before_script:
    - chmod 400 $SSH_KEY
  script:
    - ssh -o StrictHostKeyChecking=no -i $SSH_KEY root@$DEPLOY_SERVER_IP "
        docker login -u $REGISTRY_USER -p $REGISTRY_PASS &&
        docker pull $IMAGE_NAME:$IMAGE_TAG &&
        docker stop demo-app || true &&
        docker rm demo-app || true &&
        docker run -d --name demo-app -p 5000:5000 $IMAGE_NAME:$IMAGE_TAG"
```

### 5. Prueba y Depuración
a) Haz commit de todos tus cambios y súbelos a GitLab.

b) Observa la ejecución de tu pipeline en GitLab CI/CD.

c) Si hay errores, revisa los logs y haz las correcciones necesarias.

### 6. Verificación del Despliegue
a) Una vez que la pipeline se complete con éxito, verifica que tu aplicación esté funcionando correctamente en tu Droplet de DigitalOcean.

b) Accede a tu aplicación a través del navegador usando la IP de tu Droplet y el puerto correcto.