# Estimation Project #

The following explains how the project Estimation Project was implemented.

The project is structured to progress incrementaly by passing the test of different scenarios. Simillarly to the 3D controller

### Sensor Noise (scenario 6) ###

To set `MeasuredStdDev_GPSPosXY` and `MeasuredStdDev_AccelXY` I used the data obtained in `config/log/Graph2*.txt`, calculating std deviation in libreOffice calc.

<p align="center">
<img src="images_results/scenario6.png" width="500"/>
</p>

### Attitude Estimation (scenario 7) ###

We use the Non linear complementary filter. First we obtain the quaternion state `qt` and the meassurements from IMU `dq`. The predicted state is then `qt_bar` the dot product `qt` and `dq`.

Now predicted roll, pitch and yaw, are obtained from the predicted quaternion `qt_bar`.

<p align="center">
<img src="images_results/scenario7.png" width="500"/>
</p>


### Prediction Step (scenario 8 and 9) ###

In `PredictState()` we only predict the state without considering the control input.

<p align="center">
<img src="images_results/scenario8.png" width="500"/>
</p>

In `GetRbgPrime()` we calculate Rgb as defined [Estimation for Quadrotors](https://www.overleaf.com/read/vymfngphcccj)

Later we implement the complete predicton in `Predict()`. We fill gPrime using `GetRbgPrime()` and calculate `ekfCov` as defined in [Estimation for Quadrotors](https://www.overleaf.com/read/vymfngphcccj)

Finally we tune `QPosXYStd` and the `QVelXYStd` in order to get a good covariance prediction

<p align="center">
<img src="images_results/scenario9.png" width="500"/>
</p>

### Magnetometer Update (scenario 10) ###

Here we have to implement `UpdateFromMag()` we simply initialize `zFromX` with `ekfstate(6)` that was previusly updated in `UpdateFromIMU()`.

<p align="center">
<img src="images_results/scenario10.png" width="500"/>
</p>

### Closed Loop + GPS Update (scenario 11) ###

We fill `UpdateFromGPS()` we simply provide the ekfState and define hPrime.

We use our `QuadControl` implementation from the previous project. Detuning was not necessary. The previous parameters work fine also with the estimated state variables.

<p align="center">
<img src="images_results/scenario11.png" width="500"/>
</p>
