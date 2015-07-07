# Porque lo necesito.

* Probar server
* Ambientes de Desarrollo Repicables
* Optimizacion
* Tener las mismas dependencias que Produccion

.notes: These are my notes, hidden by default

--------------------------------------------------

# Ejemplo

--------------------------------------------------

# Proyecto Papita con Huevo

    !Python

    | Inicio del Proyecto |
    | ------------------- |
    | PHP 5.3             |
    | Apache 2            |
    | MySQL 5             |
    | Apache Solr 4.4     |

--------------------------------------------------

# Proyecto Papita con Huevo

====================
3 meses del proyecto
--------------------

PHP 5.5
Nginx
Apache Solr 4.8
Redis
MongoDB 3.0

=====================

--------------------------------------------------

# Casos de la vida Real

*RRHH:* Hoy Empieza el nuevo desarrollador

*1era Tarea:*

* Levantar su entorno de desarrollo

--------------------------------------------------

# Tiempo de Tarea

*1 dia o mas*

--------------------------------------------------

# Cambios en la tecnologia:

* Salio la nueva version de Apache Solr.
* no usaremos Apache Solr usaremos Elastic Search

--------------------------------------------------

# que Hacemos?

--------------------------------------------------

# Vagrant

--------------------------------------------------

# Vagrant

* Herramienta Open Source
* Multiplataforma
* Permite Virtualizar ambientes
* Soporta Virtualbox ...
* Configurable

--------------------------------------------------

# Installacion

    http://downloads.vagrantup.com/

    Vagrant usa como dependencia un virtualizador de software (virtualbox)

    https://www.virtualbox.org/

--------------------------------------------------

# Como Comienzo:

* Crear el VagrantFile (Describe los recursos, tipo de Maquina y Software que vamos a usar)

    mkdir test-vagrant
    cd test-vagrant
    vagrant init

--------------------------------------------------

# Box:

    Es la imagen del sistema operativo que usaremos.

    podemos descargar de este enlace:

    .. code-block::
        http://www.vagrantbox.es/

     como somos amantes de **debian** usaremos algo parecido ubuntu

     .. code-block::

        http://files.vagrantup.com/precise32.box

--------------------------------------------------

# Agregamos el Box:

    .. code-block::

         vagrant box add nombre_del_box http://url_del_box.box

    .. code-block::

         vagrant box add precise32 http://files.vagrantup.com/precise32.box

    **comprobemos**

    .. code-block::

         vagrant box list

--------------------------------------------------

# Usemos el Box:

    **cambiemos cosas en el Vagrantfile**

    busquemos:

    .. code-block::

        config.vm.box = "base"

    y lo cambiamos por:

    .. code-block::

        config.vm.box = "precise32"

--------------------------------------------------

# Levantemos el Ambiente:

    **Ahora si preparados**

    .. code-block::

        vagrant up

    ahora para instalar las dependencias es:

    .. code-block::

        vagrant ssh

--------------------------------------------------

# Ejemplo Instalar Git:

    .. code-block::

        vagrant ssh
        sudo apt-get update
        sudo apt-get install git

    Listo eso es Todo, Aplausos :P

--------------------------------------------------

# Gracias:

--------------------------------------------------

# Preguntas:

* Esto me ayuda a tener entornos repicables?
* La instalacion pesa mucho, no lo puedo tener en un repo.
* para que hacer todo esto si puedo usar simplemente VirtualBox.
* Donde esta la Automatizacion? ...
* Que estafador ... (El Gringo de Go ...)

--------------------------------------------------

# Exacto eso no es todo:

--------------------------------------------------

# Comandos de Vagrant:

    vagrant up
    vagrant init
    vagrant destroy
    vagrant halt
    vagrant provision
    vagrant ssh
    vagrant status

--------------------------------------------------

# Configuracion de Vagrant:

--------------------------------------------------

# Configuracion Network:

         guest_config.vm.network :private_network, ip: "192.168.33.10"
         guest_config.vm.network "public_network"

--------------------------------------------------

# Enrutamiento de Puertos:

         guest_config.vm.network :forwarded_port, guest: 80, host: 8888, auto_correct: true
         guest_config.vm.network :forwarded_port, guest: 3306, host: 8889, auto_correct: true
         guest_config.vm.network :forwarded_port, guest: 5432, host: 5433, auto_correct: true

--------------------------------------------------

# Configuracion de PC:

         guest_config.vm.hostname = "guest"
         guest_config.vm.provider :virtualbox do |v|
             v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
             v.customize ["modifyvm", :id, "--memory", "1024"]
         end

--------------------------------------------------

# Sincronizacion de Carpetas con NFS:

        guest_config.vm.synced_folder "./", "/var/www", {:mount_options => ['dmode=777','fmode=777']}

--------------------------------------------------

# Exportar Box:

       vagrant up
       (setup)
       vagrant halt
       vagrant package
       mv package.box ~/boxes/my_box.box

--------------------------------------------------

# Provision:

--------------------------------------------------

# Tipos de Provision:

* Shell
* Puppet
* Puppet Server
* Chef
* Chef Server
* Ansible
* Fabric

--------------------------------------------------

# Puppet:

    guest_config.vm.provision :puppet do |puppet|
        puppet.manifests_path = "provision/puppet/manifests"
        puppet.manifest_file  = "init.pp"
        puppet.module_path = "provision/puppet/modules"
    end

--------------------------------------------------

# Demo:

--------------------------------------------------

# Vagrant Halt:

## Preguntas?

--------------------------------------------------

# Beneficios:

* Nos Ayuda a bajar la Barrera de entrada al Proyecto
