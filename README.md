# Control en impedancia con visión basada en eventos
Recopilación de los códigos utilizados para el TFG Control en impedancia para la actuación rápida de robots con visión basada en eventos. Este proyecto ha sido llevado a cabo en el laboratorio ICB (Dijon, Francia), y trata de utilizar una tarea de aprendizaje por refuerzo profundo para la generación de ganancias necesarias para el control en impedancia de un brazo robot colaborativo (Franka Research 3). Al utilizar este tipo de generación de ganancias, obtenemos un comportamiento más adaptativo al movimiento. Además, tiende a minimizar la rigidez del sistema, provocando un movimiento más suave y un menor desgaste del robot. Por otro lado, se lleva a cabo una tarea de control visual para alcanzar un objeto, mediante una cámara RGB y una cámara de eventos.

## 📂 Estructura del Proyecto

A continuación se detalla la organización de los módulos principales del software:

```text
.
├── 📂 rl_package                     # Paquete de aprendizaje por refuerzo
│   ├── 📂 rl_package                 # Carpeta de los módulos
│   │   ├── entrenamiento.py           # Implementación final del entrenamiento
│   │   ├── perfil_trapezoidal.py      # Entrenamiento para generar ganancias siguiendo un perfil trapezoidal
│   │   ├── one_joint.py               # Entrenamiento inicial, moviendo una única articulación
│   │   ├── auxiliar.py                # Librería auxiliar para modificar SAC y generar nuevas poses
│   │   └── record_move.py             # Registro de trayectorias y demostraciones
│   └── setup.py                       # Definición de los módulos necesaria para ROS 2
│
├── 📂 robot_real                                    # Implementación con el robot real
│   ├── saveVideo.cpp                                 # Mueve al robot a una serie de trayectorias cartesianas, guardadas en txt, guardando tanto el robot_state (a 60Hz) como el video
│   ├── saveVideo_joint.cpp                           # Mueve al robot a una serie de trayectorias articulares y guarda el video
│   ├── go_home.cpp                                   # Mueve a home mediante movimiento articular
│   ├── calibracion_joint.cpp                         # Realiza la autocalibración de la cámara mediante movimiento articular, generando los parámetros intrínsecos y extrínsecos
│   ├── control_cartesiano.cpp                        # Movimiento cartesiano, mueve el end effector a una pose seleccionada, con una determinada velocidad lineal y angular
│   ├── control_impedancia.cpp                        # Utiliza la base de control_cartesiano para controlar el robot en impedancia
│   ├── moveTrajectory.cpp                            # Lee un .txt con poses y realiza una trayectoria cartesiana
│   ├── moveJointTrajectory.cpp                       # Lee un .txt con configuraciones articulares y realiza una trayectoria articular
│   ├── visual_servoing_apriltag.cpp                  # Realiza control visual con ViSP para seguir un AprilTag
│   ├── visual_servoing_pelota_rgb.cpp                # Realiza control visual con ViSP para seguir la pelota mediante la detección por RGB
│   ├── visual_servoing_pelota_yolo.cpp               # Realiza control visual con ViSP para seguir la pelota mediante la detección con YOLOv4
│   ├── visual_servoing_pelota_eventos_cliente.cpp    # Realiza control visual con ViSP para seguir la pelota con la información de la cámara de eventos
│   └── visual_servoing_pelota_eventos_server.py      # Detecta la pelota utilizando la cámara de eventos y se lo pasa al cliente
│
├── 📂 scripts_simulacion      # Scripts para la simulación en Isaac Sim
│   ├── control_adaptativo.py  # Control en impedancia con aceleración constante
│   ├── control_impedancia.py  # Control en impedancia para RL
│   └── visual_servoing.py     # Control visual IBVS para detectar la pelota
│
├── CMakeLists.txt            # Configuración de compilación para los nodos C++
└── launcher_isaac.sh         # Script de arranque para el simulador NVIDIA Isaac Sim

```
# Impedance Control with Event-Based Vision
Collection of codes used for the Bachelor's Thesis (TFG) "Impedance Control for Fast Robot Actuation with Event-Based Vision." This project was carried out at the ICB laboratory (Dijon, France) and focuses on using a deep reinforcement learning task to generate the necessary gains for the impedance control of a collaborative robot arm (Franka Research 3). By using this type of gain generation, we achieve a behavior that is more adaptive to movement. Furthermore, it tends to minimize system stiffness, resulting in smoother motion and less wear on the robot. On the other hand, a visual servoing task is implemented to reach an object using an RGB camera and an event camera.

## 📂 Project Structure

Below is the detailed organization of the main software modules:

```text
.
├── 📂 rl_package                     # Reinforcement learning package
│   ├── 📂 rl_package                 # Modules folder
│   │   ├── entrenamiento.py           # Final training implementation
│   │   ├── perfil_trapezoidal.py      # Training to generate gains following a trapezoidal profile
│   │   ├── one_joint.py               # Initial training, moving a single joint
│   │   ├── auxiliar.py                # Auxiliary library to modify SAC and generate new poses
│   │   └── record_move.py             # Trajectory and demonstration recording
│   └── setup.py                       # Module definition required for ROS 2
│
├── 📂 robot_real                                    # Implementation with the real robot
│   ├── saveVideo.cpp                                 # Moves the robot along a series of Cartesian trajectories saved in txt, recording both the robot_state (at 60Hz) and the video
│   ├── saveVideo_joint.cpp                           # Moves the robot along a series of joint trajectories and records the video
│   ├── go_home.cpp                                   # Moves to home position via joint movement
│   ├── calibracion_joint.cpp                         # Performs camera self-calibration via joint movement, generating intrinsic and extrinsic parameters
│   ├── control_cartesiano.cpp                        # Cartesian movement, moves the end effector to a selected pose with a specific linear and angular velocity
│   ├── control_impedancia.cpp                        # Uses control_cartesiano as a base to control the robot in impedance mode
│   ├── moveTrajectory.cpp                            # Reads a .txt file with poses and executes a Cartesian trajectory
│   ├── moveJointTrajectory.cpp                       # Reads a .txt file with joint configurations and executes a joint trajectory
│   ├── visual_servoing_apriltag.cpp                  # Performs visual servoing with ViSP to track an AprilTag
│   ├── visual_servoing_pelota_rgb.cpp                # Performs visual servoing with ViSP to track the ball via RGB detection
│   ├── visual_servoing_pelota_yolo.cpp                # Performs visual servoing with ViSP to track the ball via YOLOv4 detection
│   ├── visual_servoing_pelota_eventos_cliente.cpp    # Performs visual servoing with ViSP to track the ball using event camera information
│   └── visual_servoing_pelota_eventos_server.py      # Detects the ball using the event camera and passes it to the client
│
├── 📂 scripts_simulacion      # Scripts for simulation in Isaac Sim
│   ├── control_adaptativo.py  # Impedance control with constant acceleration
│   ├── control_impedancia.py  # Impedance control for RL
│   └── visual_servoing.py     # IBVS (Image-Based Visual Servoing) control to detect the ball
│
├── CMakeLists.txt            # Build configuration for C++ nodes
└── launcher_isaac.sh         # Startup script for the NVIDIA Isaac Sim simulator
