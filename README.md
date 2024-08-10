[Spanish/[English](https://github.com/AndresZU/reconocimiento-de-senas-con-mediapipe/blob/main/README_EN.md)]

# Reconocimiento de señas utilizando Mediapipe
Este es un programa de ejemplo que reconoce las señas realizadas con las manos y dedos utilizando los puntos clave detectados.
![mqlrf-s6x16](https://user-images.githubusercontent.com/37477845/102222442-c452cd00-3f26-11eb-93ec-c387c98231be.gif)

El repositorio contiene los siguientes puntos:
* Programa de ejemplo
* Modelo de reconocimiento de manos
* Modelo de reconocimiento de movimiento.
* Datos de entrenamiento para el reconocimiento de señas y el libro de entrenamiento respectivo.
* Datos de entrenamiento para el reconocimiento de movimiento y el libro de entrenamiento respectivo.

# Requisitos
* mediapipe 0.8.4
* OpenCV 4.6.0.66 o mayor
* Tensorflow 2.9.0 o mayor
* protobuf <3.20,>=3.9.2
* scikit-learn 1.0.2 or Later (Solo para los procesos de validacion del modelo)
* matplotlib 3.5.1 or Later (Solo para los procesos de validacion del modelo)

# Demo
Para ejecutar el programa con la webcam del sistema se ejecuta el siguiente comando
```bash
python app.py
```

Las siguientes opciones pueden ser especificadas cuando se ejecuta el demo
* --device<br>Se refiere al numero de dispositivo de la camara (Default：0)
* --width<br>Anchura de la interfaz de captura (Default：960)
* --height<br>Altura de la interfaz de captura (Default：540)
* --use_static_image_mode<br>Si se desea utilizar el modo de captura estatica para Mediapipe (Default：Unspecified)
* --min_detection_confidence<br>
Limite de confianza de deteccion (Default：0.5)
* --min_tracking_confidence<br>
Limite de confianza de seguimiento (Default：0.5)

# Directorio
<pre>
│  app.py
│  keypoint_classification.ipynb
│  point_history_classification.ipynb
│
├─model
│  ├─keypoint_classifier
│  │  │  keypoint.csv
│  │  │  keypoint_classifier.hdf5
│  │  │  keypoint_classifier.py
│  │  │  keypoint_classifier.tflite
│  │  └─ keypoint_classifier_label.csv
│  │
│  └─point_history_classifier
│      │  point_history.csv
│      │  point_history_classifier.hdf5
│      │  point_history_classifier.py
│      │  point_history_classifier.tflite
│      └─ point_history_classifier_label.csv
│
└─utils
    └─cvfpscalc.py
</pre>
### app.py
Este es un programa de muestra para inferencia.
Además, datos de aprendizaje (puntos clave) para el reconocimiento de señales con las manos,
También puede recopilar datos de entrenamiento (historial de coordenadas del dedo índice) para el reconocimiento de gestos con los dedos.

### keypoint_classification.ipynb
Este es un script de entrenamiento del modelo para el reconocimiento de señales con las manos.

### point_history_classification.ipynb
Este es un script de entrenamiento del modelo para el reconocimiento de movimiento de los dedos.

### model/keypoint_classifier
Este directorio almacena archivos relacionados con el reconocimiento de señales manuales.
Se almacenan los siguientes archivos.
* Datos de entrenamiento (keypoint.csv)
* Modelo entrenado (keypoint_classifier.tflite)
* Datos de etiqueta (keypoint_classifier_label.csv)
* Módulo de inferencia (keypoint_classifier.py)

### model/point_history_classifier
Este directorio almacena archivos relacionados con el reconocimiento de gestos con los dedos.
Se almacenan los siguientes archivos.
* Datos de entrenamiento (point_history.csv)
* Modelo entrenado (point_history_classifier.tflite)
* Datos de etiqueta (point_history_classifier_label.csv)
* Módulo de inferencia (point_history_classifier.py)

### utils/cvfpscalc.py
Este es un módulo para la medición de FPS.

# Entrenamiento
Al reconocimiento de señales con las manos y el reconocimiento de gestos con los dedos se le pueden agregar y cambiar los datos de entrenamiento y volver a entrenar el modelo.

### Entrenamiento de reconocimiento de señas
#### 1.Recoleccion de datos
Presione "k" para entrar al modo de deteccion（Demostrado por 「MODE:Logging Key Point」）<br>
<img src="https://user-images.githubusercontent.com/37477845/102235423-aa6cb680-3f35-11eb-8ebd-5d823e211447.jpg" width="60%"><br><br>
Si preciona los numeros del "0" al "9", los datos van a ser agregados al "model/keypoint_classifier/keypoint.csv" como se muestra abajo.<br>
La primera columna: El numero presionado (Utilizado como ID), La segunda y siguientes columnas: Las coordenadas de los puntos claves de la mano<br>
<img src="https://user-images.githubusercontent.com/37477845/102345725-28d26280-3fe1-11eb-9eeb-8c938e3f625b.png" width="80%"><br><br>
Las coordenadas de los puntos clave han sido transformadas de la siguiente forma hasta el ④.<br>
<img src="https://user-images.githubusercontent.com/37477845/102242918-ed328c80-3f3d-11eb-907c-61ba05678d54.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102244114-418a3c00-3f3f-11eb-8eef-f658e5aa2d0d.png" width="80%"><br><br>
En su estado inicial, 3 señas de LESCO vienen incluidas en el proyecto: Letra A (class ID: 0), Letra B (class ID: 1), y dinamico (class ID: 2).<br>
De ser necesario, se pueden agregar datos despues de 3, o se pueden eliminar los existentes y comenzar desde 0<br>
<img src="https://user-images.githubusercontent.com/37477845/102348846-d0519400-3fe5-11eb-8789-2e7daec65751.jpg" width="25%">　<img src="https://user-images.githubusercontent.com/37477845/102348855-d2b3ee00-3fe5-11eb-9c6d-b8924092a6d8.jpg" width="25%">　<img src="https://user-images.githubusercontent.com/37477845/102348861-d3e51b00-3fe5-11eb-8b07-adc08a48a760.jpg" width="25%">

#### 2.Entrenamiento del modelo
Abrir "[keypoint_classification.ipynb](keypoint_classification.ipynb)" en un Jupyter Notebook y ejecutar todas las celdas de arriba hacia abajo.<br>
Para cambiar la cantidad de señas manejadas por el modelo, cambiar el valor de la variable "NUM_CLASSES = 3" <br>y modificar los valores de "model/keypoint_classifier/keypoint_classifier_label.csv" segun la seña.<br><br>

#### X.Estructura del modelo
El modelo fue creado de la siguiente forma "[keypoint_classification.ipynb](keypoint_classification.ipynb)".
<img src="https://user-images.githubusercontent.com/37477845/102246723-69c76a00-3f42-11eb-8a4b-7c6b032b7e71.png" width="50%"><br><br>

### Entrenamiento de reconocimiento de movimiento de dedos
#### 1.Recoleccion de datos de entrenamiento
Presionar "h" para entrar al modo de guardado de coordenadas de los dedos (mostrado como "MODE:Logging Point History").<br>
<img src="https://user-images.githubusercontent.com/37477845/102249074-4d78fc80-3f45-11eb-9c1b-3eb975798871.jpg" width="60%"><br><br>
Si se presiona del "0" al "9", los puntos claves van a ser guardados en "model/point_history_classifier/point_history.csv" como se puede ver a continuacion.<br>
Primera columna: Numero presionado (usado como class ID), segunda y demas columnas: historial de coordenadas<br>
<img src="https://user-images.githubusercontent.com/37477845/102345850-54ede380-3fe1-11eb-8d04-88e351445898.png" width="80%"><br><br>
Las coordenadas de los puntos claves tuvieron la siguiente transformacion hasta el ④.<br>
<img src="https://user-images.githubusercontent.com/37477845/102244148-49e27700-3f3f-11eb-82e2-fc7de42b30fc.png" width="80%"><br><br>
En su estado inicial, se incluyen 3 tipos de datos de entrenamiento: Letra Z (class ID: 0), Rotacion reloj (class ID: 1) y Rotacion reloj inversa (class ID: 2). <br>
De ser necesario, se puede agregar mas datos a partir del 3, o se puede borrar lo existente y entrenar desde 0.<br>
<img src="https://user-images.githubusercontent.com/37477845/102350939-02b0c080-3fe9-11eb-94d8-54a3decdeebc.jpg" width="20%">　<img src="https://user-images.githubusercontent.com/37477845/102350945-05131a80-3fe9-11eb-904c-a1ec573a5c7d.jpg" width="20%">　<img src="https://user-images.githubusercontent.com/37477845/102350951-06444780-3fe9-11eb-98cc-91e352edc23c.jpg" width="20%">　<img src="https://user-images.githubusercontent.com/37477845/102350942-047a8400-3fe9-11eb-9103-dbf383e67bf5.jpg" width="20%">

#### 2.Entrenamiento del modelo
Abrir "[point_history_classification.ipynb](point_history_classification.ipynb)" en un Jupyter Notebook y ejecutar todas las celdas de arriba hacia abajo.<br>
Para cambiar la cantidad de señas manejadas por el modelo, cambiar el valor de la variable "NUM_CLASSES = 3" and <br>y modificar los valores de "model/point_history_classifier/point_history_classifier_label.csv" segun sea necesario. <br><br>

#### X.Estructura del modelo
El modelo creado en "[point_history_classification.ipynb](point_history_classification.ipynb)" esta definido de la siguiente forma.
<img src="https://user-images.githubusercontent.com/37477845/102246771-7481ff00-3f42-11eb-8ddf-9e3cc30c5816.png" width="50%"><br>
El modelo que utiliza "LSTM" se define de la siguiente forma. <br>Por favor cambiar la variable "use_lstm = False" a "True" cuando se use (tf-nightly requerido (desde 2020/12/16))<br>
<img src="https://user-images.githubusercontent.com/37477845/102246817-8368b180-3f42-11eb-9851-23a7b12467aa.png" width="60%">

# Referencia
* [MediaPipe](https://mediapipe.dev/)
* [Kazuhito00/mediapipe-python-sample](https://github.com/Kazuhito00/mediapipe-python-sample)

# Autor
Kazuhito Takahashi(https://twitter.com/KzhtTkhs)

# Traduccion al español
Andrés Zúñiga Umaña

# License
reconocimiento-de-senas-con-mediapipe is under [Apache v2 license](LICENSE).
