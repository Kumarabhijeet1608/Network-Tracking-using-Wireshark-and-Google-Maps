# Network-Tracking-using-Wireshark-and-Google-Maps

## Overview 👀🎯
This project demonstrates how to visualize network traffic using Python, Wireshark, and Google Maps. It covers the implementation steps needed to capture network traffic, process the data, and create a visual presentation using Google Maps.

## External Resources Required 👀
In addition to the Python code created in this tutorial, several external resources and applications are needed to ensure everything works smoothly.


## Downloading GeoLiteCity Database 🔻🔻
First, you will need to download the GeoLiteCity database. This database is used to translate an IP address into a geolocation (longitude & latitude).

### Steps to Download 🔥❗

1. **Access the Database**:
   - Visit the [GeoLiteCity Database GitHub repository](https://github.com/mbcc2006/GeoLiteCity-data).

2. **Download the Database**:
   - Clone the repository or download the ZIP file.
   ```bash
   git clone https://github.com/mbcc2006/GeoLiteCity-data.git

3. **Integrate with Your Project**:
    - Ensure the GeoLiteCity database files are accessible from your project. You will use these files to translate IP addresses into geolocation coordinates.
      


## Downloading Wireshark 🔻🔻

In addition to the GeoLiteCity database, you will also need the Wireshark application to capture network traffic on your device. The captured traffic will serve as input to our Python script and will be visualized using Google Maps.


### Steps to Download and Install Wireshark 🔥❗

1. **Access the Wireshark Website**:
   - Visit the [Wireshark download page](https://www.wireshark.org/).

2. **Download Wireshark**:
   - Choose the appropriate version for your operating system (Windows, macOS, Linux).
   
3. **Configure Wireshark**:
   - Open Wireshark and configure it to capture network traffic on your desired network interface.
   - Start capturing traffic and save the capture file for use with your Python script.

With Wireshark installed and configured, you can capture the network traffic data that will be processed and visualized using Google Maps in the subsequent steps of the readme.


## Downloading Python 3 🔻🔻
The final resource you'll need is Python 3, required to run and compile the code in this tutorial. If you do not already have Python installed on your device, you can download it from the official Python website.

### Steps to Download and Install Python 3 🔥❗

1. **Access the Python Website**:
   - Visit the [official Python download page](https://www.python.org/downloads/).

2. **Download Python 3**:
   - Choose the latest version of Python 3.x for your operating system (Windows, macOS, Linux).
   - Download the installer and follow the installation instructions provided.

3. **Verify the Installation**:
   - Open a terminal or command prompt.
   - Run the following command to verify that Python 3 is installed correctly:
     ```bash
     python3 --version
     ```
   - Ensure the version displayed is Python 3.x.x.

With Python 3 installed on your device, you will be ready to compile and run the Python scripts provided in this readme to process and visualize network traffic data using Google Maps.


## Capturing Network Traffic 📷📸
With Wireshark installed, it's time to create our input data, which will consist of a captured pcap file. This file will include all network traffic to and from our device during the capture period.

### Steps to Capture Network Traffic

1. **Open Wireshark**:
   - Launch the Wireshark application on your device.

2. **Select a Network Interface**:
   - Choose the network interface you want to capture traffic from (e.g., Wi-Fi, Ethernet).
   - Double-click on the interface to start capturing.
  
     <div> 
       <img src="https://img.shields.io/badge/-Microsoft_Sentinel-0078D4?&style=for-the-badge&logo=Microsoft&logoColor=white" /> 
     </div>

When selecting an interface, Wireshark automatically starts a new capture, which is why you immediately get prompted with network traffic.

3. **Start Capturing Traffic**:
   - Wireshark will begin capturing all network traffic on the selected interface.
   - Perform the network activities you want to monitor during this capture period.

     <div> 
       <img src="https://img.shields.io/badge/-Microsoft_Sentinel-0078D4?&style=for-the-badge&logo=Microsoft&logoColor=white" /> 
     </div>

4. **Stop the Capture**:
   - After you have captured sufficient data, click on the red square (Stop) button in Wireshark to stop the capture.

Once the capture has been stopped you need to export the captured data in pcap format, this can be done by clicking File -> Export Specified Packets.
     <div> 
       <img src="https://img.shields.io/badge/-Microsoft_Sentinel-0078D4?&style=for-the-badge&logo=Microsoft&logoColor=white" /> 
     </div>

5. **Save the Capture File**:
   - Go to `File > Save As` and choose a location to save the captured data.
   - Save the file with a `.pcap` extension.

     <div> 
       <img src="https://img.shields.io/badge/-Microsoft_Sentinel-0078D4?&style=for-the-badge&logo=Microsoft&logoColor=white" /> 
     </div>
     
This pcap file will serve as the input for our Python script and will be the data displayed on Google Maps in the subsequent steps of the readme.











## Python Script  ✔🔥
## Here's a simple Python Script using Scapy to sniff for WiFi probe requests.

```python
from scapy.all import *

interface = 'wlan0'
probeReqs = []

def sniffProbes(p):
    if p.haslayer(Dot11ProbeReq):
        netName = p.getlayer(Dot11ProbeReq).info.decode('utf-8', errors='ignore')
        if netName not in probeReqs:
            probeReqs.append(netName)
            print('[+] Detected New Probe Request: ' + netName)

sniff(iface=interface, prn=sniffProbes)
```

## Execution Steps in Kali Linux 👩‍💻👨‍💻


- `ifconfig`
- `wlan0 (Wireless Setup)`
  - `ifconfig wlan0 down                   # We are closing it.`
  - `iwconfig wlan0 mode monitor           # Set it to monitor mode.`
  - `ifconfig wlan0 up                     # We are turning it on.`
- `ifconfig                                # To check it for monitor mode.`
- `ls                                      # We need to execute the Python Script from the terminal.  `
- `cd Desktop`
- `python [File_Name.py]                   # To execute the script.`


## Features 😎💰

### 1. **Packet Capture**
- Captures live network packets on a specified wireless interface.
- Utilizes Scapy, a powerful Python library for network packet manipulation.

### 2. **Probe Request Detection**
- Identifies and captures 802.11 probe request packets.
- Extracts the SSID (network name) from probe requests.

### 3. **Unique SSID Logging**
- Maintains a list of unique SSIDs detected from probe requests.
- Avoids duplicate entries by checking if the SSID is already logged.

### 4. **Real-time Notification**
- Prints a notification to the console whenever a new probe request with a unique SSID is detected.
- Provides real-time updates for immediate insights.


## Installation
To get started, clone the repository and install the required dependencies.

```bash
git clone https://github.com/yourusername/python-sniffing-project.git
cd python-sniffing-project
pip install -r requirements.txt
