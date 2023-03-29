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


4. Creación de grupo dinámico con el nombre **FunctionGroup** con las siguientes políticas

´´´
    Allow dynamic-group FunctionGroup to manage functions-family in tenancy
    Allow dynamic-group FunctionGroup to use virtual-network-family in tenancy
    Allow dynamic-group FunctionGroup to manage repos in tenancy
    Allow dynamic-group FunctionGroup to inspect object-family in tenancy
    Allow dynamic-group FunctionGroup to manage objects in tenancy
    Allow dynamic-group FunctionGroup to manage autonomous-database-family in tenancy
    Allow dynamic-group FunctionGroup to use ons-topics in tenancy
´´´


5. asddas
