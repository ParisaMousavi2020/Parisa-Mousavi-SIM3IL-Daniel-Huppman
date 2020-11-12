# hello-world
just new repository

   Copyright 2020 Parisa Mousavi
   
   This is a test to creat a conflict

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
   
Hi Everybody
I am Parisa from Austria
I am student in master degree Energy systems Engineering.

# Python code

import pandas as pd
from pvlib.location import Location
from pvlib import solarposition, clearsky, irradiance, pvsystem, temperature, modelchain
import numpy as np
import matplotlib.pyplot as plt
from si_prefix import si_format
from datetime import date
import random


def normalize(values, actual_bounds, desired_bounds):
    return [desired_bounds[0] + (x - actual_bounds[0]) * (desired_bounds[1] - desired_bounds[0]) /
            (actual_bounds[1] - actual_bounds[0]) for x in values]


def autolabel(rects):
    """Attach a text label above each bar in *rects*, displaying its height."""
    for rect in rects:
        height = rect.get_height()
        ax.annotate('{}'.format(height),
                    xy=(rect.get_x() + rect.get_width()/2, height),
                    xytext=(0, 3),  # 3 points vertical offset
                    textcoords="offset points",
                    ha='center', va='bottom')


# Task 1
# make a location
Teh = Location(latitude=35.7, longitude=51.4, name='Tehran', altitude=1189, tz='Iran')

# Task 2

timesJun = pd.date_range('2020-06-21', periods=24, freq='H')
times_localized_Jun = timesJun.tz_localize(Teh.tz)
solPosJun = Teh.get_solarposition(times=times_localized_Jun)
azimuthJun = solPosJun.loc[:, 'azimuth']
azimuthJun_norm = normalize(azimuthJun, (0, 360), (-180, 180))
elevJun = solPosJun.loc[:, 'apparent_elevation']

start_date = date.today().replace(day=1, month=1).toordinal()
end_date = date.today().replace(day=31, month=12).toordinal()
random.seed(1)
random_day = date.fromordinal(random.randint(start_date, end_date))

timesMar = pd.date_range('{}'.format(random_day), periods=24, freq='H')
times_localized_Mar = timesMar.tz_localize(Teh.tz, ambiguous=True)
solPosMar = Teh.get_solarposition(times=times_localized_Mar)
azimuthMar = solPosMar.loc[:, 'azimuth']
azimuthMar_norm = normalize(azimuthMar, (0, 360), (-180, 180))
elevMar = solPosMar.loc[:, 'apparent_elevation']

timesDec = pd.date_range('2020-12-21', periods=24, freq='H')
times_localized_Dec = timesDec.tz_localize(Teh.tz)
solPosDec = Teh.get_solarposition(times=times_localized_Dec)
azimuthDec = solPosDec.loc[:, 'azimuth']
azimuthDec_norm = normalize(azimuthDec, (0, 360), (-180, 180))
elevDec = solPosDec.loc[:, 'apparent_elevation']

plt.figure(figsize=(8, 4))
plt.plot(azimuthJun_norm, elevJun, 'bo-', azimuthMar_norm, elevMar, 'ro-', azimuthDec_norm, elevDec, 'go-',
         linewidth=1, markersize=10, mfc='none')
plt.xlim(left=-120, right=120)
plt.xticks(np.arange(-120, 121, step=30))
plt.ylim(bottom=0)
plt.xlabel('Sun Azimuth')
plt.ylabel('Sun Elevation')
plt.title('Solar Positions')
plt.grid(True)
plt.legend(['21 Jun', '9 Mar', '21 Dec'])
plt.show()

# Task 3
timesJunMin = pd.date_range('2020-06-21', end='2020-06-22', freq='1min', closed='left', tz=Teh.tz)
timesMarMin = pd.date_range('2020-03-09', end='2020-03-10', freq='1min', closed='left', tz=Teh.tz)
# timesSepMin = timesSepMin.tz_localize(Teh.tz, ambiguous=True)
timesDecMin = pd.date_range('2020-12-21', end='2020-12-22', freq='1min', closed='left', tz=Teh.tz)
weatherJun = Teh.get_clearsky(timesJunMin)
weatherMar = Teh.get_clearsky(timesMarMin)
weatherDec = Teh.get_clearsky(timesDecMin)
ghiJun = weatherJun.loc[:, 'ghi']
dniJun = weatherJun.loc[:, 'dni']
dhiJun = weatherJun.loc[:, 'dhi']
ghiMar = weatherMar.loc[:, 'ghi']
dniMar = weatherMar.loc[:, 'dni']
dhiMar = weatherMar.loc[:, 'dhi']
ghiDec = weatherDec.loc[:, 'ghi']
dniDec = weatherDec.loc[:, 'dni']
dhiDec = weatherDec.loc[:, 'dhi']

hours = np.linspace(1, 1440, 1440)
xlim = np.arange(0, 60*25, 60*4)
plt.figure(figsize=(18, 4))
plt.subplot(131)
plt.plot(hours, ghiJun, hours, dniJun, hours, dhiJun, linewidth=1.5)
plt.xticks(xlim, [str(n).zfill(2) + ':00' for n in np.arange(0, 25, 4)])
plt.xlabel('Hours of the Day(hr)')
plt.ylabel('Irradiance (W/$m^2$)')
plt.title('Clear Sky Irradiance Components on June 21, 2020 in Tehran')
plt.legend(['GHI', 'DNI', 'DHI'])
plt.subplot(132)
plt.plot(hours, ghiMar, hours, dniMar, hours, dhiMar, linewidth=1.5)
plt.xticks(xlim, [str(n).zfill(2) + ':00' for n in np.arange(0, 25, 4)])
plt.xlabel('Hours of the Day(hr)')
plt.ylabel('Irradiance (W/$m^2$)')
plt.title('Clear Sky Irradiance Components on March 9, 2020 in Tehran')
plt.legend(['GHI', 'DNI', 'DHI'])
plt.subplot(133)
plt.plot(hours, ghiDec, hours, dniDec, hours, dhiDec, linewidth=1.5)
plt.xticks(xlim, [str(n).zfill(2) + ':00' for n in np.arange(0, 25, 4)])
plt.xlabel('Hours of the Day(hr)')
plt.ylabel('Irradiance (W/$m^2$)')
plt.title('Clear Sky Irradiance Components on December 21, 2020 in Tehran')
plt.legend(['GHI', 'DNI', 'DHI'])
plt.show()

# Task 4

time = pd.date_range(start='2020', end='2021', freq='1h', closed='left')
# timeNorm = time.tz_localize(Teh.tz, nonexistent='shift_backward', ambiguous=True)
solPos = Teh.get_solarposition(times=time)
weather = Teh.get_clearsky(time)
surface_tilt = [0, 10, 20, 30, 45, 60]
surface_azimuth = 180
irrad_yearly = list()
for angle in surface_tilt:
    total_irrad = irradiance.get_total_irradiance(surface_tilt=angle, surface_azimuth=surface_azimuth,
                                                  solar_zenith=solPos['apparent_zenith'],
                                                  solar_azimuth=solPos['azimuth'],
                                                  dni=weather['dni'], ghi=weather['ghi'], dhi=weather['dhi'])
    irrad_yearly.append(float((si_format(total_irrad['poa_global'].sum(), precision=2).split(sep=' ')[0])))
    prefix = si_format(total_irrad['poa_global'].sum()).split(sep=' ')[1]

fig, ax = plt.subplots()
rect = ax.bar(['0', '10', '20', '30', '45', '60'], irrad_yearly, width=0.8)
ax.set_ylabel('Annual Total Global Irradiation ('+prefix+'Whr/$m^2$)')
ax.set_xlabel('Surface Tilt (Degree)')

autolabel(rect)

fig.tight_layout()
plt.show()

# part 2 : Monthly plotting for two angles with the highest output [30, 45]

irrad_monthly = list()
for angle in [30, 45]:
    total_irrad = irradiance.get_total_irradiance(surface_tilt=angle, surface_azimuth=surface_azimuth,
                                                  solar_zenith=solPos['apparent_zenith'],
                                                  solar_azimuth=solPos['azimuth'],
                                                  dni=weather['dni'], ghi=weather['ghi'], dhi=weather['dhi'])
    monthly_summary = total_irrad.poa_global.resample('M').sum()
    prefix1 = monthly_summary.apply(si_format).str.split(' ', n=1, expand=True)[1][0]
    monthly_summary = monthly_summary.apply(si_format).str.split(' ', n=1, expand=True)[0].tolist()
    monthly_summary_float = list()
    for i in range(len(monthly_summary)):
        monthly_summary_float.append(float(monthly_summary[i]))

    irrad_monthly.append(monthly_summary_float)

labels = ['Jan.', 'Feb.', 'Mar.', 'Apr.', 'May.', 'Jun.', 'Jul.', 'Aug.', 'Sept.', 'Oct.', 'Nov.', 'Dec.']

x = np.arange(len(labels))  # the label locations
width = 0.35  # the width of the bars


fig, ax = plt.subplots(figsize=(12, 8))
rects1 = ax.bar(x - width/2, irrad_monthly[0], width, label='30 Degree')
rects2 = ax.bar(x + width/2, irrad_monthly[1], width, label='45 Degree')

# Add some text for labels, title and custom x-axis tick labels, etc.
ax.set_ylabel('Annual Total Global Irradiation ('+prefix1+'Whr/$m^2$)')
ax.set_xticks(x)
ax.set_xticklabels(labels)
ax.legend()

autolabel(rects1)
autolabel(rects2)

fig.tight_layout()

plt.show()


# Task 5

sandia_modules = pvsystem.retrieve_sam('SandiaMod')
sapm_inverters = pvsystem.retrieve_sam('cecinverter')
module = sandia_modules['BP_Solar_BP3232G___2010_']
inverter = sapm_inverters['ABB__PVI_3_0_OUTD_S_US_Z__208V_']

temperature_model_parameters = temperature.TEMPERATURE_MODEL_PARAMETERS['sapm']['open_rack_glass_glass']

system = pvsystem.PVSystem(surface_tilt=30, surface_azimuth=180, module='BP_Solar_BP3232G___2010_',
                           module_parameters=module, temperature_model_parameters=temperature_model_parameters,
                           modules_per_string=2, strings_per_inverter=6, inverter='ABB__PVI_3_0_OUTD_S_US_Z__208V_',
                           inverter_parameters=inverter)

# Task 6


mc = modelchain.ModelChain(system, location=Teh)
mc.run_model(weather)
annual_energy_yearly = mc.ac.sum()
annual_energy = mc.ac.resample('M').sum()
prefix2 = annual_energy.apply(si_format).str.split(' ', n=1, expand=True)[1][0]
annual_energy = annual_energy.apply(si_format, precision=2).str.split(' ', n=1, expand=True)[0].tolist()
monthly_summary_energy_float = list()
for i in range(len(annual_energy)):
    monthly_summary_energy_float.append(float(annual_energy[i]))

fig, ax = plt.subplots(figsize=(10, 8))
rect3 = plt.bar(labels, monthly_summary_energy_float, width=0.8)
ax.set_ylabel('Annual Energy Yield per Month ('+prefix2+'Whr/Month)')
ax.set_xlabel('Months')
autolabel(rect3)

fig.tight_layout()
plt.show()









