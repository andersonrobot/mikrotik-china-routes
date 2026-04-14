# Mikrotik China Routes

This repository automatically mirrors the latest China ISP (Internet Service Provider) routing lists from tcp5.com, updated **daily**.

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


### RouterOS China Routes Update Guide Best Practices (Recommended)
#### To ensure system stability, it is recommended to only update the ISPs you frequently use (e.g., China Telecom + China Mobile). Avoid importing all ISP routes at once to prevent router performance degradation or lag.
------------------------------
#### Step 1: Create the Update Script
Run the following command in the RouterOS Terminal to create the update script:
```txt
/system script add name=Update-Routes policy=read,write,policy,test source={
    :local repo "andersonrobot/mikrotik-china-routes"
    :log info "=== Starting Route Table Update ==="

    # Download and import Address List format (Recommended for most users)
    /tool fetch url="https://raw.githubusercontent.com/$repo/main/list/latest/chinatelecom_latest.rsc" mode=http dst-path="telecom.rsc" keep-result=yes
    /import file="telecom.rsc" verbose=yes

    /tool fetch url="https://raw.githubusercontent.com/$repo/main/list/latest/chinamobile_latest.rsc" mode=http dst-path="mobile.rsc" keep-result=yes
    /import file="mobile.rsc" verbose=yes

    # Optional: Add Unicom or CERNET if needed
    # /tool fetch url="https://raw.githubusercontent.com/$repo/main/list/latest/chinaunicom_latest.rsc" mode=http dst-path="unicom.rsc" keep-result=yes
    # /import file="unicom.rsc" verbose=yes

    :log info "=== Route Table Update Completed ==="
}
```
Note: Make sure to replace your-username/your-repo-name with your actual GitHub repository path.

------------------------------
#### Step 2: Create a Scheduled Task (Scheduler)
Execute the following command to set the script to run automatically every day at 10:30 AM:
```txt
/system scheduler add name=Daily-Update-Routes \
    interval=1d \
    start-time=10:30:00 \
    on-event=Update-Routes \
    policy=read,write,policy,test
```
------------------------------
#### Full Version (Update List + Table Simultaneously)
If you need to update both Address List and Routing Table formats, use this comprehensive script:
```txt
/system script add name=Update-Routes policy=read,write,policy,test source={
    :local repo "your-username/your-repo-name"

    :log info "=== Route Table Auto-Update Started ==="

    # Address List Format (Recommended)
    /tool fetch url="https://raw.githubusercontent.com/$repo/main/list/latest/chinatelecom_latest.rsc" mode=http dst-path="telecom-list.rsc"
    /import file="telecom-list.rsc" verbose=yes

    /tool fetch url="https://raw.githubusercontent.com/$repo/main/list/latest/chinamobile_latest.rsc" mode=http dst-path="mobile-list.rsc"
    /import file="mobile-list.rsc" verbose=yes

    # Table Format (Routing Table)
    /tool fetch url="https://raw.githubusercontent.com/$repo/main/table/latest/chinatelecom_latest.rsc" mode=http dst-path="telecom-table.rsc"
    /import file="telecom-table.rsc" verbose=yes

    /tool fetch url="https://raw.githubusercontent.com/$repo/main/table/latest/chinamobile_latest.rsc" mode=http dst-path="mobile-table.rsc"
    /import file="mobile-table.rsc" verbose=yes

    :log info "=== Route Table Update Completed ==="
}
```
Then add the scheduler:
```bash
/system scheduler add name=Daily-Update-Routes interval=1d start-time=10:30:00 on-event=Update-Routes
```
------------------------------
#### Management & Troubleshooting

* View created scripts: `/system script print`
* View scheduled tasks: `/system scheduler print`
* Run manually for testing: `/system script run Update-Routes`
* Disable scheduled task: `/system scheduler disable Daily-Update-Routes`

------------------------------
#### Pro Tips

   1. Initial Test: Always run the script manually once (/system script run Update-Routes) to ensure there are no network or path errors.
   2. Timing: Set the scheduler to run after your GitHub Actions workflow completes (e.g., 10:30 or 11:00 AM) to ensure you are downloading the latest data.
   3. Selective Updates: Only import the ISPs you actually need. Large address lists can impact router performance.
   4. Using Table Format: If you use the Table format, remember to configure corresponding rules in Policy Routing or Mangle.

