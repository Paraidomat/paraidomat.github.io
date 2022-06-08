# Mobile/Cellular Networks Fundamentals

## Basics/Essentials

### Evolution

| Generation | Technolgy | Speed |
| ---------- | --------- | ----- |
| 1G         | AMPS, NMTS | No data, analog voice |
| 2G         | GSM       | -     |
| 2.5G       | GPRS      | 50kbps |
| 3G         | UMTS      | 384 kbps |
| 3.5G       | UMTS/HSDPA | 10 mbps |
| 3.75G      | UMTS/HSDPA+ | 20 mbps |
| 4G         | LTE         | 20-50 mbps |
| ?          | LTE Advanced | ~100 mbps |
| 5G         | 5G           | 20 gbps |

### Uplink and Downlink communication

```
Mobile Station ----Uplink----> Base Station / Base Transceiver System (BTS)
Mobile Station <---Downlink--- Base Station / Base Transceiver System (BTS)
```

### Simplex / Half Duplex / Full Duplex

- Mobile Station (MS) uses Full Duplex mode to communicate with Base Station (BTS)

### Frequency Division Duplexing (FDD) / Time Division Duplexing (TDD)

- Two different channels for Uplink and Downlink for Frequency Division Duplex
- One Channel for Uplink and Downlink for Time Division Duplex

### Frequency Division Multiple Access (FDMA)

- How to handle three available channels (3 Uplink, 3 Downlink Channels), 
  when there are four MS that want to communicate?

### Time Division Multiple Access (TDMA)

- How to assign available timeslots to the MS?

### Combination of FDMA and TDMA

- Frequency-Based channels are divided into timeslots
- This enables more MS to communicate with the BTS.

### Modulations

#### FM

- Voice of user is converted into a signal with a bandwidth of 0-4kHz.
- The Base-Frequency of the voice is modulated on to the carrier frequency of the respective mobile channel.

![Frequency Modulation](https://upload.wikimedia.org/wikipedia/commons/a/a4/Amfm3-en-de.gif)

#### Frequency-Shift-Keying (FSK)

![Frequency-Shift-Keying](https://upload.wikimedia.org/wikipedia/commons/3/39/Fsk.svg)

- MS converts the users voice into binary data, which will be transmitted using FSK.

#### Phase Shift Keying (PSK)

![Phase Shift Keying](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-030-57484-0_11/MediaObjects/496442_1_En_11_Fig1_HTML.png)

##### Binary Phase Shift Keying (BPSK)

```
1 = 0° Phase Shift
0 = 180° Phase Shift
```

![Binary Phase Shift Keying](https://upload.wikimedia.org/wikipedia/commons/thumb/4/41/BPSK_Gray_Coded.svg/220px-BPSK_Gray_Coded.svg.png)

#### Quadrature Phase Shift Keying (QPSK)

```
00 = 45° Phase Shift
01 = 135° Phase Shift
10 = 225° Phase Shift
11 = 315° Phase Shift
```
![Quadrature Phase Shift Keying](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/QPSK_Gray_Coded.svg/1024px-QPSK_Gray_Coded.svg.png)

> Note: The bit-positions in the code-block and the picture are different.

#### Quadrature Amplitude Modulation (QAM)

- Symbols are formed by varying both amplitude and phase

Example: 16-QAM
![16-QAM Quadrature Amplitude Modulation](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/16QAM_Gray_Coded.svg/1200px-16QAM_Gray_Coded.svg.png)

### Circuit Switching vs. Packet Switching

- Circuit Switching: dedicated end-to-end circuit established
- Packet Switching: no end-to-end circuit; voice carried as packets

### The Cellular Concept

- Use many low power transmitters in one coverage area
- Cell = basic geographic unit (honeycomb)
- Cell has no regular shape (depends on wireless propagation conditions)

### Co-channel Interference

- Happens when two neighboring cells use the exact same channels 
- Solution: Duplex Channels are divided among a cluster of cells

### Link Budget

- Used to calculate the received power
- Received power should be above minimum sensivity of BTS

### Three main components of a mobile network

- Mobile Station (= Mobile Equipment + SIM)
- Radio Access Network (RAN)
- Core Network (CN)
  - Control of MS
  - call establishment
  - routing

### Back Haul Transmission Network

- Connects RAN with CN
- Can be
  - Microwave Link
  - Electrical Cable
  - Optical Fiber Cable
- Transmission protocols: [PDH](https://en.wikipedia.org/wiki/Plesiochronous_digital_hierarchy), [SDH](https://en.wikipedia.org/wiki/Synchronous_optical_networking), [DWDM](https://en.wikipedia.org/wiki/Wavelength-division_multiplexing#Dense_WDM)

### 1G Mobile Communication Systems

- Advanced Mobile Phone (AMP) Systems, USA, 1982
- Nordic Mobile Telephone (NMT) System, Scandinavia
- Total Access Communication (TAC) System, UK

- Features:
  - Used FDMA
  - Analog FM Modulation
- Weaknesses:
  - Weak Security
  - No Roaming between operators

## 2G Mobile Communication Systems

### 2G Mobile Communication Systems

- European Telecommunication Standards Institute (ETSI), formed in 1988
  - First Global System for Mobiles (GSM) System in 1991
  - Still in use in many countries
- Digital AMPs in USA -> Obsolete 
- Digital Systems
  - Voice is digitized
  - Voice encryption
  - support roaming
  - SMS & data

### GSM Access Technology & Frequency Bands

- FDMA/TDMA
- Popular Frequency bands
  - 900 MHz band
    - 890-915 MHz (Uplink)
    - 935-960 MHz (Downlink)
  - 1800 MHz band
    - 1710-1785 MHz (Uplink)
    - 1805-1880 MHz (Downlink)
  - 1900 MHz band
    - 1850-1910 MHz (Uplink)
    - 1930-1990 MHz (Downlink)
 
 ### GSM Architecture 
 
 ![GSM Architecture](https://www.researchgate.net/profile/Bodunrin-Bakare-2/publication/355397163/figure/fig2/AS:1080595870429209@1634645540744/2G-GSM-architecture-redrawn-version-11-12.jpg)
 
 - Radio Access Network is called Basestation Subsystem (BSS)
 - Core Network is called Network Subsystem (NSS)

### Bastestation Subsystem (BSS)

- Consists of 
  - one Base Station Controller (BSC)
  - one or more Base Transceiver Stations (BTSs)
- A RAN may contain one more BSSs
- Transmission between BTS and BSC: *Front Haul Link*

### Base Transceiver Station (BTS)

- Contains radio transmitter/receiver to connect with MS
- User Voice encryption
- Modulation
  - GMSK (type of frequency shift keying)
- MS Paging
- Receives MS measurement reports and passes it to BSC

### Base Station Controller (BSC)

- Controls one or more BTSs
- Switches traffic and signalling to/from BTSs and MSC
- Radio Resource Management for BSS
  - Assigns channels on Air and Abis Interface
  - Use received meaasurement report to 
    - Controls MS handover
    - Power control for BTS and MS
 
 ### Network Subsystem (NSS)
 
 - Consists of MSC and Associated databases
   - Performs all the call switching and routing
   - Tracking the location of the mobile

### Mobile Switching Center (MSC)

- Digital Switch that performs
  - Call setup
  - Call routing between MS & other MSCs/external network
  - Inter BSS, Inter MSC handovers
- Channel assignment on A Interface
- Gateway MSC (GMSC) handles interface with other [PLMN](https://en.wikipedia.org/wiki/Public_land_mobile_network)s/GMSC

### Home Location Register (HLR)

- Central database for subscriber/mobile users information
  - Subscriber ID
  - Authentication key
  - Subscriber status (registered/unregistered)
  - Services a mobile subscriber can use
  - Current location of subscriber (which VLR area)

### Authentication Center (AUC)

- Normally built as part of the HLR
- Uses authentication key to generate parameters for
  - user authentication/verification
  - generation ciphering key (for encryption between MS and BTS)
- required when user initiates connection

### Visitor Location Register (VLR)

- A PLMN service area is divited into MSC areas
- A VLR may serve more than one MSCs
  - normally one VLR is associated with one MSC
- Hold temporary data to avoid overburdening of HLR

### Equipment Identity Register (EIR)

- White List
  - Contains all approved types of mobile equipment (type approval codes)
- Black List
  - Contains all mobile equipment to be barred (complete IMEI)
- Gray List
  - Contains all mobile equipment to be traced (complete IMEI)

### Operations and Maintainence Center (OMC)

- Monitoring and performance supervision
  - Fault report and alarm handling
- configuration of network
- stores data for minimum one year
 
### GSM Channel Types

- Physical channels
- Logical channels
  - Traffic channels (to carry voice or data)
  - Control channels (signalling between MS and BTS)
    - Critical for network operation
      - MS registration
      - Handover
      - Call generation
      - Call maintainence
    - Broadcast channels
      - Broadcast control channel (BCCH)
        - Network and cell identity
        - Information required to connect to Network
      - Frequency Correction Channel (FCCH)
        - Fine frequency synchronization
      - Synchronization Channel (SCH)
        - Frame synchronization (Frame number)
    - Common Control Channels
      - Paging channel (PCH)
      - Random access channel (RACH)
      - Access grant channel (AGCH)
      - Standalone dedicated control channel (SDCCH)
    - Slow Associated Control Channel (SACCH)
      - Used to carry measurement report in uplink
        - received power level and quality of TCH
      - Used to carry power control and handover commands
    
