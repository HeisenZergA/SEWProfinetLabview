# Control SEW eurodrive MOVIKIT via Profinet and LabVIEW

Siemens provides an easy guide on how to use Siemens devices with Profinet and LabVIEW. But in fact, you can use TIA and the .dll to implement other devices into your system.
You will REQUIRE the 32-bit version of LabVIEW!
Here I will show you how to impement an SEW eurodrive frequency converter DFC20A with MOVIKIT Positioning / Velocity Drive (used for a pump) into the system.
For a full picture, here are the technical details of the converter: DFC20A-0025-503-A-T00-001
And a picture of our pump:
![witte_pump_with_sew_DFC20A](https://user-images.githubusercontent.com/60081398/124143405-61bcbb00-da8b-11eb-933f-50091c5d42d3.jpg)


Basically, you will need to follow the instructions of the Siemens guide for their "SINAMICS G/S: PROFINET Connection with LabVIEW" with some twists at the end, so please register there to download the .dll required in this project!
![siemens_website_labview](https://user-images.githubusercontent.com/60081398/124143800-b6603600-da8b-11eb-82a9-f88e97f3488c.png)
https://support.industry.siemens.com/cs/document/99684399/sinamics-g-s%3A-profinet-connection-with-labview?dti=0&lc=en-WW
The documentation is marked in blue. Download it as well as the Library marked in yellow. Unzip the library into your user.lib folder to create e.g. C:\Program Files (x86)\National Instruments\LabVIEW 2018\user.lib\PNIO 

Now you will need follow some parts of the Siemens guide:
1) Find MAC adress of your ethernet connection
![find_physical_adress_ethernet](https://user-images.githubusercontent.com/60081398/124143491-7600b800-da8b-11eb-95a5-1ab670f75c51.PNG)

By the way: No need to switch of all connections as described by the Siemens guide.
2) Download and install TIA (https://support.industry.siemens.com/cs/document/109772803/simatic-step-7-incl-safety-and-wincc-v16-trial-download?dti=0&lc=en-WW)
3) Download the GSDML file of the frequency converter to be used in the TIA portal. This can be done either by downloading it from the repo, where there is the data of the DFC20A, or by directly finding your appropriate file in the SEW documentation: https://www.sew-eurodrive.de/os/dud/?tab=software&country=DE&language=de_de&search=TIA
4) Install the GSDML file into TIA. Here is a quick video: 
https://user-images.githubusercontent.com/60081398/124129198-d8eb5280-da7d-11eb-9499-e7d77ef0a319.mp4

5) Find pump in the TIA portal:
![pump_in_TIA_portal](https://user-images.githubusercontent.com/60081398/124132657-5795bf00-da81-11eb-93a6-d1fb72ad76da.jpg)
Please mind the device type of your pump (5) and the IP adress (6). You can change the device adress later on, but it always has to match the adress of the driver in the first 3 numbers (e.g. 192.168.30.3 for the pump and 192.168.30.4 for the driver). The device name can be changed as well.
6) Configure communication
Drag and drop the PROFINET Driver as described in the Siemens guide:
![Siemens_TIA_Profinet_Driver](https://user-images.githubusercontent.com/60081398/124133775-75afef00-da82-11eb-986e-82a64d18100a.PNG)

7) Adjust the IP adress to match the pump in the first 3 numbers:
![Configure_profinet_driver_IP](https://user-images.githubusercontent.com/60081398/124134406-19010400-da83-11eb-91d9-105aa9aebdd2.jpg)
8) Drag and drop the device type of your pump and connect it to the driver using the port. The configuration has to match your physical configuration! If you are using the X1 Port 1 on your pump, you have to connect it to Port 1 in TIA!
![connect_pump_to_profinet](https://user-images.githubusercontent.com/60081398/124137075-9fb6e080-da85-11eb-9f59-7703e808f91a.jpg)
9) Now for the telegram module of the SEW converter: Click on the converter and go to network view and connect the driver to the converter:
![connect_pump_to_profinet_network](https://user-images.githubusercontent.com/60081398/124137503-076d2b80-da86-11eb-864a-30041d09a0dd.jpg)
Next, click on the converter and switch to the Device view.
10) Assign the communcation modules (drag and drop) as well as the E-Adress and A-Adress (this will be the adresses, where the information will be sent or received, e.g. 0 to 31)
![device_view](https://user-images.githubusercontent.com/60081398/124138120-a2660580-da86-11eb-964d-8d2c4f6cd63a.jpg)
11) Create XML-File and copy it into your Labview project folder:
![export_xml](https://user-images.githubusercontent.com/60081398/124138541-07b9f680-da87-11eb-8fc6-c41550a13792.jpg)
12) Open SEW.vi
13) Paste location of the .XML file and E-Adress and A-Adress into the VI, as well as the MAC adress of the ethernet connection (SMALL LETTERS ONLY!!! NO CAPITAL ONES!!!)
14) Use .VI provided to communicate with the converter. In the example, we use the converter with a pump. The .VI is supposed to control the speed of the pump, so here is the actual process data used:
![first_process_word](https://user-images.githubusercontent.com/60081398/124140581-e2c68300-da88-11eb-8ca3-d7eb951f5e40.PNG)
![speed_control](https://user-images.githubusercontent.com/60081398/124140588-e3f7b000-da88-11eb-8008-0894db54ee58.PNG)

You can find the codes for the communication here:
[25871137.pdf](https://github.com/HeisenZergA/SEWProfinetLabview/files/6748825/25871137.pdf)
In chapter 8 you will find all process data that can be used. The process data can either be statuses in bits that are combined into a single number, e.g.: 1100010111 will lead to 1817 (example from the status word of the running pump -> PI 1) or a number like PI2 that represents the actual speed:
![Interface](https://user-images.githubusercontent.com/60081398/124142372-751b5680-da8a-11eb-8311-b128dd5dfff1.PNG)

Behind all that is the PNIO.dll that is fed like a function in a real programming language:
![Block_diagram](https://user-images.githubusercontent.com/60081398/124142908-efe47180-da8a-11eb-857d-ddb42564298c.PNG)
In our .VI we use the following process input data from chapter 8.2 (the data that comes out of the converter... I know....)
![Process_input_data](https://user-images.githubusercontent.com/60081398/124141129-6e401400-da89-11eb-959d-c7dd3a0441d8.PNG)

# Troubleshooting
The main deal is to get the profinet connection established, then the profinet on the pump will switch from red to green.
![profinet_failed](https://user-images.githubusercontent.com/60081398/124143149-2a4e0e80-da8b-11eb-8ad7-019d3a3499e0.jpg)
![profinet_established](https://user-images.githubusercontent.com/60081398/124143168-2d48ff00-da8b-11eb-9a53-e843d34fd6c5.jpg)
If you have problems with my .VI upgrade to LabVIEW 18 or build your own.

# Alternative
Prior to this rapid adaptation of Profinet for LabVIEW, one of the ways you could use Profinet was through the NI hardware and software package: https://www.ni.com/de-de/shop/hardware/products/pxi-profinet-interface-module.html manufactured by KUNBUS: https://www.kunbus.de/df-profinet-io.html
This method is still valid if your are planning to use industrial ethernet and cannot use a normal ethernet switch. On the other hand, academic applications do not require this fast and stable method of communication, so if your goal is to just get a pump or motor running, we recommend our method.
