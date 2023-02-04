# MPU6050-MicroPython
A very simple library and useful for GY-521 IMU 3-axis Accelerometer/Gyro Module (MPU6050) on ESP32 or ESP8266 using MicroPython

# Usage
Initializing the I2C method for ESP32\
Pin assignment:\
SCL -> GPIO 22\
SDA -> GPIO 21\
self.i2c = SoftI2C(scl=Pin(22), sda=Pin(21), freq=100000)\
\
(You can change SCL pin and SDA pin in file MPU6050.py line 73)
##
Initializing the I2C method for ESP8266\
Pin assignment:\
SCL -> GPIO 5\
SDA -> GPIO 4\
self.i2c = I2C(scl=Pin(5), sda=Pin(4))

(You can change SCL pin and SDA pin in file MPU6050.py line 79)
##
If you working with Thonny (https://thonny.org/). \
Don't forget to show plotter\
Thonny -> View -> Plotter\
\
If you want to see data_logs file.\
Thonny -> View -> Files
##
## Example
[main.py](https://github.com/Lezgend/MPU6050-MicroPython/blob/main/main.py) is a very simple example to get started.\
\
Everytime you run the code it will keep data_logs{n} file in root (/)\
{n} means the file number increasing if old data_logs file exist.

```python
# Example code for (GY-521) MPU6050 Accelerometer/Gyro Module
# Write in MicroPython by Warayut Poomiwatracanont JAN 2023

from MPU6050 import MPU6050

from os import listdir, chdir
from machine import Pin
from time import sleep_ms

mpu = MPU6050()

# List all files directory.
print("Root directory: {} \n".format(listdir()))

# Change filename and fileformat here.
filename = "{}%s.{}".format("data_logs", "txt")

# Increment the filename number if the file already exists.
i = 0
while (filename % i) in listdir():
    i += 1

# Save file in path /
with open(filename % i, "w") as f:
    cols = ["Temp", "AcX", "AcY", "AcZ"]
    f.write(",".join(cols) + "\n")
    
    while True:
        # Accelerometer Data
        accel = mpu.read_accel_data() # read the accelerometer [ms^-2]
        aX = accel["x"]
        aY = accel["y"]
        aZ = accel["z"]
        print("x: " + str(aX) + " y: " + str(aY) + " z: " + str(aZ))
    
        # Gyroscope Data
        # gyro = mpu.read_gyro_data()   # read the gyro [deg/s]
        # gX = gyro["x"]
        # gY = gyro["y"]
        # gZ = gyro["z"]
        # print("x:" + str(gX) + " y:" + str(gY) + " z:" + str(gZ))
    
        # Rough Temperature
        temp = mpu.read_temperature()   # read the device temperature [degC]
        # print("Temperature: " + str(temp) + "°C")

        # G-Force
        # gforce = mpu.read_accel_abs(g=True) # read the absolute acceleration magnitude
        # print("G-Force: " + str(gforce))
    
        # Write to file
        data = {"Temp" : temp,
                "AcX" : aX,
                "AcY" : aY,
                "AcZ" : aZ
                }
        
        push = [  str(data[k]) for k in cols ]
        row = ",".join(push)
        f.write(row + "\n")
        
        # Time Interval Delay in millisecond (ms)
        sleep_ms(100)
```

Result of Accelerometer Data:

```
x: 0.4668693 y: -2.267309 z: 10.82179
x: 0.8762778 y: -1.209072 z: 10.78109
x: 0.4213795 y: -1.343147 z: 10.97981
x: 0.3758897 y: -1.371878 z: 10.80024
x: 0.5123591 y: -1.352724 z: 10.84334
x: 0.3830723 y: -1.33357 z: 10.87686
x: 0.6153098 y: -1.501164 z: 10.82419
x: 0.3280056 y: -1.570596 z: 10.92235
x: 0.7469909 y: -1.458069 z: 10.89362
x: 0.4525041 y: -1.625663 z: 10.63744
```

Result of Gyroscope Data:
```
x: 34.78626 y: -2.045802 z: 18.48855
x: -24.98473 y: -250.1374 z: -25.20611
x: -31.81679 y: -170.8321 z: 35.36641
x: -36.03053 y: -105.2977 z: 79.84733
x: -80.12214 y: -197.2748 z: 60.56489
x: -54.65649 y: -249.4198 z: 106.2748
x: 73.03817 y: -111.4427 z: 59.35878
x: 52.22901 y: 81.37404 z: -45.03817
x: 15.53435 y: 22.45802 z: -21.93893
x: -81.67175 y: 139.2977 z: 65.67175
x: -141.0458 y: 10.38931 z: 54.66413
```

Result of Temperature Data:

```
Temperature: 32.48294°C
Temperature: 32.57706°C
Temperature: 32.48294°C
Temperature: 32.53°C
Temperature: 32.48294°C
Temperature: 32.48294°C
Temperature: 32.53°C
Temperature: 32.57706°C
Temperature: 32.57706°C
Temperature: 32.53°C
```

Result of G-Force Data:
```
G-Force: 1.833724
G-Force: 1.848333
G-Force: 1.843979
G-Force: 1.347741
G-Force: 0.8332062
G-Force: 2.14345
G-Force: 1.266751
G-Force: 0.9586555
G-Force: 0.9757478
G-Force: 1.224127
```
