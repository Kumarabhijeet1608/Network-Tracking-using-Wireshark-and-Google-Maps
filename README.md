# Network-Tracking-using-Wireshark-and-Google-Maps

## Overview 👀🎯
This project demonstrates how to visualize network traffic using Python, Wireshark, and Google Maps. It covers the implementation steps needed to capture network traffic, process the data, and create a visual presentation using Google Maps.

## External Resources Required 👀
In addition to the Python code created in this tutorial, several external resources and applications are needed to ensure everything works smoothly.


## Downloading GeoLiteCity Database ✔🔥
<details>
  <summary>Click to expand</summary>
  
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
</details>

## Downloading Wireshark ✔🔥
<details>
  <summary>Click to expand</summary>
  
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
</details>

## Downloading Python 3 ✔🔥
<details>
  <summary>Click to expand</summary>
  
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
</details>

## Capturing Network Traffic 📷📸
With Wireshark installed, it's time to create our input data, which will consist of a captured pcap file. This file will include all network traffic to and from our device during the capture period.

### Steps to Capture Network Traffic
<details>
  <summary>Click to expand</summary>
  
1. **Open Wireshark**:
   - Launch the Wireshark application on your device.

2. **Select a Network Interface**:
   - Choose the network interface you want to capture traffic from (e.g., Wi-Fi, Ethernet).
   - Double-click on the interface to start capturing.
  
     <div> 
       <img src="https://github.com/Kumarabhijeet1608/Network-Tracking-using-Wireshark-and-Google-Maps/blob/main/1.png" /> 
     </div>

When selecting an interface, Wireshark automatically starts a new capture, which is why you immediately get prompted with network traffic.

3. **Start Capturing Traffic**:
   - Wireshark will begin capturing all network traffic on the selected interface.
   - Perform the network activities you want to monitor during this capture period.

     <div> 
       <img src="https://github.com/Kumarabhijeet1608/Network-Tracking-using-Wireshark-and-Google-Maps/blob/main/2.png" /> 
     </div>

4. **Stop the Capture**:
   - After you have captured sufficient data, click on the red square (Stop) button in Wireshark to stop the capture.

Once the capture has been stopped you need to export the captured data in pcap format, this can be done by clicking File -> Export Specified Packets.
      <div> 
         <img src="https://github.com/Kumarabhijeet1608/Network-Tracking-using-Wireshark-and-Google-Maps/blob/main/3.png" /> 
      </div>

5. **Save the Capture File**:
   - Save the file with a `.pcap` extension.
   - Ex: wire.pcap

     <div> 
       <img src="https://github.com/Kumarabhijeet1608/Network-Tracking-using-Wireshark-and-Google-Maps/blob/main/4.png" /> 
     </div>
     
This pcap file will serve as the input for our Python script and will be the data displayed on Google Maps in the subsequent steps of the readme.
</details>

# Python Implementation 👩‍💻👨‍💻
# First, import the necessary libraries

```python
import dpkt
import socket
import pygeoip

gi = pygeoip.GeoIP('GeoLiteCity.dat')
```

As you might observe, a variable is initialized with the GeoLiteCity database that was downloaded earlier. Ensure that the GeoLiteCity.dat file is placed in the root folder of your Python project unless you intend to modify the path of the variable.

# Implementing the Main Method
Next, you will implement the main method, which will open your captured data, and create the header and footer of the KML file. This KML file will serve as the output file that we will upload to Google Maps.

## Steps:

1. **Open Captured Data**:
   - Open your captured data file (`.pcap`) located at the root of the project.

2. **Create Header and Footer**:
   - Generate the header and footer of the KML file, which is the output file that we will upload to Google Maps.
  
Again be sure to place you captured .pcap file in the root of the project and change the name of the code below if you did not save it under the name wire.pcap

## Python Code:

```python
def main():
    f = open('wire.pcap', 'rb')
    pcap = dpkt.pcap.Reader(f)
    kmlheader = '<?xml version="1.0" encoding="UTF-8"?> \n<kml xmlns="http://www.opengis.net/kml/2.2">\n<Document>\n'\
    '<Style id="transBluePoly">' \
                '<LineStyle>' \
                '<width>1.5</width>' \
                '<color>501400E6</color>' \
                '</LineStyle>' \
                '</Style>'
    kmlfooter = '</Document>\n</kml>\n'
    kmldoc=kmlheader+plotIPs(pcap)+kmlfooter
    print(kmldoc)
```
If you wish to change the styling of the lines that will be drawn in Google Maps, you will do so in the above code under the <style></style> tags.
Next, we'll add the method that will loop over our captured network data and extract the IP addresses.

```python
def plotIPs(pcap):
    kmlPts = ''
    for (ts, buf) in pcap:
        try:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            src = socket.inet_ntoa(ip.src)
            dst = socket.inet_ntoa(ip.dst)
            KML = retKML(dst, src)
            kmlPts = kmlPts + KML
        except:
            pass
    return kmlPts
```

In the plotIPs(pcap) method mentioned above, the application will iterate over our pcap data and extract the source and destination IP addresses of each captured network packet.

However, our IP addresses alone cannot be used as input to Google Maps. We first need to associate each IP address with a geographic location. This will be accomplished using the following method:

```python
def retKML(dstip, srcip):
    dst = gi.record_by_name(dstip)
    src = gi.record_by_name('x.xxx.xxx.xxx')
    try:
        dstlongitude = dst['longitude']
        dstlatitude = dst['latitude']
        srclongitude = src['longitude']
        srclatitude = src['latitude']
        kml = (
            '<Placemark>\n'
            '<name>%s</name>\n'
            '<extrude>1</extrude>\n'
            '<tessellate>1</tessellate>\n'
            '<styleUrl>#transBluePoly</styleUrl>\n'
            '<LineString>\n'
            '<coordinates>%6f,%6f\n%6f,%6f</coordinates>\n'
            '</LineString>\n'
            '</Placemark>\n'
        )%(dstip, dstlongitude, dstlatitude, srclongitude, srclatitude)
        return kml
    except:
        return ''
```
In the above method, Geo locations are located for each IP address followed by a conversion into KML format.

Be aware that in the above example, I have masked my source IP address with "x.xxx.xxx.xxx". To make the code work for your network, you will need to navigate to [https://www.whatsmyip.org/](https://www.whatsmyip.org/) and locate your IP address, then replace "x.xxx.xxx.xxx" with your actual IP address in the code.

The reason for this is that our captured pcap data file contains internal IP addresses, which are not known outside our home network. Capturing the external IP automatically would require additional steps and is beyond the scope of this tutorial.

# Execute the above code using:

```python
if __name__ == '__main__':
    main()
```

## Adding Data to Google Maps

With our newly created .kml file, we’re now ready to add it to Google Maps. 

1. Navigate to [Google My Maps](https://www.google.com/mymaps).
2. Create a new map or select an existing one.
3. Click on "Import" and select "Import from file".
4. Choose the .kml file that we generated earlier.
5. Once uploaded, the data from the .kml file will be displayed on your map.
6. Now you should see the network traffic being displayed on the map.

Now you can visualize your network traffic data on Google Maps!


## Features 😎💰
- Capture network traffic data using Wireshark.
- Process captured data to extract IP addresses.
- Associate IP addresses with geographic locations using the GeoLiteCity database.
- Generate a KML file containing geolocated data.
- Upload and visualize the data on Google Maps.


## Installation
To get started, clone the repository and install the required dependencies.

```bash
git clone https://github.com/yourusername/Network-Tracking-using-Wireshark-and-Google-Maps.git
cd python-Network-Tracking-using-Wireshark-and-Google-Maps
pip install -r requirements.txt
