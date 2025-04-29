# Punto 4: Simulación de TurtleBot3 con LIDAR y SLAM en Docker

Este documento describe el proceso de desarrollo de una simulación en Docker que implementa la tecnología **SLAM** y **LIDAR** usando el robot **TurtleBot3**, según lo indicado en el **punto 4** de la guía del curso.

---

## Construcción del contenedor Docker

Para construir el entorno, se utilizó una imagen base con ROS Noetic, sobre la cual se instalaron los paquetes necesarios para TurtleBot3, simulaciones y SLAM.

### Dockerfile base

```Dockerfile
FROM osrf/ros:noetic-desktop-full

RUN apt-get update && apt-get install -y \
    ros-noetic-turtlebot3 \
    ros-noetic-turtlebot3-simulations \
    ros-noetic-slam-gmapping \
    libgl1-mesa-glx \
    libglu1-mesa \
    libxrender1 \
    libxext6 \
    libqt5widgets5 \
    libqt5gui5 \
    libqt5core5a \
    libqt5x11extras5

ENV TURTLEBOT3_MODEL=burger
```

### Construcción de la imagen

```bash
sudo docker build -t turtlebot3-slam .
```

---

##  Ejecución del entorno simulado

Antes de ejecutar cualquier contenedor con GUI, se debe habilitar el acceso gráfico:

```bash
xhost +local:root
```

### 🔹 Lanzar Gazebo con el mundo del TurtleBot3

```bash
sudo docker run -it --rm \
  --network=host \
  -e DISPLAY=$DISPLAY \
  -e TURTLEBOT3_MODEL=burger \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  turtlebot3-slam
```

---

##  Activación de SLAM (Gmapping)

En una segunda terminal, se activa el módulo de SLAM:

```bash
sudo docker run -it --rm \
  --network=host \
  -e DISPLAY=$DISPLAY \
  -e TURTLEBOT3_MODEL=burger \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  turtlebot3-slam bash -c \
"source /opt/ros/noetic/setup.bash && \
roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping"
```

---

##  Teleoperación del robot

Para mover el robot manualmente con el teclado:

```bash
sudo docker run -it --rm \
  --network=host \
  -e TURTLEBOT3_MODEL=burger \
  turtlebot3-slam bash -c \
"source /opt/ros/noetic/setup.bash && \
rosrun turtlebot3_teleop turtlebot3_teleop_key"
```

---

##  Visualización del mapa generado

Es posible observar el mapa generado en tiempo real con `Rviz`:

```bash
sudo docker run -it --rm \
  --network=host \
  -e DISPLAY=$DISPLAY \
  -e TURTLEBOT3_MODEL=burger \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  turtlebot3-slam bash -c \
"source /opt/ros/noetic/setup.bash && \
rosrun rviz rviz -d /opt/ros/noetic/share/turtlebot3_slam/rviz/turtlebot3_gmapping.rviz"
```

---

##  Trayectorias exploradas

Durante la simulación, se realizaron trayectos de prueba para validar el mapeo:

- Movimientos rectos hacia adelante y hacia atrás
- Trayectorias en curva (combinación de avance con rotación)
- Giros sobre el eje
- Exploración de una estructura hexagonal virtual
- Verificación de la actualización del mapa SLAM en tiempo real

---

##  Posibles mejoras

-  **Guardar el mapa generado** en disco (`.pgm` y `.yaml`) para reutilizarlo en futuras simulaciones.
-  Implementar SLAM con `cartographer` para mejorar precisión.
-  Añadir soporte para joystick físico usando el nodo `joy`.
-  Automatizar el sistema completo con `docker-compose`.
-  Añadir navegación autónoma con `move_base` y planificación de rutas.

---

## Conclusiones

La implementación fue exitosa, logrando lanzar y controlar un TurtleBot3 simulado con tecnología SLAM en un entorno Dockerizado. Se validó la creación de mapas en tiempo real y se logró la interacción completa con el robot usando teleoperación.
