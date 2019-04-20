# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

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

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Parameter tuning:
Manually, by trial and error method.

## Theory
PID stands for Proportional, Integral, Derivative.
One the vehicle is following the path/line, it may be away from the line, either on the left side or right side. To put the car on the line, we need to decrease this displancement also know as CTE(Cross track error). 

P: Proportional Control: How much we need to turn the steering is proportional to the CTE. when putting into equation, steering angle becomes cte * P_gain. We need to tune this parameter as it's value can drastically effect the vehicle movement.
The performance is better with higher gain. with low gain, it overshoots to the other side of the road too much. But proportional control alone is not good enough. And there is also limit on high gain, just to avoid going system out of control.

D: Derivative Control: To futher improve the behaviour, we can check on the rate of change of CTE. This will make the transition more soother. and eventually overshooting will be low. But there will be a little bit overshooting. So the steering angle equation becomes: steering _angle = cte * P_gain + dCTE/dt * D_gain
If the derivative is low, the system will be underdamped and still oscillate. if too high, it will take too much time to correct the offset. What we need is the medium value, good for our purpose. But again there are still some other forces in nature, that can deviate our car from following the line. It can be on one side of the line more time comparing to the time spent on the other side of the line. We need to fix this. The solution is easy, we need to just take in account the sum of errors so far.

I: Integral Control: It is the sum of all the CTE and find whether it is spending most of the time on one side of the trajectory. Then we need to fix this by adding another value to the steering angle:
steering_angle = cte * P_gain + dCTE/dt * D_gain + CTE_sum * I_gain
If the I_gain is too high, it will exagerate the CTE_error and car will start to oscillate, if too low, it will take too long to come back to the line. What we need is a good I_gain, that will bring the car back to track efficiently.

## Values
P_gain = 0.2;
I_gain = 0.00001;
D_gain = 4.0;

## Video
https://youtu.be/VMC91-Zq5Qg


