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

### Modulation-FM

- Voice of user is converted into a signal with a bandwidth of 0-4kHz.
- The Base-Frequency of the voice is modulated on to the carrier frequency of the respective mobile channel.

![Frequency Modulation](https://upload.wikimedia.org/wikipedia/commons/a/a4/Amfm3-en-de.gif)

