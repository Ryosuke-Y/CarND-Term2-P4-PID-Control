# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

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
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`.

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

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
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

## PID controller

**P (propoprtional)**: Generates a steering correction proportional to the cross track error (cte). The higher the P value, the larger and more responsive the correction is. But too large P would result in overshooting and oscillation in steering control.  

**D (Derivative)**: Contributes a correction proportional to the changing rate of the cte. It helps 'damp' the overshooting caused by P. However, if D is overly large, it would induce too much damping and make the system hard to take prompt responses.  

**I (Integral)**: Plays an indispensable role if there is a drift in the system. It generates a control signal that is proportional to the accumulation of the cte over time.   

## Procedure  

- Parameters Kp, Ki, and Kd were manually chosen  
- After each message from simulator with current CTE, PID control equation is applied: `steering angle = - Kp * CTE - Ki * sum(CTE) - Kd * diff (CTE)` 
- Where sum(CTE) is the summation of all CTEs (integral part)
diff(CTE) is the differential part (difference between current CTE and previous CTE)  
- After steering angle is calculated, it is passed with a constant throttle (0.3) to control the car in the simulator  

## Choosing Parameters  

** Initial P (Kp): 0.1**  
Initially, I set the small Kp value (0.1) and Ki and Kd were set to 0, and checked the performance. The car could drive until the sharp corner before the bridge, but it was driven the out of course due to oscillation. Therefore, I decided to tune the D to prevent overshooting problems.  

** Initial I (Ki): 0**  
Since the simulator doesn't seem to have a drift issue. I kept I = 0 in this project.  

** Initial D (Kd):2.0**  
I started with a value of 3.0 as given in the quiz. It's successfully drive multiple laps but I felt that the car was strange behavior. So I decreased the D value and finally chose the value of 2.0.  

## Results

The car could successfully drive multiple laps around the track smoothly with an average speed around 65 mph despite oscillating steering.  

## Improvements Work

A Twiddle algorithm could be implemented to determine the K coefficients, instead of manual testing.  
