#Acid Rain Lab Report

```python
#1 Plot measured pH of the lake versus dimensionless hydraulic residence time (t/θ).

from aguaclara.core.units import unit_registry as u
import aguaclara.research.environmental_processes_analysis as epa
from scipy import optimize
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import math
from scipy import stats

MassTank = 696 * u.g
MassTankWater = 4488 * u.g
MassWater = MassTankWater - MassTank #g
WaterDensity = 997 * u.kg/u.m/u.m/u.m #kg/m3
VolWater = (MassWater/1000)/WaterDensity
Q = 172*2/60/1000 * u.L/u.s #L/s
ResidenceTime = VolWater*1000/Q #seconds

data_file_path = "https://raw.githubusercontent.com/WesleySluga/CEE4530/master/acid_rain_lab1_kencatie.txt"

lakepHnotes = epa.notes(data_file_path)
lakepHnotes
# set the start index of the data file to one past the note indicating the start.
start=240
#The pH data is in column 1
column=1
lakepH= epa.column_of_data(data_file_path,start,column)
#extract the corresponding time data and convert to seconds
time = epa.column_of_time(data_file_path,start).to(u.s)
HydraulicResidenceTime = ((time/ResidenceTime).to(u.dimensionless)).magnitude
#Now plot the graph
fig, ax = plt.subplots()
ax.plot(HydraulicResidenceTime,lakepH,'r')
plt.xlabel('Hydraulic Residence time')
plt.ylabel('pH')
#plt.yscale('log')

plt.savefig('Lab2Plot1.png')
plt.show()


K1 = 10**(-6.3)
K2 = 10**(-10.3)
KH = 10**(-1.5) * u.mol/u.L/u.atm
Pco2 = 10**(-3.5) * u.atm
Kw = 10**(-14)
ANCin = -10**(-3) * u.eq/u.L
ANCo = 50*10**(-6) * u.eq/u.L

#2 Graph the expected ANC in the lake effluent versus the hydraulic residence time (t/θ) based on the completely mixed flow reactor equation with the plot labeled (in the legend) as conservative ANC.

ANCout = ANCin*(1-2.71828**(-HydraulicResidenceTime)) + ANCo*(2.71828**(-HydraulicResidenceTime))
plt.plot(HydraulicResidenceTime,ANCout)
plt.xlabel('Hydraulic Residence Time')
plt.ylabel('Conservative ANC')
plt.savefig('Lab2Plot2.png')
plt.show()

#3 Calculate the ANC under the assumption of a closed system and plot it on the same graph produced in answering question #2 with the plot labeled (in the legend) as closed ANC.

massNaHCO3 = 0.625 *u.g
#wants total carbonates in mol/L
MmNaHCO3 = 84.007 * u.g/u.mol
Total_Carbonates = massNaHCO3/MmNaHCO3/VolWater
ClosedANC = epa.ANC_closed(lakepH, Total_Carbonates)
plt.plot(HydraulicResidenceTime,ClosedANC)
plt.xlabel('Hydraulic Residence Time')
plt.ylabel('Closed ANC')
plt.savefig('Lab2Plot3.png')
plt.show()

#4  Calculate the ANC under the assumption of an open system and plot it on the same graph produced in answering question #2 with the plot labeled (in the legend) as open ANC.
OpenANC = epa.ANC_open(lakepH)
plt.plot(HydraulicResidenceTime,OpenANC)
plt.xlabel('Hydraulic Residence Time')
plt.ylabel('Open ANC')
plt.savefig('Lab2Plot4.png')
plt.show()



#5 Analyze the data from the second experiment and graph the data appropriately. What did you learn from the second experiment?

data_file_path2 = "https://raw.githubusercontent.com/WesleySluga/CEE4530/master/acidrainpt2_kencatie.txt"
lakepHnotes2 = epa.notes(data_file_path2)
lakepHnotes2
# set the start index of the data file to one past the note indicating the start.
start2=40
#The pH data is in column 1
column2=1
lakepH2= epa.column_of_data(data_file_path2,start2,column2)
#extract the corresponding time data and convert to seconds
time2 = epa.column_of_time(data_file_path2,start2).to(u.s)
HydraulicResidenceTime2 = ((time2/ResidenceTime).to(u.dimensionless)).magnitude
#Now plot the graph
fig, ax = plt.subplots()
ax.plot(HydraulicResidenceTime2,lakepH2,'r')
plt.xlabel('Hydraulic Residence time')
plt.ylabel('pH')
#plt.yscale('log')

plt.savefig('Lab2Plot5.png')
plt.show()
```

# Questions
1)What do you think would happen if enough NaHCO3 were added to the lake to maintain an ANC greater than 50μeq/L for 3 residence times with the stirrer turned off?
With the stirrer turned off, NaHCO3 will sink to the bottom of the lake. In this case, only the bottom part of the lake is working as solvent instead of the entire lake. Thus not a lot of NaHCO3 will dissolve.

2)What are some of the complicating factors you might find in attempting to remediate a lake using CaCO3? Below is a list of issues to consider.

a)extent of mixing
CaCO3 will not mix well in the lake because the solubility of CaCO3 in water is really low. The part of the CaCO3 that does not dissolve will sink to the bottom of the lake.

b)solubility of CaCO3 (find the solubility and compare with NaHCO3)
Solubility of NaHCO3 in pure water is 10.3g/100 mL water at 25 degree Celsius.
Solubility of CaCO3 in pure water is 15 mg/L at 25 degree Celsius, which is really low.
Solubility of NaHCO3 in pure water is much higher than that of CaCO3.
However, in water saturated with CO2, the solubility of CaCO3 increases due to formation of Ca(HCO3)2 which has solubility of 16.6g/100 mL water at 20 degree Celsius.

c)density of CaCO3 slurry (find the density of CaCO3)
density of CaCO3: 2.71 g/cm^3
It's denser than the water, and it does not dissolve well, so CaCO3 will sink to the bottom of the lake and will not spread well.
