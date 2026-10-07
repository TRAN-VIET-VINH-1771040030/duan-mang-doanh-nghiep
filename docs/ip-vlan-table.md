# Bảng địa chỉ IP và VLAN

## VLAN
| VLAN | Tên | Mạng | Gateway | DHCP | Dải loại trừ |
|---|---|---|---|---|---|
| 10 | KINHDOANH | 192.168.10.0/24 | 192.168.10.1 | Có | .1 - .10 |
| 20 | KETOAN | 192.168.20.0/24 | 192.168.20.1 | Có | .1 - .10 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 | Có | .1 - .10 |
| 40 | SERVER | 192.168.40.0/24 | 192.168.40.1 | Không (IP tĩnh) | - |
| 50 | GUEST | 192.168.50.0/24 | 192.168.50.1 | Có | .1 - .10 |

DNS cấp qua DHCP: 192.168.40.10.

## Thiết bị IP tĩnh
| Thiết bị | IP | Gateway |
|---|---|---|
| SRV1 | 192.168.40.10 | 192.168.40.1 |
| Máy in KD 1 / KD 2 | 192.168.10.5 / .6 | 192.168.10.1 |
| Máy in KT 1 / KT 2 | 192.168.20.5 / .6 | 192.168.20.1 |
| Máy in IT 1 | 192.168.30.5 | 192.168.30.1 |
| WEB-INTERNET | 192.0.2.10 | 192.0.2.1 |

## Liên kết giữa các thiết bị mạng
| Liên kết | Mạng | Bên 1 | Bên 2 |
|---|---|---|---|
| R1 ↔ FW1 | 10.0.0.0/30 | R1 g0/1: 10.0.0.1 | FW1 g1/2 (inside): 10.0.0.2 |
| FW1 ↔ R-ISP | 203.0.113.0/30 | FW1 g1/1 (outside): 203.0.113.1 | R-ISP g0/0: 203.0.113.2 |
| R-ISP ↔ WEB-INTERNET | 192.0.2.0/24 | R-ISP g0/1: 192.0.2.1 | WEB-INTERNET: 192.0.2.10 |

## Kết nối cổng
| Từ | Cổng | Đến | Cổng |
|---|---|---|---|
| R1 | g0/0 (subinterface .10-.50) | SW-CORE | Gi0/1 (trunk) |
| SW-CORE | Fa0/1, Fa0/2, Fa0/3 | SW-KD, SW-KT, SW-IT | Fa0/1 (trunk) |
| SW-CORE | Fa0/10 | SRV1 | Fa0 (VLAN 40) |
| SW-CORE | Fa0/11 | AP1 | Port 0 (VLAN 50) |
| SW phòng ban | Fa0/2 - Fa0/11 | 10 PC | Fa0 |
| SW phòng ban | Fa0/12 - Fa0/13 | Máy in | Fa0 |
| R1 | g0/1 | FW1 | g1/2 |
| FW1 | g1/1 | R-ISP | g0/0 |
| R-ISP | g0/1 | WEB-INTERNET | Fa0 |
