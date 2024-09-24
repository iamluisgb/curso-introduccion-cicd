# Curso Introduccion CI/CD


1. **Introducción**:
   - ¿Es necesaria la entrega continua? ¿Por qué es importante? CD es crucial para asegurar que el software pueda ser entregado de manera segura y automática en cualquier momento.
   - CI/CD son procesos que permiten integrar cambios de código de múltiples ingenieros y garantizar que el código funcione adecuadamente.

2. **Pipeline básica**:
   - Componentes fundamentales de una **pipeline** de CI/CD, como tareas de **linting**, **testing**, **construcción de imágenes** y **despliegue**.
   - La pipeline se automatiza a través de webhooks para que los cambios en el código desencadenen la ejecución de pruebas y despliegues de forma inmediata.

3. **Control de versiones**:
   - La importancia del control de versiones en CI/CD, donde se rastrean los cambios en el código, permitiendo revertir errores y asegurar que los repositorios estén siempre en un estado publicable.

4. **Linting**:
   - El **linting** detecta errores de programación y estilo de código, se revisan estrategias para aplicar estas herramientas de manera efectiva en proyectos grandes o con código heredado.

5. **Testing**:
   - Se enfatiza la importancia de las pruebas en CI/CD, proponiendo enfoques como el uso de la pirámide de pruebas, que divide las pruebas en **unitarias**, **de integración**, y **extremo a extremo**, para asegurar una cobertura adecuada sin comprometer la velocidad.

6. **Construyendo de forma segura y confiable**:
   - Se proponen estrategias para garantizar construcciones fiables, como la **automatización total del proceso de construcción** y el uso de la **construcción como código**, almacenando todo en un sistema de control de versiones.
   - Es crucial mantener el código en un estado siempre desplegable, permitiendo la detección temprana de errores mediante **pruebas automatizadas** en entornos reproducibles.
