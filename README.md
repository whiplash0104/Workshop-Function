# Workshop Functions

### Requerimientos:

- Cuenta de Oracle Cloud Infrastructure(test gratuito https://www.oracle.com/cloud/free/)
- Cuenta de Github (https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home)
- Poseer servidor linux con las siguientes dependencias instaladas:
  - git
  - python36-oci-cli
  - fn 

### ¿Qué vamos a hacer?
Todo el código está en https://github.com/whiplash0104/Workshop-Function


### Paso a Paso

1. Crear Cuenta en git, usuar correo empresarial o personal (github.com)


https://user-images.githubusercontent.com/14284928/219779574-ad656998-a63d-402a-87cd-b9658c84e401.mov


2. Creación de compartment con el nombre **FunctionTest**

https://user-images.githubusercontent.com/14284928/228681682-6075d0de-1227-451c-a74a-74c880238f47.mov



3. Creación de VCN con el nombre **VCNFunction** dentro del compartment **FunctionTest** recién creado

https://user-images.githubusercontent.com/14284928/228682080-54806ccb-07a4-4c7d-8ab4-3cb2ca32c705.mov


4. Creación de grupo dinámico con el nombre **FunctionGroup** con la siguiente regla

```
ALL {resource.type = 'fnfunc'}
```

https://user-images.githubusercontent.com/14284928/228682912-9d82ec85-b83e-45f2-825b-df364f230566.mov



5. Creación de políticas con el nombre **FunctionPolicies**, estas deben ir en el compartment **root** 

```
Allow dynamic-group FunctionGroup to manage functions-family in tenancy
Allow dynamic-group FunctionGroup to use virtual-network-family in tenancy
Allow dynamic-group FunctionGroup to manage repos in tenancy
Allow dynamic-group FunctionGroup to inspect object-family in tenancy
Allow dynamic-group FunctionGroup to manage objects in tenancy
Allow dynamic-group FunctionGroup to manage autonomous-database-family in tenancy
Allow dynamic-group FunctionGroup to use ons-topics in tenancy
```


https://user-images.githubusercontent.com/14284928/228683128-d2e6f437-473d-4cdc-bf33-c9a12df88eb1.mov



5. Creación de tópico con el nombre **FunctionTopic** dentro del compartment **FunctionTest**. Una vez creado el tópico se debe crear una suscripción, definir en esta el correo de cada uno.
Importante señalar que se debe aceptar la suscripción, esta llegará al correo

https://user-images.githubusercontent.com/14284928/228685015-576493a1-7664-48f5-b986-dfaf0d069ae9.mov


6. Copiar el OCID del topico, se utilizará más adelante

https://user-images.githubusercontent.com/14284928/228685250-3844778c-b99e-4b89-8c14-d5f7c91275ea.mov


7. Creación Autonomous Database dentro del compartment **FunctionTest** con los siguientes parámetros:

```
Display name: FunctionADB
Database name: functionadb
Selecionar la opción "Always Free"
Password: ClavE.012356,         La ',' va incluida en la password
```

https://user-images.githubusercontent.com/14284928/228686294-acce7ee2-2daf-4954-8456-9dbee344cabf.mov



8. Una vez creada la base de datos guardar OICD, Password y DBSVC

Estos deberían ser del tipo:
```
DBSVC: functionadb_high
DB_OCID: ocid1.autonomousdatabase.oc1.iad.XXXXXXXXXXXXXXXXXXXXXXXXXXXX
DBUSER: ADMIN
DBPWD:  ClavE.012356,
```

https://user-images.githubusercontent.com/14284928/228686821-3446ac39-8fbe-417f-96b5-863a9b6e4cb1.mov


9. Cración de token para usuario, llamarlo **FunctionToken** y guardarlo, este no se volverá a mostrar

https://user-images.githubusercontent.com/14284928/228687211-79883498-5203-40a1-b352-8158a1844d32.mov


10. Creación de la aplicación (llamarla **AppTest**) dentro del compartment **FunctionTest** usar la VCN **VCNFunction** y la subred **pública**

https://user-images.githubusercontent.com/14284928/228687760-84778199-ce53-4ef1-92a2-b732a30eb0a6.mov



11. Configuración de OCI cli (en caso de no estar instalado previamente)
```
$ sudo dnf install python36-oci-cli git ansible  -y
$ oci setup config

Enter a location for your config [/home/fbasso/.oci/config]:          **ENTER**
Enter a user OCID: ocid1.user.oc1..XXXXXXXXX                          **Pegar OCID de usuario**
Enter a tenancy OCID: ocid1.tenancy.oc1..XXXXXX                       **Pegar OCID de tenant**
Enter a region by index or name(e.g. ...                              **Selecionar la región que corresponde, en este caso us-ashburn-1**
Do you want to generate a new API ....                                **ENTER**
Enter a name for your key [oci_api_key]:                              **ENTER**
Public key written to: /home/fbasso/.oci/oci_api_key_public.pem       **ENTER**
Enter a passphrase for your private key (empty for no passphrase):    **ENTER**
Private key written to: /home/fbasso/.oci/oci_api_key.pem             **ENTER**
Fingerprint: 29:05:f7:6d:49:ce:91:e3:5e:3d:e3:74:5c:59:33:5b
Config written to /home/fbasso/.oci/config
...
```


https://user-images.githubusercontent.com/14284928/228689165-d81f73f2-8781-4767-b39d-ec9a5fb40117.mov


Una vez definidos todos los puntos anteriores copiar la llave ssh, copiar la llave pública **oci_api_key_public.pem**  dentro de APÏ Keys del usuario en la opción ** Paste Public Key**, posterior a esto, validar fingerprints creados, deberían ser los mismos

```
$ cat .oci/oci_api_key_public.pem
```

https://user-images.githubusercontent.com/14284928/228689760-0e286acf-00d6-4904-a5bc-8e813e0973d5.mov

Para testearla conexión listar Availability Domains
```
oci iam availability-domain list
```


12. Instalación de docker (en caso de no estar instalado previamente). Estos pasos corresponden a sistema Oracle Linux

```
$ sudo yum install -y yum-utils
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum update
$ sudo yum install docker-ce docker-ce-cli containerd.io -y
$ sudo systemctl enable --now docker
```

13. Habilitar usuario para la ejecutar docker, cambiar **USUSARIO** por el personal
```
$ sudo usermod -aG docker USUSARIO
```

14. Instalar Function cli (fn). Referencia: https://fnproject.io/tutorials/install/

```
curl -LSs https://raw.githubusercontent.com/fnproject/cli/master/install | sh
```

15. Configurar fn cli, reemplazar XXXX por los valores de cada uno
```
$ fn list context
$ fn create context oci --provider oracle
$ fn list context
$ fn use context oci
$ fn update context oracle.compartment-id ocid1.compartment.oc1..XXXXXXXXXXXXX    OCID de compartment
$ fn update context api-url https://XXXXXXXX.oci.oraclecloud.com                  Validar dependiendo de cada región en https://docs.oracle.com/en-us/iaas/api/#/en/functions/20181201/
$ fn list apps
```
https://user-images.githubusercontent.com/14284928/228692363-17c406d2-ace9-4a06-b6cf-d9764aeace0f.mov


16. Clonar repositorio git git@github.com:whiplash0104/Workshop-Function.git

```
$ git clone https://github.com/whiplash0104/Workshop-Function.git
```
https://user-images.githubusercontent.com/14284928/228693614-da10593d-759e-4845-bd78-95cac2b95570.mov


17. Modificar los siguientes parámetros del archivo **func.yaml** dentro del directorio Workshop-Function:
```
  TOPIC_OCID: ocid1.onstopic.oc1.iad.XXXXXXXXXXXXXXXX
  ADB_OCID: ocid1.autonomousdatabase.oc1.iad.XXXXXXXX
  COMPANY: XXXXXXX
  DBPWD: XXXXXXXX
  DBSVC: XXXXXXX_high
```
Estos parámetros fueron almacenados durante los pasos 5, 7 y 8

https://user-images.githubusercontent.com/14284928/228694241-6e29edd5-a76e-408b-b571-7969560dc49f.mov


18. Creación de registry con el nombre **apptest** dentro del compartment **FunctionTest**

https://user-images.githubusercontent.com/14284928/228694517-62f99cfa-4db6-46c5-bc65-45c0c2e0f257.mov


19. Login de docker a registry recién creado
```
$ docker login  -u 'NAMESPACE/USUARIO' -p 'TOKEN'  REGISTRY_URL

Donde:
NAMESPACE:        Es el namespace donde fue creado el registry
USUARIO:          Es el usuario de OCI
TOKEN:            Es el token que se creó en el paso 9
REGISTRY_URL:     Viene en base a la región https://docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm ej: gru.ocir.io
```
https://user-images.githubusercontent.com/14284928/228696297-7621d96b-2622-4957-8b9f-e41c8d597166.mov


20. Deploy de aplicación, ejecutar comando desde el directorio Workshop-Function
```
$ cd Workshop-Function
$ fn update context registry REGISTRY_URL/NAMESPACE
$ fn -v deploy --app apptest
```

Esta tarea tarde aproximadamente 4 minutos, denemos esperar el mensaje
```
...
f46abc7ec396: Layer already exists
0.0.125: digest: sha256:6f848be04f6b7db8064c437f8907ebe4e9e446a0f8ebef5437b61dc41a2329e5 size: 1782
Updating function apptest using image iad.ocir.io/id5xe8my93ee/apptest:0.0.125...
Successfully created function: apptest with iad.ocir.io/id5xe8my93ee/apptest:0.0.125
```


21. asd

22. asdsda

23. das

24. d

25. a

26. das


