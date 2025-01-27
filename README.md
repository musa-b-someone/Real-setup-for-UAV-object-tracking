# Real-setup-for-UAV-object-tracking
Real setup for-UAV-object-tracking

If any issues arise, let me know, and I’ll help troubleshoot further. 😊 (you know how to do that)
---

### **1. Hardware Setup (The Building Phase)**

#### **1.1 Gather All the Equipment**
- **Drone Frame:** Choose a pre-built frame with motor mounts, like the F450 Quadcopter frame. 
- **Motors & ESCs:** Brushless motors (e.g., 2212/920KV motors) and compatible ESCs (Electronic Speed Controllers).
- **PixHawk 4 Flight Controller:** The brain of the drone.
- **Raspberry Pi 4 (with power supply):** Get the 4GB or 8GB model.
- **Raspberry Pi Camera Module V2:** Official Pi camera with 8MP resolution.
- **Telemetry Module:** 915 MHz or 433 MHz telemetry radios for communication.
- **Battery & Propellers:** A LiPo battery (e.g., 11.1V 3S 5200mAh) and balanced propellers.
- **Ground Station Computer:** A laptop or PC to run Mission Planner and the detection scripts.
- **OTG Receiver (Optional):** A small USB video receiver for live video streaming.

#### **1.2 Building the Drone**
1. **Attach the Motors:**
   - Mount the 4 motors onto the drone frame. Connect each motor to its ESC.
2. **Install PixHawk 4:**
   - Mount the PixHawk securely on the drone using screws or adhesive pads. Use a vibration-dampening mount if possible.
3. **Connect Components to PixHawk:**
   - **ESCs:** Connect each ESC to the PixHawk’s motor outputs (e.g., M1–M4).
   - **GPS Module:** Plug the GPS module into the GPS port on the PixHawk.
   - **Telemetry Module:** Connect the telemetry radio to the telemetry port (TELEM1 or TELEM2).
   - **Power Module:** Use a power distribution board or connect the battery to PixHawk via the power port.
4. **Install the Raspberry Pi:**
   - Secure the Raspberry Pi to the frame using screws or tape.
   - Connect the Raspberry Pi to PixHawk using a USB cable (USB-A to Micro USB or USB-C depending on the Pi).
5. **Attach the Camera Module:**
   - Connect the Raspberry Pi Camera Module to the Pi via the camera ribbon cable.

#### **1.3 Pre-Flight Checks**
1. Make sure **all connections are secure.**
2. Charge the LiPo battery.
3. Test each motor manually by spinning it via Mission Planner.

---

### **2. Mission Planner Setup (Configuring the PixHawk)**

#### **2.1 Install Mission Planner**
1. Go to [Mission Planner’s Download Page](http://ardupilot.org/planner/docs/mission-planner-installation.html) and install it.
2. Connect the PixHawk to your Ground Station computer using a USB cable.

#### **2.2 Calibrate the Drone**
1. **Accelerometer Calibration:**
   - Mission Planner will guide you to place the drone in different orientations (flat, on its sides, upside-down).
2. **Compass Calibration:**
   - Rotate the drone in all directions while following on-screen instructions.
3. **Radio Calibration:**
   - Connect your RC transmitter and calibrate it by moving the sticks as instructed.
4. **GPS Setup:**
   - Ensure GPS has a clear view of the sky and wait until it locks onto satellites.

#### **2.3 Failsafe Configuration**
1. Set up **RTL (Return to Launch)** to ensure the drone automatically returns home if communication is lost.
2. Set a geofence to keep the drone from flying out of bounds.

---


### **3. Raspberry Pi Setup (Companion Computer)**

#### **3.1 Install Raspberry Pi OS**
First, you'll need to get the Raspberry Pi ready. Here’s how to install the operating system and make sure everything works:

##### **Step 1: Download Raspberry Pi Imager**
1. Go to the [Raspberry Pi Downloads page](https://www.raspberrypi.com/software/).
2. Download the **Raspberry Pi Imager** for your computer (it’s available for Windows, macOS, and Ubuntu).
3. Install the Raspberry Pi Imager on your computer.

##### **Step 2: Write Raspberry Pi OS onto an SD Card**
1. Insert a **microSD card** (at least 16GB, Class 10 recommended) into your computer using an SD card reader.
2. Open **Raspberry Pi Imager**, select the operating system. Choose **Raspberry Pi OS (32-bit)** for general use.
3. Choose the **SD card** from the list of available drives.
4. Click **Write**. The software will download and write the OS to the SD card. This may take a few minutes.
5. Once the process is complete, remove the SD card from your computer.

##### **Step 3: Set up Raspberry Pi OS**
1. Insert the microSD card into the **Raspberry Pi 4**.
2. Plug in the **keyboard**, **mouse**, and **monitor** into the Raspberry Pi.
3. Connect the **Raspberry Pi to Wi-Fi**:
   - On the initial boot, the Pi will ask you to set your country, language, and Wi-Fi network.
   - Enter your Wi-Fi details and continue with the setup.

---

#### **3.2 Enable Necessary Features on Raspberry Pi**

To make the Raspberry Pi work correctly with the camera and other components, there are a few features we need to enable.

##### **Step 1: Enable the Camera**
1. Open a terminal window on the Raspberry Pi (you can press **Ctrl+Alt+T**).
2. Type the following command to open the configuration menu:
   ```bash
   sudo raspi-config
   ```
3. Use the arrow keys to navigate to **Interface Options**.
4. Select **Camera** and choose **Enable**.
5. **Reboot the Raspberry Pi** when prompted.

##### **Step 2: Update the Raspberry Pi OS**
Updating ensures that the Pi has the latest software, which will help avoid compatibility issues later.

1. In the terminal, type:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
   This command will update the list of available software and install any updates.
2. After the update is complete, reboot the Raspberry Pi:
   ```bash
   sudo reboot
   ```

---

#### **3.3 Install Software and Libraries**

We need to install the required libraries and tools that the detection and tracking system will rely on.

##### **Step 1: Install Python and Pip**
Python is the main programming language used to run YOLOv7 and other scripts. Pip is the package manager that allows us to install Python libraries.

1. Open a terminal and type the following command to install Python 3 and pip:
   ```bash
   sudo apt install python3 python3-pip -y
   ```

##### **Step 2: Install OpenCV (For Camera and Video Processing)**
OpenCV is a powerful library used to process video streams and images from the camera. YOLOv7 and SORT will both use it.

1. Install OpenCV by typing:
   ```bash
   pip3 install opencv-python
   ```

##### **Step 3: Install PyTorch and torchvision (For YOLOv7)**
YOLOv7 uses the PyTorch library for running deep learning models. `torchvision` is used for computer vision tasks.

1. Install PyTorch and torchvision using pip:
   ```bash
   pip3 install torch torchvision
   ```

##### **Step 4: Install Geopy (For Geofencing)**
Geopy will help us check if the detected target is inside a predefined geofence or not.

1. Install Geopy by typing:
   ```bash
   pip3 install geopy
   ```

##### **Step 5: Install DroneKit (For Communication with Pixhawk)**
DroneKit is used to communicate with the Pixhawk flight controller. It helps send target coordinates and control the drone.

1. Install DroneKit by typing:
   ```bash
   pip3 install dronekit
   ```

##### **Step 6: Clone the Repositories for YOLOv7 and SORT**
You’ll need to clone the repositories for YOLOv7 (for detection) and SORT (for tracking).

1. First, clone the YOLOv7 repository:
   ```bash
   git clone https://github.com/WongKinYiu/yolov7.git
   cd yolov7
   pip install -r requirements.txt
   ```
   This will install all dependencies required to run YOLOv7.

2. Next, clone the SORT repository for tracking:
   ```bash
   git clone https://github.com/abewley/sort.git
   cd sort
   pip install .
   ```

---

#### **3.4 Install Additional Dependencies (If Required)**
Sometimes, you may run into missing dependencies (e.g., numpy, matplotlib). If that happens, you can install them like this:

1. To install NumPy:
   ```bash
   pip3 install numpy
   ```

2. To install Matplotlib (used for plotting):
   ```bash
   pip3 install matplotlib
   ```

---

#### **3.5 Test the Camera**
Before continuing, you should test if the Raspberry Pi camera is working properly.

1. Run the camera test using this command:
   ```bash
   raspistill -o test_image.jpg
   ```
   This will take a picture and save it as `test_image.jpg`.
2. If everything works fine, you’ll see the image saved to your Pi. If not, double-check the camera connection.

---

### **3.6 Final Steps Before Running the Script**
Now that the Raspberry Pi is ready, you’ll need to make sure all the pieces are connected and configured properly before running the main detection and tracking script.

1. **Connect the Raspberry Pi Camera to the Pi:**
   - Ensure the ribbon cable from the camera is properly inserted into the Pi’s camera port (CSI connector).
   
2. **Connect the Raspberry Pi to the Pixhawk:**
   - Connect the **USB cable** from the Raspberry Pi to the Pixhawk flight controller. This connection allows the Pi to send commands and receive telemetry from the drone.

3. **Ensure Wi-Fi or Ethernet is Set Up:**
   - The Raspberry Pi should be connected to the internet for updates or downloading any dependencies you might need.

4. **Connect the Telemetry Module to the Ground Station:**
   - Ensure the **telemetry radio** is plugged into the Raspberry Pi (or your computer if you’re using a USB telemetry module).

---

### **Recap of the Raspberry Pi Setup**
You’ve now completed setting up the Raspberry Pi as the companion computer for your drone. Here’s a quick recap of what you’ve done:
- Installed Raspberry Pi OS.
- Enabled the camera and updated the system.
- Installed necessary software and libraries (OpenCV, PyTorch, etc.).
- Cloned YOLOv7 and SORT repositories.
- Tested the camera to ensure it’s working.
- Made sure the Raspberry Pi is ready for communication with Pixhawk and the Ground Station.

Your Raspberry Pi is now ready to stream video, detect and track objects, and communicate with the Pixhawk for autonomous control!

---

This detailed explanation should make the process smooth for your brother! If he follows these steps carefully, he’ll have the Raspberry Pi set up without facing confusion. Feel free to let me know if any part of the setup needs further clarification. 😊
### **4. Running the Detection Script**

#### **4.1 Setting Up the Ground Station**
1. Clone the UAV tracking repo on the Ground Station computer:
   ```bash
   git clone https://github.com/adityachintala/UAV-tracking-tank
   cd UAV-tracking-tank
   pip install -r requirements.txt
   ```
2. Connect the telemetry module to the computer and note the COM port (e.g., COM3 on Windows or `/dev/ttyUSB0` on Linux).

#### **4.2 Run the Script**
1. Run the detection and tracking script from the Ground Station:
   ```bash
   python detect_and_track.py --source 0 --weights yolov7.pt --baud 57600 --altitude 4 --connect com3 --view-img
   ```
   Replace `--connect com3` with your PixHawk’s connection port.

#### **4.3 Understand the Workflow**
- The camera feed from the drone is analyzed using YOLOv7 to detect the target (e.g., a tank).
- SORT tracks the movement of the detected object.
- Geopy checks if the target is inside a geofence or locked within the drone’s center.

---

### **5. Testing and Troubleshooting**

#### **5.1 Pre-Flight Tests**
1. Power up the drone and Raspberry Pi.
2. Ensure Mission Planner shows all systems (GPS, telemetry, compass) working.
3. Verify the video feed is visible on the Ground Station.

#### **5.2 Common Errors and Fixes**
| **Error**                       | **Solution**                                                                                   |
|----------------------------------|-----------------------------------------------------------------------------------------------|
| Raspberry Pi Camera not detected | Ensure the ribbon cable is connected properly and the camera is enabled in `raspi-config`.    |
| Telemetry not connecting         | Check the COM port and baud rate. Verify the module is powered and paired.                    |
| YOLOv7 script crashes            | Ensure `torch` and `torchvision` are installed. Run `pip install torch torchvision`.          |
| GPS signal weak                  | Test outdoors with a clear sky view. Use an external GPS module if necessary.                |
| Drone doesn't move autonomously  | Check if coordinates are sent to PixHawk via Dronekit. Debug with logs in `Mission Planner`. |

#### **5.3 First Flight**
1. Test in an open field with no obstacles.
2. Keep the drone at a low altitude during initial tests.
3. Always have the RC transmitter ready for manual control in emergencies.

---

### **6. Safety Checklist**
- Ensure all batteries are fully charged.
- Test failsafe mechanisms (RTL, geofence).
- Verify the target detection accuracy before flight.
- Avoid flying in high winds or near people.

---

If any issues arise, let me know, and I’ll help troubleshoot further. 😊 (you know how to do that)
