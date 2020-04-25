# Virtual Box

### Capturing a USB Device

There are two ways to capture a USB device in a guest in VirtualBox:

* A "transient" solution, either by using the icon on the bottom of the window, or the menu "Devices » USB » ...". The captured devices are indicated by the check next to them. The rest of the devices in the list, simply show you the devices currently present in the host. That does NOT mean you can capture them at will. See"
* USB filter: A more "permanent" solution and one that captures the device as soon as it is inserted into the host, but only after the VM is already up and running A filter should be ideally created specifically for each device

