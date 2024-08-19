## Creación de Claves usando OpenSSL

Las claves son fundamentales para permitir la comunicación entre Azure y las diferentes organizaciones de Salesforce donde se requiere ejecutar el despliegue. 

Antes de iniciar con los comandos a ejecutar, es importante que el administrador encargado de generar las claves cuente con un repositorio centralizado, organizado por clientes. Por ejemplo: ORGS_KEY/$CLIENTE/$ALIAS_ORG. Dentro de cada carpeta se crearán las diferentes claves con el nombre distintivo de la instancia.

<img width="707" alt="image" src="https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/bae34afb-c3e8-4935-b2c0-8a2c98b98744">

Una vez ubicados en la carpeta que corresponde a la ORG, procedemos a ejecutar los siguientes comandos:
(Por favor, recuerde sustituir $ALIAS_ORG con el nombre distintivo de la organización).

```
openssl genrsa -des3 -passout pass:x -out $ALIAS_ORG.pass.key 2048
```
```
openssl rsa -passin pass:x -in $ALIAS_ORG.pass.key -out $ALIAS_ORG.key
```
```
openssl req -new -key $ALIAS_ORG.key -out $ALIAS_ORG.csr
```
Este comando va a solicitar los siguientes datos:

- Country Name: CO
- State or Province Name: TOL
- Locality Name: IBA
- Organization Name: NESPON
- Organizational Unit Name: GLOBAL
- Common Name: UAT
- Email Address: juan.obando@nespon.com
- Password: *** (Se recomienda ingresar la misma de la ORG)
- Company Name: GLOBAL

```
openssl x509 -req -sha256 -days 365 -in $ALIAS_ORG.csr -signkey $ALIAS_ORG.key -out $ALIAS_ORG.crt
```
Este es el listado de archivos que deberían haberse creado después de ejecutar los cuatro scripts:

<img width="376" alt="image" src="https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/24f32b8a-3a1b-4b5d-9812-10d2e32c8ccd">

## Configuración de Connected App en Salesforce

1. En el Quick Find, escriba "App Manager" y selecciónelo.
2. Click en New Connected App.
3. Complete los detalles necesarios. Asegúrese de habilitar la configuración de OAuth y proporcionar acceso completo (completo) para los ámbitos de OAuth.
    
    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/c9531e02-0455-45f0-8893-987f782b9689)
    * Callback URL: http://localhost:1717/OauthRedirect
    * Use digital signatures: Cargue el archivo .CRT que se creo en los archivos generados usando OpenSSL.

    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/66c88b83-0a6a-416c-839d-848e6a85690f)
    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/30cce86c-ec94-479e-bbe8-72c7edb11472)

4. Después de la creación, tome nota de la clave del Consumer Key y del Consumer Secret. Son necesarios más adelante.
  
    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/e71133ef-513a-4c1d-b508-553e61520b40)

5. En la Connected APP creada, haga clic en "Manage" y luego en "Edit Policies". Asegúrese de seleccionar la siguiente opción:

    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/dd459b40-4a5d-4ddb-a30f-ce5e5940d50f)

6. En la sección 'Manage', seleccione el perfil del usuario con el cual se realizarán los despliegues.

    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/411f0161-f051-475f-b44a-b3a1c4bb4995)

7. En el Quick Find, escriba "Auth Providers" y selecciónelo.

    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/7e75e14e-70d2-4f15-b1cb-69ded7832c65)

   Cree uno y complete los siguientes detalles:

  * Provider Type: Open ID Connect
  * Name: DevOps Center Azure
  * Consumer Key: (Se generan en el paso #4 )
  * Consumer Secret:	(Se generan en el paso #4 )
  * Authorize Endpoint URL: https://login.salesforce.com/services/oauth2/authorize
  * Token Endpoint URL: https://login.salesforce.com/services/oauth2/token

## Configuración de Variables en AZURE

1. En el repositorio se debe subir archivo .KEY que se creo en los archivos generados usando OpenSSL.

    <img width="117" alt="image" src="https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/2efd0f99-3a72-4b91-9728-b23da9306670">

2. Se crean las variables a utilizar en el PIPE-LINE.
  * SF_$ALIAS_ORG_ClientKey: Salesforce Connected App Consumer Key
  * SF_$ALIAS_ORG_OrgUrl: URL de la instancia, ejemplo. (https://globalseguros--qa01.sandbox.my.salesforce.com/)
  * SF_$ALIAS_ORG_UserName: Usuario con el que se ejecutarán los despliegues, ejemplo. (ivan@cloudblue.us.global.prod.uat)

    ![image](https://github.com/CBJuanCarlos/CICD_STRATEGY/assets/142612672/260b747e-ef3f-4955-aa91-4423c14ce33c)

3. sas
4. assas
   
  

   
