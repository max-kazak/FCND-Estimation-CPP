# Estimation Project #

In this project, we will be developing the estimation portion of the controller used in the CPP simulator.

## The Tasks ##

Project outline:

 - [Step 1: Sensor Noise](#step-1-sensor-noise)
 - [Step 2: Attitude Estimation](#step-2-attitude-estimation)
 - [Step 3: Prediction Step](#step-3-prediction-step)
 - [Step 4: Magnetometer Update](#step-4-magnetometer-update)
 - [Step 5: Closed Loop + GPS Update](#step-5-closed-loop--gps-update)
 - [Step 6: Adding Your Controller](#step-6-adding-your-controller)



### Step 1: Sensor Noise ###

Determine the standard deviation of the measurement noise of both GPS X data and Accelerometer X data.

The calculated standard deviation should correctly capture ~68% of the sensor measurements.

After capturing sensor data, standard deviation was calculated with [calc_std.ipynd](notebooks/calc_std.ipynb) and stored in [config/06_SensorNoise.txt#L3-L4](config/06_SensorNoise.txt#L3-L4).

### Step 2: Attitude Estimation ###

Implement a better rate gyro attitude integration scheme in the UpdateFromIMU() function.

The improved integration scheme should result in an attitude estimator of < 0.1 rad for each of the Euler angles for a duration of at least 3 seconds during the simulation. The integration scheme should use quaternions to improve performance over the current simple integration scheme.

This implementation at [src/QuadEstimatorEKF.cpp#L96-L108](src/QuadEstimatorEKF.cpp#L96-L108) follows section 7.1.2 of [Estimation for Quadrotors](https://www.overleaf.com/read/vymfngphcccj) describing a good non-linear complimentary filter for attitude using quaternions.


### Step 3: Prediction Step ###

Implement all of the elements of the prediction step for the estimator.

The prediction step should include the state update element (PredictState() function), a correct calculation of the Rgb prime matrix, and a proper update of the state covariance. The acceleration should be accounted for as a command in the calculation of gPrime. The covariance update should follow the classic EKF update equation.

This step is implemented in:

 - [src/QuadEstimatorEKF.cpp#L171-L178](src/QuadEstimatorEKF.cpp#L171-L178)
 - [src/QuadEstimatorEKF.cpp#L206-L215](src/QuadEstimatorEKF.cpp#L206-L215)
 - [src/QuadEstimatorEKF.cpp#L262-L276](src/QuadEstimatorEKF.cpp#L262-L276)

**Hint: see section 7.2 of [Estimation for Quadrotors](https://www.overleaf.com/read/vymfngphcccj) for a refresher on the the transition model and the partial derivatives you may need**


### Step 4: Magnetometer Update ###

Implement the magnetometer update.

The update should properly include the magnetometer data into the state. Note that the solution should make sure to correctly measure the angle error between the current state and the magnetometer value (error should be the short way around, not the long way).

 - [src/QuadEstimatorEKF.cpp#L328-L339](src/QuadEstimatorEKF.cpp#L328-L339)

**Hint: see section 7.3.2 of [Estimation for Quadrotors](https://www.overleaf.com/read/vymfngphcccj) for a refresher on the magnetometer update.**


### Step 5: Closed Loop + GPS Update ###

Implement the GPS update.

The estimator should correctly incorporate the GPS information to update the current state estimate.

 - [src/QuadEstimatorEKF.cpp#L302-L305](src/QuadEstimatorEKF.cpp#L302-L305)

**Hint: see section 7.3.1 of [Estimation for Quadrotors](https://www.overleaf.com/read/vymfngphcccj) for a refresher on the GPS update.**


### Step 6: Adding Your Controller ###

After copying PID Controller from previos project I de-tuned my controller to successfully fly the final desired box trajectory with my estimator and realistic sensors.
