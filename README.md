## Broadlink Devices API implementation

### Description

This package is originally in this repo [kwkoo/golang_broadlink_rm](kwkoo/golang_broadlink_rm), originally this project was 
also based on famous one of the first implementations of Broadlink API on Python here: [mjg59/python-broadlink](https://github.com/mjg59/python-broadlink).
I've trimmed the API by removing some extra things (e.g. Proxy server, Web interface, JSON's etc.) since it was not documented and 
i believe it should be separate projects and it would be great to have just clean code of the API, additionally i've 
added some improvements for the usage in my real "smart home" project. 

I like this implementation of Broadlink protocol more than all other GOlang implementations 
(although all of them i believe based on mjg59/python-broadlink implementation), because it is 
working out of the box and the code is self explainable, as a result it was very easy to adjust it without any 
preliminary refactoring. For example in my case it was easy to add two new devices (SC1, SP2 (OEM?)) and make some
other changes, needed for the usage in my project.

### Supported devices

The following devices are supported and test: 

- RM3 Pro (Well known infrared blaster)
- SC1 Connector (added and tested, similar to SP2)
- SP2 Socket (tested, but i am not sure that i have an official version)
- ... some others

### Usage

Even that the usage of this package is pretty straightforward, there are some quick useful examples.

## Discover devices:

This will print out all devices within current network. 

```go
broadlink := broadlinkrm.NewBroadlink()

broadlink.Discover()
```

## Add device

Any previously discovered device can be added manually later on. For example all the data from the ```Discover()```
output can be stored as json and used to send a command to the device.

```go
broadlink := broadlinkrm.NewBroadlink()

broadlink.AddManualDevice("192.168.1.10","XX:XX:XX:XX:XX:XX","00000000000","00001", 0x2733)
```

Arguments here are:

- `192.168.1.10` - the ip of the device
- `XX:XX:XX:XX:XX:XX` - mac address
- `00000000000` - authentication key
- `00001` - device internal id
- `0x2733` - device type, that will be matched in `knownDevices` array

## Run command

To send a command to a certain device you should know the code of this command. For example for some switches
like SP2/SP3/SC1 it is easy because the command codes are predefined and are: `00` - switch off the power,
`01` - switch on the power and some others to change the light of the button on switch. For RM3 Pro the
command codes can be retrieved via learning mode and the also stored e.g. in some json.

```go
broadlink := broadlinkrm.NewBroadlink()

// in case you know the device's data
broadlink.AddManualDevice("192.168.1.10","XX:XX:XX:XX:XX:XX","00000000000","00001", 0x2733)

// or you can discover all the devices first, but obviously the device must support your command
// broadlink.Discover()

broadlink.Execute("XX:XX:XX:XX:XX:XX", "00")

```  

more documentation will be added later