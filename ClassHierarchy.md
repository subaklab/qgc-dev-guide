# Class Hierarchy (high level)

## LinkManager, LinkInterface

A "Link" in QGC is a specific type of communication pipe with the vehicle such as a serial port or UDP over WiFi. The base class for all links is LinkInterface. Each link runs on it's own thread and sends bytes to MAVLinkProtocol.

The LinkManager object keeps track of all open links in the system. LinkManager also manages automatic connections through serial and udp links.

## MAVLinkProtocol

There is a single MAVLinkProtocol object in the system. It's job is to take incoming bytes from a link and translate them into mavlink messages. Mavlink HEARTBEAT messages are routed to MultiVehicleManager. All mavlink messages are routed to Vehicle's which are associated with the link.

## MultiVehicleManager

There is a single MultiVehicleManager object within the system. When it receives a HEARTBEAT on a link which has not been previsouly seen it creates a Vehicle object. MultiVehicleManager also keeps tracks of all Vehicles in the system and handles switching from one active vehicle to another and correctly handling a vehicle being removed.

## Vehicle

The Vehicle object is the main interface through which the QGC code communicates with the physical vehicle.

Note: There is also a UAS object associated with each Vehicle which is a deprecated class and is slowly being phased out with all functionality moving to the Vehicle class. No new code should be added here.

## FirmwarePlugin, FirmwarePluginManager

The FirmwarePlugin class is used an the base class for firmware plugins. A firmware plugin contains the firmware specific code, such that the Vehicle object is clean with respect to it supporting a single standard interface to the ui.

FirmwarePluginManager is a factory class which creates a FirmwarePlugin instance based on the MAV_AUTOPILOT/MAV_TYPE combination for the Vehicle.

Note: AutoPilotPlugin and AutoPilotPluginManager are deprecated class which also contains firmware specific code. All functionality in these are being moved to the newer FirmwarePlugin and FirmwarePluginManager implementations. No new code should be added here.
