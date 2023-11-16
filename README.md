# TS_Duck_Bash_Scripting
##### Step 1
I pulled ubuntu:23.04 Docker image and ran a Docker container of it. 
Installed openssl, libsrt1.5, libssl3, libedit2, libusb-1.0-0, libpcsclite1, libcurl4 and wget packages from apt-get.

Downloaded tsduck.deb file from GitHub:
```sh
wget https://github.com/tsduck/tsduck/releases/download/v3.35-3258/tsduck_3.35-3258.ubuntu23_amd64.deb -O tsduck.deb
```
#### Verifying TS duck works 
```sh 
tsp --version
``` 
Output:
```sh  
tsp: TSDuck - The MPEG Transport Stream Toolkit - version 3.35-3258
```

##### Step 2
For PAT and PMT configs used xml file (hint.xml).

The command
```sh
tsp --bitrate 30000000 -I null 1000 -P tables --xml hint.xml -O file testing.ts
```
Command exited with no errors.
```sh
tsanalyze testing.ts 
```
Output:
```sh
===============================================================================
|  TRANSPORT STREAM ANALYSIS REPORT                                           |
|=============================================================================|
|  Transport Stream Id: ............. Unknown  |  Services: .............. 0  |
|  Bytes: ........................... 188,000  |  PID's: Total: .......... 1  |
|  TS packets: ........................ 1,000  |         Clear: .......... 1  |
|     With invalid sync: .................. 0  |         Scrambled: ...... 0  |
|     With transport error: ............... 0  |         With PCR's: ..... 0  |
|     Suspect and ignored: ................ 0  |         Unreferenced: ... 0  |
|-----------------------------------------------------------------------------|
|  Transport stream bitrate, based on ....... 188 bytes/pkt    204 bytes/pkt  |
|  User-specified: ................................... None             None  |
|  Estimated based on PCR's: ...................... Unknown          Unknown  |
|  Selected reference bitrate: .................... Unknown             None  |
|-----------------------------------------------------------------------------|
|  Broadcast time: ................................................. Unknown  |
|-----------------------------------------------------------------------------|
|  Srv Id  Service Name                              Access          Bitrate  |
|                                                                             |
|  Note 1: C=Clear, S=Scrambled                                               |
|  Note 2: Unless specified otherwise, bitrates are based on 188 bytes/pkt    |
===============================================================================

===============================================================================
|  SERVICES ANALYSIS REPORT                                                   |
|=============================================================================|
|  Global PID's                                                               |
|  TS packets: 1,000, PID's: 1 (clear: 1, scrambled: 0)                       |
```
