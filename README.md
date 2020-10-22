# NASA-Space-Apps-Covid-19-Challenge-2020

Data Visualization of Covid-19 affected, deadth and recovered of different countries around the world.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression, BayesianRidge
from sklearn.model_selection import RandomizedSearchCV, train_test_split
from sklearn.preprocessing import PolynomialFeatures
from sklearn.svm import SVR
from sklearn.metrics import mean_squared_error, mean_absolute_error
import datetime

plt.style.use('fivethirtyeight')

confirmed_df = pd.read_csv(
    'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv')
deaths_df = pd.read_csv(
    'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv')
recoveries_df = pd.read_csv(
    'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv')
latest_data = pd.read_csv(
    'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports/05-07-2020.csv')
us_medical_data = pd.read_csv(
    'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports_us/05-07-2020.csv')

confirmed_df = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv')
deaths_df = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv')
recoveries_df = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv')
latest_data = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports/05-28-2020.csv')
us_medical_data = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports_us/05-28-2020.csv')

latest_data.head()
confirmed_df.head()
us_medical_data.head()

cols = confirmed_df.keys()

# Confirmed Cases, Deaths And Recoveries DataFrame & Here World Variable means SAARC (South Asian Association for Regional Cooperation)

confirmed = confirmed_df.loc[:, cols[4]:cols[-1]]
deaths = deaths_df.loc[:, cols[4]:cols[-1]]
recoveries = recoveries_df.loc[:, cols[4]:cols[-1]]
dates = confirmed.keys()
world_cases = []
total_deaths = []
mortality_rate = []
recovery_rate = []
total_recovered = []
total_active = []

# Country wise Confirmed Cases List:

china_cases = []
italy_cases = []
us_cases = []
spain_cases = []
france_cases = []
germany_cases = []
uk_cases = []
russia_cases = []
brazil_cases = []
india_cases = []
bangladesh_cases = []
bhutan_cases = []
maldives_cases = []
nepal_cases = []
pakistan_cases = []
srilanka_cases = []
afghanistan_cases = []

denmark_cases = []
belgium_cases = []
croatia_cases = []
czech_cases = []
romania_cases = []
sweeden_cases = []
switzerland_cases = []
turkey_cases = []
slovakia_cases = []

# Country wise Deaths List:

china_deaths = []
italy_deaths = []
us_deaths = []
spain_deaths = []
france_deaths = []
germany_deaths = []
uk_deaths = []
russia_deaths = []
brazil_deaths = []
india_deaths = []
bangladesh_deaths = []
bhutan_deaths = []
maldives_deaths = []
nepal_deaths = []
pakistan_deaths = []
srilanka_deaths = []
afghanistan_deaths = []

denmark_deaths = []
belgium_deaths = []
croatia_deaths = []
czech_deaths = []
romania_deaths = []
sweeden_deaths = []
switzerland_deaths = []
turkey_deaths = []
slovakia_deaths = []

# Country wise Recoveries List:
china_recoveries = []
italy_recoveries = []
us_recoveries = []
spain_recoveries = []
france_recoveries = []
germany_recoveries = []
uk_recoveries = []
russia_recoveries = []
brazil_recoveries = []
india_recoveries = []
bangladesh_recoveries = []
bhutan_recoveries = []
maldives_recoveries = []
nepal_recoveries = []
pakistan_recoveries = []
srilanka_recoveries = []
afghanistan_recoveries = []

denmark_recoveries = []
belgium_recoveries = []
croatia_recoveries = []
czech_recoveries = []
romania_recoveries = []
sweeden_recoveries = []
switzerland_recoveries = []
turkey_recoveries = []
slovakia_recoveries = []

# adding datas in our list from the dataset according to dates

for i in dates:
    confirmed_sum = confirmed[i].sum()
    death_sum = deaths[i].sum()
    recovered_sum = recoveries[i].sum()

    # confirmed, deaths, recovered, and active
    world_cases.append(confirmed_sum)
    total_deaths.append(death_sum)
    total_recovered.append(recovered_sum)
    total_active.append(confirmed_sum - death_sum - recovered_sum)

    # calculate rates
    mortality_rate.append(death_sum / confirmed_sum)
    recovery_rate.append(recovered_sum / confirmed_sum)

    # case studies
    china_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'China'][i].sum())
    italy_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Italy'][i].sum())
    us_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'US'][i].sum())
    spain_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Spain'][i].sum())
    france_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'France'][i].sum())
    germany_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Germany'][i].sum())
    uk_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'United Kingdom'][i].sum())
    russia_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Russia'][i].sum())
    brazil_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Brazil'][i].sum())
    india_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'India'][i].sum())
    bangladesh_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Bangladesh'][i].sum())
    bhutan_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Bhutan'][i].sum())
    maldives_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Maldives'][i].sum())
    nepal_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Nepal'][i].sum())
    pakistan_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Pakistan'][i].sum())
    srilanka_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Sri Lanka'][i].sum())
    afghanistan_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Afghanistan'][i].sum())

    denmark_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Denmark'][i].sum())
    belgium_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Belgium'][i].sum())
    croatia_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Croatia'][i].sum())
    czech_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Czech'][i].sum())
    romania_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Romania'][i].sum())
    sweeden_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Sweeden'][i].sum())
    switzerland_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Switzerland'][i].sum())
    turkey_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Turkey'][i].sum())
    slovakia_cases.append(confirmed_df[confirmed_df['Country/Region'] == 'Slovakia'][i].sum())

    china_deaths.append(deaths_df[deaths_df['Country/Region'] == 'China'][i].sum())
    italy_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Italy'][i].sum())
    us_deaths.append(deaths_df[deaths_df['Country/Region'] == 'US'][i].sum())
    spain_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Spain'][i].sum())
    france_deaths.append(deaths_df[deaths_df['Country/Region'] == 'France'][i].sum())
    germany_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Germany'][i].sum())
    uk_deaths.append(deaths_df[deaths_df['Country/Region'] == 'United Kingdom'][i].sum())
    russia_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Russia'][i].sum())
    brazil_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Brazil'][i].sum())
    india_deaths.append(deaths_df[deaths_df['Country/Region'] == 'India'][i].sum())
    bangladesh_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Bangladesh'][i].sum())
    bhutan_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Bhutan'][i].sum())
    maldives_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Maldives'][i].sum())
    nepal_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Nepal'][i].sum())
    pakistan_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Pakistan'][i].sum())
    srilanka_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Sri Lanka'][i].sum())
    afghanistan_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Russia'][i].sum())

    denmark_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Denmark'][i].sum())
    belgium_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Belgium'][i].sum())
    croatia_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Croatia'][i].sum())
    czech_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Czech'][i].sum())
    romania_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Romania'][i].sum())
    sweeden_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Sweeden'][i].sum())
    switzerland_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Switzerland'][i].sum())
    turkey_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Turkey'][i].sum())
    slovakia_deaths.append(deaths_df[deaths_df['Country/Region'] == 'Slovakia'][i].sum())

    china_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'China'][i].sum())
    italy_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Italy'][i].sum())
    us_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'US'][i].sum())
    spain_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Spain'][i].sum())
    france_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'France'][i].sum())
    germany_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Germany'][i].sum())
    uk_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'United Kingdom'][i].sum())
    russia_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Russia'][i].sum())
    brazil_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Brazil'][i].sum())
    india_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'India'][i].sum())
    bangladesh_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Bangladesh'][i].sum())
    bhutan_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Bhutan'][i].sum())
    maldives_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Maldives'][i].sum())
    nepal_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Nepal'][i].sum())
    pakistan_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Pakistan'][i].sum())
    srilanka_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Sri Lanka'][i].sum())
    afghanistan_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Afghanistan'][i].sum())

    denmark_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Denmark'][i].sum())
    belgium_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Belgium'][i].sum())
    croatia_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Croatia'][i].sum())
    czech_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Czech'][i].sum())
    romania_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Romania'][i].sum())
    sweeden_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Sweeden'][i].sum())
    switzerland_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Switzerland'][i].sum())
    turkey_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Turkey'][i].sum())
    slovakia_recoveries.append(recoveries_df[recoveries_df['Country/Region'] == 'Slovakia'][i].sum())

    # Summation of the CONFIRMED CASES are taken into the variable confirmed_sum

    confirmed_sum = confirmed_df[confirmed_df['Country/Region'] == 'China'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Italy'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'US'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Spain'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'France'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Germany'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'United Kingdom'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Russia'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Brazil'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'India'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Bangladesh'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Bhutan'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Maldives'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Nepal'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Pakistan'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Afghanistan'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Sri Lanka'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Denmark'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Belgium'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Croatia'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Czech'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Romania'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Sweeden'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Switzerland'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Turkey'][i].sum() + \
                    confirmed_df[confirmed_df['Country/Region'] == 'Slovakia'][i].sum()

    # Summation of the DEATH CASES are taken into the variable death_sum

    death_sum = deaths_df[deaths_df['Country/Region'] == 'China'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Italy'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'US'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Spain'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'France'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Germany'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'United Kingdom'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Russia'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Brazil'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'India'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Bangladesh'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Bhutan'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Maldives'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Nepal'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Pakistan'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Afghanistan'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Sri Lanka'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Denmark'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Belgium'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Croatia'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Czech'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Romania'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Sweeden'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Switzerland'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Turkey'][i].sum() + \
                deaths_df[deaths_df['Country/Region'] == 'Slovakia'][i].sum()

    # Summation of the RECOVERIES are taken into the variable recovered_sum

    recovered_sum = recoveries_df[recoveries_df['Country/Region'] == 'China'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Italy'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'US'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Spain'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'France'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Germany'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'United Kingdom'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Russia'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Brazil'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'India'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Bangladesh'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Bhutan'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Maldives'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Nepal'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Pakistan'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Afghanistan'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Sri Lanka'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Denmark'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Belgium'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Croatia'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Czech'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Romania'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Sweeden'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Switzerland'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Turkey'][i].sum() + \
                    recoveries_df[recoveries_df['Country/Region'] == 'Slovakia'][i].sum()

def daily_increase(data):
    d = []
    for i in range(len(data)):
        if i == 0:
            d.append(data[0])
        else:
            d.append(data[i] - data[i - 1])
    return d


# confirmed cases
world_daily_increase = daily_increase(world_cases)
china_daily_increase = daily_increase(china_cases)
italy_daily_increase = daily_increase(italy_cases)
us_daily_increase = daily_increase(us_cases)
spain_daily_increase = daily_increase(spain_cases)
france_daily_increase = daily_increase(france_cases)
germany_daily_increase = daily_increase(germany_cases)
uk_daily_increase = daily_increase(uk_cases)
russia_daily_increase = daily_increase(russia_cases)
brazil_daily_increase = daily_increase(brazil_cases)
india_daily_increase = daily_increase(india_cases)
bangladesh_daily_increase = daily_increase(bangladesh_cases)
bhutan_daily_increase = daily_increase(bhutan_cases)
nepal_daily_increase = daily_increase(nepal_cases)
pakistan_daily_increase = daily_increase(pakistan_cases)
srilanka_daily_increase = daily_increase(srilanka_cases)
maldives_daily_increase = daily_increase(maldives_cases)
afghanistan_daily_increase = daily_increase(afghanistan_cases)

denmark_daily_increase = daily_increase(denmark_cases)
belgium_daily_increase = daily_increase(belgium_cases)
croatia_daily_increase = daily_increase(croatia_cases)
czech_daily_increase = daily_increase(czech_cases)
romania_daily_increase = daily_increase(romania_cases)
sweeden_daily_increase = daily_increase(sweeden_cases)
switzerland_daily_increase = daily_increase(switzerland_cases)
turkey_daily_increase = daily_increase(turkey_cases)
slovakia_daily_increase = daily_increase(slovakia_cases)

# deaths
world_daily_death = daily_increase(total_deaths)
china_daily_death = daily_increase(china_deaths)
italy_daily_death = daily_increase(italy_deaths)
us_daily_death = daily_increase(us_deaths)
spain_daily_death = daily_increase(spain_deaths)
france_daily_death = daily_increase(france_deaths)
germany_daily_death = daily_increase(germany_deaths)
uk_daily_death = daily_increase(uk_deaths)
russia_daily_death = daily_increase(russia_deaths)
brazil_daily_death = daily_increase(brazil_deaths)
india_daily_death = daily_increase(india_deaths)
bangladesh_daily_death = daily_increase(bangladesh_deaths)
bhutan_daily_death = daily_increase(bhutan_deaths)
nepal_daily_death = daily_increase(nepal_deaths)
pakistan_daily_death = daily_increase(pakistan_deaths)
srilanka_daily_death = daily_increase(srilanka_deaths)
maldives_daily_death = daily_increase(maldives_deaths)
afghanistan_daily_death = daily_increase(afghanistan_deaths)

denmark_daily_death = daily_increase(denmark_deaths)
belgium_daily_death = daily_increase(belgium_deaths)
croatia_daily_death = daily_increase(croatia_deaths)
czech_daily_death = daily_increase(czech_deaths)
romania_daily_death = daily_increase(romania_deaths)
sweeden_daily_death = daily_increase(sweeden_deaths)
switzerland_daily_death = daily_increase(switzerland_deaths)
turkey_daily_death = daily_increase(turkey_deaths)
slovakia_daily_death = daily_increase(slovakia_deaths)

# recoveries
world_daily_recovery = daily_increase(total_recovered)
china_daily_recovery = daily_increase(china_recoveries)
italy_daily_recovery = daily_increase(italy_recoveries)
us_daily_recovery = daily_increase(us_recoveries)
spain_daily_recovery = daily_increase(spain_recoveries)
france_daily_recovery = daily_increase(france_recoveries)
germany_daily_recovery = daily_increase(germany_recoveries)
uk_daily_recovery = daily_increase(uk_recoveries)
russia_daily_recovery = daily_increase(russia_recoveries)
brazil_daily_recovery = daily_increase(brazil_recoveries)
india_daily_recovery = daily_increase(india_recoveries)
bangladesh_daily_recovery = daily_increase(bangladesh_recoveries)
bhutan_daily_recovery = daily_increase(bhutan_cases)
nepal_daily_recovery = daily_increase(nepal_recoveries)
pakistan_daily_recovery = daily_increase(pakistan_recoveries)
srilanka_daily_recovery = daily_increase(srilanka_recoveries)
maldives_daily_recovery = daily_increase(maldives_recoveries)
afghanistan_daily_recovery = daily_increase(afghanistan_recoveries)

denmark_daily_recovery = daily_increase(denmark_recoveries)
belgium_daily_recovery = daily_increase(belgium_recoveries)
croatia_daily_recovery = daily_increase(croatia_recoveries)
czech_daily_recovery = daily_increase(czech_recoveries)
romania_daily_recovery = daily_increase(romania_recoveries)
sweeden_daily_recovery = daily_increase(sweeden_recoveries)
switzerland_daily_recovery = daily_increase(switzerland_recoveries)
turkey_daily_recovery = daily_increase(turkey_recoveries)
slovakia_daily_recovery = daily_increase(slovakia_recoveries)

# reshaping the data

days_since_1_22 = np.array([i for i in range(len(dates))]).reshape(-1, 1)
world_cases = np.array(world_cases).reshape(-1, 1)
total_deaths = np.array(total_deaths).reshape(-1, 1)
total_recovered = np.array(total_recovered).reshape(-1, 1)

# setting the prediction parameter of days = 10 days in future

days_in_future = 10
future_forcast = np.array([i for i in range(len(dates) + days_in_future)]).reshape(-1, 1)
adjusted_dates = future_forcast[:-10]

# future forecasting from the start date

start = '1/22/2020'
start_date = datetime.datetime.strptime(start, '%m/%d/%Y')
future_forcast_dates = []
for i in range(len(future_forcast)):
    future_forcast_dates.append((start_date + datetime.timedelta(days=i)).strftime('%m/%d/%Y'))

adjusted_dates = adjusted_dates.reshape(1, -1)[0]
# plt.figure(figsize=(8, 12))
plt.plot(adjusted_dates, world_cases)
plt.title('# of Coronavirus Cases Over Time')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
# plt.xticks()
# plt.yticks()
# plt.savefig('World_Cases_Over_Time.png')
plt.show()
print()

# Plotting Number of  Coronavirus DEATHS, RECOVERIES, ACTIVE CASES Over Time

# plt.figure(figsize=(12, 6))
plt.plot(adjusted_dates, total_deaths)
plt.title('# of Coronavirus Deaths Over Time')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
# plt.xticks(size=20)
# plt.yticks(size=20)
# plt.savefig('World_Deaths_Over_Time.png')
plt.show()
print()

# plt.figure(figsize=(12, 6))
plt.plot(adjusted_dates, total_recovered)
plt.title('# of Coronavirus Recoveries Over Time')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
# plt.xticks(size=20)
# plt.yticks(size=20)
# plt.savefig('World_Recoveries_Over_Time.png')
plt.show()
print()

# plt.figure(figsize=(12, 6))
plt.plot(adjusted_dates, total_active)
plt.title('# of Coronavirus Active Cases Over Time')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Active Cases')
# plt.xticks(size=20)
# plt.yticks(size=20)
# plt.savefig('World_Active_Cases_Over_Time.png')
plt.show()
print()

# Plotting Daily Increases in Confirmed Cases

# plt.figure(figsize=(12, 6))
plt.bar(adjusted_dates, world_daily_increase)
plt.title('Daily Increases in Confirmed Cases')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
# plt.xticks(size=20)
# plt.yticks(size=20)
# plt.savefig('Daily_Increase_Cases_Over_Time.png')
plt.show()
print()

# Plotting = Daily Increases in Confirmed Deaths

# plt.figure(figsize=(12, 6))
plt.bar(adjusted_dates, world_daily_death)
plt.title('Daily Increases in Confirmed Deaths')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
# plt.xticks(size=20)
# plt.yticks(size=20)
# plt.savefig('Daily_Increase_Deaths_Over_Time.png')
plt.show()
print()

# Plotting Daily Increases in Confirmed Recoveries

# plt.figure(figsize=(12, 6))
plt.bar(adjusted_dates, world_daily_recovery)
plt.title(' Daily Increases in Confirmed Recoveries')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
# plt.xticks(size=20)
# plt.yticks(size=20)
# plt.savefig('Daily_Increase_Confirmed_Cases_Over_Time.png')
plt.show()
print()

# Function of  plotting total confirmed cases, daily increasing (Confirmed cases, Death Cases, Recovery Cases)

def country_plot(x, y1, y2, y3, y4, country):
    # plt.figure(figsize=(12, 6))
    plt.plot(x, y1)
    plt.title('{} Confirmed Cases'.format(country))
    plt.xlabel('Days Since 1/22/2020')
    plt.ylabel('# of Cases')
    # plt.xticks(size=20)
    # plt.yticks(size=20)
    plt.show()
    print()

    # plt.figure(figsize=(12, 6))
    plt.bar(x, y2)
    plt.title('{} Daily Increases in Confirmed Cases'.format(country))
    plt.xlabel('Days Since 1/22/2020')
    plt.ylabel('# of Cases')
    # plt.xticks(size=20)
    # plt.yticks(size=20)
    plt.show()
    print()

    # plt.figure(figsize=(12, 6))
    plt.bar(x, y3)
    plt.title('{} Daily Increases in Deaths'.format(country))
    plt.xlabel('Days Since 1/22/2020')
    plt.ylabel('# of Cases')
    # plt.xticks(size=20)
    # plt.yticks(size=20)
    plt.show()
    print()

    # plt.figure(figsize=(12, 6))
    plt.bar(x, y4)
    plt.title('{} Daily Increases in Recoveries'.format(country))
    plt.xlabel('Days Since 1/22/2020')
    plt.ylabel('# of Cases')
    # plt.xticks(size=20)
    # plt.yticks(size=20)
    plt.show()
    print()

# plotting confirmed cases

plt.plot(adjusted_dates, china_cases,'g')
plt.plot(adjusted_dates, italy_cases,'b')
plt.plot(adjusted_dates, us_cases,'r')
plt.plot(adjusted_dates, spain_cases,'c')
plt.plot(adjusted_dates, russia_cases,'m')
plt.plot(adjusted_dates, brazil_cases,'k')
plt.title('Top Countries # of Coronavirus Cases')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend(['China', 'Italy', 'US', 'Spain', 'Russia', 'Brazil',])
plt.show()
# plt.savefig('Top_countries_Cases_Over_Time.png')


plt.plot(adjusted_dates, bangladesh_cases, 'g')
plt.plot(adjusted_dates, india_cases, 'b')
plt.plot(adjusted_dates, nepal_cases, 'r')
plt.plot(adjusted_dates, bhutan_cases, 'c')
plt.plot(adjusted_dates, maldives_cases, ':')
plt.plot(adjusted_dates, pakistan_cases, 'm')
plt.plot(adjusted_dates, afghanistan_cases, 'k')
plt.plot(adjusted_dates, srilanka_cases, color='orange')
plt.title('SAARC Countries# of Coronavirus Cases')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend([ 'Bangladesh', 'India', 'Nepal', 'Bhutan', 'Maldives','Pakistan', 'Afghanistan', 'Sri Lanka'])
plt.show()

# plt.savefig('SAARC_Cases_Over_Time.png')

plt.plot(adjusted_dates, denmark_cases,'g')
plt.plot(adjusted_dates, belgium_cases,'b')
plt.plot(adjusted_dates, croatia_cases,'c')
plt.plot(adjusted_dates, czech_cases,'m')
plt.plot(adjusted_dates, romania_cases,':')
plt.plot(adjusted_dates, sweeden_cases,'k')
plt.plot(adjusted_dates, switzerland_cases,'r')
plt.plot(adjusted_dates, turkey_cases,color='orange')
plt.plot(adjusted_dates, slovakia_cases,color='pink')

plt.title('European Union # of Coronavirus Cases')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend([ 'Denmark', 'Belgium', 'Croatia', 'Czech', 'Romania', 'Sweeden','Switzerland', 'Turkey', 'Slovakia'])
plt.show()

# plt.savefig('Europe_countries_Cases_Over_Time.png')

print()

# plotting death cases
plt.plot(adjusted_dates, china_deaths,'g')
plt.plot(adjusted_dates, italy_deaths,'b')
plt.plot(adjusted_dates, us_deaths,'r')
plt.plot(adjusted_dates, spain_deaths,'c')
plt.plot(adjusted_dates, russia_deaths,'m')
plt.plot(adjusted_dates, brazil_deaths,'k')
plt.title('Top Countries # of Coronavirus Deaths')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend(['China', 'Italy', 'US', 'Spain', 'Russia', 'Brazil'])
plt.show()

# plt.savefig('Top_countries_Deaths_Over_Time.png')

plt.plot(adjusted_dates, bangladesh_deaths, 'g')
plt.plot(adjusted_dates, india_deaths, 'b')
plt.plot(adjusted_dates, nepal_deaths, 'r')
plt.plot(adjusted_dates, bhutan_deaths, 'c')
plt.plot(adjusted_dates, maldives_deaths, ':')
plt.plot(adjusted_dates, pakistan_deaths, 'm')
plt.plot(adjusted_dates, afghanistan_deaths, 'k')
plt.plot(adjusted_dates, srilanka_deaths, color='orange')
plt.title('SAARC Countries # of Coronavirus Deaths')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend(['Bangladesh', 'India', 'Nepal', 'Bhutan', 'Maldives','Pakistan', 'Afghanistan', 'Sri Lanka'])
plt.show()

# plt.savefig('SAARC_countries_Deaths_Over_Time.png')

plt.plot(adjusted_dates, denmark_deaths,'g')
plt.plot(adjusted_dates, belgium_deaths,'b')
plt.plot(adjusted_dates, croatia_deaths,'r')
plt.plot(adjusted_dates, czech_deaths,'c')
plt.plot(adjusted_dates, romania_deaths,'m')
plt.plot(adjusted_dates, sweeden_deaths,':')
plt.plot(adjusted_dates, switzerland_deaths,'k')
plt.plot(adjusted_dates, turkey_deaths,color='orange')
plt.plot(adjusted_dates, slovakia_deaths,color='pink')

plt.title('European Union # of Coronavirus Deaths')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend([ 'Denmark', 'Belgium', 'Croatia', 'Czech', 'Romania', 'Sweeden','Switzerland', 'Turkey', 'Slovakia'])
plt.show()

# plt.savefig('Europe_countries_Deaths_Over_Time.png')

print()

# plotting recovery cases
plt.plot(adjusted_dates, china_recoveries,'g')
plt.plot(adjusted_dates, italy_recoveries,'b')
plt.plot(adjusted_dates, us_recoveries,'r')
plt.plot(adjusted_dates, spain_recoveries,'c')
plt.plot(adjusted_dates, russia_recoveries,'k')
plt.plot(adjusted_dates, brazil_recoveries,':')
plt.title('Top Countries # of Coronavirus Recoveries')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend(['China', 'Italy', 'US', 'Spain', 'Russia', 'Brazil'])
plt.show()

# plt.savefig('Top_countries_Recoveries_Over_Time.png')

plt.plot(adjusted_dates, bangladesh_recoveries, 'g')
plt.plot(adjusted_dates, india_recoveries, 'b')
plt.plot(adjusted_dates, nepal_recoveries, 'r')
plt.plot(adjusted_dates, bhutan_recoveries, 'c')
plt.plot(adjusted_dates, maldives_recoveries, ':')
plt.plot(adjusted_dates, pakistan_recoveries, 'm')
plt.plot(adjusted_dates, afghanistan_recoveries, 'k')
plt.plot(adjusted_dates, srilanka_recoveries, color='orange')
plt.title('#SAARC Countries of Coronavirus Recoveries')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend(['Bangladesh', 'India', 'Nepal', 'Bhutan', 'Maldives',
            'Pakistan', 'Afghanistan', 'Sri Lanka'])

# plt.savefig('SAARC_countries_Recoveries_Over_Time.png')

plt.show()

plt.plot(adjusted_dates, denmark_recoveries,'g')
plt.plot(adjusted_dates, belgium_recoveries,'b')
plt.plot(adjusted_dates, croatia_recoveries,'r')
plt.plot(adjusted_dates, czech_recoveries,'c')
plt.plot(adjusted_dates, romania_recoveries,':')
plt.plot(adjusted_dates, sweeden_recoveries,'m')
plt.plot(adjusted_dates, switzerland_recoveries,'k')
plt.plot(adjusted_dates, turkey_recoveries,color='orange')
plt.plot(adjusted_dates, slovakia_recoveries,color='pink')

plt.title('European Union # of Coronavirus Recoveries')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')
plt.legend(['Denmark', 'Belgium', 'Croatia', 'Czech', 'Romania', 'Sweeden',
            'Switzerland', 'Turkey', 'Slovakia'])

# plt.savefig('Europe_countries_Recoveries_Over_Time.png')

plt.show()
print()


# plot predictions via SVM, Polynomial Regression,

def plot_predictions(x, y, pred, algo_name, color):
    plt.plot(x, y)
    plt.plot(future_forcast, pred, linestyle='dashed', color=color)
    plt.title('# of Coronavirus Cases Over Time')
    plt.xlabel('Days Since 1/22/2020')
    plt.ylabel('# of Cases')
    plt.legend(['Confirmed Cases', algo_name])
    plt.show()


# Plotting Mortality Rate Of SAARC

mean_mortality_rate = np.mean(mortality_rate)
plt.plot(adjusted_dates, mortality_rate, color='orange')
plt.axhline(y=mean_mortality_rate, linestyle='--', color='black')
plt.title('Mortality Rate of Coronavirus Over Time')
plt.legend(['mortality rate', 'y=' + str(mean_mortality_rate)])
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('Mortality Rate')

# plt.savefig('Mortality_Rate.png')

plt.show()
print()

# Plotting Recovery Rate

mean_recovery_rate = np.mean(recovery_rate)
plt.plot(adjusted_dates, recovery_rate, color='blue')
plt.axhline(y=mean_recovery_rate, linestyle='--', color='black')
plt.title('Recovery Rate of Coronavirus Over Time')
plt.legend(['recovery rate', 'y=' + str(mean_recovery_rate)])
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('Recovery Rate')

# plt.savefig('Recovery_Rate.png')

plt.show()
print()

# plotting total death and total recovere in one graph

plt.plot(adjusted_dates, total_deaths, color='r')
plt.plot(adjusted_dates, total_recovered, color='green')
plt.legend(['death', 'recoveries'], loc='best')
plt.title('# of Coronavirus Cases')
plt.xlabel('Days Since 1/22/2020')
plt.ylabel('# of Cases')

# plt.savefig('Death vs Recoveries.png')

plt.show()

print()

# plotting Death vs Recoveries

# plt.plot(total_recovered, total_deaths)
# plt.title('# of Coronavirus Deaths vs. # of Coronavirus Recoveries')
# plt.xlabel('# of Coronavirus Recoveries')
# plt.ylabel('# of Coronavirus Deaths')
#
# plt.show()

```


    
![png](output_0_0.png)
    


    



    
![png](output_0_2.png)
    


    



    
![png](output_0_4.png)
    


    



    
![png](output_0_6.png)
    


    



    
![png](output_0_8.png)
    


    



    
![png](output_0_10.png)
    


    



    
![png](output_0_12.png)
    


    



    
![png](output_0_14.png)
    



    
![png](output_0_15.png)
    



    
![png](output_0_16.png)
    


    



    
![png](output_0_18.png)
    



    
![png](output_0_19.png)
    



    
![png](output_0_20.png)
    


    



    
![png](output_0_22.png)
    



    
![png](output_0_23.png)
    



    
![png](output_0_24.png)
    


    



    
![png](output_0_26.png)
    


    



    
![png](output_0_28.png)
    


    



    
![png](output_0_30.png)
    


    

