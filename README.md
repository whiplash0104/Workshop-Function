# Workshop Functions

### Requerimientos:

- Cuenta de Oracle Cloud Infrastructure(test gratuito https://www.oracle.com/cloud/free/)
- Cuenta de Github (https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home)
- Poseer servidor linux con las siguientes dependencias instaladas:
  - git
  - python36-oci-cli 

### ¿Qué vamos a hacer?
saddasasd


### Paso a Paso

0. Crear Cuenta en git, usuar correo empresarial o personal (github.com)


https://user-images.githubusercontent.com/14284928/219779574-ad656998-a63d-402a-87cd-b9658c84e401.mov


1. Clonar git en servidor linux


https://user-images.githubusercontent.com/14284928/228680988-aacf437d-e9bd-4cf2-9558-ee82b494db6f.mov


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

Enter a location for your config [/home/fbasso/.oci/config]:    **ENTER**

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
$ sudo chown USUARIO:docker /var/run/docker.sock
```

14. sda
15. as
16. as
17. asd
18. asd
19. as
20. dasd
21. asd



