# Project: Control of a 3D Quadrotor
## [Rubric](https://review.udacity.com/#!/rubrics/1807/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
## Implemented Controller
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. 
You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Sensor Noise
After ran the 06_NoisySensors, two log files (Graph1.txt and Graph2.txt) are provided with reads of the sensors. In this way, it`s possible to estimate the standard deviation of the sensors` noise that should be ~68%. With these values on hands, the MeasuredStdDev_GPSPosXY and MeasuredStdDev_AccelXY parameters (config/6_Sensornoise.txt) are updated.

```
PASS: ABS(Quad.GPS.X-Quad.Pos.X) was less than MeasuredStdDev_GPSPosXY for 70% of the time
PASS: ABS(Quad.IMU.AX-0.000000) was less than MeasuredStdDev_AccelXY for 69% of the time
```

#### 2. Attitude Estimation
The purpose here provides the information to IMU improve the attitude filter correcting the yaw angle. To reduce the error a non-linear approach (forward Euler method) introduced on UpdateFromIMU method. 
```
PASS: ABS(Quad.Est.E.MaxEuler) was less than 0.100000 for at least 3.000000 seconds
```
The implementation can be checked in lines [35 to 111](https://github.com/flaviol-souza/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp) reference the UpdateFromIMU Method of the QuadEstimatorEKF.cpp file.

#### 3. Prediction Step
the Prediction feature is composed of two parts (Prediction and Covariance). As soon as there are two methods to that. O `PredictState` method, that you can check it on  [172 to 179](https://github.com/flaviol-souza/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp) in lines, updates part of the position and velocity. Already the second method is the Covariance step implemented it in lines [262 to 269](https://github.com/flaviol-souza/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp) the function `Predict` in QuadEstimatorEKF.cpp.
Lastly, the GetRbgPrime method available in [205 to 217](https://github.com/flaviol-souza/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp) in QuadEstimatorEKF.cpp calculate the partial derivative of the body-to-global rotation matrix used in `09_PredictCovariance` scenario.


#### 4. Magnetometer Update
the UpdateFromMag method corrects the magnetometer measurements using the error between magnetometer and the current state estimate. The magnetometer update is implemented at [317 to 327](https://github.com/flaviol-souza/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp) in QuadEstimatorEKF.cpp 
```
PASS: ABS(Quad.Est.E.Yaw) was less than 0.120000 for at least 10.000000 seconds
PASS: ABS(Quad.Est.E.Yaw-0.000000) was less than Quad.Est.S.Yaw for 63% of the time
```
#### 5. Closed Loop and GPS Update
This method is more easy to implement because use the P Controller to the yaw control providing the current and commanded yaws. To control the yaw a  linear/proportional controller is used in this method.
```
PASS: ABS(Quad.Est.E.Pos) was less than 1.000000 for at least 20.000000 seconds
```

### Execute the flight
#### 1. Does it work?
It works!