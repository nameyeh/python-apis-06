
# WeatherPy Data Analysis 
#### Objective: Visualize the weather of 500+ cities across the world of varying distance from the equator to illustrate what the weather is like as we approach the equator. 
## Observed Trends 
* Trend 1 
* Trend 2 
* Trend 3


```python
# Import Dependencies
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import requests as req
import time
import datetime
from citipy import citipy
import random
from config import openweather_api_key
from matplotlib import style
style.use('ggplot')
import warnings
warnings.filterwarnings('ignore')
```

# Generate Cities List


```python
# Create empty df to store city data 
city_df = pd.DataFrame()
# Create empty lists to store coordinates 
lat = []
long = []
# Loop to generate random coordinates 
for x in range(0,2500):
    lat.append(random.uniform(-90,90))
    long.append(random.uniform(-180, 180))
# Add generated coordinates to previously created df     
city_df["Lat"] = lat
city_df["Long"] = long
#city_df
#city_df.count()

# Find cities corresponding to coordinates
# Create empty list to store city names 
cities = []
# Loop to append city names 
for index, row in city_df.iterrows():
    city=citipy.nearest_city(row["Lat"],row["Long"])
    cities.append(city.city_name)

# Add city names to previously created df   
city_df['City'] = cities
#check length to confirm 
print(len(city_df))
# Remove duplicates 
city_df2 = city_df.drop_duplicates("City",keep="first")
# Confirm at least 500 unique (non-repeat) cities in df 
#city_df2.count()
```

    2500
    

# Perform API Calls and Print Log


```python
# Create empty lists for weather data 
temp = []
humidity = []
clouds = []
wind = []
counter = 0
url = "https://api.openweathermap.org/data/2.5/weather?"
units = "imperial"
# Iterate through the rows to pull data from API; 
# Print log of each city as it's being processed with the city number, city name, and requested URL.
start = time.time()
print("---------------Beginning Data Retrieval---------------")
print("                                                      ")
try:
    for index, row in city_df2.iterrows():
        counter +=1
        city = row["City"]
        query_url = url+"lat="+str(row["Lat"])+"&lon="+str(row["Long"])+"&appid="+openweather_api_key+"&units="+units
        print("Now retrieving city number "+str(counter)+": city name => "+row["City"])
        print("Requested URL: "+query_url)
        print("---------------------------------------------------------------------------------------------------------------")
        weather_response = req.get(query_url).json()
        temp.append(weather_response['main']['temp'])
        humidity.append(weather_response['main']['humidity'])
        clouds.append(weather_response['clouds']['all'])
        wind.append(weather_response['wind']['speed'])
        time.sleep(1)
except:
    pass

end = time.time()
print("---------------Data Retrieval Complete----------------")
print("                                                      ")
print("                                                      ")
run_time = round(end-start,0)
run_time_str = str(datetime.timedelta(seconds=(run_time)))
print("------------------------------------------------------")
print("Total Time Elapsed for Loop Completion = "+run_time_str)
print("------------------------------------------------------")
print("                                                      ")
```

    Now retrieving city number 1: city name = saldanha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-39.74971767630991&lon=-2.5595786488447914&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 2: city name = shelburne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.27648621030812&lon=-65.46588689637548&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 3: city name = vaini
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-83.83746791841035&lon=-174.1889892886632&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 4: city name = olinda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.660537499987555&lon=-32.56504701063534&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 5: city name = ternate
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.0554726447907257&lon=127.9335340656366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 6: city name = khatanga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.22275125212869&lon=103.46039157236015&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 7: city name = rikitea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-36.8011821876874&lon=-140.82229104081242&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 8: city name = bluff
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-80.50008370858735&lon=173.65436942239688&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 9: city name = busselton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-88.20068015202914&lon=81.18585755917178&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 10: city name = lazaro cardenas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.174529051316682&lon=-106.08943346953171&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 11: city name = attawapiskat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.34375841416005&lon=-80.44918986018212&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 12: city name = karaul
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.55305675113644&lon=82.34580288515951&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 13: city name = praia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.207891789432963&lon=-23.30483077775301&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 14: city name = kodiak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.69611966289963&lon=-145.79155541569023&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 15: city name = mataura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-80.44350489973141&lon=-158.06842022729745&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 16: city name = camapua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.898648652915128&lon=-53.77173700337592&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 17: city name = rocha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-45.39951514964654&lon=-45.44272545315701&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 18: city name = port lincoln
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-36.83198195765363&lon=131.77628498728484&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 19: city name = punta arenas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-68.86731741536533&lon=-82.84924241180074&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 20: city name = ushuaia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-75.7556594557528&lon=-70.8633943393009&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 21: city name = kantang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.001458769511288&lon=99.44609248108378&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 22: city name = kailua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.198739968225766&lon=-155.98803718906152&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 23: city name = sao joao da ponte
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.90736927704711&lon=-43.85833478214579&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 24: city name = andevoranto
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.44679918328707&lon=51.143754004526585&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 25: city name = east london
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-60.59101988871178&lon=47.33339244931847&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 26: city name = mahebourg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-54.25431617193844&lon=78.26597690630285&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 27: city name = albany
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-77.10532260515053&lon=116.36627374359432&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 28: city name = macesu de jos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.858709789285626&lon=23.705895489563034&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 29: city name = puerto ayora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.06624846424063&lon=-108.9207693553787&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 30: city name = tuggurt
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.293321252877774&lon=6.572395065660203&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 31: city name = batemans bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-40.74894162143523&lon=154.15822237912414&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 32: city name = lebu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.16714376613822&lon=-86.89831513032513&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 33: city name = hithadhoo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.960041121980623&lon=76.42720590330043&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 34: city name = illoqqortoormiut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=84.37993702316618&lon=-26.35739835137548&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 35: city name = bac lieu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.981830084261333&lon=107.27000518540189&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 36: city name = hereford
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.98327581348519&lon=-2.678317013647103&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 37: city name = taolanaro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-55.65375350230656&lon=63.1741355245282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 38: city name = hermanus
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-88.3844795841375&lon=4.140604809848298&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 39: city name = cape town
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-51.46489797242392&lon=3.806567304334976&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 40: city name = cooma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-36.6740248257842&lon=149.45459426463378&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 41: city name = paamiut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.49536831570509&lon=-50.92948864587129&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 42: city name = watertown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.78696385284215&lon=-75.91069659859015&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 43: city name = sao filipe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.635452315263706&lon=-25.199165125140212&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 44: city name = yellowknife
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.1495525409324&lon=-102.21131954723013&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 45: city name = margate
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.892982415159658&lon=31.73703231220796&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 46: city name = solok
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.8553572052476852&lon=100.75390693035308&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 47: city name = hobart
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-65.68427821845941&lon=150.93982259436348&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 48: city name = coquimbo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.63023163315065&lon=-75.24742129623301&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 49: city name = chokurdakh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=87.8471148184594&lon=144.2051801833676&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 50: city name = dikson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=83.94818963806154&lon=90.0398464072124&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 51: city name = kytlym
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.812518913685125&lon=59.23798268244215&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 52: city name = port alfred
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-87.72842528129776&lon=51.24395956287273&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 53: city name = maragogi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.152652595522724&lon=-31.909083592114115&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 54: city name = deputatskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.29386609974853&lon=142.08308210505027&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 55: city name = georgetown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.0225062835966696&lon=-18.92933629679007&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 56: city name = halvad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.774839684288295&lon=71.26427142876173&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 57: city name = namibe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.765405319621749&lon=3.734250333743006&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 58: city name = salalah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.853334206489905&lon=56.60653807501663&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 59: city name = provideniya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.506421503332916&lon=-176.8738145448149&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 60: city name = cervo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.383115333797946&lon=-7.560957282868912&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 61: city name = jamestown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.461395028063663&lon=-3.762595135729555&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 62: city name = barentsburg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=86.62713886556261&lon=1.3376008792582468&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 63: city name = kaitangata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-64.45316516806832&lon=174.60717857119676&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 64: city name = samusu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.681058999116729&lon=-163.15467083253623&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 65: city name = qaanaaq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=89.01045877153425&lon=-67.77091336415356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 66: city name = saskylakh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=87.66654116562052&lon=114.37503936862123&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 67: city name = lucera
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.5359861094376&lon=15.242825169894388&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 68: city name = klaksvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=75.80457596765149&lon=-7.077267269610047&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 69: city name = palana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.12408084494879&lon=158.08388576809602&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 70: city name = avarua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.345981809769143&lon=-163.30548728540944&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 71: city name = chulumani
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.564884762480503&lon=-67.77108004330975&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 72: city name = rasca
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.325678896218534&lon=26.16191961442553&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 73: city name = pangnirtung
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.44885439362773&lon=-62.31259575877857&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 74: city name = mezen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.64649151906275&lon=45.02978774827781&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 75: city name = norman wells
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.65425453595893&lon=-121.5911294311584&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 76: city name = bredasdorp
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-37.44847462087361&lon=21.45748679130594&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 77: city name = sentyabrskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.96625396365566&lon=159.9616681470444&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 78: city name = port elizabeth
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-41.444337519024394&lon=26.259772502283937&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 79: city name = dir
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.0176884518541&lon=72.11519247418076&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 80: city name = san quintin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.060108072917146&lon=-129.2202610275658&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 81: city name = torbay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.725848125476716&lon=-43.9983557345698&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 82: city name = srednekolymsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=76.80694064295344&lon=155.2788066432546&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 83: city name = castro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-44.2120313905658&lon=-106.09435385139159&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 84: city name = mar del plata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-60.903400508141&lon=-30.428542941732758&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 85: city name = clyde river
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.89465088965053&lon=-67.43747928842589&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 86: city name = port-cartier
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.81268002072619&lon=-68.06003433421682&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 87: city name = hilo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.538327682440823&lon=-162.37799622448932&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 88: city name = kasane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.04996563791407&lon=25.02319460665484&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 89: city name = alice springs
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.696376505883975&lon=135.72626185752392&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 90: city name = barrow
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.02265464453444&lon=-161.01233305032326&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 91: city name = esperance
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-33.82735128164218&lon=123.65999766494895&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 92: city name = vila franca do campo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.44374006988957&lon=-25.37629897176876&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 93: city name = forio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.962965238275245&lon=12.739784198777954&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 94: city name = miri
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.592920713929118&lon=112.59372505189475&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 95: city name = kargil
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.41813796099237&lon=76.13309100991887&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 96: city name = vangaindrano
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.701139342040975&lon=46.63159937282683&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 97: city name = laguna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.88048622857525&lon=-38.4983429448078&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 98: city name = nanortalik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.227985582780775&lon=-38.816031504451615&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 99: city name = tasiilaq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.729395944930644&lon=-40.149844245569994&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 100: city name = kiruna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.464463720595&lon=20.581447987397155&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 101: city name = hienghene
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.601440507380204&lon=164.9227339167524&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 102: city name = butaritari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.862327227887107&lon=162.0789420636725&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 103: city name = zalantun
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.78368093787773&lon=122.34639303305363&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 104: city name = upata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.852969889278313&lon=-63.0127187698413&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 105: city name = kruisfontein
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-89.40540045736725&lon=30.632497075652566&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 106: city name = quesnel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.90808704525003&lon=-121.83171115164747&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 107: city name = sokolo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.769587406240092&lon=-5.492301795554624&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 108: city name = saint-philippe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-40.33881680005091&lon=65.2936705794007&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 109: city name = verkhnevilyuysk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.253894793427435&lon=120.54024093456565&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 110: city name = xining
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.09333991589081&lon=99.19082211811718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 111: city name = arman
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.97541259012863&lon=146.9059915558708&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 112: city name = hamilton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.746582736301264&lon=-66.26995726667101&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 113: city name = necochea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-47.4908462089938&lon=-55.173350779768654&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 114: city name = bambous virieux
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.35196302208268&lon=69.6077526094623&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 115: city name = matara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.1064271286105765&lon=81.04870944912801&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 116: city name = la ronge
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.1134229709217&lon=-103.85936147167197&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 117: city name = ponta do sol
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.560307591728744&lon=-37.52423652627306&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 118: city name = kapaa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.576334436393495&lon=-160.7310653496837&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 119: city name = vardo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=75.38125449990022&lon=34.94617363827271&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 120: city name = goundam
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.473979409431735&lon=-4.948280032889926&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 121: city name = longyearbyen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=79.15448262992021&lon=16.808430997501716&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 122: city name = kadykchan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.455074327789816&lon=146.81773624298648&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 123: city name = shahr-e babak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.377323252828404&lon=54.797464772484375&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 124: city name = nizhneyansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=79.35040602395827&lon=134.77836705277286&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 125: city name = atuona
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.084370080223366&lon=-143.05155205989337&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 126: city name = vilhena
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.936191410692501&lon=-59.917680807957126&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 127: city name = akyab
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.681276765713633&lon=91.08689422631141&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 128: city name = fevralsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.25619108101478&lon=130.49321391574068&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 129: city name = prince rupert
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.15042398675973&lon=-132.03340078539034&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 130: city name = aden
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.062901101964187&lon=47.13123132335184&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 131: city name = sebrovo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.46655966895821&lon=43.41898407073106&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 132: city name = dudinka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.87396042784192&lon=86.05080202961756&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 133: city name = upernavik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=81.96260884658088&lon=-47.31703190726151&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 134: city name = karratha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.320219312877924&lon=116.91512437925064&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 135: city name = krasnokamensk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.88606202460093&lon=93.78713194577222&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 136: city name = north bend
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.38142819983943&lon=-127.59958620320617&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 137: city name = pierre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.50377504293766&lon=-101.6264007832883&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 138: city name = tuatapere
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-49.63563261434963&lon=157.8793779601711&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 139: city name = bayangol
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.568787784149436&lon=104.06761151838526&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 140: city name = bubaque
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.372941110988478&lon=-22.428629877192463&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 141: city name = chicama
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.01361698429335&lon=-90.29690244782101&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 142: city name = seydi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.43363910255849&lon=62.019825081021&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 143: city name = sao joao da barra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.309545289199335&lon=-34.33733840867916&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 144: city name = poum
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.670530010551744&lon=163.89122734457925&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 145: city name = takoradi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.060499850019994&lon=-1.4730554766931903&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 146: city name = iqaluit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.20229184188992&lon=-66.95460308736435&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 147: city name = pascagoula
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.13494972926432&lon=-88.33470029205235&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 148: city name = ilulissat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.4435065038702&lon=-49.55401100271041&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 149: city name = katsuura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.011114597995785&lon=149.45855716903708&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 150: city name = semey
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.04537656855476&lon=78.29356908963098&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 151: city name = new norfolk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-65.67534724315077&lon=132.51581511227977&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 152: city name = lavrentiya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.98402993569869&lon=-168.85683099047935&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 153: city name = naze
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.119422584015027&lon=137.00497784288882&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 154: city name = casas grandes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.178115886844296&lon=-107.74344207565713&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 155: city name = aitape
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.9200165878906716&lon=142.53348831965155&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 156: city name = grand river south east
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.849547655526536&lon=78.4565423870418&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 157: city name = santa maria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.098689613363447&lon=-21.058047206666373&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 158: city name = kraslava
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.0208649754496&lon=27.11306792502333&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 159: city name = cap-haitien
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.664667342749368&lon=-72.12991473901884&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 160: city name = tiksi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.87525162264257&lon=131.616657289888&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 161: city name = geraldton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.86166244972611&lon=96.65981864453863&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 162: city name = rumoi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.98630766401101&lon=141.70504004382883&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 163: city name = yining
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.008860004898196&lon=81.92593241658119&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 164: city name = jacksonville beach
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.26714538309365&lon=-81.47001671521996&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 165: city name = bengkulu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.650280887142046&lon=88.73403013221042&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 166: city name = komsomolskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=77.94525987753843&lon=174.66094807844013&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 167: city name = anar darreh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.21733173543245&lon=60.61831618511718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 168: city name = fortuna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.21135026794394&lon=-135.33312399831505&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 169: city name = aksarka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.02135838324594&lon=68.61603495141873&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 170: city name = loiza
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.47467867706935&lon=-65.25626294034578&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 171: city name = kuche
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.42159210357519&lon=82.39633060816226&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 172: city name = barra patuca
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.308934844392397&lon=-83.44526381565942&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 173: city name = bang saphan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.48742173267496&lon=99.43356575929818&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 174: city name = los llanos de aridane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.1023634984969&lon=-22.237051041409302&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 175: city name = linqiong
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.577483091326215&lon=102.43547085069503&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 176: city name = maningrida
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.145485217355741&lon=133.7812617172675&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 177: city name = ancud
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-40.24408458037904&lon=-90.42023247146443&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 178: city name = tumannyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.91120017773841&lon=38.0430355555998&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 179: city name = deh rawud
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.098382511843866&lon=65.6493232062958&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 180: city name = carnarvon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.124516860966494&lon=86.58862544674827&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 181: city name = kachikau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.92764899202433&lon=24.507285006427168&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 182: city name = saleaula
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.4177742865293226&lon=-168.99458498749965&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 183: city name = arraial do cabo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-43.77964231209882&lon=-20.263904666726376&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 184: city name = ginda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.520229851710582&lon=39.19885181922257&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 185: city name = villa bruzual
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.313782822020528&lon=-68.88819907154739&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 186: city name = matongo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.3908127371410757&lon=33.90389913505521&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 187: city name = mys shmidta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=78.51336730370033&lon=-173.99835179050118&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 188: city name = naryan-mar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.03865563899825&lon=50.70515211570947&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 189: city name = ucluelet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.58573374739228&lon=-126.12403013095837&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 190: city name = kopyevo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.74400544074129&lon=89.86791882265959&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 191: city name = kalevala
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.07747581793438&lon=32.159629866897205&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 192: city name = novikovo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.68222014905322&lon=145.60203992505188&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 193: city name = lerwick
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.463806301475785&lon=1.5988862121389218&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 194: city name = halalo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.387780982255137&lon=-174.76796056981883&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 195: city name = melilla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.30360098429777&lon=-2.619308025777144&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 196: city name = kondinskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.76960508978212&lon=66.88354732524576&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 197: city name = faya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.83045890645728&lon=16.88262695969192&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 198: city name = sabha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.29884681484664&lon=13.198860538280854&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 199: city name = mauswagon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.904020482339845&lon=124.31528635167183&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 200: city name = padang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.431458438180869&lon=95.42232937359313&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 201: city name = vestmannaeyjar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.41539223032805&lon=-22.276407738483186&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 202: city name = homer
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.11298303551854&lon=-151.7207131444904&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 203: city name = kahului
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.884610571489233&lon=-151.06110567101297&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 204: city name = lardos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.703895792350224&lon=29.07972186771957&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 205: city name = trelew
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-43.934078771344495&lon=-65.42017528308577&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 206: city name = warwick
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.502822456935405&lon=151.30480853804522&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 207: city name = tuktoyaktuk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=80.73941805854972&lon=-138.97422604711429&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 208: city name = severo-kurilsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.18864584269744&lon=154.79053811241243&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 209: city name = hay river
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.86792228143213&lon=-115.8762621905825&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 210: city name = elko
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.71549508785813&lon=-115.85450947201845&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 211: city name = rungata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.9794881646102738&lon=178.697961690051&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 212: city name = cidreira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-61.10000996755193&lon=-18.947246459690405&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 213: city name = puerto princesa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.55611222000266&lon=118.93190199857179&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 214: city name = nakhon thai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.4052021993775&lon=100.50790445574205&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 215: city name = codrington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.987725842876003&lon=-54.874367902779625&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 216: city name = cabo san lucas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.682170401491604&lon=-123.41352836037817&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 217: city name = bargal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.268874884015474&lon=59.907459389157054&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 218: city name = acajutla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.499014635838222&lon=-91.88456386251863&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 219: city name = pevek
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=82.50425277501324&lon=167.7851820808832&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 220: city name = umzimvubu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-47.822136929843204&lon=42.33970789083773&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 221: city name = narasannapeta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.80776555901727&lon=85.45399742838322&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 222: city name = batticaloa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.13347529732819&lon=84.39463195616668&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 223: city name = campoverde
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.415860637464647&lon=-75.06814207423176&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 224: city name = shahgarh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.338611525342742&lon=79.23090488367268&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 225: city name = ballina
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.39986429261799&lon=-11.753545658078991&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 226: city name = taoudenni
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.356705299493868&lon=-8.094368252633132&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 227: city name = oranjestad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.973399468290893&lon=-63.29917974982048&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 228: city name = thompson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.88713376825905&lon=-93.03787217769838&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 229: city name = tynda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.91264780479398&lon=124.32435929245429&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 230: city name = bethel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.64192718227255&lon=-161.40891888138344&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 231: city name = anito
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.313781869207304&lon=126.35763643499877&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 232: city name = anadyr
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.73832408892639&lon=174.55799393917385&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 233: city name = pacific grove
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.504484775128375&lon=-133.156051624748&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 234: city name = santiago
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.732821214684463&lon=-76.20696426229445&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 235: city name = stavern
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.50024784823748&lon=9.924799063120702&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 236: city name = nikolskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.75548712952505&lon=169.92918537699342&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 237: city name = petropavlovsk-kamchatskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.55478143475568&lon=159.29878673237874&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 238: city name = airai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.63712548863937&lon=142.667731183226&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 239: city name = altay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.56921428682671&lon=89.4998030504056&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 240: city name = maple creek
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.50728051777179&lon=-109.72474352773499&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 241: city name = shawnee
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.41611256561161&lon=-96.63852224853142&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 242: city name = yarmouth
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.934062461139916&lon=-66.2193966744805&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 243: city name = baykit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.50850058921844&lon=93.78521978685228&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 244: city name = belushya guba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=80.00640388204155&lon=45.35065380566479&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 245: city name = west bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.72137173702164&lon=-82.03249895912961&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 246: city name = hualmay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.4740742765444&lon=-92.71920658102843&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 247: city name = quelimane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.839068374439194&lon=37.81044366823056&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 248: city name = kolimvari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.68397681101331&lon=23.8847653237558&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 249: city name = longkou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.6058763463966&lon=119.73675124971908&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 250: city name = kamloops
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.63243756212992&lon=-120.12609336916113&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 251: city name = yanam
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.306886222799278&lon=84.38717324568324&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 252: city name = bur gabo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.7390394462711214&lon=43.925504783707424&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 253: city name = pakxan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.715603543661558&lon=103.16114935256991&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 254: city name = vostok
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.535057702501376&lon=150.02609740985542&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 255: city name = dicabisagan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.002295838445036&lon=123.59886973159132&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 256: city name = cap malheureux
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.457329355309255&lon=57.10038692625233&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 257: city name = pehowa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.97787530669686&lon=76.45409047623752&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 258: city name = cherskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=81.22560673997646&lon=160.6892458803983&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 259: city name = saint-francois
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.445025309930998&lon=-51.769020371242306&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 260: city name = sam roi yot
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.363595639697778&lon=99.63363082865197&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 261: city name = sarangani
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.404860085349057&lon=125.03341043283382&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 262: city name = palabuhanratu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.78496640613183&lon=98.94836897851269&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 263: city name = chuy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-48.83116442927979&lon=-39.42735955789743&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 264: city name = stromness
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.95077118493478&lon=-4.93873484047333&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 265: city name = bonthe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.5674738519976756&lon=-16.501578171122617&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 266: city name = umm jarr
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.672336274204795&lon=32.72455608445304&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 267: city name = borama
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.3579899845964&lon=43.183880478869554&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 268: city name = lusambo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.220534186595444&lon=24.675108302472523&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 269: city name = kavieng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.8392069038856818&lon=151.52613039462477&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 270: city name = nhulunbuy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.057548359562205&lon=139.25262308500265&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 271: city name = quatre cocos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.672797023130627&lon=68.77149012727762&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 272: city name = san patricio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.944240966675082&lon=-114.68841763333246&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 273: city name = ponta delgada
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.30913002525898&lon=-27.883025571154974&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 274: city name = skibbereen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.349166187794424&lon=-11.129330856291858&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 275: city name = saint-pierre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.806402347347586&lon=-52.67256078054358&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 276: city name = safwah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.889397538121457&lon=49.34302154716755&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 277: city name = kanye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.88855060103225&lon=25.348031751887305&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 278: city name = te anau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-39.564930526176795&lon=165.09620710550882&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 279: city name = beringovskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.11944584296117&lon=179.10101416144704&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 280: city name = antanifotsy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.338461612510287&lon=47.79430639049326&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 281: city name = mayo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.92562644320307&lon=-141.58858598168416&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 282: city name = tsihombe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-53.15815071728438&lon=49.764832644520965&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 283: city name = buchanan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.029863789024347&lon=-11.249425013188642&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 284: city name = port hawkesbury
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.94647874729574&lon=-61.102831557673014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 285: city name = suzhou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.54819198960739&lon=116.8381279334962&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 286: city name = taltal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.61437501649793&lon=-68.88438036473683&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 287: city name = wuwei
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.95399356019226&lon=103.78816874839765&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 288: city name = caravelas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.199191890727874&lon=-23.238485748049612&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 289: city name = port hardy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.5069086674103&lon=-129.49072627895805&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 290: city name = constitucion
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.40974244798339&lon=-113.75129351026638&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 291: city name = alofi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.006372004304346&lon=-168.92550444582065&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 292: city name = ocean springs
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.752308480126302&lon=-88.94118773291108&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 293: city name = rawannawi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.334920158132846&lon=174.78754755755858&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 294: city name = durango
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.89574665789914&lon=-105.06795499772909&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 295: city name = guarda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.68085087272766&lon=-6.708012003364132&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 296: city name = cayenne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.142908824081516&lon=-46.166436321095546&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 297: city name = sola
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.255966022997072&lon=171.58336320711845&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 298: city name = moree
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.304268935093262&lon=146.90269273697783&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 299: city name = lagoa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.38774510950188&lon=-26.318660450453933&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 300: city name = vaida
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.2177772816884&lon=24.969430272604058&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 301: city name = copiapo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.69698378674869&lon=-70.4370819813222&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 302: city name = itarema
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.821424327975521&lon=-38.946067580622696&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 303: city name = moose factory
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.256924701472855&lon=-80.55906357507658&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 304: city name = disna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.885359719281894&lon=28.239284476495868&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 305: city name = litoral del san juan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.222973356396764&lon=-78.32960318292581&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 306: city name = labuhan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.376754988346292&lon=92.5538969919312&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 307: city name = rosaryville
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.76770057464216&lon=-76.61669163546091&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 308: city name = haines junction
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.91247891706328&lon=-143.32484722213806&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 309: city name = mehamn
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=85.22547179455046&lon=29.1893844169673&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 310: city name = metlika
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.713512598251896&lon=15.505436541456845&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 311: city name = manokwari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.9520437905979549&lon=133.68449210949728&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 312: city name = aleksandrov gay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.40722926244169&lon=48.98520377661512&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 313: city name = bathsheba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.453715108699342&lon=-43.581468669700314&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 314: city name = saint george
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.612710233411534&lon=-59.30736510386882&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 315: city name = guerrero negro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.096602390529142&lon=-130.15854339956488&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 316: city name = mbouda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.5570116319320135&lon=10.214227402203363&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 317: city name = vaitupu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.364711136995439&lon=-177.13814543726022&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 318: city name = sychevka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.76845647974855&lon=33.90270457702283&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 319: city name = hami
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.37601922363322&lon=94.02357980347631&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 320: city name = alenquer
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.0827725503729084&lon=-54.61880147674347&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 321: city name = marcona
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.385218941943194&lon=-80.02630915388873&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 322: city name = uren
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.448157179804184&lon=45.764203686702075&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 323: city name = redlands
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.698922236745005&lon=-108.68027261749809&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 324: city name = ushtobe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.54010943601432&lon=76.78973186042697&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 325: city name = gorahun
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.493526575184717&lon=-11.11451943968089&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 326: city name = auki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.746371545219532&lon=163.61609323899478&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 327: city name = sao miguel do araguaia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.513813798533704&lon=-52.456162625075535&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 328: city name = yerbogachen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.1224323458641&lon=105.72553958867633&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 329: city name = leningradskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=75.7269821598943&lon=175.9933134218017&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 330: city name = stoyba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.05055324178622&lon=131.99337274927888&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 331: city name = sanok
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.43247566442966&lon=22.048978442069398&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 332: city name = olafsvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.30016224711864&lon=-26.17605944744699&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 333: city name = xai-xai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.603864818866995&lon=35.28398262887072&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 334: city name = cookeville
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.55736676029788&lon=-85.27255588197602&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 335: city name = huatulco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.946038485512048&lon=-96.477138221891&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 336: city name = magistralnyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.242991543143205&lon=106.63808428098565&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 337: city name = awbari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.74921199512984&lon=12.11646002218231&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 338: city name = rio gallegos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-49.95631965958198&lon=-71.45506266720204&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 339: city name = borogontsy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.13583906864196&lon=131.95851424024096&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 340: city name = albania
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.3604184947580222&lon=-75.87336206576279&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 341: city name = east angus
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.51825918517483&lon=-71.61488464526997&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 342: city name = winnemucca
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.85738789739685&lon=-116.79277621061237&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 343: city name = progreso
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.980516961878166&lon=122.09072747207671&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 344: city name = mackay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.198219089768102&lon=154.67470955994105&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 345: city name = bandarbeyla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.198180458664964&lon=54.01985135643713&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 346: city name = san jeronimo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.8754475373331445&lon=-107.59503120810218&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 347: city name = huarmey
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.181215939558172&lon=-86.7810897521961&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 348: city name = rumonge
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.406825722341935&lon=29.3678009606071&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 349: city name = rawson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-51.33441191184414&lon=-56.01417307275334&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 350: city name = huancabamba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.119750454569555&lon=-79.46184947260188&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 351: city name = ouadda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.821322685830637&lon=21.605681616568432&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 352: city name = chapada dos guimaraes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.179158846922434&lon=-55.60478674967433&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 353: city name = tashtyp
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.936307714852774&lon=89.73194529856528&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 354: city name = namatanai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.5069019380116515&lon=159.5138896728655&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 355: city name = yangambi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.7073004758286601&lon=24.186080442095033&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 356: city name = tidore
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.5300879202524271&lon=128.3252882910935&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 357: city name = farmington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.639332928333715&lon=-90.31216655378194&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 358: city name = amderma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=80.75165200666768&lon=61.44602323521866&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 359: city name = thetford mines
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.12055697371929&lon=-71.50542186421166&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 360: city name = hailey
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.7155244546376&lon=-113.84768487414323&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 361: city name = faanui
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.979523357721703&lon=-150.5452295392954&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 362: city name = yabelo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.9922873750828245&lon=37.90293628676997&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 363: city name = snasa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.191808762932&lon=12.581988489254144&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 364: city name = tura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.92832645987207&lon=96.83334850187379&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 365: city name = kerrabe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.20878832209209&lon=19.982180366093672&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 366: city name = karkaralinsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.075077551386215&lon=76.15589911950178&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 367: city name = acuna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.064822339642802&lon=-101.79986867084665&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 368: city name = sinnamary
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.943793486037137&lon=-50.10881255847556&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 369: city name = glens falls
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.41500928610051&lon=-73.30907889947385&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 370: city name = belyy yar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.34803631463336&lon=85.37920417196108&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 371: city name = manta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.46144763341665396&lon=-84.63231072048143&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 372: city name = artyk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.67998221895442&lon=145.34161373676613&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 373: city name = souillac
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-52.174466075378206&lon=75.22206423766215&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 374: city name = waitati
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-45.69917448148984&lon=171.87454616976453&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 375: city name = buraydah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.22549588599594&lon=46.03004447398101&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 376: city name = boguchany
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.25682897072309&lon=96.5440403194358&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 377: city name = yerofey pavlovich
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.089884297550384&lon=121.57503789324306&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 378: city name = tezu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.363668840302807&lon=97.72824425976864&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 379: city name = klichka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.11766428280137&lon=117.69789155848201&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 380: city name = nemuro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.21040985167204&lon=153.49077172438564&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 381: city name = dwarka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.61375787628664&lon=64.64043768703667&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 382: city name = sao gabriel da cachoeira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.9108153692071994&lon=-66.31940346200263&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 383: city name = sohag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.58473610100458&lon=28.397682228006232&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 384: city name = puerto ayacucho
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.037146211731823&lon=-68.50373630431181&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 385: city name = linfen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.03649299658426&lon=112.10437489948964&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 386: city name = beatrice
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.36759846322147&lon=-96.33365907986345&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 387: city name = victoria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.449963108123242&lon=55.29946372359038&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 388: city name = dingle
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.388597140914584&lon=-17.75476682118523&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 389: city name = richards bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.191769589179025&lon=35.35342607244627&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 390: city name = alappuzha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.970236129857653&lon=74.51662505021315&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 391: city name = guia de isora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.39928029019721&lon=-17.117733980005994&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 392: city name = beya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.211482565294375&lon=90.94245282412629&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 393: city name = narsaq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.97691415655692&lon=-62.632898824437476&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 394: city name = guozhen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.268890765821595&lon=107.18327099450033&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 395: city name = leh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.10722877843709&lon=81.21861719175917&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 396: city name = carutapera
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.22467091397894&lon=-41.206816898475296&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 397: city name = podyuga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.48163818966671&lon=41.04136647322713&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 398: city name = patacamaya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.99682076220091&lon=-68.34536922594413&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 399: city name = mirina
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.773995960593595&lon=24.501270531520703&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 400: city name = balkhash
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.465766811170596&lon=74.43623563310558&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 401: city name = tiznit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.55095789804787&lon=-10.611412606097048&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 402: city name = waipawa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-47.4132633983215&lon=179.23497236588878&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 403: city name = llanes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.63524420848279&lon=-4.779844921309575&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 404: city name = lashio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.27282552909358&lon=98.5426510952451&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 405: city name = raudeberg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.75710653383933&lon=0.6842775404146835&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 406: city name = jizan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.20141677691072&lon=42.09433374113863&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 407: city name = port-gentil
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.5034685533311034&lon=5.380898597976994&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 408: city name = marsh harbour
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.83977460709893&lon=-74.52544394481684&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 409: city name = dubenskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.08707619220607&lon=56.17248395274174&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 410: city name = log
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.611148555631786&lon=43.759428460253105&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 411: city name = sitka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.23681566240771&lon=-140.06036230935254&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 412: city name = honningsvag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.5047749312466&lon=25.93389423003282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 413: city name = palmer
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.36678615999557&lon=-144.06400303489386&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 414: city name = avera
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.18033675834178&lon=-154.66301207458497&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 415: city name = bolungarvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.69819076204632&lon=-25.20817276092356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 416: city name = zatoka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.67369106756206&lon=31.46289288533194&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 417: city name = kozhva
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.93725266131639&lon=56.30229320696762&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 418: city name = mayor pablo lagerenza
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.608786277054264&lon=-60.511160813063526&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 419: city name = mullaitivu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.246442957260953&lon=82.42782700097536&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 420: city name = porto novo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.217414907615222&lon=-35.40300479721466&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 421: city name = okmulgee
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.014147150777845&lon=-95.69675800215755&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 422: city name = la ciotat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.20020252173856&lon=5.6634919920674065&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 423: city name = breytovo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.53789760996719&lon=38.16716022591527&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 424: city name = hualahuises
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.923050374976555&lon=-99.70291816413358&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 425: city name = zinapecuaro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.808904471697588&lon=-100.75859917883412&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 426: city name = narimanov
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.82373332756697&lon=47.52690758905763&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 427: city name = barcelos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.46432004075018085&lon=-63.91503175395803&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 428: city name = poshekhonye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.47286979608975&lon=39.377582372780324&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 429: city name = louisbourg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.345148965326814&lon=-58.1473567968732&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 430: city name = college
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.37585602178757&lon=-149.6574243226753&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 431: city name = flinders
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.08359641194377&lon=130.2920691830244&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 432: city name = iquitos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.362445977394998&lon=-74.27964252291324&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 433: city name = ndjole
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.12611394526466313&lon=10.56053835967495&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 434: city name = liberty
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.29150337354821&lon=-94.44675206143484&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 435: city name = kamennogorsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.2660917435104&lon=29.40368697960662&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 436: city name = tyrma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.210660357029695&lon=132.20028618667135&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 437: city name = chengde
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.373883459092184&lon=117.04955481371417&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 438: city name = tabarqah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.47754459911447&lon=8.883643557437154&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 439: city name = luganville
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.292741594941702&lon=168.41537126517147&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 440: city name = dallas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.95596857523614&lon=-123.22475437226817&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 441: city name = ribeira grande
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.913589076651647&lon=-38.29761696069238&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 442: city name = antofagasta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.728190364196976&lon=-73.3842788112398&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 443: city name = burica
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.0354793445054185&lon=-86.43922811757596&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 444: city name = harper
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.15303935001991&lon=-8.088425566278715&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 445: city name = bairiki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.3361581046551265&lon=163.67296706877784&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 446: city name = kavaratti
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.003477306057817&lon=69.60337215637063&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 447: city name = teguise
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.34145219246477&lon=-13.37503562998836&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 448: city name = bull savanna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.785179720102718&lon=-77.12213973636504&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 449: city name = tupik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.107229552224226&lon=120.56080761847369&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 450: city name = san andres
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.309503016535686&lon=-79.63592746430196&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 451: city name = port keats
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.821179923043601&lon=128.41344807566617&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 452: city name = ozernovskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.781382518500465&lon=155.974757833272&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 453: city name = mareeba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.073433678915336&lon=142.22307130866653&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 454: city name = pag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.03777170228119&lon=14.559739920215463&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 455: city name = lolua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.49875172107626&lon=176.99166714596043&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 456: city name = novoagansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.990162844563315&lon=77.95779295203926&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 457: city name = maniitsoq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.14572480821511&lon=-57.851486944811455&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 458: city name = sibut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.564656209576256&lon=19.52671470707938&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 459: city name = talnakh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=79.86828513093758&lon=90.86381986605909&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 460: city name = catalao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.11140464117689&lon=-47.733037390090516&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 461: city name = tarbagatay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.3965000253406&lon=107.26345432729767&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 462: city name = marquard
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.79620282273433&lon=27.222943302625737&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 463: city name = chifeng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.015644197144155&lon=117.55307174091803&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 464: city name = phan thiet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.032880637405995&lon=107.82235551272066&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 465: city name = shache
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.55676869789022&lon=79.18155554595762&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 466: city name = salinopolis
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.9046116733548644&lon=-44.61546945124087&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 467: city name = parabel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.66016776820783&lon=82.11875779664138&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 468: city name = dengfeng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.41247320645083&lon=113.2022317933862&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 469: city name = pudozh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.33243825838895&lon=37.42688183605307&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 470: city name = ketchikan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.9444979031353&lon=-133.41670261342279&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 471: city name = redmond
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.563798051299756&lon=-120.23445985148999&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 472: city name = barrhead
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.614310680345454&lon=-114.22362094980355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 473: city name = palmas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.41651479691059&lon=-47.41777584008747&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 474: city name = cockburn town
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.93857522228103&lon=-69.67291036526629&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 475: city name = doka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.467946376975547&lon=35.54211145276932&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 476: city name = carroll
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.89977137191957&lon=-94.81125440831967&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 477: city name = hasaki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.631363370432695&lon=145.65849396892713&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 478: city name = bud
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.48823752656597&lon=3.866664541588136&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 479: city name = khasan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.835462553109465&lon=129.84646490003854&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 480: city name = filadelfia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.85907757216205&lon=-59.932252561254415&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 481: city name = san lorenzo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.31423289747113&lon=-57.9436927456085&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 482: city name = mandalgovi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.69198240370079&lon=105.67810314305376&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 483: city name = deniliquin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.225885100014374&lon=144.54582485140622&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 484: city name = visby
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.00757786592726&lon=18.941591319813966&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 485: city name = divnomorskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.34728716120091&lon=37.498910009503476&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 486: city name = santa cruz de la palma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.230218582241733&lon=-17.32641868619936&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 487: city name = guatire
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.217991139670517&lon=-65.60959379299898&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 488: city name = mergui
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.90622246176109&lon=97.79758255111233&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 489: city name = aasiaat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.11617298624643&lon=-53.30570710695852&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 490: city name = tay ninh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.54850168365543&lon=105.7217127289631&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 491: city name = vinkovci
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.24236052395207&lon=18.728003691809477&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 492: city name = salamiyah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.0675746041137&lon=37.71605032881041&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 493: city name = houma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.14243730168971&lon=-91.08400145139943&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 494: city name = beidao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.60593046791735&lon=105.97254532780522&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 495: city name = san cristobal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.493415368936027&lon=-87.01956582482293&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 496: city name = noumea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.91499015211349&lon=163.94004232877842&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 497: city name = atar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.865354217606708&lon=-11.66377811660027&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 498: city name = zhitikara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.70792827717375&lon=61.424737231149095&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 499: city name = soyo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.258445043497517&lon=6.040795178269519&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 500: city name = jiayuguan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.91859819617409&lon=98.60333549322445&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 501: city name = suhut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.59112921930634&lon=30.33194502113932&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 502: city name = mitsamiouli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.061102909329804&lon=42.546384262502016&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 503: city name = djibo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.008436225107843&lon=-1.3227587376063639&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 504: city name = turukhansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.86030217616897&lon=89.58984598332052&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 505: city name = keti bandar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.124772303138826&lon=65.87131596131292&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 506: city name = ventspils
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.12821566641111&lon=21.19218587706797&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 507: city name = mglin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.18661695010118&lon=32.97346967002403&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 508: city name = atbasar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.31073136902796&lon=68.44092162490531&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 509: city name = vanimo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.108995130888516&lon=143.75214611198146&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 510: city name = belmonte
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.71125257405899&lon=-33.97416615925701&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 511: city name = mazara del vallo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.08599485095337&lon=12.212744257559677&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 512: city name = holland
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.65028036120461&lon=-86.22355276000117&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 513: city name = savannah bight
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.178471577771575&lon=-86.00700383882632&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 514: city name = jian
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.780432321497628&lon=114.6031256175462&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 515: city name = kloulklubed
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.32198389438949&lon=131.05307574544963&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 516: city name = strezhevoy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.223164991473055&lon=77.41540063843581&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 517: city name = umm lajj
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.668530152013176&lon=39.24961597095731&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 518: city name = seminole
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.11095567506719&lon=-85.60015149204018&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 519: city name = ruatoria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-35.02660488667451&lon=179.34125440550685&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 520: city name = carlos antonio lopez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.441967177209477&lon=-54.92644852465854&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 521: city name = ayan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.736226848247185&lon=139.7120862923244&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 522: city name = dzilam gonzalez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.78626189605839&lon=-88.76473505454916&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 523: city name = volksrust
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.101167582071064&lon=29.597027743704587&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 524: city name = trincomalee
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.880294875560693&lon=81.34862414352864&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 525: city name = bismarck
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.942802913018994&lon=-99.91507268670175&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 526: city name = garden city
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.86498223237841&lon=-100.65697251825691&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 527: city name = nguiu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.920802088989902&lon=127.892141981956&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 528: city name = northam
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.980825457253317&lon=120.00844422365782&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 529: city name = khorramshahr
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.75069052255799&lon=46.10170199342977&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 530: city name = cartagena del chaira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.7352277102705784&lon=-74.19599830486243&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 531: city name = lata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.098438772392115&lon=165.06011696126228&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 532: city name = kurmanayevka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.21025974574533&lon=52.06152390896955&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 533: city name = saint anthony
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.77607904684163&lon=-52.78354871424135&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 534: city name = zhetybay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.71633389274035&lon=52.37799802081307&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 535: city name = karachi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.142122030551647&lon=67.11045836134852&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 536: city name = vrangel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.55957263829069&lon=133.9001692444063&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 537: city name = lakes entrance
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-38.98868207081945&lon=149.98961112279574&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 538: city name = klyuchi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.162067280946104&lon=160.8220093298931&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 539: city name = halmstad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.63780360628752&lon=12.602321673750936&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 540: city name = hovd
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.37526039236883&lon=104.40650661890402&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 541: city name = westpunt
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.52973910474995&lon=-69.46749520220837&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 542: city name = araouane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.22243219243545&lon=-2.02633374057973&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 543: city name = superior
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.099821199734606&lon=-91.36255702852148&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 544: city name = mariakani
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.4465079905585725&lon=39.221674686759656&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 545: city name = half moon bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.990891779086624&lon=-134.69865677153214&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 546: city name = teahupoo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.4813856040885&lon=-148.0429225742731&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 547: city name = marsa matruh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.373439909550683&lon=27.151741676373177&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 548: city name = vanderhoof
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.9460568804453&lon=-124.94452492173491&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 549: city name = inhambane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.922394170106145&lon=37.06370694243219&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 550: city name = mandurah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.27033918610214&lon=114.59288179072433&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 551: city name = mogadishu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.9493109859641464&lon=48.1815648813683&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 552: city name = erzin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.411342781076655&lon=95.97167484868578&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 553: city name = amapa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.220170933460551&lon=-47.66947349192094&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 554: city name = oktyabrskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.70467936952258&lon=66.21901084909197&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 555: city name = sakskobing
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.71458571122372&lon=11.654769187747092&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 556: city name = lasa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.175581689800524&lon=88.80039103027502&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 557: city name = mbandaka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.17929348560619474&lon=17.394604345663964&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 558: city name = yar-sale
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.11861663397463&lon=69.25294409466252&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 559: city name = lompoc
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.558062587539695&lon=-125.52744180900683&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 560: city name = rantepao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.1128204054559347&lon=119.2322327151752&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 561: city name = hattiesburg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.637786992502186&lon=-89.68911882504672&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 562: city name = grande-riviere
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.075336292595665&lon=-63.98167092789561&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 563: city name = lao cai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.452527735689998&lon=104.47097073360771&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 564: city name = diego de almagro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.303390913244208&lon=-70.84684134843998&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 565: city name = amarpur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.75466263217446&lon=91.65517058661038&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 566: city name = jieshi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.60571484004643&lon=116.07347999799379&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 567: city name = metro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.326494533154872&lon=106.70525481890365&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 568: city name = muros
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.28590304596037&lon=-11.120037633231078&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 569: city name = batagay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.42537168504288&lon=134.28713608895504&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 570: city name = volsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.87354794680826&lon=47.35406033682733&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 571: city name = mansa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.287520534326191&lon=28.4071588890709&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 572: city name = pitimbu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.73988632457575&lon=-28.840065868049436&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 573: city name = labutta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.868917480644313&lon=89.49188611480395&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 574: city name = nouadhibou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.985718819270673&lon=-17.18749564805853&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 575: city name = pisco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.94452105002827&lon=-95.5618275833908&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 576: city name = nome
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.7743771849745&lon=-166.21669591472235&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 577: city name = nueva loja
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.134941011230211&lon=-76.02918470373595&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 578: city name = xam nua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.52400113005035&lon=103.95723552863967&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 579: city name = beloha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.553686768471024&lon=42.90926968970169&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 580: city name = asfi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.17992072935489&lon=-11.985987710020908&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 581: city name = andra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.921653886465805&lon=65.817773666552&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 582: city name = tallahassee
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.51021627487802&lon=-84.19390500359911&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 583: city name = bay roberts
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.14926635052538&lon=-53.28324997534229&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 584: city name = aswan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.10338529321453&lon=31.08136851247079&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 585: city name = impfondo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.326322629271118&lon=19.41532749636977&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 586: city name = tessalit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.037872554758493&lon=3.6952739822499154&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 587: city name = najran
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.035062527279734&lon=44.029318472727596&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 588: city name = wilmington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.47617123639574&lon=-83.45896958024292&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 589: city name = velika gorica
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.48115858976027&lon=15.967530414882532&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 590: city name = mpika
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.054389656178898&lon=31.022676501739312&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 591: city name = grajewo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.47441098432816&lon=22.492669111953063&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 592: city name = brae
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.68632589136928&lon=-1.864541195904195&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 593: city name = saint-joseph
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.860463655975884&lon=55.71207287894475&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 594: city name = chimbote
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.246495196094997&lon=-79.70671476110158&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 595: city name = bratsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.84482646441589&lon=101.45744539269623&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 596: city name = grand centre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.45362330389722&lon=-111.05218073335031&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 597: city name = rudbar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.461639331749637&lon=63.17563448867517&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 598: city name = urubamba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.287047794894221&lon=-72.178639304002&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 599: city name = palora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.8983632438577018&lon=-77.5997645323777&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 600: city name = verkhoyansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.37267504375438&lon=132.7432132529388&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 601: city name = benguela
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.348783104914972&lon=10.268819934013123&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 602: city name = oranjemund
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.12695886056307&lon=6.563131326974087&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 603: city name = cagayan de tawi-tawi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.121845917722013&lon=118.53865509118418&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 604: city name = ledyard
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.288516083795685&lon=-94.47332835574716&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 605: city name = warqla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.39226132005237&lon=7.839397852133317&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 606: city name = khandyga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.40177368318422&lon=134.2625353266311&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 607: city name = amarwara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.158610030463706&lon=79.23513635214351&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 608: city name = naftah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.31108022912271&lon=7.53051149877362&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 609: city name = karabulak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.86850142643891&lon=78.6002762224648&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 610: city name = lyngdal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.41484576273808&lon=6.975294419957777&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 611: city name = denpasar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.098561863995684&lon=114.76088256060751&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 612: city name = rapid valley
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.87585357822513&lon=-103.07223266648067&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 613: city name = myitkyina
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.641207303186178&lon=97.06215769314946&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 614: city name = gat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.85701686227867&lon=9.749961288790246&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 615: city name = shyryayeve
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.44768248620184&lon=30.407035689973696&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 616: city name = hobyo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.2762828581159198&lon=51.53857006787581&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 617: city name = buala
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.7265317574260735&lon=162.62358749700928&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 618: city name = conceicao do araguaia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.626590454693101&lon=-49.080022325454394&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 619: city name = kutum
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.123197589085237&lon=25.694927099479315&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 620: city name = ixtapa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.395055667213484&lon=-104.04774299205002&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 621: city name = plouzane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.05329360106575&lon=-5.222206420119079&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 622: city name = maceio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.236534793435155&lon=-29.478088088751008&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 623: city name = praia da vitoria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.38259874086438&lon=-23.05014810161677&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 624: city name = ostrovnoy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.63784462986695&lon=42.047182991219614&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 625: city name = cozumel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.591265604769447&lon=-86.44396342427771&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 626: city name = zonguldak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.707131051148025&lon=32.48856263367102&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 627: city name = kenitra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.268107593750784&lon=-7.356233562984585&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 628: city name = oistins
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.812443196967081&lon=-58.458018687679555&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 629: city name = porto recanati
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.82166713732187&lon=14.167615984566709&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 630: city name = nola
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.841162633500815&lon=15.858499374652013&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 631: city name = plettenberg bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-38.68608784974177&lon=23.904644838577013&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 632: city name = nantucket
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.873809890571664&lon=-65.96579691489625&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 633: city name = husavik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=80.01401801112846&lon=-7.493198863191282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 634: city name = sisimiut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.04954546448508&lon=-58.950377032460196&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 635: city name = sakakah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.69537782229129&lon=43.03750624287491&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 636: city name = el prat de llobregat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.8570086560276&lon=2.230182512919839&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 637: city name = fuerte olimpo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.245688461420144&lon=-58.213409424369914&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 638: city name = beterou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.158257580046723&lon=2.050990742247791&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 639: city name = severnyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.47269683622613&lon=64.03460853915402&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 640: city name = rivers
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.46095068885779&lon=-100.5024213413288&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 641: city name = esna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.321941273410687&lon=32.06052125676891&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 642: city name = luba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.393957352596942&lon=7.563418033959607&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 643: city name = ugoofaaru
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.568610512381412&lon=63.34330345380718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 644: city name = san rafael del sur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.211899183087112&lon=-87.36300432747325&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 645: city name = mattru
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.4790961237753777&lon=-15.4105914655743&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 646: city name = manzhouli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.99647416654548&lon=117.00157269546906&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 647: city name = kapustin yar-1
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.83665222215083&lon=45.698506317972885&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 648: city name = ebbs
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.750320966791605&lon=12.265669965295388&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 649: city name = kiboga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.8707551869768508&lon=31.63733385892226&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 650: city name = tres arroyos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-38.47733578543327&lon=-59.53989084286887&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 651: city name = wenceslau braz
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.708306610137&lon=-49.832982620522216&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 652: city name = chara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.10280190660944&lon=116.58891889139807&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 653: city name = pokhara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.547517167060064&lon=84.83906478770797&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 654: city name = mount isa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.846475471799295&lon=136.539450317086&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 655: city name = iztapa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.872349159046948&lon=-90.60349840765774&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 656: city name = punta alta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-39.07450260779599&lon=-61.475208762689235&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 657: city name = amga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.65683221797423&lon=132.48450152776837&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 658: city name = eureka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.586770828982&lon=-130.8812646867286&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 659: city name = huron
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.223395553626744&lon=-97.96287465383728&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 660: city name = votkinsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.962852808258646&lon=54.143992629960536&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 661: city name = ahuachapan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.93273770712105&lon=-89.89137571542966&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 662: city name = apodi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.73449000504398&lon=-37.74882950076446&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 663: city name = hue
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.314611347517243&lon=108.46811397906833&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 664: city name = tilichiki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.488201349269076&lon=167.41298460758804&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 665: city name = merauke
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.607388025692288&lon=139.9499426196391&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 666: city name = champerico
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.463875685359213&lon=-96.57713337678042&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 667: city name = santa rosalia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.457936774314206&lon=-113.25637392959489&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 668: city name = marzuq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.651360000243017&lon=17.420428409346016&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 669: city name = vila velha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.011449375844933&lon=-24.119893957349205&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 670: city name = marystown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.771956736345544&lon=-54.471498204468375&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 671: city name = ruteng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.429236433465789&lon=120.6389585525925&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 672: city name = meulaboh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.9551243108994356&lon=95.87332254417726&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 673: city name = svetlaya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.944853221437796&lon=139.22907865052213&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 674: city name = otradnoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.54918966794702&lon=145.59978464869909&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 675: city name = fare
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.260360868206462&lon=-149.19549294806268&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 676: city name = jiddah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.651012057752325&lon=36.82832101200603&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 677: city name = alto araguaia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.474714344978082&lon=-53.92604729506215&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 678: city name = mount gambier
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-58.01247374583569&lon=127.4528302351095&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 679: city name = kontagora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.210563716406341&lon=5.0258722758058525&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 680: city name = coahuayana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.18927618539503&lon=-110.27075030847632&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 681: city name = eyl
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.252928853480185&lon=50.60007674900115&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 682: city name = luderitz
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.42043226850992&lon=7.022143432245201&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 683: city name = bure
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.750566567224453&lon=35.86810163464611&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 684: city name = haapiti
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.835877912319077&lon=-150.60782664257428&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 685: city name = balkanabat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.19976420679072&lon=54.15454900125647&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 686: city name = kegayli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.098232361772006&lon=58.124288203345486&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 687: city name = rodbyhavn
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.542046625403685&lon=11.420619325871627&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 688: city name = westport
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.957675608626346&lon=-15.012476271733675&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 689: city name = nova cruz
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.515241644165869&lon=-35.43462518038774&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 690: city name = karamay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.44010581700701&lon=84.8060814669629&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 691: city name = kaseda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.738047452769763&lon=130.80458366635634&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 692: city name = san pedro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.4152456696996&lon=-62.89560761831551&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 693: city name = medea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.18973968465494&lon=1.968131270423072&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 694: city name = takhatpur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.998026598825007&lon=81.87833235956845&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 695: city name = kapoeta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.143416197808378&lon=34.7145991657961&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 696: city name = pirawa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.102512802856566&lon=76.10720293255912&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 697: city name = montepuez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.041851742491275&lon=38.770085006453826&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 698: city name = brcko
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.93808624946777&lon=18.924185663246902&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 699: city name = izvestkovyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.512083801004394&lon=131.59211169347054&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 700: city name = biak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.7096438276501402&lon=139.65968488534838&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 701: city name = arzgir
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.392617631687074&lon=44.40860152714578&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 702: city name = polunochnoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.179296119046626&lon=60.78972448206147&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 703: city name = nuuk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.70399661763855&lon=-55.36950467812672&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 704: city name = nizwa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.73987006289309&lon=56.867883515743785&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 705: city name = sioux lookout
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.165454447407626&lon=-89.21938351828605&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 706: city name = nikolsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.280234601003855&lon=45.09887870868167&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 707: city name = nanga eboko
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.361110505998141&lon=12.547520594582636&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 708: city name = beira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.190974282319686&lon=36.826452025146324&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 709: city name = chake chake
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.36397281620863&lon=43.866074660680994&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 710: city name = agva
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.13060311610391&lon=30.227650998328613&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 711: city name = kalat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.75992940708123&lon=66.6381874500193&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 712: city name = pangai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.070865369816005&lon=-173.43150559040876&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 713: city name = comodoro rivadavia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-49.1223403075761&lon=-64.63729842211472&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 714: city name = fort nelson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.13858769981127&lon=-123.37668429203381&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 715: city name = viligili
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.5134421734926207&lon=75.27788157957534&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 716: city name = yulara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.87028396742531&lon=127.36595568840272&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 717: city name = acarau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.55652214144493&lon=-39.322821206343576&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 718: city name = port hedland
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.745677582657777&lon=122.89992408486614&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 719: city name = ustrzyki dolne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.244121081342996&lon=22.397738599250232&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 720: city name = korla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.28683841444581&lon=89.95463254346157&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 721: city name = port blair
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.946585214432616&lon=93.95642069114524&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 722: city name = sydney
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.8166783484144&lon=-60.396536466767316&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 723: city name = makokou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.3531313138566077&lon=12.868116706526678&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 724: city name = dubbo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.181413965308735&lon=145.60991988461262&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 725: city name = ust-karsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.31342867312591&lon=119.27423040526463&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 726: city name = timmins
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.96962229948775&lon=-81.41273017397961&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 727: city name = formoso do araguaia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.42430720266968&lon=-51.57982859683756&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 728: city name = zhigansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.4024724560914&lon=122.13847233010807&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 729: city name = aklavik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.77837609373884&lon=-138.19366694612793&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 730: city name = karlstad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.034328024665115&lon=13.659629774286486&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 731: city name = townsville
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.079051382397196&lon=147.45235553650195&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 732: city name = ulverstone
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-42.08245665548555&lon=146.15611984931684&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 733: city name = santarem
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.098834566589531&lon=-54.0429266528774&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 734: city name = kobojango
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.301719056678152&lon=28.43776607373536&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 735: city name = cabedelo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.45592559185225&lon=-33.156680301891214&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 736: city name = pangkalanbuun
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.8699909450724732&lon=111.35200886967851&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 737: city name = dandong
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.002125793305&lon=124.38194902319356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 738: city name = asau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.761382783251833&lon=177.71104089569047&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 739: city name = chopinzinho
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.732501290627397&lon=-52.37778916295116&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 740: city name = inzhavino
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.148616610545474&lon=42.79363892287557&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 741: city name = kalanguy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.100866731719776&lon=116.30685183067425&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 742: city name = kiunga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.821333736060893&lon=140.57073984281192&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 743: city name = khonuu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.7880829546769&lon=140.35339839388052&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 744: city name = santa cruz de tenerife
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.630026723241087&lon=-15.661045357234087&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 745: city name = milkovo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.597948646600315&lon=158.4052377695029&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 746: city name = mutsu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.672245048615&lon=141.13730479748074&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 747: city name = cahors
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.7740698910759&lon=1.5383177448548793&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 748: city name = chitral
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.97145943865054&lon=72.75430926530669&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 749: city name = mingyue
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.85859129858662&lon=128.81734443593103&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 750: city name = arlit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.76478421266451&lon=7.302024105255015&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 751: city name = illertissen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.227338171889954&lon=10.181327593603129&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 752: city name = sorland
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.38681187622944&lon=10.744946496221758&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 753: city name = ola
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.23609676590871&lon=151.33561248924508&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 754: city name = salmas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.06102040468056&lon=44.995295372865&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 755: city name = port moresby
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.69800514478861&lon=148.32234104775478&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 756: city name = rio branco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.73029215714955&lon=-53.557076455628646&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 757: city name = taranagar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.84047493083662&lon=75.18204694081405&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 758: city name = high level
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.44484628262333&lon=-117.97032702981366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 759: city name = kendari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.891383090234868&lon=121.72439086591606&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 760: city name = lodwar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.89050078649484&lon=35.132581589491&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 761: city name = odienne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.199297902363426&lon=-8.017878378324355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 762: city name = hemsedal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.050258769829895&lon=8.735917295790756&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 763: city name = madawaska
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.27107466490432&lon=-68.41745007134021&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 764: city name = macae
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.364105861063337&lon=-41.79663894657&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 765: city name = isangel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.505427387565035&lon=176.49175904855446&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 766: city name = dalnerechensk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.21931622536562&lon=133.58816790181277&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 767: city name = cotonou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.789930072584426&lon=2.83694365755224&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 768: city name = nisia floresta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.4355932315033186&lon=-26.416878644066657&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 769: city name = bilibino
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.52313557839793&lon=165.31523756204814&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 770: city name = tabiauea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.0658187966481023&lon=168.65354628371256&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 771: city name = ikirun
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.889076744131515&lon=4.65721435116464&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 772: city name = orcopampa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.218470236227347&lon=-72.84852908915575&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 773: city name = sijunjung
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.8268744846746046&lon=101.02039231312682&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 774: city name = ji-parana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.468235451487274&lon=-61.699552458666744&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 775: city name = orumiyeh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.778885156155496&lon=44.74267829294527&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 776: city name = mexico
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.38840778424046&lon=-91.60193529480402&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 777: city name = chagda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.45674748025368&lon=129.1276722345239&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 778: city name = marrakesh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.641951077983805&lon=-5.453661856069033&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 779: city name = nehe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.2891995914016&lon=124.02491280444718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 780: city name = tianpeng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.2543563549759&lon=102.83856316273915&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 781: city name = kautokeino
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.883784259755&lon=21.9853861560091&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 782: city name = hofn
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.13174389388439&lon=-15.7215637433886&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 783: city name = banes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.248961577748062&lon=-75.78035295100835&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 784: city name = kununurra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.489562939238866&lon=128.01035287157043&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 785: city name = lufkin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.388300515471997&lon=-95.08890493312376&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 786: city name = caraquet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.619147312400656&lon=-65.1450516419607&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 787: city name = bestobe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.02595404780445&lon=72.58705251343662&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 788: city name = yinchuan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.054159598717234&lon=108.17527749097968&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 789: city name = mbekenyera
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.296274185376888&lon=38.94386659051224&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 790: city name = sergeyevka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.19332101023534&lon=67.96540319747362&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 791: city name = jalu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.937713734479686&lon=22.20615957694173&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 792: city name = quthing
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.569323053726436&lon=27.44875668768688&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 793: city name = ambon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.161746608476804&lon=126.30607358439016&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 794: city name = placido de castro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.209559011403172&lon=-67.5989388601433&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 795: city name = khorion
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.89000174101146&lon=26.854638486637555&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 796: city name = qaqortoq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.09246059247164&lon=-47.019322427494075&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 797: city name = clinton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.10897901287245&lon=-84.08142016937137&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 798: city name = zeya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.72423390678085&lon=128.83759361504832&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 799: city name = alexandria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.23584559790197&lon=-95.3926443525296&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 800: city name = oliveira do hospital
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.43368836721589&lon=-7.865099332603705&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 801: city name = tigil
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.707069149882756&lon=158.05523312812653&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 802: city name = bonavista
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.028896129832106&lon=-49.9591759858468&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 803: city name = srandakan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.691972574218838&lon=108.61178357668592&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 804: city name = tahta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.509992534928315&lon=28.152051946309&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 805: city name = lingao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.928633114024507&lon=108.38885610918595&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 806: city name = acu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.703952768442235&lon=-36.883446536364545&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 807: city name = ibra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.214981372336993&lon=58.44206741082195&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 808: city name = miyako
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.07697464777476&lon=144.60053775751868&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 809: city name = mouzakion
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.50420995571773&lon=20.399846689750802&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 810: city name = bartica
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.418076248752229&lon=-58.23287960822884&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 811: city name = singkang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.986628317302504&lon=120.05203957629135&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 812: city name = bilgi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.24540895979024&lon=75.46180535888121&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 813: city name = owando
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.13636651692046087&lon=15.569359534969436&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 814: city name = muzhi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.22872731470503&lon=65.11873887463355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 815: city name = samarai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.805698394604377&lon=153.3805884716436&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 816: city name = bilma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.341277265508978&lon=13.644503893569237&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 817: city name = marfino
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.83840540874914&lon=49.123095290080926&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 818: city name = colac
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-38.86608749438124&lon=143.58762486345222&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 819: city name = sucre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.386768430565823&lon=-80.2841493292589&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 820: city name = lethem
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.727881283787198&lon=-60.073992479775995&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 821: city name = preobrazheniye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.7036630510417&lon=134.92025896137977&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 822: city name = boffa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.35706125672337&lon=-15.152390122632312&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 823: city name = camacha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.202575881896095&lon=-16.848470643113217&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 824: city name = vysokogornyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.26052213443859&lon=139.2059515933003&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 825: city name = mangochi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.749023706399228&lon=35.24280675633253&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 826: city name = koslan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.77162182562782&lon=48.82073286040449&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 827: city name = kurumkan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.01115642417261&lon=110.95607876252501&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 828: city name = chupa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.19875925433314&lon=33.5993779777277&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 829: city name = kyren
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.60143292199018&lon=101.79856632152854&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 830: city name = broken hill
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.206922844930695&lon=141.30546798772565&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 831: city name = lloydminster
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.185591386745585&lon=-109.99690750502307&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 832: city name = grand forks
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.62848321843936&lon=-96.43102921629503&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 833: city name = dunedin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-49.12290122131268&lon=175.3672731220684&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 834: city name = nyurba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.04214193483602&lon=117.03778022914082&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 835: city name = daloa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.062228167361212&lon=-6.922623335656624&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 836: city name = kelheim
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.01317902553575&lon=11.913202037373111&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 837: city name = oktyabrskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.60889423726934&lon=64.18229778394664&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 838: city name = ulladulla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-36.909258969427576&lon=153.3617841258968&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 839: city name = zorgo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.238286575594017&lon=-0.8780404603436693&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 840: city name = racaciuni
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.35503893273503&lon=26.963843674903245&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 841: city name = morehead
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.871480676166712&lon=141.98644149306006&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 842: city name = guanica
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.498612840277133&lon=-67.41609875509315&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 843: city name = mukhen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.770172201247476&lon=136.16315401085706&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 844: city name = aljezur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.6383292209627&lon=-12.78556361462256&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 845: city name = general pico
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.36157604710526&lon=-63.67828023702137&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 846: city name = parkersburg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.935345054652714&lon=-80.96513288481667&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 847: city name = bielsk podlaski
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.82248709932679&lon=23.156796862487425&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 848: city name = same
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.0727566342933414&lon=37.36136725937203&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 849: city name = singaparna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.908168993403379&lon=107.25563169408593&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 850: city name = mahaicony
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.674476979093924&lon=-55.604286035860355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 851: city name = choix
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.48532279088994&lon=-107.70291084440817&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 852: city name = coihaique
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-48.205920196984046&lon=-77.7875707063702&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 853: city name = bourne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.77481131888723&lon=-0.4603813729146111&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Total Time = 1164.596700668335
    


```python
# Combine data into one df 
city_df2["Temperature"] = temp
city_df2["Humidity"] = humidity
city_df2["Clouds"] = clouds
city_df2["Wind Speed"] = wind
# Save as csv 
city_df2.to_csv("city_df2.csv",encoding="utf-8", index=False)
city_df2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Lat</th>
      <th>Long</th>
      <th>City</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Clouds</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-39.749718</td>
      <td>-2.559579</td>
      <td>saldanha</td>
      <td>61.00</td>
      <td>100</td>
      <td>76</td>
      <td>18.30</td>
    </tr>
    <tr>
      <th>1</th>
      <td>42.276486</td>
      <td>-65.465887</td>
      <td>shelburne</td>
      <td>45.61</td>
      <td>98</td>
      <td>92</td>
      <td>46.26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-83.837468</td>
      <td>-174.188989</td>
      <td>vaini</td>
      <td>6.01</td>
      <td>65</td>
      <td>44</td>
      <td>13.04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-8.660537</td>
      <td>-32.565047</td>
      <td>olinda</td>
      <td>81.88</td>
      <td>100</td>
      <td>44</td>
      <td>11.14</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.055473</td>
      <td>127.933534</td>
      <td>ternate</td>
      <td>82.60</td>
      <td>90</td>
      <td>88</td>
      <td>7.78</td>
    </tr>
  </tbody>
</table>
</div>



# Temperature vs Latitude Plot 


```python
# Temperature vs Latitude
plt.scatter(city_df2["Lat"],city_df2["Temperature"],marker=".")
plt.title("Temperature (F) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.savefig("1_Temp_v_Lat.png")
plt.show()
```


![png](output_8_0.png)


# Humidity vs Latitude Plot 


```python
# Humidity vs Latitude
plt.scatter(city_df2["Lat"],city_df2["Humidity"],marker =".")
plt.title("Humidity (%) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.savefig("2_Humidity_v_Lat.png")
plt.show()
```


![png](output_10_0.png)


# Cloudiness vs Latitude Plot 


```python
# Cloudiness vs Latitude 
plt.scatter(city_df2["Lat"],city_df2["Clouds"],marker =".")
plt.title("Cloudiness (%) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.savefig("3_Clouds_v_Lat.png")
plt.show()
```


![png](output_12_0.png)


# Wind Speed vs Latitude Plot 


```python
# Wind Speed vs Latitude 
plt.scatter(city_df2["Lat"],city_df2["Clouds"],marker =".")
plt.title("Wind Speed (mph) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
plt.savefig("4_Wind_v_Lat.png")
plt.show()
```


![png](output_14_0.png)

