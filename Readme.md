###### Attribution required : Fabien Furfaro (CC 4.0 BY NC ND SA)

---

# Astuce

Utilisation de plusieurs onglets de Terminal : Maj+Ctrl+T
Sublime text : 
	- Glisser le dossiers pour avoir l'arborescence
	- Installer des modules/packages via Ctrl+Maj+p
	- Package "Terminus" et editer Key-Binding : Alt+1 (tips : ipython3) --> pour testing

# ROS installation

``` 
sudo apt install ros-desktop-full
sudo apt install libhwloc-contrib-plugins proj-bin mpi-default-bin vtk9-doc vtk9-examples
#sudo apt install gazebo
# test result (catkin_make)
sudo apt install libgtest-dev libgmock-dev python3-nose
```

ROS basique est inclus dans Debian (https://wiki.debian.org/DebianScience/Robotics/ROS/Packages), mais le reste est à builder à la main (https://answers.ros.org/question/399664/will-ros-noetic-support-ubuntu-2204/). Néanmoins, ROS1 ne sera pas supporté sur 22.04, il sera préférable de faire un portage vers ROS2 ensuite.

# Create a project

```
mkdir -p ProjectName/src
cd ProjectName/
catkin_make
```

# Create ros package

```
cd src/
catkin_create_pkg pkg_name dependency1 dependency2 # (...) dependency is python module
```

Dans pkg_name, le fichier package.xml contient les informations pour le build.

En python, on utilise principalement les dépendances "rospy" et "std_msgs".


# Create Nodes

Pour lancer les nodes "rosrun", il est necessaire que "roscore" soit executé dans une fenetre.

Le roscore est le coeurs "master" du middleware ROS. Un noeud est un fichiers et les Topics sont moyen de communiquer avec les noeuds.

Les noeuds sont contenus dans les packages et en Python, c'est un fichier avec l'extension ".py". Pour cela, on creer un dossiers "scripts" dans le package qui contiendra les différents noeuds python.

Il faut ensuite preciser les noeuds dans le CMakeLists.txt :
```
catkin_install_python(PROGRAMS script/node1.py script/node2.py ... )
```

Aller dans le dossier parent et lancer la commande "catkin_make" pour recompiler. À partir d'ici, le script peut etre modifié sans avoir besoin de recompiler (pour python). Pour lancer le node :

```
# terminal 1
roscore
# terminal 2
source devel/setup.bash
rosrun tutorial publisher_node.py
# terminal 3
source devel/setup.bash
rosrun tutorial subscriber_node.py
```

Remarque : La source doit toujours etre lancé pour que rosrun fonctionne. Si on arrete le rosrun publisher, le subscriber est en attente.

Pour voir les topic en cours et les messages :

```
rostopic list
rostopic echo /talking_topic
```

# Create Message

Un message est un moyen d'utiliser des Topics préparamétré. Pour cela on creer un dossier "msg" contenant un fichier "MSG.msg" avec des variables (string, float, etc.). Dans le code, on importe le fichier msg comme un module python (from import)

Pour utiliser des messages, il faut le preciser dans le fichier "package.xml" en décommentant les lignes :

```
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>
```

Ensuite, dans le CMakeList, on precise :

```
add_message_files(
   FILES
   MSG.msg
    )

generate_messages(
   DEPENDENCIES
   std_msgs
 )
```

# Objectif


Utilisation de python et ROS + websocket GODOT + Flask

