# Primer Laboratorio - Robótica Industrial No. 1

Primer laboratorio de la asignatura Robótica de la Universidad Nacional de Colombia en su sede Bogotá.

## Autores

|              Nombre              |GitHub nickname|
|----------------------------------|---------------|
|    Andrés Felipe Forero Salas    |[fore1806](https://github.com/fore1806)|
|  Iván Mauricio Hernández Triana  |[elestrategaactual](https://github.com/elestrategaactual)|

## Introducción

La realización de tareas de forma autónoma cobran cada vez mayor importancia en la vida del ser humano, y es aquí donde la utilización de sistemas robóticos automatizados capaces de seguir con fidelidad y presición una serie de directrices definidas por un usuario se convierte en una necesidad dentro de la formación profesional de un ingeniero mecatrónico. De esta forma, mediante la realización de este laboratorio, se genera un primer acercamiento a la programación de instrucciones para el robot con el fin de obtener la escritura de un conjunto de letras sobre un tablero inclinado.





## Solución planteada.

### Diseño y Manufactura de la herramienta
La realización de la herramienta de trabajo se planteó en tres fases la primera consistió en la búsqueda de posibles requerimientos para esta los cuales principalmente consistieron en el tamaño del marcador, la geometría del plato del manipulador IRB-140 y la habilitación de un recorrido interno del marcador el cual mediante un resorte debería responder a fuerzas aplicadas sobre el mismo.

La segunda fase consistió en el diseño y modelaje 3D mediante software CAD de la herramienta para esto se realizó la adquisición de las medidas mediante el uso de un calibrador de los elementos adquiridos los cuales fueron un marcador y un resorte; adicional con el fin de que el diseño cumpliera un diseño modular para posteriores optimizaciones se dividió en tres partes para su posterior ensamble; estas partes fueron; la base, el cuerpo y el cabezal obteniéndose los siguientes modelos.


    
    ![herramientas](https://user-images.githubusercontent.com/51063748/188247661-9e952753-16d4-4eb1-805e-c50a31a0f45d.png)

    
    ![base](https://user-images.githubusercontent.com/51063748/188247765-216ed949-6dd4-40a0-83f5-2f7475df3b7a.png)

    
    ![cabezal](https://user-images.githubusercontent.com/51063748/188247766-469c5305-05a1-40bc-afe1-c9df12339ba5.png)
    
    
    ![cuerpo](https://user-images.githubusercontent.com/51063748/188247767-974aab33-8129-4268-a564-16191c19270d.png)



Finalmente, la última fase consistió en la fabricación y ensamble de la herramienta para esto se decidió utilizar un proceso de manufactura aditiva por los beneficios de esta a baja escala y al ser esta herramienta un prototipo el cual se debe seguir optimizando para futuras aplicaciones. 

### Diseño e Implementación de la solución

#### Primer acercamiento

El equipo de trabajo comenzó el laboratorio, realizando una calibración de la herramienta para lo cual se utilizó el método de tres puntos. Este proceso fue realizado a mano, lo que no dotaba la precisión suficiente el proceso de calibración; sin embargo, le permitió al equipo realizar algunos trazos sobre el tablero de la base del robot y de esta manera comprobar la pertinencia de la herramienta diseñada y la fiabilidad de su manufactura. En la siguiente imagen se muestran las coordenadas del TCP obtenidos a través de este proceso de calibración.

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/tool.JPG)

TCP calibración manual de la herramienta con tres puntos

#### Trabajo con RobotStudio

El proceso en RobotStudio comienza con la creación del controlador virtual, que permite incluir en la estación adicionalmente el manipulador IRB 140 para el caso del laboratorio. A continuación se debían importar los modelos CAD de la herramienta y el tablero inclinado sobre el cual se habian tallado las letras "I" y "F" correspondientes a nuestras iniciales, para la importación de estos modelos se utilizaron los archivos en formato *"sat"*, ya que el formato *"stl"* modificaba las dimensiones de los objetos, produciendo resultados indeseados.

El proceso continuaba con la creación de la herramienta, para lo cual se debía ubicar la herramienta diseñada en el origen de coordenadas del sistema y orientarla apropiadamente para que esta coincidiera con la forma en la que se unirá mecánicamente al robot en el LABSIR. Una vez creada la herramienta y ubicado su correspondiente TCP se ubica en el robot.

Para el caso del tablero, se ubica dentro del espacio de trabajo de tal forma que cada uno de los puntos de interés puedan ser alcanzados de manera apropiada por el robot. Posteriormente se crea el WorkObject utilizando tres puntos consecutivos sobre los vertices de la tabla.

Una vez creado este WorkObject, debe ser seleccionado en el entorno de RobotStudio. En este punto, se crean las trayectorias de las dos letras, con la ayuda del creador de trayectorias automáticas del software, seleccionando cada una de las aristas de las mismas. Este comando creará los *Target* necesarios referentes al WorkObject, para orientar de mejor manera el robot, se busca que estos objetivos se alineen respecto a la herramienta diseñada. Finalmente se configura la velocidad y la tolerancia de las trayectorias, en un primer intento, se seleccionó una velocidad de *v200* y una tolerancia *fine*; sin embargo, al experimentar problemas que serán comentados más adelante, se decidió utilizar una velocidad de *v150* y una tolerancia *z5*.

Finalmente se crearon dos target adicionales para el correcto funcionamiento del robot, una posición *Home* en la que las posiciones angulares de cada uno de los ejes del robot es 0°; y una segunda posición denominada *MidPoint*, en la que el robot fue posicionado con la postura deseada, y utilizando la herrramienta de *Programar posición* de RobotStudio se almacenaron dichas coordenadas.

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/RobotHomeRobotStudio.png)

Robot en la posición Home

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/RobotMidPointRobotStudio.png)

Robot en la posición MipPoint

Posterior a la creación de estos dos Target, se debian definir las dos trayectorias que permitieran alcanzar estos Targets, llamadas *Homing* y *MidGoing*. Para culminar el proceso, se accedio al archivo *main* del *Module1* dentro del entorno RAPID del RobotStudio, definiendo cual será el orden en que las trayectorias deben seguirse. Se muestra a continuación este apartado de código.

```AMPL
    PROC main()
        Homing;
        MidGoing;
        IRoute;
        FRoute;
        MidGoing;
        Homing;
    ENDPROC
```

El video de la simulación de este programa puede ser consultado en el [enlace](PONER URL))|

#### Trabajo con el Robot

Una vez el código se encontraba listo, para comenzar con el trabajo con el Robot, comenzó por actualizar los controladores del mismo, posteriormente se debió cargar el programa al controlador IRC5, a través del flex pendant. Al seleccionarlo, se continuaba el proceso con seleccionar la herramienta y el WorkObject definidos en el software RobotStudio. Dada la alta fidelidad entre el diseño CAD y la manufactura de la herramienta, el proceso de calibración del TCP de la herramienta se obvió; sin embargo, ubicar de manera precisa el tablero inclinado en la posición definida en el software, ubiese sido un despropósito, aprovechando la presencia del resorte en la herramienta, se realizó un procedimiento de definición de la misma con tres puntos de manera manual.

Esto se realizó en el Robot número 2 del LABSIR, donde dada la presencia de una banda transportadora en el espacio de trabajo del robot, se debió ubicar el tablero en el cudrante de x positivo e y negativo. Esto generó una gran cantidad de problemas para el equipo de trabajo, pues el código estaba pensado para realizarse en la sección positiva del eje y; a pesar de que los puntos de las trayectorias y sus recorridos, fueron correctamente definidos en el WorkObject, al ejecutar cada una de las instrucciones, el controlador indicaba una alerta de advertencia, como se muestra a continuación.

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/Warning.jpeg)

Después de un exhaustivo análisis y de la orientación de diferentes expertos, se concluyó que la advertencia era arrojada debido a que al cambiar el cuadrante, la forma de orientarse por parte del robot seleccionada en el software, no era la más optima y este problema se agudizaba por la mínima tolerancia definida para cada uno de los trazos al definir el parámetro *z* como *fine*. A continuación se muestra fotos de la ubicación del objeto de trabajo, el resultado final y un video del robot ejecutando el programa.

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/WorkObjectRobot2.jpg)

PONER FOTOOOOOOOOOOOOOOOOOOOOOO

PONER VIDEOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO

Con lo anterior en mente, se logró utilizar el robot número 1 que contaba con el cuadrante especificado libre para la realización de la rutina, y posterior a un ajuste de velocidad y tolerancias, se logró dibujar la trayectoria de manera continúa con un resultado muy favorable como se observa en el siguiente video.



PONER FOTOOOOOOOOOOOOOOOOOOOOOO

PONER VIDEOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO

Finalmente, se realizó un último trazo sobre el tablero, modificando nuevamente su ubicación y adicionalmente su orientación. Al ubicarlo nuevamente en la sección negativa del eje y, el equipo de trabajo era consciente que se debía realizar una instrucción a la vez. A continuación se muestran algunas imágenes de la ubicación del tablero, el resultado final y un video del robot en funcionamiento.

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/WorkObjectRobot1.jpg)

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/FOTOS/IF-Robot1-orientacionDif.jpg)

POONER VIDEOOOOOOOOOOOOOOOOOOOOO
https://user-images.githubusercontent.com/51063748/188247526-80a4a80a-81eb-4860-9bf5-a3321f45c4a1.mp4
## Conclusiones

-   Se evidenció la importancia de conocer el cuadrante sobre el cual el Robot trabajará en base al sistema de coordenadas ubicado en su base, lo cual determinará la configuración requerida para el robot, y permitirá alcanzar los diferentes target de manera óptima.

-   Se encuentra que a pesar de que la tolerancia *z5* no permite obtener ángulos de 90° casi perfectos, como si se lograron con el parámetro en *fine*, le permite al robot realizar trayectorias de manera más eficiente y permitiría obtener velocidades superiores.

-   Se observó la gran importancia de la definición de objetos de trabajo (WorkObject) para realizar tareas repetitivas, en diferentes ubicaciones espaciales dentro del espacio de trabajo.

-   Se concluye que el éxito en la realización del laboratorio partió de un proceso bien definido de diseño y manufactura de la herramienta de trabajo.
