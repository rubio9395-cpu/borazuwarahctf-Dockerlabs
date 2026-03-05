![foto](1.png)

En este laboratorio se analiza la máquina **BorazuwarahCTF**, clasificada con un nivel de dificultad **muy fácil** y disponible en la plataforma **DockerLabs**.

![foto](2.png)

Se realiza una prueba de conectividad mediante **ping** hacia la máquina objetivo. La respuesta confirma que el host se encuentra activo y devuelve un valor **TTL=64**, el cual es comúnmente asociado a sistemas basados en **Linux/Unix**. Este indicador permite inferir que el sistema operativo del objetivo probablemente pertenece a esta familia.

![foto](3.png)

Se lleva a cabo un escaneo de puertos mediante **Nmap** para identificar los servicios disponibles en el sistema objetivo. 
El análisis revela que los puertos **22 (SSH)** y **80 (HTTP)** se encuentran abiertos, lo que sugiere la presencia de un servicio de administración remota y un servidor web activo.

![foto](4.png)

Se accede al servicio web mediante un navegador introduciendo la **dirección IP del objetivo**. Al analizar el contenido de la página, se identifica una imagen como único recurso visible.
Dicha imagen es descargada al sistema local con el fin de realizar un **análisis de metadatos**, ya que estos pueden contener información oculta o pistas relevantes para avanzar en el proceso de enumeración.

![foto](5.png)

Se procede a analizar los metadatos de la imagen utilizando la herramienta **ExifTool**. 
El análisis revela la presencia del usuario **borazuwarah** dentro de la información EXIF del archivo, lo que podría indicar un posible **nombre de usuario válido en el sistema**, útil para futuras pruebas de autenticación o enumeración de servicios.

![foto](6.png)

Una vez identificado el posible nombre de usuario **borazuwarah**, se procede a realizar un ataque de **fuerza bruta** contra el servicio **SSH** utilizando la herramienta **Hydra**, con el objetivo de descubrir credenciales válidas.
Tras ejecutar el ataque, se logra identificar la contraseña **123456**, permitiendo así obtener acceso al sistema mediante el usuario previamente descubierto.


![foto](7.png)

Se establece acceso al sistema mediante SSH. Posteriormente, se ejecuta el comando `sudo -l` con el objetivo de enumerar los privilegios sudo del usuario actual.
La salida indica la regla `NOPASSWD: /bin/bash`, lo que permite ejecutar `/bin/bash` como superusuario sin requerir contraseña. Aprovechando esta configuración.

![foto](8.png)

Investigamos posibles vectores de escalada de privilegios relacionados con `/bin/bash`, identificando que puede ejecutarse con privilegios elevados mediante `sudo`, lo que permite obtener una shell con privilegios de superusuario.

![foto](9.png)

Finalmente, ejecutamos `sudo /bin/bash` para aprovechar el permiso `NOPASSWD`. Una vez obtenida la shell con privilegios elevados, verificamos el contexto de usuario mediante el comando `whoami`, confirmando acceso como `root`.   :)))))))))
