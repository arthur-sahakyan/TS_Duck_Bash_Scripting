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


##### Step 3
The Bash script file is in GitHub repo.
```sh
tsp --bitrate 30000000 -I null 1000 -P merge "bash ./pat_pmt_inject.sh" -O file testing1.ts
```
```sh
tsanalyze testing1.ts 
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

##### Step 4
```sh
tsp --bitrate 30000000 -I file mux1rai.ts -P merge "bash ./pat_pmt_inject.sh" -O file testing2.ts
```
```sh
tsanalyze testing2.ts
```
Output:
```sh
===============================================================================
|  TRANSPORT STREAM ANALYSIS REPORT                                           |
|=============================================================================|
|  Transport Stream Id: .......... 1 (0x0001)  |  Services: .............. 8  |
|  Bytes: ........................ 97,421,788  |  PID's: Total: ......... 41  |
|  TS packets: ...................... 518,201  |         Clear: ......... 41  |
|     With invalid sync: .................. 0  |         Scrambled: ...... 0  |
|     With transport error: ............... 0  |         With PCR's: ..... 8  |
|     Suspect and ignored: ................ 0  |         Unreferenced: ... 0  |
|-----------------------------------------------------------------------------|
|  Transport stream bitrate, based on ....... 188 bytes/pkt    204 bytes/pkt  |
|  User-specified: ................................... None             None  |
|  Estimated based on PCR's: ............... 23,751,525 b/s   25,772,932 b/s  |
|  Selected reference bitrate: ............. 23,751,525 b/s   25,772,932 b/s  |
|-----------------------------------------------------------------------------|
|  Broadcast time: ................................... 32 sec (0 min 32 sec)  |
|-----------------------------------------------------------------------------|
|  Srv Id  Service Name                              Access          Bitrate  |
|  0x0D49  Rai 1 ........................................ C    3,598,885 b/s  |
|  0x0D4A  Rai 2 ........................................ C    8,818,482 b/s  |
|  0x0D4B  Rai 3 TGR Sardegna ........................... C    4,638,139 b/s  |
|  0x0D4C  Rai Radio1 ................................... C      379,235 b/s  |
|  0x0D4D  Rai Radio2 ................................... C      380,152 b/s  |
|  0x0D4E  Rai Radio3 ................................... C      380,106 b/s  |
|  0x0D52  Test HEVC main10 ............................. C      384,002 b/s  |
|  0x0D53  Rai News 24 .................................. C    3,471,236 b/s  |
|                                                                             |
|  Note 1: C=Clear, S=Scrambled                                               |
|  Note 2: Unless specified otherwise, bitrates are based on 188 bytes/pkt    |

```
