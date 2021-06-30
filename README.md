# SEWProfinet

Siemens provides an easy guide on how to use Siemens devices with Profinet and LabVIEW. But in fact, you can use TIA and the .dll to implement other devices into your system.
Here I will show you how to impement an SEW eurodrive frequency converter (used for a pump) into the system:

1) Find MAC adress of your switch
2) Install TIA
3) Configure communication
4) Create XML-File
5) Paste location of the .XML file into the VI, as well as the MAC adress of the switch
6) Use .VI provided to communicate with the pump
