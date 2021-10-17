- [CarND-Path-Planning-Project](#carnd-path-planning-project)
    - [Simulator.](#simulator)
    - [Goals](#goals)
      - [The map of the highway is in data/highway_map.txt](#the-map-of-the-highway-is-in-datahighway_maptxt)
  - [Basic Build Instructions](#basic-build-instructions)
      - [Main car's localization Data (No Noise)](#main-cars-localization-data-no-noise)
      - [Previous path data given to the Planner](#previous-path-data-given-to-the-planner)
      - [Previous path's end s and d values](#previous-paths-end-s-and-d-values)
      - [Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)](#sensor-fusion-data-a-list-of-all-other-cars-attributes-on-the-same-side-of-the-road-no-noise)
  - [Details](#details)
  - [Tips](#tips)
  - [Dependencies](#dependencies)
  - [Editor Settings](#editor-settings)
  - [Code Style](#code-style)
  - [Project Instructions and Rubric](#project-instructions-and-rubric)
  - [Call for IDE Profiles Pull Requests](#call-for-ide-profiles-pull-requests)
  - [How to write a README](#how-to-write-a-readme)
- [Important Notes](#important-notes)
- [Project Rubric ](#project-rubric-)
  - [Compilation](#compilation)
    - [The code compiles correctly.](#the-code-compiles-correctly)
  - [Valid Trajectories](#valid-trajectories)
    - [The car is able to drive at least 4.32 miles without incident..](#the-car-is-able-to-drive-at-least-432-miles-without-incident)
    - [The car drives according to the speed limit.](#the-car-drives-according-to-the-speed-limit)
    - [Max Acceleration and Jerk are not Exceeded.](#max-acceleration-and-jerk-are-not-exceeded)
    - [Car does not have collisions.](#car-does-not-have-collisions)
    - [The car stays in its lane, except for the time between changing lanes.](#the-car-stays-in-its-lane-except-for-the-time-between-changing-lanes)
    - [The car is able to change lanes](#the-car-is-able-to-change-lanes)
  - [Reflection](#reflection)
    - [There is a reflection on how to generate paths](#there-is-a-reflection-on-how-to-generate-paths)
  - [Suggestions to Make Your Project Stand Out](#suggestions-to-make-your-project-stand-out)
# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).  

To run the simulator on Mac/Linux, first make the binary file executable with the following command:
```shell
sudo chmod u+x {simulator_file_name}
```

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the **car's localization and sensor fusion data**, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the **50 MPH** speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the **6946m highway**. Since the car is trying to go 50 MPH, it should take a little over **5 minutes** to complete 1 loop. Also the car should not experience total acceleration over **10 m/s^2** and jerk that is greater than **10 m/s^3**.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

```bash
cmake .. && make && ./path_planning
```
5. Others:
-  Change Directory

```bash
cd CarND-Path-Planning-Project
```
   - Please tell me who you are

```bash
git config --global user.email "Peter.Forez@gmail.com"
git config --global user.name "Peter Forez"
```
- Pull changes
```bash
git pull https://github.com/PeterForez/CarND-Path-Planning-Project
```


```bash
cd CarND-Path-Planning-Project
git config --global user.email "Peter.Forez@gmail.com"
git config --global user.name "Peter Forez"
git pull https://github.com/PeterForez/CarND-Path-Planning-Project
```
Here is the data provided from the Simulator to the C++ Program

#### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

#### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

#### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

#### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. Acceleration both in the tangential and normal directions is measured along with the jerk, the rate of change of total Acceleration. The (x,y) point paths that the planner recieves should not have a total acceleration that goes over 10 m/s^2, also the jerk should not go over 50 m/s^3. (NOTE: As this is BETA, these requirements might change. Also currently jerk is over a .02 second interval, it would probably be better to average total acceleration over 1 second and measure jerk from that.

2. There will be some latency between the simulator running and the path planner returning a path, with optimized code usually its not very long maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, because of this its a good idea to store the last points you have used so you can have a smooth transition. previous_path_x, and previous_path_y can be helpful for this transition since they show the last points given to the simulator controller with the processed points already removed. You would either return a path that extends this previous path or make sure to create a new path that has a smooth transition with this last path.

## Tips

A really helpful resource for doing this project and creating smooth trajectories was using http://kluge.in-chemnitz.de/opensource/spline/, the spline function is in a single hearder file is really easy to use.

---

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!


## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

# Important Notes
1. The highway track has other vehicles, all going different speeds, but approximately obeying the 50 MPH speed limit.
2. The car transmits its location, along with its sensor fusion data, which estimates the location of all the vehicles on the same side of the road.
3. Every 20 ms the car moves to the next point on the list.
4. The speed goal is to have the car traveling at (but not above) the 50 MPH speed limit as often as possible.
5. In `main.cpp`, instead of setting the speed directly, we pass `next_x_vals`, and `next_y_vals` to the simulator. We will set the points `0.5` m apart. Since the car moves `50` times a second, a distance of `0.5`m per move will create a velocity of `25` m/s. `25` m/s is close to `50` MPH.

# [Project Rubric ](https://review.udacity.com/#!/rubrics/1971/view)
## Compilation
### The code compiles correctly.
> Code must compile without errors with cmake and make.
> Given that we've made CMakeLists.txt as general as possible, it's recommend that you do not change it unless you can guarantee that your changes will still compile on any platform.

![](\Evidence\Build.PNG)

## Valid Trajectories
The Car was able to drive more than 4.32 miles without any incident. This is done while keeping the speed to the maximum as feasible as possible.
![](\Evidence\05.PNG)
### The car is able to drive at least 4.32 miles without incident..
> The top right screen of the simulator shows the current/best miles driven without incident. Incidents include exceeding acceleration/jerk/speed, collision, and driving outside of the lanes. Each incident case is also listed below in more detail.

### The car drives according to the speed limit.
>The car doesn't drive faster than the speed limit. Also the car isn't driving much slower than speed limit unless obstructed by traffic.

### Max Acceleration and Jerk are not Exceeded.
> The car does not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3.

### Car does not have collisions.
> The car must not come into contact with any of the other cars on the road.

### The car stays in its lane, except for the time between changing lanes.
> The car doesn't spend more than a 3 second length out side the lane lanes during changing lanes, and every other time the car stays inside one of the 3 lanes on the right hand side of the road.

### The car is able to change lanes
> The car is able to smoothly change lanes when it makes sense to do so, such as when behind a slower moving car and an adjacent lane is clear of other traffic.

## Reflection
### There is a reflection on how to generate paths
> The code model for generating paths is described in detail. This can be part of the README or a separate doc labeled "Model Documentation".

I started with the [Project Q&A](https://www.youtube.com/watch?v=7sI3VHFPP0w) starter code. 
The Steps of the program is as follow:
1. Getting from the simulator
   1. The main car's localization Data from the simulator.
   2. The previous path data given to the Planner.
   3. The previous path's end s and d values.
2. Update car_s with the end_path_s.
3. Read Sensor Fusion Data which is a list of all other cars on the same side of the road.
4. Detect if there is a car in front of us.
5. If there is a car in front of us, we will consider change the lane or decrease the speed.
   1. Consider Left lane shift if it is safe.
   2. Consider Right lane shift if it is safe.
   3. Decrease the speed if the shifing the lane is not safe.
6.  Adjust the velocity
    1.  If there is a car in front of us, and shifting lanes is not safe; decrease the velocity.
    2.  If there is no car in front of us, increase the velocity gradually to the maximum velocity.




## Suggestions to Make Your Project Stand Out
> Create a path planner that performs optimized lane changing, this means that the car only changes into a lane that improves its forward progress.
> 
This is the main contribution. I defined a function that check if turning right or left. The function return that is safe to take the turn unless there is cars in front or in back of us in the desired lane. 
```cpp
bool isTurnSafe(int lane, vector<vector<double>> sensor_fusion, double car_s, double prev_size, int dir)
{
  bool safe = true;
  for (int i = 0; i < sensor_fusion.size(); i++)                            // Loop over all the car in the track
  {
    float d = sensor_fusion[i][6]; 
    if (d < (2+4*(lane+dir)+2) && d > (2+4*(lane+dir)-2) && d)              // Detect if the car is present in the desired lane
    {
      double vx = sensor_fusion[i][3];                                      // Velocity component in x direction                           
      double vy = sensor_fusion[i][4];                                      // Velocity component in y direction                      
      double check_speed = sqrt(vx*vx + vy*vy);                             // Car Speed
      double check_car_s = sensor_fusion[i][5];                             // Car s

      check_car_s += (double)prev_size * 0.02 * check_speed;                // Check next car s
      if(
        ((check_car_s > car_s) && (check_car_s - car_s) < SAFE_DISTANCE) || // Enough Gap distance for the car Ahead
        ((check_car_s < car_s) && (car_s - check_car_s) < SAFE_DISTANCE)    // Enough Gap distance for the car behind
      )
      {
        safe = false;
      }  
    }
  }
  return safe;
}
```



