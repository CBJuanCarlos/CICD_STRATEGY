## Creación de Claves usando OpenSSL

Las claves son fundamentales para permitir la comunicación entre Azure y las diferentes organizaciones de Salesforce donde se requiere ejecutar el despliegue. 

Antes de iniciar con los comandos a ejecutar, es importante que el administrador encargado de generar las claves cuente con un repositorio centralizado, organizado por clientes. Por ejemplo: ORGS_KEY/$CLIENTE/$ALIAS_ORG. Dentro de cada carpeta se crearán las diferentes claves con el nombre distintivo de la instancia.

<img width="707" alt="image" src="https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/bae34afb-c3e8-4935-b2c0-8a2c98b98744">

Una vez ubicados en la carpeta que corresponde a la ORG, procedemos a ejecutar los siguientes comandos:

  1. openssl genrsa -des3 -passout pass:x -out $ALIAS_ORG.pass.key 2048

  2. openssl rsa -passin pass:x -in $ALIAS_ORG.pass.key -out $ALIAS_ORG.key
