# dbus-wago-smartmeter Service

### Purpose

This service is meant to be run on a raspberry Pi with Venus OS from Victron.

The Python script cyclically reads data from the WAGO EDOMI LBS via the EDOMI HTTP API and publishes information on the dbus, using the service name com.victronenergy.grid. This makes the Venus OS work as if you had a physical Victron Grid Meter installed.

### Configuration

In the Python file, you should put the IP of your EDOMI Server that hosts the WAGO LBS. The WAGO LBS cyclically retrieves the smart meter data via Modbus TCP from the WAGO Energy meter.

### Installation

1. Copy the files to the /data folder on your venus:

   - /data/dbus-wago-smartmeter/dbus-wago-smartmeter.py
   - /data/dbus-wago-smartmeter/kill_me.sh
   - /data/dbus-wago-smartmeter/service/run

2. Set permissions for files:

   `chmod 755 /data/dbus-wago-smartmeter/service/run`

   `chmod 744 /data/dbus-wago-smartmeter/kill_me.sh`

3. Get two files from the [velib_python](https://github.com/victronenergy/velib_python) and install them on your venus:

   - /data/dbus-wago-smartmeter/vedbus.py
   - /data/dbus-wago-smartmeter/ve_utils.py

4. Add a symlink to the file /data/rc.local:

   `ln -s /data/dbus-wago-smartmeter/service /service/dbus-wago-smartmeter`

   Or if that file does not exist yet, store the file rc.local from this service on your Raspberry Pi as /data/rc.local .
   You can then create the symlink by just running rc.local:
  
   `rc.local`

   The daemon-tools should automatically start this service within seconds.

### Debugging

You can check the status of the service with svstat:

`svstat /service/dbus-wago-smartmeter`

It will show something like this:

`/service/dbus-wago-smartmeter: up (pid 10078) 325 seconds`

If the number of seconds is always 0 or 1 or any other small number, it means that the service crashes and gets restarted all the time.

When you think that the script crashes, start it directly from the command line:

`python /data/dbus-wago-smartmeter/dbus-wago-smartmeter.py`

and see if it throws any error messages.

If the script stops with the message

`dbus.exceptions.NameExistsException: Bus name already exists: com.victronenergy.grid"`

it means that the service is still running or another service is using that bus name.

#### Restart the script

If you want to restart the script, for example after changing it, just run the following command:

`/data/dbus-wago-smartmeter/kill_me.sh`

The daemon-tools will restart the scriptwithin a few seconds.

### Hardware

In my installation at home, I am using the following Hardware:

- WAGO Smart Meter 879-3020 4PS - (three phases)
- Raspberry Pi 3B+ - For running Venus OS

