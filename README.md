# Simulink-Drone_Model
Simple drone simulation on a basic dynamical model with add on wind disturbance as an input as well as getting waypoints so that the trajectory taken by drone can be observed using matlab plots 

-----

In this simulation, waypoints are inputted to the system with simple inner and outer PID loops which then returns the updated linear and angular position of the drone at each instance of time. 

## Approach

Here roll, pitch, yaw and throttle are in terms of percentage which then through **human_control** block is manipulated to output the voltage values for each motor as per the required autonomous maneuver done by the drone. The battery used here is any 3S 11.4V battery. This voltage distribution on each motor is then used to calculate RPM and correspondingly calculate Thrust, torque and current through each motors in the **Motor Propellor** block. Now torque and Thrust from each motor is used in both **rotational** and **linear dynamics** blocks. These blocks actually represents the dynamical model which is one of the most important blocks here and maybe similar for most of the drones of similar configuration. Here a quadcopter is been simulated so if you want to simulate any hexacopter then dynamical blocks may change a little bit. Finally using integrator blocks, angular position and linear position is calculated and these parameters are fedback for the loops.

There is also a **Disturbance** block, which takes input a 1x3 wind velocity vector and does calculations to output the external disturbance force experienced by the drone which is then again fed to linear dynamics block calculating the overall acceleration on each axis and thus getting overall position of drone with wind disturbances.

A **Saturation** block is there so that ground can be visualized as z = 0.

For outer and inner loop inter-dependancy, x-position is controlled by linking it to roll motion (about y-axis), y-position is controlled by linking it to pitch motion (about x-axis), z-position is controlled directly by linking it to throttle.

![Quadcopter-body-frame-ph-is-the-rotation-on-the-X-axis-th-is-the-rotation-on-the-Y-axis](https://user-images.githubusercontent.com/69386934/124609829-9bc3fd80-de8d-11eb-9801-9d63c9fef858.png)

Finally, every PID block should be tuned based on the dynamical model been given for both the loops and so PID Tuner App was used to tune every block. If it doesnt show then that means you need to Add-On it in MATLAB.

![2021-07-06](https://user-images.githubusercontent.com/69386934/124611287-eabe6280-de8e-11eb-80a3-11290956e212.png)

These are the final PID values after alot of tuning and has a decent response time as well is robust. To make it more aggressive, it can be done using the PID Tuner App but make sure you are not making your system unstable while doing this. Try out this model giving different sets of waypoints as well as wind disturbance.

For plotting a 3D graph of the drone trajectory taken, **To Workspace** block named out.position is been taken to get position as object in MATLAB workspace.

A sample plot is been attached in this repository of how the 3D plot will look like !
