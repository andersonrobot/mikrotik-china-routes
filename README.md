# Mikrotik China Routes - Mirror of tcp5.com

This repository automatically mirrors the latest China ISP (Internet Service Provider) routing lists from [tcp5.com](http://tcp5.com/), updated **daily**.

It provides both **List format** (for address-list) and **Table format** (for routing-table), suitable for MikroTik RouterOS.

---

## 📁 Directory Structure

```plaintext
├── list/                  # Address List format (most commonly used)
│   ├── latest/            # ← Always contains the newest files
│   └── history/YYYY-MM-DD/
│
├── table/                 # Routing Table format
│   ├── latest/            # ← Always contains the newest files
│   └── history/YYYY-MM-DD/
```


### Latest Files (Direct Download Links)

#### List Format (Recommended for most users)

- [China Telecom](list/latest/chinatelecom_latest.rsc)  
- [China Unicom](list/latest/chinaunicom_latest.rsc)  
- [China Mobile](list/latest/chinamobile_latest.rsc)  
- [CERNET (Education Network)](list/latest/cernet_latest.rsc)  
- [Other](list/latest/other_latest.rsc)  
- [All China](list/latest/all_china_latest.rsc)

#### Table Format

- [China Telecom](table/latest/chinatelecom_latest.rsc)  
- [China Unicom](table/latest/chinaunicom_latest.rsc)  
- [China Mobile](table/latest/chinamobile_latest.rsc)  
- [CERNET](table/latest/cernet_latest.rsc)  
- [Other](table/latest/other_latest.rsc)  
- [All China](table/latest/all_china_latest.rsc)

> **Tip**: Always use the files under `list/latest/` or `table/latest/` — they are automatically kept up-to-date every day.

---

## How to Use

### 1. In MikroTik RouterOS (Recommended)

```routeros
# Import latest China Telecom list (example)
 /tool fetch url="https://raw.githubusercontent.com/andersonrobot/mikrotik-china-routes/main/list/latest/chinatelecom_latest.rsc" mode=http dst-path=telecom.rsc
 /import file=telecom.rsc
```
