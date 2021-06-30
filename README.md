# SEWProfinet

Siemens provides an easy guide on how to use Siemens devices with Profinet and LabVIEW. But in fact, you can use TIA and the .dll to implement other devices into your system.
You will REQUIRE the 32-bit version of LabVIEW!
Here I will show you how to impement an SEW eurodrive frequency converter (used for a pump) into the system:

1) Find MAC adress of your switch
2) Install TIA
3) Configure communication
4) Create XML-File
5) Paste location of the .XML file into the VI, as well as the MAC adress of the switch
6) Use .VI provided to communicate with the pump


Materials needed:
1) https://support.industry.siemens.com/cs/document/99684399/sinamics-g-s%3A-profinet-anbindung-an-labview?dti=0&lc=de-WW
2) https://www.sew-eurodrive.de/os/dud/?tab=software&country=DE&language=de_de&search=TIA
3) https://download.sew-eurodrive.com/download/soft/TIA_V13_GSDML-Import_V1_0_DE.mp4
