# 5G Network Slicing with Per-Slice UPF Isolation

## Overview

This project implements a multi-slice 5G Core network using Open5GS and UERANSIM. Two logical network slices are configured with distinct S-NSSAI values, dedicated User Plane Functions (UPFs), separate DNNs, and isolated UE address spaces.

The project demonstrates S-NSSAI-based traffic steering, per-slice UPF selection, simultaneous UE operation, and cross-slice traffic isolation.

## Objectives

- Configure multiple 5G network slices using distinct S-NSSAI values.
- Deploy a dedicated UPF for each slice.
- Register different UEs to specific slices.
- Demonstrate per-slice DNN routing and UPF selection.
- Verify traffic isolation between slices.
- Measure per-slice network performance.

## Technology Stack

- Open5GS 2.8.0
- UERANSIM v3.3.0
- iperf3
- Ubuntu 24.04
- MongoDB

## Slice Configuration

| Parameter | Slice 1 | Slice 2 |
|---|---|---|
| S-NSSAI | SST 1 / SD 000001 | SST 2 / SD 000002 |
| DNN | internet | internet2 |
| Dedicated UPF | UPF1 | UPF2 |
| UPF Address | 127.0.0.7 | 127.0.0.8 |
| Interface | ogstun | ogstun2 |
| UE Subnet | 10.45.0.0/16 | 10.46.0.0/16 |
| UE | UE1 | UE2 |

## Architecture

```text
                    UERANSIM gNB
                         |
                    Open5GS AMF
                         |
                    S-NSSAI Selection
                     /           \
                    /             \
               Slice 1           Slice 2
                  |                 |
                UPF1              UPF2
             127.0.0.7          127.0.0.8
                  |                 |
               ogstun            ogstun2
          10.45.0.0/16        10.46.0.0/16
                  |                 |
                 UE1               UE2
```

## Implementation

### Slice 1

UE1 uses SST 1, SD 000001, DNN internet, dedicated UPF1, and address space 10.45.0.0/16.

### Slice 2

UE2 uses SST 2, SD 000002, DNN internet2, dedicated UPF2, and address space 10.46.0.0/16.

Both UEs successfully established their respective PDU sessions and were operated simultaneously.

## Traffic Isolation

- Slice 1 to Slice 2 traffic: BLOCKED
- Slice 2 to Slice 1 traffic: BLOCKED

Firewall DROP rules and packet counters were used as evidence of cross-slice isolation.

## Performance Results

### TCP Throughput

| KPI | Slice 1 | Slice 2 |
|---|---:|---:|
| Test Duration | 20 seconds | 20 seconds |
| Transfer | 62.6 GBytes | 63.1 GBytes |
| Throughput | 26.9 Gbit/s | 27.1 Gbit/s |
| Retransmissions | Not recorded | 0 |

## Limitations

### Internet Connectivity

UE1 and UE2 were tested for external Internet connectivity using 8.8.8.8. Both tests resulted in 100% packet loss. Internet breakout is therefore not claimed as a successful feature.

### UDP Jitter

A valid UE-to-host UDP jitter measurement was not obtained. No UDP jitter value has been fabricated or reported.

### Gateway Ping

Some direct UE-to-UPF gateway tests were affected by Linux local-route behavior. Isolation conclusions therefore rely primarily on firewall configuration and packet-counter evidence.

## Repository Structure

```text
5G-Network-Slicing/
├── README.md
├── configs/
├── isolation_demo/
└── kpi_report/
    ├── REPORT.md
    ├── slice1_throughput.txt
    └── slice2_throughput.txt
```

## Result

The project successfully demonstrates multiple S-NSSAIs, dedicated UPF per slice, UE-to-slice assignment, per-slice DNN routing, simultaneous slice operation, cross-slice traffic isolation, and per-slice TCP performance measurement.
