**CNRF Protocol**


# Connect Request (Wireless > CNRF)
| 0 | 1 | 2 | 3 | 4 | 5  | 6 |  7  |
|---|---|---|---|---|----|---|-----|
|SYN|TY |   |   |SZ |    |   | CHK | 
|fc |5a | 4 | 3 | 2 | ca | 1 | d2  | 

# Connect Reply (CNRF > Wireless)
| 0 | 1 | 2 | 3 | 4 | 5  | 6 |
|---|---|---|---|---|----|---|
|SYN|TY |   |   | SZ|    |CHK| 
|fc |7a | 4 | 3 | 1 | 0  | 7e| 

# Request inital settings (Wireless > CNRF)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |cf | cu|   |   |   |   |   |   |   |   |   |   |   |   |   |CHK| 
|fc |4c | 4 | 3 |10 | 32 | 3 | ff| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |69 |
* cf : Bitmask of number of RCs
  * 00000001 = RC1
  * 00000011 = RC1 & RC2
* cu : Unknown

# Reply inital settings (CNRF > Wireless)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11  |12  |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|----|----|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |Pwr|SyO|Z1 |z1o|DHW|HOL| T |   |RCS|EC |unk |O2  | Z2|OP2|   |CHK| 
|fc |6c | 4 | 3 |10 | 0  | 1 | 2 | a3| 0 | 0 | 0 | 5 | 0 |21 | 0 |ac  | 0  | a4| 2 | 0 |5f |
* Pwr - Power
  * 0 : Standby
  * 1 : On
* SyO - System Operation Mode
  * 0 : Off
  * 1 : Hot Water On
  * 2 : Heating On
  * 3 : Cooling On
  * 5 : Frost Protect
  * 6 : Legionella
* Z1 : Zone 1 Setpoint
* Z1O/Z2O - Zone1/2 Operation Mode (Zone 2 TBC):
  * 0 : Temperature Mode (heating)
  * 1 : Flow Control Mode (heating)
  * 2 : Compensation Curve Mode
  * 3 : Temperature Mode (cooling)
  * 4 : Flow Control Mode (cooling)
* T - Timer/prohibit setting
  * 0 : no restriction
  * 4 : timer mode heating
  * 5 : prohibit dhw | timer mode heating
  * 6 : prohibit heating
* z2 : Zone 2 Setpoint
* RCS: Main thermistor selection
    * 0xff : 1111 1111 TH1 (this prevents current temp syncs)
    * 0xf1 : 0000 0001 RC1 for Z1
    * 0x21 : 0010 0001 (RC1 for Z1 and RC2 for Z2 ?)* unk2 : Unknown 2
* unk : Unknown
* EC : Error Code


  

# Normal Request (Wireless > CNRF)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD | c1| c2| c3|c4 |c5 |c6 |c7 |c8 |cf |cu |   |   |   |   |   |CHK|
|fc |48 | 4 | 3 |10 | 27 | a8| a8|ff |ff |ff |ff |ff |ff | 3 |ff | 0 | 0 | 0 | 0 | 0 |2e |
* c1 - c8 : controller 1 current temperature (see hex > dec conversion)
  * ff : Where controller not in use
* cf : Bitmask of number of RCs
  * 00000001 = RC1
  * 00000011 = RC1 & RC2
* cu : Unknown


# Normal Reply (CNRF > Wireless)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |Pwr|SyO|Z1 |z1o|DHW|HOL| T |   |RCS|EC |unk|Z2 |OP2|unk|   |CHK| 
|fc |68 | 4 | 3 |10 | 0  | 1 | 2 | a3| 0 | 0 | 0 | 5 | 0 |21 | 0 |0  | a4| 2 | 0 | 0 | f |    (During normal op state)
|fc |68 | 4 | 3 |10 | 0  | 1 | 0 | a2| 0 | 0 | 0 | 5 | 0 |21 |90 |0  | a2| 2 | 0 | 0 | 84|    (During Error 1 State/J0 on FTC)
|fc |68 | 4 | 3 |10 | 0  | 1 | 1 | a2| 0 | 1 | 0 | 5 | 0 |21 | 0 |0  | a2| 2 | 0 | 0 |12 |    (During Hot Water Boost)
* Pwr - Power
  * 0 : Standby
  * 1 : On
* SyO - System Operation Mode
  * 0 : Off
  * 1 : Hot Water On
  * 2 : Heating On
  * 3 : Cooling On
  * 5 : Frost Protect
  * 6 : Legionella
* Z1 : Zone 1 Setpoint
* Z1O/Z2O - Zone1/2 Operation Mode (Zone 2 TBC):
  * 0 : Temperature Mode (heating)
  * 1 : Flow Control Mode (heating)
  * 2 : Compensation Curve Mode
  * 3 : Temperature Mode (cooling)
  * 4 : Flow Control Mode (cooling)
* T - Timer/prohibit setting
  * 0 : no restriction
  * 4 : timer mode heating
  * 5 : prohibit dhw | timer mode heating
  * 6 : prohibit heating
* z2 : Zone 2 Setpoint
* RCS: Main thermistor selection
    * 0xff : 1111 1111 TH1 (this prevents current temp syncs)
    * 0xf1 : 0000 0001 RC1 for Z1
    * 0x21 : 0010 0001 (RC1 for Z1 and RC2 for Z2 ?)* unk2 : Unknown 2
* EC : Error Code
* unk: Unknown Flag


# Set Request Temperature (Wireless > CNRF)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2  | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |trg|c1  | c2| c3| c4| c5| c6| c7| c8|   |cf |cu |   | Z?|   |CHK|   
|fc |49 | 4 | 3 |10 | 28 | a4| a4 |a4 |ff |ff |ff |ff |ff | ff| 0 | 3 | ff| 0 | 1 | 0 |8f |
* trg : Set Target Temperature
* c1 - c8 : controller 1 current temperature (see hex > dec conversion)
  * ff : Where controller not in use
* cf : Bitmask of number of RCs
  * 00000001 = RC1
  * 00000011 = RC1 & RC2
* cu : Unknown
* Z : zone (0 = Zone1/1 = Zone2) or sending RC Number (0 = RC1, 1 = RC2)

# Set Reply (CNRF > Wireless)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |Pwr|SyO|Z1 |z1o|DHW|HOL| T |   |RCS|EC|unk2|Z2 |OP2|unk|   |CHK| 
|fc |69 | 4 | 3 |10 | 0  | 1 |2  |a3 |0  |0  |0  | 5 |0  | 21| 0 | 0 | a4| 2 | 0 | 0 | e |
* Pwr - Power
  * 0 : Standby
  * 1 : On
* z1 : Zone 1 Setpoint
* z2 : Zone 2 Setpoint
* O/O2 - Operation
  * 0 : Off
  * 1 : Hot Water On
  * 2 : Heating On
  * 3 : Cooling On
  * 5 : Frost Protect
  * 6 : Legionella
* OP/OP2 - Operation Mode: 
  * 0 : Temperature Mode (heating)
  * 1 : Flow Control Mode (heating)
  * 2 : Compensation Curve Mode
  * 3 : Temperature Mode (cooling)
  * 4 : Flow Control Mode (cooling)
* HOL : Holiday Mode Active
* T : Timer Modes
* Z1/Z2A : Zone 1/2 Active Input Temperature?
* HW : Hot Water Boost Active
* EC : Error Code
* unk: Unknown Flag


# Set Request Hot Water (Wireless > CNRF)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2  | 3 | 4 |5  |6  | 7 |8  | 9 | 10 |11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|----|---|---|---|---|---|---|---|----|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD | c1|c2  | c3| c4| c5| c6| c7| c8|DHW |cf |cu |   |   | Z?|   |CHK|   
|fc |4a | 4 | 3 |10 | 29 | a3| a4 |ff |ff |ff |ff |ff |ff | 1  | 3 | ff| 0 | 0 | 0 | 0 |31 |
* c1 - c8 : controller 1 current temperature (see hex > dec conversion)
  * ff : Where controller not in use
* cf : Bitmask of number of RCs
  * 00000001 = RC1
  * 00000011 = RC1 & RC2
* DHW : Forced Hot Water
* cu : Unknown
* Z : zone (0 = Zone1/1 = Zone2) or sending RC Number (0 = RC1, 1 = RC2)

# Reply Request Hot Water (CNRF > Wireless)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |Pwr|SyO|Z1 |z1o|DHW|HOL| T |   |RCS|EC |unk|Z2 |OP2|unk|   |CHK| 
|fc |6a | 4 | 3 |10 | 0  | 1 | 1 | a2| 0 | 1 | 0 | 5 | 0 |21 | 0 |0  | 0 | a2| 2 | 0 |10 |
* Pwr - Power
  * 0 : Standby
  * 1 : On
* z1 : Zone 1 Setpoint
* z2 : Zone 2 Setpoint
* O/O2 - Operation
  * 0 : Off
  * 1 : Hot Water On
  * 2 : Heating On
  * 3 : Cooling On
  * 5 : Frost Protect
  * 6 : Legionella
* OP/OP2 - Operation Mode: 
  * 0 : Temperature Mode (heating)
  * 1 : Flow Control Mode (heating)
  * 2 : Compensation Curve Mode
  * 3 : Temperature Mode (cooling)
  * 4 : Flow Control Mode (cooling)
* HOL : Holiday Mode Active
* T : Timer Modes
* Z1/Z2A : Zone 1/2 Active Input Temperature?
* HW : Hot Water Boost Active
* EC : Error Code
* unk: Unknown Flag


# Set Request Holiday (Wireless > CNRF)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2  | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |c1 |c2 | c3| c4| c5| c6| c7| c8|HOL |   |cf |cu |   | Z?|   |CHK|   
|fc |4b | 4 | 3 |10 | 30 | a6| a6 |ff |ff |ff |ff |ff|ff | 1  | 0 | 3 | ff| 0 | 0 | 0 |25 |
* c1 - c8 : controller 1 current temperature (see hex > dec conversion)
  * ff : Where controller not in use
* cf : Bitmask of number of RCs
  * 00000001 = RC1
  * 00000011 = RC1 & RC2
* HOL : Holiday Mode
* cu : Unknown
* Z : zone (0 = Zone1/1 = Zone2) or sending RC Number (0 = RC1, 1 = RC2)


# Reply Request Holiday (CNRF > Wireless)
| 0 | 1 | 2 | 3 | 4 | 0  | 1 | 2 | 3 | 4 |5  |6  | 7 |8  | 9 | 10|11 |12 |13 |14 |15 |16 |
|---|---|---|---|---|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|SYN|TYP|   |   | SZ|CMD |Pwr|SyO|Z1 |z1o|DHW|HOL| T |   |RCS|EC|unk2|Z2 |OP2|unk|   |CHK| 
|fc |6b | 4 | 3 |10 | 0  | 1 | 0 | a5| 0 | 0 | 1 | 5 | 0 |21 | 0 |0  |a2 | 2 | 0 | 0 |10 |
* Pwr - Power
  * 0 : Standby
  * 1 : On
* z1 : Zone 1 Setpoint
* z2 : Zone 2 Setpoint
* O/O2 - Operation
  * 0 : Off
  * 1 : Hot Water On
  * 2 : Heating On
  * 3 : Cooling On
  * 5 : Frost Protect
  * 6 : Legionella
* OP/OP2 - Operation Mode: 
  * 0 : Temperature Mode (heating)
  * 1 : Flow Control Mode (heating)
  * 2 : Compensation Curve Mode
  * 3 : Temperature Mode (cooling)
  * 4 : Flow Control Mode (cooling)
* HOL : Holiday Mode Active
* T : Timer Modes
* Z1/Z2A : Zone 1/2 Active Input Temperature?
* HW : Hot Water Boost Active
* EC : Error Code
* unk: Unknown Flag


# Temperatures:
min = 0x94 (10degC)
max = 0xbc (30degC)
(value - 128.0f) / 2;
