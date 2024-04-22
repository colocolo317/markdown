

# Features
* BLE PER test (TX, RX & statistics)
* Enhanced Wi-Fi transmit test configuration
  * 802.11ax mode
  * Delay
  * Number of packets
  * Rate flags 
  * RU allocation subfield
  * USER_IDX
* Gain and frequency offset calibration
* Configure RF regulatory region
* Read BLE devce info
* BLE advertising

Tested with WiSeConnect 3 SDK v3.1.4

# Installation
1. Create a new CLI Demo Application in Simplicity Studio 
2. Copy the contents of this repository to the project root folder and overwrite existing files
3. Add the following Software Component to the project: *WiSeConnect 3 SDK v3.1.4* > *Device* > *Si91x* > *Wireless* > *BLE* 
4. Compile and run the application

# New/Extended Functions

## wifi_init
Configure device operation mode and RF regulatory region 

**Syntax:**
```perl
wifi_init [-i <mode>] [-r <region>]
```

|Parameter     |Description                                                                                                                                                                    |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|*mode*        |Optional, Allowed values: [ap, apsta, ble_coex, client, client_ipv6, eap, transmit_test, ble], Default=apsta                                                                          |
|*region*      |Optional, Allowed values: [factory_default, us, eu, jp, world, kr, sg], Default depends on mode (see source code)                                                                           |

## wifi_transmit_test_start 
Enable Transmit Test mode Wi-Fi transmission

**Syntax:**
```perl
wifi_transmit_test_start <power> <rate> <length> <mode> <channel> [-f <rate_flags>] [-a <aggr_enable>] [-n <no_of_pkts>] [-d <delay>]  [-x <11ax_mode>] [-h <he_ppdu_type>] [-r <ru_alloc>]
``` 

|Parameter     |Description                                                                                                                                                                    |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|*power*       |Transmit power in dbm, set to 127 to use the max power index from Gain Table                                                                                                   |
|*rate*        |Data rate id, as specified in sl_wifi_data_rate_t (e.g. 1 → 802.11b / 1 Mbps, 263 → MCS7)                                                                                      |
|*length*      |Length of the transmit packet in bytes (valid range is [24 … 1500] in burst mode and [24 … 260] in continuous mode)                                                            |
|*mode*        |0 → Burst (Packet) Mode, 1 → Continuous (Stream) Mode, 2 → CW Mode with tone at zero offset, 3 → CW Mode with tone at -2.5 MHz offset, 4 → CW Mode with tone at + 5 MHz offset |
|*channel*     |WLAN channel (1-14)                                                                                                                                                            |
|*rate_flags*  |Optional, Default=0                                                                                                                                                            |
|*aggr_enable* |Optional, 0 → Disable frame aggregation (Default), 1 → Enable frame aggregation                                                                                                |
|*no_of_pkts*  |Optional, Number of packets to transmit (Default=0 -> Continuous)                                                                                                              |
|*delay*       |Optional, Default=0                                                                                                                                                            |
|*11ax_mode*   |Optional, 0 → 802.11ax mode disabled (Default), 1 → 802.11ax mode enabled                                                                                                      |
|*he_ppdu_type*|Optional, he_ppdu_type 0-HE SU PPDU, 1-HE ER SU PPDU, 2-HE TB PPDU, 3-HE MU PPDU (Default=0)                                                                                   |
|*ru_alloc*    |Optional, RU Allocation Subfield for 20MHz BW. Must be in the range 0-255 (Default=0)                                                                                          |


> **Note:**  Before starting CW mode, it is required to start Continuous mode with the power and channel values which are intended to be used in CW mode

## ble_per_transmit
Enable/disable BLE PER (transmit test) mode transmission

**Syntax:**
```perl
ble_per_transmit <enable> <pkt_len> <phy_rate> <channel> <tx_power> <transmit_mode> [-h <freq_hop_en>] [-a <ant_sel>] [-d <inter_pkt_gap>] [-c <rf_chain>] [-n <num_pkts>]
```

|Parameter       |Description                                                                                |
|----------------|-------------------------------------------------------------------------------------------|
|*enable*        |Enable/disable BLE per TX, 1 → PER Transmit Enable, 0 → PER Transmit Disable.              |
|*pkt_len*       |Length of the packet to be transmitted.                                                    |
|*phy_rate*      |1 → 1Mbps, 2 → 2 Mbps, 4 → 125 Kbps Coded, 8 → 500 Kbps Coded.                             |
|*channel*       |BLE channel                                                                                |
|*tx_power*      |Power index                                                                                |
|*transmit_mode* |0 → Burst, 1 → Continuous stream, 2 → Continuous Wave (CW)                                 |
|*freq_hop_en*   |Optional, 0 → No Hopping (Default), 1 → Fixed Hopping, 2 → Random Hopping                  |
|*ant_sel*       |Optional, 2 → ONBOARD_ANT_SEL (Default) 3 → EXT_ANT_SEL                                    |
|*inter_pkt_gap* |Optional, Number of 1250 us slots to be skipped between two packets (Default 0)            |
|*rf_chain*      |Optional, RF Chain (HP/LP) to be used: 2 → BT_HP_CHAIN (Default), 3 → BT_LP_CHAIN.         |
|*num_pkts*      |Optional, Number of packets to be transmitted, Use 0 (Default) for continuous transmission |

## ble_per_receive

Enable/disable BLE  PER mode reception

**Syntax:**
```perl
ble_per_receive <enable> <phy_rate> <channel> [-a <ant_sel>] [-c <rf_chain>]
```

|Parameter       |Description                                                                                |
|----------------|-------------------------------------------------------------------------------------------|
|*enable*        |Enable/disable BLE per RX, 1 → PER Receive Enable, 0 → PER Receive Disable.              |
|*phy_rate*      |1 → 1Mbps, 2 → 2 Mbps, 4 → 125 Kbps Coded, 8 → 500 Kbps Coded.                             |
|*channel*       |BLE channel                                                                                |
|*ant_sel*       |Optional, 2 → ONBOARD_ANT_SEL (Default), 3 → EXT_ANT_SEL                                   |
|*rf_chain*      |Optional, RF Chain (HP/LP) to be used: 2 → BT_HP_CHAIN (Default), 3 → BT_LP_CHAIN.         |

## bt_per_stats
Read BLE transmit & receive statistics

**Syntax:**
```perl
bt_per_stats
```

## bt_get_local_name
Get BLE device name

**Syntax:**
```perl
bt_get_local_name
```

## bt_set_local_name
Set BLE device name to a predefined value

**Syntax:**
```perl
bt_set_local_name
```

## bt_get_local_device_address
Get BLE device address

**Syntax:**
```perl
bt_get_local_device_address
```

## ble_set_advertise_data
Set BLE advertising data to device name

**Syntax:**
```perl
ble_set_advertise_data
```

## ble_start_advertising
Start BLE advertising

**Syntax:**
```perl
ble_start_advertising
```

## ble_stop_advertising
Stop BLE advertising

**Syntax:**
```perl
ble_stop_advertising
```

## si91x_calibration_write

Write frequency and gain offset calibration data

**Syntax:**
```perl
si91x_calibration_write <flags> <gain_offset_low> <gain_offset_mid> <gain_offset_high> [-c <xo_ctune>] [-t target]
```

## si91x_calibration_read

Read frequency and gain offset calibration data

**Syntax:**
```perl
si91x_calibration_read [-t <target>]
```

## si91x_frequency_offset

Compensate frequency offset

**Syntax:**
```perl
si91x_frequency_offset <frequency_offset_in_khz>
```

# Examples

## 802.11ax TX Test with 26 tone RU's (BW~=2MHz)

1. Init Wi-Fi transmit test mode and set regulatory region to Worldwide:
>```perl
>wifi_init -i transmit_test -r world
>```

2. Enable infinite TX with power index=11, MCS=0, packet length=495, packet mode, at channel 7 (2442 MHz) and with 26 tone RU's (BW~=2MHz):
>```perl
>wifi_transmit_test_start 11 0 495 0 7 -f 64 -x 1 -h 2
>```


## BLE PER TX

1. Init BLE mode:
>```perl
>wifi_init -i ble
>```

2. Enable PER mode continuous stream TX with 1 Mbps PHY at channel 10 and max power setting specified by the Gain Table:
>```perl
>ble_per_transmit 1 32 1 10 127 1
>```

## BLE PER RX

1. Init BLE mode:
>```perl
>wifi_init -i ble
>```

2. Enable PER RX mode with 1 Mbps PHY at channel 10:
>```perl
>ble_per_receive 1 1 10
>```

3. Read PER statistics:
>```perl
>bt_per_stats
>```

## Enable BLE Advertising

1. Init BLE mode:
>```perl
>wifi_init -i ble
>```

2. Read device address:
>```perl
>bt_get_local_device_address
>```

3. Enable advertising:
>```perl
>ble_start_advertising
>```
