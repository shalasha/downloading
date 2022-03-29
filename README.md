
   
Первичная установка

1)  Установка драйверов
Ссылка туториал установки драйверов:
https://www.arducam.com/docs/camera-for-jetson-nano/mipi-camera-modules-for-jetson-nano/driver-installation/


cd ~
wget https://github.com/ArduCAM/MIPI_Camera/releases/download/v0.0.3/install_full.sh
chmod +x install_full.sh
./install_full.sh -m arducam

2) Установка библиотек
Ссылка туториал с демонстрацией камеры:

https://www.arducam.com/docs/camera-for-jetson-nano/mipi-camera-modules-for-jetson-nano/camera-demonstration/

intall python lib

sudo pip3 install v4l2-fix 

2) Запуск примера демонстрации экрана

git clone https://github.com/ArduCAM/MIPI_Camera.git 
cd MIPI_Camera/Jetson/Jetvariety/example 
sudo python3  arducam_displayer.py

![image](https://user-images.githubusercontent.com/101719007/160607959-92ae537b-d47a-4924-a1e9-1311572474b0.png)

Чтобы закрыть окно, необходимо нажать «q» на клавиатере, находясь на экране вывода изображения с камер.  

22.03.22

Установка ROS 

http://wiki.ros.org/melodic/Installation/Ubuntu

1) Настройка sources.list
Настройка компьютера для установки программного обеспечения ROS

$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

2) Настройка ключей

$ sudo apt install curl # if you haven't already installed curl
$ curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

3) Установка ROS

$ sudo apt update

![image](https://user-images.githubusercontent.com/101719007/160608034-2568e43a-d28f-4555-ad4d-ecd0b515d3dc.png)

4) Установка всех предлженных библиотек ROS

$ sudo apt install ros-melodic-desktop-full


5) Настройка среды

$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
$ source ~/.bashrc

6) Установка необходимых инструментов для ROS

$ sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential

7) Инициализация rosdep

$ sudo apt install python-rosdep
$ sudo rosdep init
$ rosdep update

8) Инициализация rosdep

$ sudo apt install python-rosdep

Создание ROS окружения 

http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment#Create_a_ROS_Workspace

1) Создание окружения catkin:

$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
$ ls 
$ cd devel/
$ ls 
$ pwd

Копируем ответ путь, который выдала нам консоль на команду pwd

$ vim -/. bashrc
![image](https://user-images.githubusercontent.com/101719007/160608098-4f19c901-7eb5-4144-aa1d-9bc756a0041c.png)



Дописываем в самый конец строчку:

source /home/nvidia/catkin_ws/devel/setup.bash

 Чтобы сохранить изменения и закрыть файд, нажимаем комбинацию клавиш:
Ctrl+O
Прописываем
:wq
![image](https://user-images.githubusercontent.com/101719007/160608130-7300a8b5-1a65-4c25-99d8-4d2153bc6d71.png)


$ source -/. bashrc

Дополнительная установка 

$ wget https://bootstrap.pypa.io/get-pip.py && python get-pip.py
$ sudo pip install v4l2
$ sudo apt install ros-melodic-camera-info-manager-py

Установка пакета arducam_stereo_camera

$ git clone https://github.com/ArduCAM/Camarray_HAT.git
$ cd Camarray_HAT/Jetson/ROS/
$ cp -r Camarray_HAT/Jetson/ROS/arducam_stereo_camera ~/catkin_ws/src

1) Открываем новое окно и пишем: 

$ cd ~/catkin_ws/ && catkin_make




Запуск Stereo pipeline package

1) Проверка настроек камеры


![image](https://user-images.githubusercontent.com/101719007/160608168-1e38f27f-cd37-42d4-9110-c964c7b1a7a5.png)



Запоминаем разрешение 2560х800

2) Запуск камеры
Мы можем изменить параметры запуска 

vim ~/catkin_ws/src/arducam_stereo_camera/launch/arducam_stereo_camera.launch

Меняем разрешение на 2560х800. 

Чтобы сохранить изменения и закрыть файд, нажимаем комбинацию клавиш:
Ctrl+O
Прописываем
:wq

Запускаем в двух разных консольных окнах код.
В окне 1: 
$ roscore #run roscore

В окне 2: 
$ roslaunch arducam_stereo_camera arducam_stereo_camera.launch #run camera

Оба окна не закрываем!

Калибровка камеры

http://wiki.ros.org/camera_calibration/Tutorials/StereoCalibration

Заранее распечатаем шахматную доску и измерим размер квадратов. 
У нас он 24 мм. 

Открываем третье окно и пишем: 

rosrun camera_calibration cameracalibrator.py --approximate 0.1 --size 9x6 --square 0.024 right:=/arducam/right/image_raw left:=/arducam/left/image_raw right_camera:=/arducam/right left_camera:=/arducam/left 

Предварительно изменяя выделенные значения под свою шахматную доску. 
9x6 — это размер шахматного поля. Надо посчитать количество клеток (белых+черных) по обеим осям, а потом вычесть из этого количества 1. 

![image](https://user-images.githubusercontent.com/101719007/160608269-996c6471-ef65-4d0f-81cb-958261f7fba4.png)



Далее необходимо поднести шахматную доску к камере и начать перемещать так, чтобы 4 полосы позеленели и появилась кнопка Calibrate. 


После завершения калибровки это окно можно закрыть. 
Первые два окна должны быть всё ещё открыты, чтобы построить карту глубины. 
