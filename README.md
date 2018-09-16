This package uses the **AMCL(Adaptive Monte Carlo Localization)** in ROS to localize a robot in a mapped environment. AMCL, dynamically adjust the number of particle over a period of time, as the robot navigates around the environment.

To start with autonomous navigation there is a need of designed simulated differential drive robot that satisfy the necessary prerequisites: tf tree, odometry, laserscaner and base controller. To move the robot its own the package is using the **ROS's Navigation Stack.**

### Steps involve in developing this package-
* Creating a differential drive robot model(urdf and gazebo plugins).
* Plug in the ROS Navigation Stack.
* Parameter tunning.
* Create a map.
* Navigation with the map.

To implement the navigation stack first we need a map, here, this package uses a saved map(jackal\_race). As the map and robot model get loaded in the rviz it is very important to localize the robot with respect to that map. The ROS amcl\_package implement this variant, but what you need to do is to integrate this package with your robot to localize it in a saved map.
![alt text](https://github.com/proneetsharma/WhereAmI/blob/master/media/RobotWithPoseArray.png)

In **amcl.launch** file, three nodes are added. First node will load the saved map using **map\_server** package. second will launch the **amcl\_package**. Now the robot is localizing itself, but it is still stationary. That's where the ROS Navigation stack comes in! It added the **move\_base** package which is launched by third node. Using it you can define a goal position to the robot, and the robot will navigate to that position. This package(move_base) uses **costmap** and **local planners**. Costmap, which dived the map in two parts one which is occupied by the obstacles and the other one, which is unoccupied. Local planners which creates the path and navigates the robot to that path.
![alt text](https://github.com/proneetsharma/WhereAmI/blob/master/media/GlobalAndLocalCostmap.png)


## Steps to launch the package-

#### Step 1. Clone Package
```sh
$ cd ~/catkin_ws/scr
$ https://github.com/proneetsharma/WhereAmI.git
```
#### Step 2. Build package
```sh
$ cd ..
Change the name of package WhereIAm to udacity_bot
$ catkin_make
$ source devel/setup.zsh
```
#### Step 3. Launch Nodes
```sh
$ roslaunch udacity_bot udacity_world.launch
$ roslaunch udacity_bot amcl.launch   
```

Click on **Goal Pose** in rviz and give robot a goal position on the map.
![alt text](https://github.com/proneetsharma/WhereAmI/blob/master/media/GoalPosition.png)

The Robot will start navigating in the goal pose direction and finally reach to the goal position.
![alt text](https://github.com/proneetsharma/WhereAmI/blob/master/media/FinalPosition.png)


