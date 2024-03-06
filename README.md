# Proofs Exercise:

1. Terraform Plan

![image](https://github.com/santiagoarevalo/azfunction-tf/assets/71450411/d67dd783-3c34-4d3d-82b3-3dd1f8dd41f9)

2. Terraform Apply

![image](https://github.com/santiagoarevalo/azfunction-tf/assets/71450411/bde7e046-bbd9-473c-afe5-6380631029d5)

3. Azure Function App Enabled

![image](https://github.com/santiagoarevalo/azfunction-tf/assets/71450411/c5d630e6-7f96-4710-ab80-bed61c61dd67)

4. App Running

![image](https://github.com/santiagoarevalo/azfunction-tf/assets/71450411/508a997a-697e-446a-a330-15dd8e010561)


# azfunction-tf
###  **Infracestructura como codigo**

* Utilizar archivos de definicion => todas las herramientas de infracestructura como codigo tienen un formato propio para definir infracestructura
* Los procesos y los sitemas deben ser autodocumentados => al utlizar el enfoque de infracestrutura como codigo nos permite obviamnete reutilizar el codigo, por lo que si otras personas que pueden llegar a utlizar nuestro codigo, es importante que este este documentado para que las personas sepan, que es lo que esta haciendo el modulo que nosotros estamos creando
* versionar todas las cosas => lo cual nos permite trazar los cambios que se hacen, para al momento de realizar un cambio y que pueda salir mal, podamos regresar en el tiempo el archivo y regesarlo a cuando era estable.
* preferir cambios pequeños => realizar cambios pequeños con el fin de no impactar tanto
* mantener los servicios continuamente disponibles

**beneficios de la infracestructura como codigo**

* Crear rapidamente y bajo demanda => como nosotrsos creamos un unico archivo de defincion de infracestructura, en donde almacenamos todas nuestras configuraciones, esto ahra que nuestro esfuerzo solo lo vamos a realizar al principio, puesto que podres crear n veces la infracestructura que necesitemos ya con nuerstro archivo de definicion
* Atomatizacion => una vez tegamos nuestro archivo de deifinicon, podremos utlizar herramintas dde **continuos integration** para automatizar la infracestructura
* Visibilidad y trazabilidad => al hacer uso del versionamiento en la infracestructura como codigo, nos permitira una vibilidad y trazabilidad dado que todos los cambio quedaran registrados
* Ambitnes homogeneos => podemos crear varios ambientes a partir de el mismo archivo de defincion que tengamos, solo cambiando algunos parametros

### **¿que es terraform?**

es una herramienta open source, creada por Hashicorp, desrarrollada en Go, con el fin de crear y administrar infracestructura cono codigo.

Una vez de conecta al provedor de nube, este sabe que recursos tiene disponible para crear en ese provedor.
````
    provider "aws" {
        region = "us-east-1"
    }
    resource "ws_instance" "web" {
        ami
    }
    "${data.aws_ami.ubuntu.id}"
        instance_type = "t2.micro"

        tags = {
            Name = "HelloWorld"
        }
````

Podemos definir un **provider** para identificar cual es el provedor de nube que utilizaremos, teniedo varios cloud provider que soporta terraform.
Tenemos **resource** que hace referencia a un recurso de una instancia en aws, podiendo ver todos los elementos que nosotros podemos enviar a una instancia, esto dependera del api o recurso que nos estemos conectando

### ***Terraform, crear una instancia ec2 aws***

* Se Debe crear un archivo denominado main.tf no es necesario que se llame main, solo por comvencion, simplemente basta con tener la extension .tf par que terraform lo pueda interpretar.
* Terraform maneja su propio lenguaje que es desarrollado por Hashicorp, que se llama **HCL => Hashicorp Configuration Language** para la definicion de estos archivos
```tf
/*lo primero que tendremos que hacer es declara el provider al que nos vamos a conectar, es decir nuetro provedor de nube que en este caso sera aws, detro de este se coloca atributos como la region a la que nos estamos conectando, en este caso virginia, y las credenciales, en caso de no estar como vaaraibles de entorno*/
provider "aws" {
  region = "us-east-1"
  access_key = "mi-access-key"
  secret_key = "mi-secret-key"
}

/*una vez definido el provider al que nos conectaremos, la region y credenciales, procederemos a indicar el tipo de recurso que crearemos, recordando que cada provider tiene su dieferen froma de nombrer sus recurcos y demas...
en este caso decimos que crearemos una instancia de aws y seguido va el identificador de dicha instanica o nombre, abrimos llaves, e indicamos los parametros que usaremos la creacion de dicha instancia*/

resource "aws_instance" "devops-tf-instance" {

    /*inicamos que crearemos la isntancia desde una ami que tengamos, para ello al paramtro ami, le definimos el id de nustra ami desde la cual crearemos la instancia*/
  ami = "ami-0ca0c67309196175e"

  /*mecionamos el tipo de instancia que estaremos creando, en este caso una t2-micro*/
  instance_type = "t2.micro"

  /*y podemos proceder asignarles tags, para la instancia, como pueden ser variables este parametro sera un objeto, donde dentro de el, definimos las diferentes tag, bajo clave valor*/
  tags = {
    Name = "demo-terraform"
    Enviroment = "Dev"
  }
}
```
* Una vez que tengamos creado el archivo de definicion, tenemos que correr el comando ```terraform init``` el cual nos permitirá inicializar nuestro entorno, es decir que dependiento del provider, descargara los plugin necesarios
* Podemos proceder a escribir el comando ```terraform validate``` para validar que nuestra sintaxis en nuestro archivo de defincion esta correctamente bien para la creacion de la ifracestrutura como codigo que estamos creando **lac**
* ```terrafomr plan``` este comando lo que nos permitira es hechar un vistaso a la infraestructura que estamos creando
* tambien podremos palicar un formateo a nuestro codigo en el archivo de definicion que estanos creando con terrafom, utlizando el comando ```terraform ftm```
* ```terraform apply``` y finalmente para nosotros proceder a crear la infraestrutura

### ***manejo de variables en terraform***

al momento de querer que nuestro archivo de definicion de infraestrutura de terraform sea escalable, si queremos que sea reutilizable, crear mas instancias, que alguien mas lo pueda utilizar, no debemos tener los valores hardcode en codigo, sino que por el contario, como principio de laC es que al momento de escojer una herramienta debemos tener en cuenta que esta sea parametrizable, que podamos psar los valores por varibles.
* terrafor nos permite creara varaibles de tipo:
* string
* number
* boolean
* map
* list
si no declaramos el tipo, el valor poe default sera un string, esto en las versiones anteriores de terraform, pero ahora nosotros podemos no especificar el tipo, y eterraform podere ainteligenteme diferer que tipo es, al momento que nosotros le estemo pasando el valor. **por buenas practicas es mejor especificar el tipo de varaible que vamos a usar**
* para declarar varibles en terrafor lo hacemos con el uso de la palrba recervada **varaible** seguido entre comillas el nombre de l avariable, abrimos llaves y dentro escribimos las propiedasd de esa varible, es decir:
* el type => tipo de la varible
* el default => para nosotros darle un valor por default a la variable, en caso de que no le den un valor al momento del uso
* description => dado que los sitemas tiene que estar documentados como prinicipio de desarrollo de software, esta varible nos es de gran utlidad en terraform, para nosotros agregar una descripcion e indicar a que esta haciendo referencia nuestra varible
* exiten mas propiedades, de igual manera no se nos obliga a colocar todas esa propiedas, perfectamente podemos solo declarar la varibles sin colarle todas esas propiedades
* **nota:** como buena practica al momento de declarar las variables se recomienda realizar esto en una archivo, para no mezclarlo con la definicion de la infracestructura que estemos realizando
```tf
variable "ami_id" {
  default = ""
  description = "esta varaible es el ide del ami"
  #type = string | number | boolean | map | list
  type = string
  
}

variable "instance_type" {
  type = string
  description = "este es el tipo de instancia a crear"
}

variable "tags" {
  type = map
  description = "esta son las etiquestas definidas para la instancia"
}
```
**asignarle valor a nuestras varibles** para esto se puede hacer de tes formas:
* con la utlizancion de varibles de entorno
* mandarlas por comando
* agregando un archivo .tfvars el cual manejariamos como key-propertie, en el cual solamente tenemos que colocar el nombre de la varible que creamos y colocarle su respetivo valor, or ejemplo:
```tf
ami_id        = "ami-0ca0c67309196175e"
instance_type = "t2.micro"
tags = {
  Name       = "devops-tf"
  Enviroment = "Dev"
}
```

* podemos utilizar varibales en terraform una vez ya hallan sido creadas, con el uso del prefijo **var** y el nombre de la variable por ejemplo ```vara.ami_id```
```tf
provider "aws" {
  region     = "us-east-1"
  access_key = "mi_access_key"
  secret_key = "mi-secret_key"
}
resource "aws_instance" "terraform-platzi-instance" {
  ami           = var.ami_id
  instance_type = var.instance_type
  tags           = var.tags
}
```
* una vez tengamos nuestros 3 archivos con la sintaxis validada con terrafomr, podemos proceder a crear nuestra infraestrutura, pero tendremos que pasa el archivo donde se le asgino el valor a las varibles esto lo hacemos con la bandera **-var-file** y el path del archivo para la contruccion, por ejemplo:
 ```terraform apply --var-file .\dev.tfvars```
**nota:** en el caso que queramos crear o elimnar la insfraestrucrua, para que no se noes pregunte si queremos realizarlo, podemos hacer uso de labandera **-auto-approve**, por ejemplo:
```terraform destroy --var-file .\dev.tfvars -auto-aprove``
