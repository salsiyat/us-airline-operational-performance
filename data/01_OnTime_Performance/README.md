# Download Guide – Flight On-Time Performance Data

This folder stores the BTS On-Time Performance flight-level data for all U.S. airlines.

---

## Source
BTS TranStats – On-Time Performance  
https://www.transtats.bts.gov/DL_SelectFields.aspx?gnoyr_VQ=FGJ

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
- Year: Select ONE year only (2022, then 2023, then 2024)
- Carrier: Leave blank (ALL airlines)
- Geography: All

---

### Step 4: Download
- Format: CSV
- Click Download
- Save file as:
  - `OnTime_2022.csv`
  - `OnTime_2023.csv`
  - `OnTime_2024.csv`

---

## Expected Output
- ~3–4 GB per file
- ~8–9 million rows per year
