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
