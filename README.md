
# Install OpenCV 3.4.0 with Python 3 on Raspberry Pi

# Steps 

# Step 1: Expand filesystem

Type the following command to expand the Raspberry Pi3 file system
• sudo raspi-config

Then select the following

• Advanced Options > A1 Expand filesystem > Press “Enter”

# Step 2: Install Dependencies

The first step is to update and upgrade any existing packages:
Then reboot your pi.

Install CMAKE developer packages

• sudo apt-get install build-essential cmake pkg-config -y

Install Image I/O packages

• sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev -y

Install Video I/O packages

• sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev -y

• sudo apt-get install libxvidcore-dev libx264-dev -y

Install the GTK development library for basic GUI windows

• sudo apt-get install libgtk2.0-dev libgtk-3-dev -y

Install optimization packages (improved matrix operations for OpenCV)

• sudo apt-get install libatlas-base-dev gfortran -y

• sudo apt-get update

• sudo apt-get upgrade

# Step 3: Install Python 3, setuptools, dev and Numpy

Install Python 3 and numpy

• sudo apt-get install python3 python3-setuptools python3-dev -y

• wget https://bootstrap.pypa.io/get-pip.py

• sudo python3 get-pip.py

• sudo pip3 install numpy

# Step 4: Download the OpenCV 3.4 and contrib extra modules

• cd ~

• wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.4.0.zip

• unzip opencv.zip

• wget -O opencv_contrib.zip

https://github.com/Itseez/opencv_contrib/archive/3.4.0.zip

• unzip opencv_contrib.zip

# Step 5: Compile and Install OpenCV 3.4.0 for Python 3

• cd opencv-3.4.0

• mkdir build

• cd build

• cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D BUILD_opencv_java=OFF \
-D BUILD_opencv_python2=OFF \
-D BUILD_opencv_python3=ON \
-D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D BUILD_EXAMPLES=ON\
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.4.0/modules \
-D WITH_CUDA=OFF \
-D BUILD_TESTS=OFF \
-D BUILD_PERF_TESTS= OFF ..

# Step 6 Swap Space size before compiling to add more virtual memory
It will enable OpenCV to compile with all four cores of the Raspberry PI without any
memory issues. Open your /etc/dphys-swapfile and then edit the CONF_SWAPSIZE variable

• sudo nano /etc/dphys-swapfile
It will open the nano editor for editing the CONF_SWAPSIZE. Change it like below:
.#set size to absolute value, leaving empty (default) then uses computed value
.#you most likely don't want this, unless you have an special disk situation
.#CONF_SWAPSIZE=100
CONF_SWAPSIZE=1024

Then save the changes you’ve made

Then type the following lines to take it into effect

• sudo /etc/init.d/dphys-swapfile stop

• sudo /etc/init.d/dphys-swapfile start

# Step 7: Finally, Ready to be Compile

Type the following command to compile it using 4 cores of pi

• make -j4

Step Optional: Compile with a single core of Pi

If you face any error while compiling due to memory issue you can start the compilation

again with only one core using the following command

• make clean

• make
 
# Step 8: Install the build on raspberry pi

After the successful build install the build using the following command

• sudo make install

• sudo ldconfig

# Step 9: Verify the OpenCV build

After running make install, OpenCV + Python bindings should be installed in

usr/local/lib/python3.5/dist-packages or usr/local/lib/python3.5/site-packages.

You need to use the site-packages or dist-packages. Look where it has been created and use
that site-packages or dist-packages. You can verify this with the ls command:

• ls -l /usr/local/lib/python3.5/dist-packages/

Look for a name like cv2.so and if it is not there then look for a name like cv2.cpython-35marm-linux-gnueabihf.so (name starting with cv2. and ending with .so). It might happen due
to some bugs in Python binding library for Python 3.
We need to rename cv2.cpython-35m-arm-linux-gnueabihf.so to cv2.so using the following
command:

• cd /usr/local/lib/python3.5/dist-packages/

• sudo mv /usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-arm-linuxgnueabihf.so cv2.so

# Step 10: Testing OpenCV 3.4.0 install

• $ python3

• >>> import cv2
>>> cv2.__version__
'3.4.0'

# Step 11: Don’t forget to change your swap size back!
Open your /etc/dphys-swapfile and then edit the CONF_SWAPSIZE variable

• sudo nano /etc/dphys-swapfile

It will open the nano editor for editing the CONF_SWAPSIZE. Change it like below:

.#set size to absolute value, leaving empty (default) then uses computed value
.#you most likely don't want this, unless you have an special disk situation
CONF_SWAPSIZE=100
.#CONF_SWAPSIZE=1024

Then save the changes you’ve made
Then type the following lines to take it into effect

• sudo /etc/init.d/dphys-swapfile stop

• sudo /etc/init.d/dphys-swapfile start
