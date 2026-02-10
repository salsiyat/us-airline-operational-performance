# Download Guide – Flight On-Time Performance Data

This folder stores the BTS On-Time Performance flight-level data for all U.S. airlines.

---

## Source
BTS TranStats – On-Time Performance  
https://www.transtats.bts.gov/DL_SelectFields.aspx?gnoyr_VQ=FGJ&QO_fu146_anzr=b0-gvzr

---

## Download Steps (REPEAT FOR EACH YEAR)

### Step 1: Open the BTS Download Page
Go to the link above.

---

### Step 2: Select Fields (CHECK ALL)
Time:
- Year
- Quarter
- Month
- DayofMonth
- DayOfWeek
- FlightDate

Airline:
- Reporting_Airline
- DOT_ID_Reporting_Airline
- Tail_Number
- Flight_Number_Reporting_Airline

Origin:
- OriginAirportID
- Origin
- OriginCityName
- OriginState
- OriginStateName

Destination:
- DestAirportID
- Dest
- DestCityName
- DestState
- DestStateName

Departure:
- CRSDepTime
- DepTime
- DepDelay
- DepDelayMinutes
- DepDel15

Arrival:
- CRSArrTime
- ArrTime
- ArrDelay
- ArrDelayMinutes
- ArrDel15

Cancellation / Diversion:
- Cancelled
- CancellationCode
- Diverted

Delay Causes:
- CarrierDelay
- WeatherDelay
- NASDelay
- SecurityDelay
- LateAircraftDelay

Flight Details:
- CRSElapsedTime
- ActualElapsedTime
- AirTime
- Distance

---

### Step 3: Apply Filters
- Year: Select ONE year only (2023, then 2024, then 2025)
- Carrier: Leave blank (ALL airlines)
- Geography: All

---

### Step 4: Download
- Format: CSV
- Click Download
- Save file as:
  - `OnTime_2023.csv`
  - `OnTime_2024.csv`
  - `OnTime_2025.csv`

---

- ### Expected Output
- **File size:** ~3-4 GB per year
- **Rows:** ~8-9 million per year
- **Total:** ~25 million rows for 2023-2025

