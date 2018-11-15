# Modulos en Go.

Go propone la implementacion de los Go Modules para eliminar la necesidad de configurar el GOPATH y utilizar una carpeta vendor para manejar dependencias.

Al utilizar Modulos no habra que depender del GOPATH para hacer proyectos en Go, Tambien servira como un gestor de dependencias.

## GIT Tags.

Go Modules utiliza la estructura de versionamiento semantico en GIT para manejar la version de modulos, Ejemplo : v1.0.0

    V     1     .     2     .     1
        Major       Minor       Patch

Patch : Es la seccion de el versiona que se usa para la correccion de Bugs.

Minor : Se usa cuando se añade funcionalidades pero sigue siendo compatible con la API ya existente.

Major : Cuando se hacen cambios incompatibles con la API existente.

## Variable de entorno.

Actualmente en la version 1.11 de Go, Los modulos estan en fase experimental, Para activarlos se necesita exportar la siguiente variable de entorno :

    export GO111MODULE=on

# Pasos para Crear un modulo en Go.

1. Inicializamos nuestro modulo como un repositorio GIT y configuramos el remote para GitHub :

    git init  &&

    git add . &&

    git commit -m "Primer commit" &&

    git remote add origin git@github.com:ArthurNavaH/TuModulo

2. Se crea la etiqueta para el commit actual para la primera version del modulo :

    git tag v1.0.0

3. Se suben las etiquetas creadas :

    git push --tags

4. Se crea una rama para desarrollar (Opcional) :

    git checkout -b v1

5. Se sube la rama :

    git push origin v1


6. Inicializamos nuestro modulo con la ruta del repositorio en GitHub:

    go mod init github.com/arthurnavah/TuModulo

## Actualizar un modulo en Go. (Minor y Patch)

1. Realizar los cambios en el codigo.

2. Hacer un git commit del cambio :

    git add .
    git commit -m "Cambio"

3. Hacer un git tag dependiendo el cambio que se hizo, Ejemplo : v1.0.1 , En el caso de ser la correccion de un error :

    git tag v1.0.1

4. Subir los cambios :

    git push origin v1

5. Subir las etiquetas :

    git push --tags

## Actualizar un modulo en Go. (Major)

1. Crear la rama de la nueva version Major :

    git checkout -b v2

2. Entrar al go.mod y agregar un /v2 a la ruta del modulo :

    module github.com/arthurnavah/TuModulo/v2

3. Realizar los cambios en el codigo.

4. Hacer un git commit del cambio :

    git add .
    git commit -m "Cambio"

5. Hacer un git tag dependiendo el cambio que se hizo, Ejemplo : v2.0.0 , En el caso de ser un cambio Major :

    git tag v2.0.0

6. Subir los cambios :

    git push origin v2

7. Subir las etiquetas :

    git push --tags

# Pasos para usar un modulo en Go.

1. Declarar el paquete actual como un modulo :

    go mod init

2. Agregar la ruta del modulo en el import(). (Teniendo en cuenta su versionado (Ejemplo: /v2)) :

    import (
        "github.com/arthurnavah/TuModulo/v2"
    )

Al compilar se crea el archivo go.mod, Que tendra la seccion require, Con el cual tendra un listado de los modulos requeridos, Esto sirve como control de dependencias.

El archivo go.sum es la lista de dependencias utilizadas especificando su version.

## Actualizar un modulo utilizado.

Si el modulo tiene una nueva version Major, Se bajara automaticamente al compilar, En el caso de no ser Major, Ejecutar los siguientes comandos :

1. Actualizar modulos :

    go get -u

2. Limpiar lista de modulos que ya no se estan utilizando :

    go mod tidy