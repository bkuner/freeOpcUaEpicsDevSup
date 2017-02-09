#!../../bin/linux-x86_64/OPCUAIOC

# Change to top directory
cd ../..

################## Set up Environment ######################################### 

epicsEnvSet IOC OPCUAIOC
################## Register all support components ############################  

dbLoadDatabase "dbd/OPCUAIOC.dbd",0,0
${IOC}_registerRecordDeviceDriver pdbbase

################### Configure drivers ######################################### 

# local server from freeOpcUaEpicsDevSup project
drvOpcUaSetup("opc.tcp://hazel.acc.bessy.de:4841","","hazel",0)

################### Load Databases ############################################ 

# Load IOC System Stats database (flat)
dbLoadRecords "db/freeopcuaTEST.db"


################### Configure IOC Core ######################################## 

# IOC Log Server Connection  0=enabled 1=disabled
setIocLogDisable 1

# Configure Access Security
#asSetFilename db/no_security.acf

################### Ignition... ############################################### 

iocInit
