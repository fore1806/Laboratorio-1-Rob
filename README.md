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

![](https://github.com/fore1806/Laboratorio-1-Rob/blob/main/VIDEOS-FOTOS/Robot-1-cuadrante-x(+)-y(-).MOV)

#### Trabajo con el Robot

Una vez el código se encontraba listo, para comenzar con el trabajo con el Robot
