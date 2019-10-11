# Iot-by-roster1
Iot by roster1
Oscilloscope in raspberry pi
Step 1: Enable Raspberry Pi I2C interface 
To enable the I2C, from the terminal, run; 

sudo raspi-config 


When the configuration panels open, select interface options, select I2C and click enable. 

Step 2: Update the Raspberry pi 
The first thing I do before starting any project is updating the Pi. Through this, I am sure every thing on the OS is up to date and I won’t experience compatibility issue with any latest software I choose to install on the Pi. To do this, run below two commands: 

sudo apt-get update 
sudo apt-get upgrade 







Step 3: Install the Adafruit ADS1115 library for ADC 
With the update done, we are now ready to install the dependencies starting with the Adafruit python module for the ADS115 chip. Ensure you are in the Raspberry Pi home directory by running; 

cd ~


then install the build-essentials by running; 

sudo apt-get install build-essential python-dev python-smbus git 


Next, clone the Adafruit git folder for the library by running; 

git clone https://github.com/adafruit/Adafruit_Python_ADS1x15.git



Change into the cloned file’s directory and run the setup file; 
cd Adafruit_Python_ADS1x1z 
sudo python setup.py install 


After installation, your screen should look like the image below. 




Step 4: Test the library and 12C communication.
 
To test the library and ensure the ADC can communicate with the raspberry pi over I2C, we will use an example script that comes with the library. 

While still in the Adafruit_Python_ADS1x15 folder, change directory to the examples directory by running; 

cd examples 


Next, run the sampletest.py example which displays the value of the four channels on the ADC in a tabular form. Run the example using: 

python simpletest.py 


If the I2C module is enabled and connections good, you should see the data as shown in the image below. 

If an error occurs, check to ensure the ADC is well connected to the PI and I2C communication is enabled on the Pi. 

Step 5: Install Matplotlib
 
To visualize the data we need to install the matplotlib module which is used to plot all kind of graphs in python. This can be done by running; 

sudo apt-get install python-matplotlib 











You should see an outcome like the image below. 

Step6: Install the Drawnow python module 

Lastly, we need to install the drawnow python module. This module helps us provide live updates to the data plot. 

We will be installing drawnow via the python package installer; pip, so we need to ensure it is installed. This can be done by running; 

sudo apt-get install python-pip 


We can then use pip to install the drawnow package by running: 

sudo pip install drawnow 

You should get an outcome like the image below after running it. 

With all the dependencies installed, then next step is to write the code. 

Python Code for Raspberry Pi Oscilloscope: 

The python code for this Pi Oscilloscope is fairly simple especially if you are familiar with the python matplotlib module. 
At this stage it is important to switch to a monitor through which you can see your Raspberry Pi’s desktop, as the graph being plotted won’t show on the terminal. 

With the monitor as the interface open a new python file. 

sudo nano scope.py 






Python Program:
import time
import matplotlib.pyplot as plt
from drawnow import *
import Adafruit_ADS1x15
adc=Adafruit_ADS1x15.ADS1115()

GAIN=1
val=[]
cnt=0
plt.ion()
adc.start_adc(0,gain=GAIN)
print('Reading ADS1x15 channel 0')
def makeFig():
  plt.ylim(-5000,5000)
  plt.title('Oscilloscope')
  plt.grid(True);
  plt.ylabel('ADC outputs')
  plt.plot(val,'ro-',label='Channel 0')
  plt.legend(loc='lower right')
while (True):
    value=adc.get_last_result()
    print('Channel 0: {0}'.format(value))
    time.sleep(0.5)
    val.append(int(value))
    drawnow(makeFig)
    plt.pause(.000001)
    cnt=cnt+1
    if(cnt>50):
        val.pop(0)
