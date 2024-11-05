# Gitflow

Gitflow es un modelo de ramas para Git creado por Vincent Driessen. Aquí, la discusión tratará sobre los requisitos y casos de uso de Gitflow.<br />
El flujo de trabajo de Gitflow define un modelo de ramas estricto diseñado en torno al ciclo de lanzamiento del proyecto, proporcionando un marco robusto para gestionar proyectos más grandes. Gitflow es ideal para proyectos con un ciclo de lanzamiento programado y para la práctica recomendada de DevOps en entrega continua. Asigna roles muy específicos a las diferentes ramas y define cómo y cuándo deben interactuar. Utiliza ramas individuales para preparar, mantener y registrar los lanzamientos.

## Implementación

1. **Ramas Develop y Master**: En lugar de una sola rama master, Git Flow utiliza dos ramas para registrar el historial del proyecto. Se basa en dos ramas principales con una duración infinita, a saber, master y develop:
  - **Rama Master**: La rama master contiene el código de producción y almacena el historial oficial de lanzamientos.
  - **Rama Develop**: La rama develop contiene el código en preproducción y sirve como una rama de integración para las características.
  - **Creando una rama Develop**:<br />
  
### Sin usar las extensiones de Gitflow:
```
git branch develop
git push -u origin develop
```
### Usando las extensiones de Gitflow:
Al usar la biblioteca de extensiones de Gitflow, ejecutar `git flow init` en un repositorio existente creará la rama *develop*.
```
git flow init
```

2. **Rama de Característica (Feature Branch)**: Cada nueva característica debe residir en su propia rama, que puede ser subida al repositorio central para respaldo o colaboración. Las ramas de características usan la última versión de *develop* como su rama principal. Cuando una característica está completa, se fusiona nuevamente en *develop*. Las características nunca deben interactuar directamente con la rama *master*.
   - **Crear una rama de característica**:  
     Sin extensiones de git-flow:
     ```
     git checkout develop
     git checkout -b feature_branch
     ```
     Con las extensiones de Gitflow:
     ```
     git flow feature start feature_branch
     ```
   - **Finalizar una rama de característica**:  
     Sin extensiones de git-flow:
     ```
     git checkout develop
     git merge feature_branch
     ```
     Con las extensiones de git-flow:
     ```
     git flow feature finish feature_branch
     ```

3. **Rama de Lanzamiento (Release Branch)**: Una vez que *develop* haya adquirido suficientes características para un lanzamiento (o se acerque una fecha de lanzamiento predeterminada), se crea una rama de lanzamiento a partir de *develop*. Crear esta rama inicia el próximo ciclo de lanzamiento, por lo que no se pueden agregar nuevas características después de este punto—solo deben realizarse correcciones de errores, generación de documentación y otras tareas orientadas al lanzamiento. La rama de lanzamiento puede bifurcarse de *develop* y debe fusionarse tanto con *master* como con *develop*.  
   Usar una rama dedicada para preparar los lanzamientos permite que un equipo pula el lanzamiento actual mientras otro continúa trabajando en las características para el siguiente lanzamiento.
   - **Crear una rama de lanzamiento**:  
     Sin las extensiones de git-flow:
     ```
     git checkout develop
     git checkout develop
     git checkout -b release/0.1.0
     ```
     Al usar las extensiones de Gitflow:
     ```
     git flow release start 0.1.0
     ```
     Se cambió a la nueva rama 'release/0.1.0'.
   - **Finalizar una rama de lanzamiento**:  
     Sin las extensiones de git-flow:
     ```
     git checkout master
     git merge release/0.1.0
     ```
     Con las extensiones de git-flow:
     ```
     git flow release finish 0.1.0
     ```

4. **Rama de Corrección Rápida (Hotfix Branch)**: Las ramas de mantenimiento o "hotfix" se utilizan para corregir rápidamente los lanzamientos en producción. Las ramas hotfix son necesarias para actuar inmediatamente ante un estado no deseado de la rama *master*. Las ramas hotfix son muy similares a las ramas de lanzamiento y características, excepto que se basan en *master* en lugar de *develop*. Esta es la única rama que debe bifurcarse directamente de *master*. Una vez que se completa la corrección, debe fusionarse tanto en *master* como en *develop* (o en la rama de lanzamiento actual), y la rama *master* debe ser etiquetada con un número de versión actualizado.
   - **Crear una rama de corrección rápida**:  
     Sin las extensiones de git-flow:
     ```
     git checkout master
     git checkout -b hotfix_branch
     ```
     Con las extensiones de git-flow:
     ```
     git flow hotfix start hotfix_branch
     ```
   - **Finalizar una rama de corrección rápida**:  
     Sin las extensiones de git-flow:
     ```
     git checkout master
     git merge hotfix_branch
     git checkout develop
     git merge hotfix_branch
     ```
     Con las extensiones de git-flow:
     ```
     git branch -D hotfix_branch
     git flow hotfix finish hotfix_branch
     ```

---

## Ventajas

- Asegura un estado limpio de las ramas en cualquier momento del ciclo de vida de un proyecto.
- La convención de nombres de las ramas sigue un patrón sistemático, lo que facilita su comprensión.
- Tiene extensiones y soporte en las herramientas de Git más utilizadas.
- Ideal para mantener múltiples versiones en producción.
- Excelente para un flujo de trabajo basado en lanzamientos.
- Ofrece un canal dedicado para correcciones rápidas en producción.

## Desventajas

- El historial de Git puede volverse ilegible.
- La división de las ramas *master* y *develop* se considera redundante y hace que la integración y entrega continua (CI/CD) sea más difícil.
- No se recomienda cuando solo se mantiene una versión en producción.

## Resumen

Aquí discutimos el flujo de trabajo de Git Flow. Git Flow es uno de los muchos estilos de flujos de trabajo en Git que tú y tu equipo pueden utilizar. Vamos a resumir todo el flujo de trabajo de Git Flow:
1. Se crea una rama *develop* a partir de *master*.
2. Se crean ramas de características a partir de *develop*.
3. Cuando una característica está completa, se fusiona con la rama *develop*.
4. Se crea una rama de lanzamiento a partir de *develop*.
5. Cuando la rama de lanzamiento se completa, se fusiona tanto en *develop* como en *master*.
6. Si se detecta un problema en *master*, se crea una rama de corrección rápida desde *master*.
7. Una vez que la corrección rápida se completa, se fusiona tanto en *develop* como en *master*.