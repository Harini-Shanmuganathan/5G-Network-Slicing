# Slot 20 — Network Slicing with Per-Slice UPF Isolation and S-NSSAI Traffic Steering

## Final KPI Report

**Stack:** Open5GS 2.8.0, UERANSIM v3.3.0, iperf3, Ubuntu 24.04

## 1. Slice Configuration

| Feature | Slice 1 | Slice 2 |
|---|---|---|
| SST / SD | 1 / 000001 | 2 / 000002 |
| DNN | internet | internet2 |
| Dedicated UPF | UPF1 | UPF2 |
| UPF Address | 127.0.0.7 | 127.0.0.8 |
| TUN Interface | ogstun | ogstun2 |
| UE Subnet | 10.45.0.0/16 | 10.46.0.0/16 |
| UE | UE1 | UE2 |

## 2. Implementation Status — VERIFIED

- Two network slices were configured using distinct S-NSSAI values.
- Separate DNNs and dedicated UPFs were configured for the two slices.
- UE1 successfully established a PDU session through Slice 1.
- UE2 successfully established a PDU session through Slice 2.
- Both slices were successfully operated simultaneously.
- Per-slice traffic steering to the corresponding UPF was demonstrated.

## 3. Traffic Isolation — VERIFIED

- Slice 1 to Slice 2 traffic was blocked.
- Slice 2 to Slice 1 traffic was blocked.
- Firewall DROP rules and packet counters were used as evidence of isolation.
- Each slice has its own dedicated address space and UPF interface.

**Evidence files:**
- `cross_slice_blocked.txt`
- `cross_slice_reverse_blocked.txt`
- `slice1_upf_reachable.txt`
- `slice2_upf_reachable.txt`

## 4. Per-Slice KPI Results

### 4.1 TCP Throughput — VERIFIED

| KPI | Slice 1 | Slice 2 |
|---|---:|---:|
| Test Duration | 20 s | 20 s |
| Transfer | 62.6 GBytes | 63.1 GBytes |
| Throughput | 26.9 Gbit/s | 27.1 Gbit/s |
| Retransmissions | Not recorded | 0 |

### 4.2 UDP Jitter — NOT OBTAINED

A valid UE-to-host UDP jitter measurement was not obtained. The attempted test was affected by the host treating its own IP address as a local route.

Therefore, **no UDP jitter value is reported or fabricated**.

## 5. Known Limitations

### Internet Connectivity

UE1 and UE2 were tested for external Internet connectivity using 8.8.8.8. Both tests resulted in 100% packet loss.

Therefore, Internet breakout is currently **not working** and is not claimed as a successful result.

### Direct UPF Gateway Ping

Some direct UE-to-UPF gateway tests were affected by Linux routing behavior because the gateway address can be treated as locally owned by the host.

Therefore, gateway-ping results are not used as the primary evidence for slice isolation. The isolation conclusion is based on the configured firewall rules and their packet counters.

## 6. Deliverables

The project deliverables contain:

- Open5GS slice and UPF configuration files
- UERANSIM gNB and UE configuration files
- Cross-slice isolation evidence
- Per-slice TCP throughput measurements
- This consolidated KPI report

## 7. Overall Result

The implementation successfully demonstrates:

1. Multiple S-NSSAIs.
2. Dedicated UPF per slice.
3. UE-to-slice assignment.
4. Per-slice DNN routing.
5. Simultaneous operation of both slices.
6. Cross-slice traffic isolation.
7. Per-slice TCP throughput measurement.

Internet breakout and UDP jitter measurement remain documented limitations of the current implementation.

**Conclusion:** The core network-slicing, per-slice UPF isolation, S-NSSAI traffic steering, and KPI evidence requirements have been demonstrated and documented.
