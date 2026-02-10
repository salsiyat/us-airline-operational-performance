# Download Guide – Airline Financial Data (Form 41)

This folder stores quarterly airline financial summary data.

---

### Source
https://www.transtats.bts.gov/DL_SelectFields.aspx?gnoyr_VQ=FMI&QO_fu146_anzr=Nv4%20Pn44vr4%20Sv0n0pvny
---

### Fields to Select

**NET INCOME:**
- [ ] NetIncome

**OPERATING PROFIT/LOSS:**
- [ ] OpProfitLoss

**OPERATING REVENUES:**
- [ ] TransRevPax
- [ ] OpRevenues

**OPERATING EXPENSES:**
- [ ] FlyingOps
- [ ] Maintenance
- [ ] PaxService
- [ ] OpExpenses

**CARRIER INFORMATION (REQUIRED):**
- [ ] AirlineID
- [ ] UniqueCarrier
- [ ] UniqueCarrierName
- [ ] CarrierName

**TIME PERIOD (REQUIRED):**
- [ ] Year
- [ ] Quarter

### Filters
- **Filter Year:** 2025 (or select 2022, 2023, 2024 individually)
- **Filter Period:** All Quarters
- **Filter Geography:** Not Applicable

### Download
- Click **[Download]** button (top right)
- Save as: `Airline_Financial_2023_2025.csv`

### Expected Output
- **Rows:** ~120-150
- **Columns:** 14 fields
- **Grain:** One row per airline per quarter

