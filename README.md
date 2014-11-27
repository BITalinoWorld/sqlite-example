sqlite-example
==============

BITalino SQLite example application that uses the Python API.

This script connects to a specified BITalino device using the Python API and saves the data into an SQLite database.
The data saved is an absolute average of the values acquired over a specified amount of time (Time Cycle).

Changeable Parameters in the Script:

- Database: name/path to the database file
- BITalino's MAC address
- Acquisition Channels - list of channels to acquire (ex: [0,1,5], [2,4,5], [0,1,2,3,4,5])
- Sampling Frequency
- Time Cycle - amount of time (in seconds) for the data to be averaged.
- Acquisition Time - amount of time (in seconds) for the acquisition.

The database used is SQLite, a self-contained, serveless and zero-configuration database. Moreover, Python contains the driver to this database. Althought very simple, this database allows one to use it with only python installed. A single file containing the database will be created in the path specified in the script parameters ("Database").

The database will contain 2 tables:

Table Configuration: contains the configuration used in each acquisition. The rows are as follows:

- Id: Primary key that identifies the acquisition. Used as Foreign Key in the other table (Data Table)
- MacAddress: Device MAC Address
- SamplingFreq: Sampling Frequency
- InitTime: String with date and time of the beginning of the acquisition
- timeCycle: Amount of time for the data to be averaged (seconds)
- acqChannels: String of the Channels that will be acquired
- channelSize: Number of Channels to be acquired

Table Data: contains the data. The rows are as follows:

- Configuration: Foreign Key to the Configuration Table. Identifies the acquisition.
- Time: time (in seconds), starting from 0.
- Channel [0 - 5]: Data for Channel [0 - 5], if specified to acquire.

Example:

Configurations:

Id | MacAddress | SamplingFreq | InitTime | timeCycle | acqChannels | channelSize
--- | --- | --- | --- | --- | --- | ---
1 | '98:D3:31:B1:84:2C' | 1000 | '11/10/14 17:22:03' | 1 | '[0, 1, 2, 3, 4, 5]' | 6
2 | '98:D3:31:B1:84:2C' | 1000 | '11/10/14 17:47:27' | 1 | '[0]' | 1

Data:

Configuration | Time | Channel0 | Channel1 | Channel2 | Channel3 | Channel4 | Channel5 
--- | --- | --- | --- | --- | --- | --- | ---
1 | 0 | 482.88 | 0.0 | 506.351 | 277.877 | 0.0 | 38.0
1 | 1 | 480.467 | 0.0, 506.643 | 277.875 | 0.0 | 38.0
1 | 2 | 480.947 | 0.0, 511.022 | 277.906 | 0.0 | 38.0
1 | 3 | 481.843 | 0.0, 512.41 | 277.866 | 0.0 | 38.0
1 | 4 | 487.19 | 0.0, 501.329 | 277.877 | 0.0 | 38.0
1 | 5 | 495.273 | 0.0, 507.714 | 277.866 | 0.0 | 38.0
1 | 6 | 491.372 | 0.0, 512.412 | 277.887 | 0.0 | 38.0
1 | 7 | 491.054 | 0.0, 508.18 | 277.876 | 0.0 | 38.0
1 | 8 | 492.112 | 0.0, 511.323 | 277.887 | 0.0 | 38.0
1 | 9 | 491.585 | 0.0, 512.74 | 277.867 | 0.0 | 38.0
2 | 0 | 476.403 | None | None | None | None | None
2 | 1 | 461.972 | None | None | None | None | None
2 | 2 | 456.289 | None | None | None | None | None
2 | 3 | 460.468 | None | None | None | None | None
2 | 4 | 466.202 | None | None | None | None | None
2 | 5 | 457.18 | None | None | None | None | None
2 | 6 | 453.12 | None | None | None | None | None
2 | 7 | 458.135 | None | None | None | None | None
2 | 8 | 463.014 | None | None | None | None | None
2 | 9 | 469.338 | None | None | None | None | None

To restart the database on script run, uncomment the following code (this will delete all the data):

```
# Restart Database
# cursor.execute("Drop table Configuration")
# cursor.execute("Drop table Data")
```

To print all the tables on script run, uncomment the code under:

```
# UnComment to Print Tables
```

This script requires python with the following dependencies:

- pyBluez
- pySerial
- numpy

To run this script for multiple devices, make a copy of it for each device and edit the necessary parameters. Run each script either by double-clicking on it or by opening an individual command line console and typing:

python {path_to_file}
