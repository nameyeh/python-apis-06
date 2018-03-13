
# WeatherPy Data Analysis 
#### Objective: Visualize the weather of 500+ cities across the world of varying distance from the equator to illustrate what the weather is like as we approach the equator. 
## Observed Trends 
* As we approach the equator, temperatures increase. Fig 1 shows temperatures at the equator concentrated around 70-85 degrees F. Similarly, temperatures start to gradually drop as we move further and further away from the equator (Fig 1).
* Although the list of cities was randomly generated, all the figures show cities concentrated in the Northern Hemisphere as expected, since the southern hemisphere has fewer cities. 
* Figures 2, 3, and 4 show more scatter and no strong correlation between distance from equator and humidity, clouds, or wind. 


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
# print(len(city_df))
# Remove duplicates 
city_df2 = city_df.drop_duplicates("City",keep="first")
# Confirm at least 500 unique (non-repeat) cities in df 
# city_df2.count()
```

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

    ---------------Beginning Data Retrieval---------------
                                                          
    Now retrieving city number 1: city name => nanortalik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.05878779164627&lon=-40.211975313663004&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 2: city name => illoqqortoormiut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.8382144986787&lon=-18.439735977542313&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 3: city name => chokurdakh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.30301948157239&lon=151.32046390576625&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 4: city name => kaeo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.258019959983933&lon=176.25679391933&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 5: city name => albany
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-83.10194295437921&lon=98.46252586935515&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 6: city name => nguiu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.75975672416999&lon=128.32390181201282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 7: city name => sokoni
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.662035379306985&lon=43.02024156030103&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 8: city name => belushya guba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.1190170733075&lon=48.880453577009234&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 9: city name => dikson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=84.42085546965205&lon=89.84998305412847&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 10: city name => vaini
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-67.5685288740453&lon=-173.36365008565699&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 11: city name => saint-philippe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-66.43474516693479&lon=77.05614939255014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 12: city name => pionerskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.50557324919214&lon=20.285355744548696&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 13: city name => tuktoyaktuk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=76.43229632190767&lon=-135.50622888551925&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 14: city name => hobart
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-79.56148669550862&lon=146.87398913010372&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 15: city name => beringovskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.273166075562415&lon=175.46668928779872&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 16: city name => puerto ayora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.8005482876458245&lon=-113.71577257944338&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 17: city name => avarua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-74.05289137218978&lon=-167.26208389304313&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 18: city name => alugan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.404616434511482&lon=126.77737519115391&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 19: city name => bredasdorp
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-75.92900442928186&lon=23.549856210435337&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 20: city name => stoyba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.93812750261813&lon=131.0379874818421&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 21: city name => saint-pierre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.76998607841168&lon=-55.70368286662662&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 22: city name => caravelas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.140001066782958&lon=-26.28341799414511&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 23: city name => port elizabeth
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-79.17762568249047&lon=35.65202496734025&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 24: city name => bilaua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.099058528884882&lon=78.49343421672461&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 25: city name => yellowknife
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.04691037474382&lon=-117.76743332778518&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 26: city name => ust-maya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.55850724941163&lon=133.79810375899973&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 27: city name => simpang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.3702091187147971&lon=104.2434438772878&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 28: city name => rocha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-43.41269315501969&lon=-46.76563454716131&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 29: city name => tumannyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=83.00406184512585&lon=39.79637646047803&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 30: city name => hobyo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.857702094526573&lon=52.21845867422013&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 31: city name => novyy urengoy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.53289510871599&lon=76.15104273173165&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 32: city name => busselton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-51.37257455514456&lon=101.15144516539357&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 33: city name => chara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.885393046813334&lon=117.24991031513582&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 34: city name => kavieng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.842060824423044&lon=151.87069545001555&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 35: city name => nikolskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.97032004987871&lon=173.16535126885805&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 36: city name => murwillumbah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.45062533659933&lon=153.06337279562302&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 37: city name => cape town
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-74.11173046002143&lon=-9.079848697446323&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 38: city name => grand centre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.28897808262295&lon=-109.80807028690904&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 39: city name => faanui
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.297905186634324&lon=-153.11429583498543&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 40: city name => port keats
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.435152344959349&lon=130.79138220944282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 41: city name => georgetown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.429497857335178&lon=-10.04680722092857&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 42: city name => paso de los toros
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.235641482230804&lon=-56.57404658027728&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 43: city name => barrow
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=84.70095308722642&lon=-151.51220855590444&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 44: city name => cherskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=85.30175617912602&lon=159.62170287992797&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 45: city name => severo-kurilsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.94209408830427&lon=162.87376321138976&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 46: city name => harper
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.4648220788670443&lon=-8.58539980074272&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 47: city name => ushuaia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-85.37126684645524&lon=-23.412872349563116&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 48: city name => touho
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.519757022601837&lon=165.71220109994522&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 49: city name => stornoway
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.56296565450393&lon=-7.6033644380696614&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 50: city name => cabo san lucas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.531021773189607&lon=-119.53823632909356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 51: city name => taolanaro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-64.172956460451&lon=62.583884003772255&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 52: city name => kondagaon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.679420152078293&lon=81.12602620294382&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 53: city name => college
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.56651053183936&lon=-147.1704035871538&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 54: city name => atlantic beach
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.492811527071296&lon=-81.18775198122799&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 55: city name => esperance
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.77398615196043&lon=122.39735284322512&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 56: city name => rakkestad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.408869887987834&lon=11.48262614560457&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 57: city name => mabopane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.345859171246758&lon=28.068909271270996&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 58: city name => punta arenas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-68.61834949663347&lon=-113.24605129061716&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 59: city name => souillac
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.355574827524165&lon=58.074147617287565&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 60: city name => saint george
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.62497556911954&lon=-50.86882039492784&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 61: city name => hermanus
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-78.38978035358905&lon=6.870326420061019&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 62: city name => katsuura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.561605634560735&lon=142.84253098833716&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 63: city name => zhirnovsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.13978487846899&lon=45.11933378386232&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 64: city name => bluff
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-55.02210261837347&lon=169.7310261928625&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 65: city name => ndiekro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.42310388111558&lon=-4.545143872868948&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 66: city name => tasiilaq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.96751595927924&lon=-41.98645651826618&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 67: city name => vao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.662134855488503&lon=165.0614439895146&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 68: city name => atuona
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.824965828424254&lon=-137.72067788719605&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 69: city name => rikitea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-64.3089751297208&lon=-117.53953442095481&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 70: city name => torbay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.087577092202835&lon=-46.28160178016179&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 71: city name => killybegs
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.61074374220868&lon=-10.538280087331088&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 72: city name => vaitupu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.5632560922433782&lon=-177.07048994918804&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 73: city name => butaritari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.429262068372893&lon=164.01120783671894&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 74: city name => mutoko
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.165644030307504&lon=32.324013296778276&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 75: city name => san jeronimo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.677326786513007&lon=-103.44210463239344&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 76: city name => duobao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.76268281157941&lon=112.46857881083969&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 77: city name => grand gaube
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.97188545006871&lon=60.08773990772602&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 78: city name => cayenne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.836257216498112&lon=-44.7228007196658&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 79: city name => jamestown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.696629642610418&lon=-0.1106760207868831&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 80: city name => port lincoln
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-39.23228523377726&lon=132.16035348825602&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 81: city name => saleaula
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.6697985107695814&lon=-169.31095965960165&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 82: city name => codrington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.2750121457064&lon=-53.18297527837262&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 83: city name => saint-paul-les-dax
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.90793490667588&lon=-0.8721951590485162&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 84: city name => mataura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-71.14014693417181&lon=-162.12033568040752&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 85: city name => new norfolk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-65.65424761745022&lon=132.53252911365695&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 86: city name => hoquiam
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.082784204235566&lon=-124.37487058516612&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 87: city name => wum
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.775426690816474&lon=9.679350082030027&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 88: city name => manokwari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.6160864285076286&lon=133.13084446163936&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 89: city name => campbeltown
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.39529891048457&lon=-5.541520986884933&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 90: city name => bogorodskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.90279932920228&lon=140.22023438170106&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 91: city name => cullman
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.26752812926097&lon=-86.81801570049257&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 92: city name => arraial do cabo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-37.76771927450595&lon=-34.67544997070286&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 93: city name => port hueneme
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.346957491130908&lon=-120.97134570645088&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 94: city name => brae
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.23659182809251&lon=0.569206919930366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 95: city name => nioki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.6657779655015474&lon=18.157201259688833&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 96: city name => mocuba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.873272017331587&lon=38.250308306045724&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 97: city name => koumac
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.857181005644847&lon=162.27984155761231&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 98: city name => kapaa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.66656452539725&lon=-156.0554208503078&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 99: city name => auki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.345451114665792&lon=165.50008928034464&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 100: city name => asosa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.29639176599305&lon=34.43406766628769&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 101: city name => narsaq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=81.64914232404217&lon=-66.35510855370345&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 102: city name => rjukan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.834132119378154&lon=8.276327322353836&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 103: city name => grand river south east
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.528055963865327&lon=77.87621841227673&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 104: city name => cervia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.44151760676965&lon=12.538027357866468&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 105: city name => tura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.94095585723178&lon=98.76147098617724&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 106: city name => hilo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.21119412920706&lon=-144.20703418533773&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 107: city name => pochutla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.438811100250888&lon=-96.22384389619704&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 108: city name => tsihombe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-33.84451469114412&lon=45.00750347313962&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 109: city name => suntar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.5009985817116&lon=117.1441599556133&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 110: city name => saskylakh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=88.65159077014565&lon=111.62726702679942&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 111: city name => victoria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.045179223568724&lon=52.342141431975335&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 112: city name => magadan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.22354845455297&lon=150.73249324364014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 113: city name => isangel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.271664494335553&lon=178.03241235954584&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 114: city name => carnarvon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.060799364763852&lon=86.74700897624439&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 115: city name => khatanga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=81.79232449733851&lon=103.8979555197181&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 116: city name => nome
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.94039529997863&lon=-164.68038111604966&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 117: city name => vanavara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.802533354771896&lon=105.0503041637412&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 118: city name => kahului
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.549572882025785&lon=-147.1243719549276&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 119: city name => dibaya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.199688743763446&lon=22.25463572304176&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 120: city name => bandarbeyla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.490984675958373&lon=54.07998212595649&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 121: city name => bathsheba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.54568185712428&lon=-48.05451251652025&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 122: city name => thompson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.37956099132737&lon=-97.78586864937759&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 123: city name => severomuysk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.37633870188577&lon=113.53623070587241&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 124: city name => bengkulu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.88489128548838&lon=89.3985084231931&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 125: city name => hamilton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.644674939851157&lon=-67.20505457370962&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 126: city name => touros
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.5983972612622637&lon=-27.649769117723025&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 127: city name => jackson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.114238387140176&lon=-110.25098900494419&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 128: city name => livingston
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.860579669319606&lon=-109.66644817677312&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 129: city name => lavrentiya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=76.7811580014457&lon=-168.80206164494132&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 130: city name => chuy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-66.02176899853418&lon=-20.400057116421237&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 131: city name => mrirt
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.84047487737527&lon=-4.909239504417485&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 132: city name => grand-santi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.4153695504355&lon=-53.6277556876766&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 133: city name => churachandpur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.416475180440344&lon=93.46009625769938&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 134: city name => pulheim
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.99118810258386&lon=6.834758606159141&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 135: city name => chudniv
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.252890478960836&lon=28.25649792970421&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 136: city name => flinders
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.23626068192272&lon=130.7612467856755&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 137: city name => marawi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.361571819698256&lon=26.646227265564562&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 138: city name => caarapo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.64851601716019&lon=-55.13148317556167&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 139: city name => trinidad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.26205803427793&lon=-104.44773777729506&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 140: city name => casa grande
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.600877978917424&lon=-112.14809145671258&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 141: city name => buala
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.6251138625645325&lon=159.95203275870915&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 142: city name => chake chake
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.193866540987926&lon=41.684946170440014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 143: city name => pangnirtung
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.63479369199882&lon=-59.80083962564812&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 144: city name => sept-iles
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.147290593361134&lon=-66.62861953598156&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 145: city name => la romana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.29975293463474&lon=-68.92861632843545&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 146: city name => cidreira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-50.00567619287211&lon=-25.86335250422195&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 147: city name => lata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.23055151694814&lon=168.90485329041002&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 148: city name => pennsauken
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.006728355367386&lon=-75.05706494073469&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 149: city name => galle
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.6460197702957657&lon=78.86470261405083&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 150: city name => nishihara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.726688665044293&lon=131.59215843000243&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 151: city name => kelvington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.836012319048194&lon=-103.58138737565216&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 152: city name => itanhaem
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.072518910960824&lon=-46.93393169899815&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 153: city name => naze
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.726207313763837&lon=134.99757824860632&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 154: city name => mezen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.96610723545493&lon=45.54506235258603&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 155: city name => shaunavon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.98497032436072&lon=-108.5740218123639&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 156: city name => lujiang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.335135210496887&lon=117.38988848883207&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 157: city name => pervomaysk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.85487493817175&lon=43.50242945544835&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 158: city name => seoul
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.44173417914476&lon=127.20168659319938&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 159: city name => hualmay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.238101955541268&lon=-90.47551997373303&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 160: city name => ponta do sol
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.625176809691197&lon=-37.423228436703965&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 161: city name => necochea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-48.43006765181612&lon=-56.081183270111424&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 162: city name => eastlake
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.68230096835197&lon=-81.45343271964356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 163: city name => vila do maio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.12157541087781&lon=-21.5755504040649&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 164: city name => adre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.461511043537442&lon=23.162142458488574&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 165: city name => asau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.524264916451514&lon=177.6775434785519&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 166: city name => rosario oeste
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.816633000012843&lon=-56.47723016309635&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 167: city name => camocim
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.6146828792280274&lon=-40.89711179995447&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 168: city name => mahibadhoo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.437168339727791&lon=69.87856490580688&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 169: city name => kruisfontein
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-50.425126652026954&lon=25.273421015217707&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 170: city name => uray
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.97909797917555&lon=65.29620482823901&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 171: city name => angoche
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.525719369781953&lon=40.33386172943793&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 172: city name => hervey bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.681821067607615&lon=157.40323605932156&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 173: city name => yambio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.736759483478323&lon=28.598786819382155&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 174: city name => pontianak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.9567669190392252&lon=108.80362635509925&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 175: city name => mar del plata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-60.22672056823287&lon=-34.619815467896956&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 176: city name => taonan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.64874154787958&lon=122.01329582769421&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 177: city name => provideniya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.619301129177614&lon=-177.9224936379835&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 178: city name => hithadhoo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.1592427850651745&lon=68.82108058863426&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 179: city name => tiksi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.12353675016013&lon=129.39100373059193&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 180: city name => dukat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.84060923486956&lon=154.524643158584&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 181: city name => airai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.60566001293992&lon=142.6024022505295&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 182: city name => ribeira grande
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.121734024909173&lon=-43.12692816337366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 183: city name => pisco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.409789845277786&lon=-82.05941589100952&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 184: city name => freeport
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.129267332005412&lon=-95.48566901704413&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 185: city name => palomares
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.265567074341277&lon=-95.3694509105025&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 186: city name => east london
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-63.54942578968445&lon=46.25266719872752&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 187: city name => svay rieng
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.319885517057628&lon=105.57804573899563&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 188: city name => tarrega
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.466920399142964&lon=1.1473059641456587&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 189: city name => boyolangu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.7363007844599&lon=111.87912847453288&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 190: city name => naral
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.323048387854286&lon=89.55486051777746&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 191: city name => hovd
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.018379401644324&lon=98.99679696100492&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 192: city name => mizdah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.536611031413855&lon=12.978166727913532&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 193: city name => castro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-44.574992312206035&lon=-103.7875524071335&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 194: city name => maceio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.518061386625192&lon=-31.16184732901789&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 195: city name => antofagasta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.591996252813445&lon=-69.59536027181852&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 196: city name => saraland
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.978212663008335&lon=-87.86318366141222&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 197: city name => bethel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.19041972923554&lon=-161.3689321389296&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 198: city name => ha giang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.63335150157907&lon=104.68554585364478&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 199: city name => hasaki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.828943232965244&lon=151.04454150447702&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 200: city name => saint-georges
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.387414829123301&lon=-47.76327936566241&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 201: city name => cuiluan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.64312848236557&lon=128.5086339067003&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 202: city name => umba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=66.8724741614935&lon=34.138203087981566&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 203: city name => ayan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.11744986885864&lon=140.6304135627421&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 204: city name => soyo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.455981454069061&lon=12.754533636397014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 205: city name => salalah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.13077258646466&lon=57.81367908498163&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 206: city name => upernavik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=80.72392981236507&lon=-52.00831394672757&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 207: city name => aswan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.15433226783064&lon=27.97575275542772&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 208: city name => husavik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.28761194789388&lon=-14.390539416956528&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 209: city name => namibe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.318475265385686&lon=6.28989634160115&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 210: city name => moranbah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.281814039702823&lon=146.883299531251&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 211: city name => bozeman
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.98059260180926&lon=-111.2075824923147&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 212: city name => halalo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.933808018816535&lon=-179.601450928674&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 213: city name => mubi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.319697323069548&lon=12.80865247529718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 214: city name => calvinia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.304488173987174&lon=20.11272611115973&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 215: city name => onega
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.25347998842503&lon=38.72641708787447&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 216: city name => trebinje
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.473860267500555&lon=18.54739060868033&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 217: city name => mount gambier
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-40.390256658014756&lon=139.23134289384893&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 218: city name => zeya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.20408897132509&lon=127.04258130284734&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 219: city name => saldanha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-35.929568419973954&lon=2.9473655130220493&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 220: city name => sovetskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=74.10746362820723&lon=67.83994859367519&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 221: city name => san mateo del mar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.13396983423624&lon=-94.94151059270168&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 222: city name => wanning
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.253281928291074&lon=113.12856947003667&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 223: city name => san quintin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.969268383613084&lon=-123.676042606143&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 224: city name => krasnogvardeyskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.042895921932285&lon=39.55152025616954&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 225: city name => melville
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.937363905568816&lon=-103.23665270043756&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 226: city name => mys shmidta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=72.42628443341235&lon=-178.4838739338338&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 227: city name => belawan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.345621410917474&lon=98.6449461809733&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 228: city name => astana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.88488931950215&lon=70.74547978335997&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 229: city name => ancud
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-42.46102647337265&lon=-99.4889565260953&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 230: city name => abu samrah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.633543105671308&lon=52.24850299982879&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 231: city name => lafiagi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.421798377014227&lon=5.60858664404779&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 232: city name => pangkalanbuun
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.7370001011163367&lon=110.55588724779966&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 233: city name => fort nelson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.99246352716361&lon=-122.12000030887016&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 234: city name => bonavista
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.73112174906632&lon=-49.27744061737087&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 235: city name => namie
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.199495749488335&lon=142.3438730560905&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 236: city name => aykhal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.37741217548157&lon=106.80419076220534&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 237: city name => aguilas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.86226243649858&lon=-1.55688507210769&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 238: city name => inhambane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.074503455619308&lon=37.04312088677909&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 239: city name => ugoofaaru
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.189286307307185&lon=61.715625106722655&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 240: city name => boddam
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.46788882971853&lon=-0.17537507961955612&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 241: city name => richards bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.041034140335306&lon=33.84497736008481&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 242: city name => marsh harbour
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.374066765603033&lon=-76.36164063121413&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 243: city name => parita
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.960550411400305&lon=-80.57429593729407&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 244: city name => lugovskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.48178812310968&lon=113.32081678006256&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 245: city name => attawapiskat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.78791915284589&lon=-79.67066455044423&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 246: city name => amderma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=89.90531041857093&lon=64.26410145385995&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 247: city name => norman wells
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.47312140828438&lon=-126.7683974605433&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 248: city name => ossora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.64908010028941&lon=163.1858613008498&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 249: city name => rehti
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.830399511243712&lon=77.3562579952739&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 250: city name => qaqortoq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.43366141980215&lon=-46.44890273818359&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 251: city name => rio tercero
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.368351969299646&lon=-64.52829127215627&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 252: city name => itum-kale
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.78462974412267&lon=45.343844861868746&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 253: city name => constitucion
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.731788034181406&lon=-126.54847293508544&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 254: city name => mahanoro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.43078324307109&lon=50.87322822022122&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 255: city name => bonthe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.8051728273476897&lon=-17.01543606129448&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 256: city name => port alfred
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-80.47698613750076&lon=52.363054755029395&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 257: city name => lompoc
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.109997864307132&lon=-131.90370134921562&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 258: city name => yuli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.865174076316876&lon=122.30906935836668&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 259: city name => malanje
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.300230793569796&lon=16.27640429201702&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 260: city name => mabaruma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.503569735330231&lon=-59.70949777262372&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 261: city name => labuhan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.069011380168476&lon=99.59032344269951&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 262: city name => alofi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.74412920733988&lon=-167.162274715544&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 263: city name => bar harbor
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.714087127086344&lon=-67.82935425023254&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 264: city name => henties bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.09829491507736&lon=5.034856798602277&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 265: city name => bud
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.1712183360969&lon=5.509358081560151&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 266: city name => kodiak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.685141670474735&lon=-154.19485305416586&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 267: city name => nevelsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.82361221153198&lon=140.4669868732057&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 268: city name => nago
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.66224477269847&lon=127.8891440497411&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 269: city name => puerto del rosario
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.359442374713424&lon=-12.89479998549632&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 270: city name => dwarka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.017526030894132&lon=65.71350052300303&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 271: city name => samarai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.840534292301243&lon=151.15902655086353&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 272: city name => pemagatsel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.999431145136825&lon=91.34589280392083&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 273: city name => carnduff
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.381710982635525&lon=-101.6050819487444&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 274: city name => talakan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.61684829578729&lon=130.78069469006078&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 275: city name => canatlan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.654101559475237&lon=-104.72313513579384&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 276: city name => san javier
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.2760568450784&lon=-62.878662138109604&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 277: city name => abiy adi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.56559185101483&lon=39.306728428445666&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 278: city name => goderich
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.571287535472379&lon=-18.70608202635296&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 279: city name => kayerkan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.06980535009859&lon=87.8927415583301&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 280: city name => geraldton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.05116930386731&lon=106.44843648942958&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 281: city name => wajir
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.6740515496155979&lon=39.29923995147462&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 282: city name => niscemi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.03608607226428&lon=14.386406612405153&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 283: city name => benghazi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.70429436278191&lon=18.90631894663389&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 284: city name => gryazovets
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.677079746804054&lon=40.35393461416234&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 285: city name => dawson creek
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.923265933074646&lon=-120.4024469816045&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 286: city name => rajshahi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.743478027744715&lon=88.66448621822985&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 287: city name => kolyvan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.13645792894039&lon=82.40250349238511&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 288: city name => karratha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.825641011228555&lon=115.50281694558277&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 289: city name => caraquet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.38857605252707&lon=-64.99884180119851&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 290: city name => itarema
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.86791458565763&lon=-35.042607904264884&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 291: city name => sergeyevka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.58538404239954&lon=66.68962800013455&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 292: city name => ondjiva
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.901141130417827&lon=16.159501587537704&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 293: city name => stephenville crossing
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.42756306689208&lon=-57.91792737775867&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 294: city name => vysokogornyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.41930820241009&lon=138.8602685529424&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 295: city name => shimoda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.193065837720454&lon=144.43894877172465&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 296: city name => tucuman
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.90117420997403&lon=-65.7115729086356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 297: city name => edeia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.528799350642842&lon=-49.901734742999366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 298: city name => kaitangata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-58.307403727792384&lon=176.26182532134698&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 299: city name => maningrida
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.340838273785593&lon=133.24440683366305&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 300: city name => iqaluit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.88835444712882&lon=-79.52881130085619&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 301: city name => kulunda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.27922760911687&lon=78.7923814450453&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 302: city name => clinton
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.68097105381614&lon=-89.97184761706806&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 303: city name => balabac
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.522886904634035&lon=116.97117597739333&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 304: city name => vardo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=82.87833538193061&lon=37.245005290439366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 305: city name => shache
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.903903077682784&lon=77.61539837539385&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 306: city name => verkhnyaya inta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.42351178888569&lon=60.373798982241965&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 307: city name => fernan-nunez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.70245756928176&lon=-4.72932985761156&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 308: city name => marau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.075734107959065&lon=-38.824100855419914&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 309: city name => akdepe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.94534352822237&lon=57.34760313028718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 310: city name => lagoa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.650147205030464&lon=-26.69209698486995&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 311: city name => parras
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.763727804442624&lon=-102.17982269809822&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 312: city name => belmonte
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.00266729107699&lon=-29.583073836883045&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 313: city name => khani
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.20706106410702&lon=121.8080792431337&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 314: city name => umzimvubu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-46.97088029085956&lon=43.69987608744901&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 315: city name => toliary
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.368190583372936&lon=43.40122519728206&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 316: city name => palmas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.4721808958475&lon=-51.90334422263214&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 317: city name => rafraf
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.12433235009938&lon=10.444329417870335&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 318: city name => salinopolis
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.7424754751834826&lon=-46.44075161849284&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 319: city name => samusu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.109486239723665&lon=-162.91685362715148&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 320: city name => sitka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.16760107919944&lon=-140.93663051228276&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 321: city name => corinto
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.483207994557503&lon=-88.21105400579219&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 322: city name => deer lake
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.78569913163568&lon=-57.70106433246224&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 323: city name => ketchikan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.18681872541464&lon=-132.47429655186286&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 324: city name => leningradskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=77.53651278839442&lon=175.83773329769832&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 325: city name => laguna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-35.2967481664825&lon=-37.99565700812789&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 326: city name => tawkar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.74203545953712&lon=38.248361433250096&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 327: city name => bambous virieux
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.356394694215794&lon=75.72882319238352&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 328: city name => ahipara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-36.35201393772164&lon=167.68751646288536&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 329: city name => takhtamygda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.17732910072945&lon=123.15266319809905&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 330: city name => ilam
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.77409126118121&lon=46.068371211047236&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 331: city name => chabahar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.35322752420332&lon=60.213249317808135&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 332: city name => saint-augustin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.20771431389048&lon=-58.37569060924292&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 333: city name => san patricio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.530425861278829&lon=-113.86612198234926&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 334: city name => havre-saint-pierre
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.44193265488164&lon=-64.38966633737739&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 335: city name => tapaua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.920685169916382&lon=-62.71083508463316&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 336: city name => nouakchott
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.33523567733907&lon=-18.43470404466663&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 337: city name => thunder bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.26066594210704&lon=-88.69787872377844&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 338: city name => miranda do corvo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.04816331522039&lon=-8.37432136712789&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 339: city name => lemnia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.11119597859229&lon=26.227349183894518&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 340: city name => klaksvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=75.00871683856576&lon=-6.26023732274777&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 341: city name => mbanza-ngungu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.109560953196322&lon=15.179047444749841&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 342: city name => kirillov
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.76638905591787&lon=39.008296911027685&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 343: city name => qaanaaq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=89.02732390818687&lon=-72.84139788436016&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 344: city name => namatanai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.056103078149846&lon=153.4287842099401&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 345: city name => angren
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.37439282296194&lon=70.28026225757108&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 346: city name => ambatofinandrahana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.56831768560258&lon=46.92166276869324&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 347: city name => poronaysk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.75244938076156&lon=145.8420975799603&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 348: city name => kaoma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.468138524373828&lon=25.503151618853366&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 349: city name => lolua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.185715378368926&lon=176.93876020573487&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 350: city name => rungata
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.5659862756681378&lon=176.78074035886436&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 351: city name => meyungs
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.015766640376555&lon=133.1131572246715&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 352: city name => shubarkuduk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.99564736005445&lon=56.2221321907399&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 353: city name => cockburn town
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.912780558300184&lon=-69.07245937809279&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 354: city name => kidal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.61393697485937&lon=2.377946097642024&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 355: city name => mitsamiouli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.941479373053099&lon=45.80771089094114&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 356: city name => tieli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.536215262825664&lon=128.33533864971776&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 357: city name => tuatapere
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-48.10177600340685&lon=157.88205755929528&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 358: city name => saint anthony
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.036429511704114&lon=-57.18912920167875&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 359: city name => lamut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.66015952956397&lon=121.17939759818927&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 360: city name => barentsburg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=83.8291184888295&lon=1.7050650180551088&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 361: city name => lasa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.588956063642883&lon=92.46556865117293&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 362: city name => plettenberg bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-43.60882128336029&lon=23.74702610914818&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 363: city name => paradwip
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.924061111797897&lon=88.67611231206564&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 364: city name => maniitsoq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.67995432776485&lon=-53.26925703642412&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 365: city name => high level
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.11639124109928&lon=-115.84385064685715&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 366: city name => ojinaga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.716289460187028&lon=-103.2208901604478&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 367: city name => te anau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-41.567112326052154&lon=161.69264198120567&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 368: city name => nizhneyansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=84.81007532159168&lon=135.16412857007418&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 369: city name => sao bento do una
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.691752142159217&lon=-36.44034503356548&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 370: city name => kibaek
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.960158301672806&lon=8.879439553785431&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 371: city name => tecoanapa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.179781434181493&lon=-99.53636894679472&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 372: city name => quixada
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.914911852202081&lon=-38.88836615457831&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 373: city name => kavaratti
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.072072329368837&lon=70.78547823686853&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 374: city name => kangaba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.55200168486553&lon=-8.174517709824727&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 375: city name => margate
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-43.776649416139875&lon=41.77708167707334&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 376: city name => port macquarie
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.696737577422326&lon=155.35011060795586&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 377: city name => amga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.75289577279497&lon=132.7620280928474&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 378: city name => verkhoyansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.89842645475565&lon=133.5470032867279&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 379: city name => haines junction
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.73933304076587&lon=-137.7071990314996&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 380: city name => nyurba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.27964169074065&lon=116.69352613555355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 381: city name => vila franca do campo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.29101460111467&lon=-23.12422657482452&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 382: city name => manyana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.24621676115683&lon=21.040057567621886&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 383: city name => vieste
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.141369626053745&lon=16.891867425472043&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 384: city name => porbandar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.08711988233287&lon=64.66170413578322&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 385: city name => dharmavaram
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.395957265112557&lon=77.50555070098545&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 386: city name => tazovskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.29365578613016&lon=80.54164173581046&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 387: city name => tubruq
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.71331091636843&lon=24.638631409558286&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 388: city name => carauari
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.249566192159108&lon=-67.61799060013037&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 389: city name => dunedin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-50.10000132473814&lon=175.66171455361103&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 390: city name => fairbanks
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.56800318836304&lon=-142.0193279307216&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 391: city name => bozoum
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.820024450193287&lon=16.732688270806932&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 392: city name => taksimo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.856276618076635&lon=114.5657189563845&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 393: city name => alice springs
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.53217993309802&lon=136.744086815804&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 394: city name => luoyang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.052850067213654&lon=112.21718014196&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 395: city name => malakal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.504662536523611&lon=31.083688524186726&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 396: city name => crestview
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.240482919109425&lon=-86.6024123138354&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 397: city name => longyearbyen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=87.33261620053096&lon=18.249704283126107&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 398: city name => tibagi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.745246196530317&lon=-50.363479541402256&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 399: city name => ponferrada
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.34655752034132&lon=-6.533587146981176&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 400: city name => kanda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.795466699593504&lon=131.11703640037877&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 401: city name => roma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-27.952736700280525&lon=148.89965175787802&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 402: city name => padang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.4438186497319805&lon=95.77929973745904&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 403: city name => dzilam gonzalez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.662231133141148&lon=-89.2523344631868&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 404: city name => mahebourg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-41.64201392513091&lon=73.79395122853109&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 405: city name => muisne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=1.3824451483084204&lon=-82.15765735891677&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 406: city name => riyadh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.959299353997196&lon=46.557320957233514&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 407: city name => jalu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.855515629115388&lon=23.23134199861198&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 408: city name => haldwani
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.09084594026318&lon=79.61257090910789&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 409: city name => buena vista
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.45714070064585&lon=-63.60171625429783&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 410: city name => honningsvag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=72.60935558117993&lon=26.026882913001515&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 411: city name => fare
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.825216613505503&lon=-149.21427637486448&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 412: city name => parrita
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.302367370295116&lon=-84.6587204639165&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 413: city name => sembakung
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.635927373608169&lon=115.68938795767332&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 414: city name => pevek
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=86.90387345650672&lon=167.1212563835836&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 415: city name => qasigiannguit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.95478463125977&lon=-46.86216740537313&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 416: city name => evans
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.83729851613961&lon=-82.27969632971882&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 417: city name => jasper
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.11425672035645&lon=-117.66963448264627&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 418: city name => vila velha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.631414397605987&lon=-25.601786741286332&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 419: city name => berlevag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.505264958825&lon=29.20984697032469&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 420: city name => khovu-aksy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.044522705284606&lon=93.96967051697186&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 421: city name => ayagoz
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.51546059130868&lon=79.98787068476179&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 422: city name => beidao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.78572449717058&lon=106.10203539602111&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 423: city name => galia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.11238775196513&lon=24.810921115925396&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 424: city name => stephenville
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.27004996824782&lon=-98.6711922009409&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 425: city name => achit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.92590563185831&lon=57.66725971616478&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 426: city name => huangchuan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.30826013595728&lon=115.28471569383424&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 427: city name => worthington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.35019549874656&lon=-95.64822869872575&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 428: city name => zolochiv
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.28354565008658&lon=36.0588109879325&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 429: city name => nara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.217629007999093&lon=-7.190432809921276&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 430: city name => puerto pinasco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.617845066558274&lon=-58.39944385312822&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 431: city name => luis correia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.14034725184993135&lon=-41.262414707299286&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 432: city name => sabang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.252029198968273&lon=93.11294825258216&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 433: city name => inuvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.73560121709562&lon=-132.99772427733427&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 434: city name => dharchula
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.84252476940067&lon=81.73670047827954&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 435: city name => havoysund
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=73.7136267477403&lon=24.207285803122602&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 436: city name => mount isa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.404649005550795&lon=141.94683095270062&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 437: city name => bagan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.22521052312405&lon=78.08206221030986&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 438: city name => moerai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.812944487439353&lon=-150.46941890766&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 439: city name => avera
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.952366605880755&lon=-157.37347890578718&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 440: city name => sentyabrskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.87834052025315&lon=158.38304515043177&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 441: city name => kassala
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.05325277872791&lon=35.991993033126704&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 442: city name => stillwater
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.44095640543941&lon=-92.27359767638389&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 443: city name => altamira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.6074425377049693&lon=-52.90759040789918&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 444: city name => puerto ayacucho
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.408580412991526&lon=-64.67865130386038&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 445: city name => rawson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-49.342819605530096&lon=-60.29344436366513&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 446: city name => chagda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.3517673757687&lon=131.55846629824202&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 447: city name => stutterheim
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.62160704910786&lon=27.691799469268176&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 448: city name => dalby
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.84761346348683&lon=150.964225216108&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 449: city name => clyde river
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.83060301057179&lon=-84.45871340272515&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 450: city name => pangai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.690447313306166&lon=-172.78603195995206&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 451: city name => ixtapa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.899269907166598&lon=-103.80694957904319&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 452: city name => hami
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.13291077386904&lon=91.6045863647754&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 453: city name => kedrovyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.109879656568864&lon=78.82239683856949&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 454: city name => sao filipe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.399001196678427&lon=-31.879133879060362&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 455: city name => pacific grove
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.587903536344342&lon=-129.76129053674185&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 456: city name => varhaug
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.20689734778372&lon=2.3606300542967347&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 457: city name => mudyuga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.30706579802387&lon=38.982628883821036&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 458: city name => tual
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.385670570672133&lon=131.428981320623&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 459: city name => pozo colorado
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.218650471376193&lon=-58.6081498572089&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 460: city name => veshenskaya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.61447211000842&lon=41.58423188177946&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 461: city name => chapais
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.920882329708576&lon=-75.62409748647045&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 462: city name => phan thiet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.596464432042652&lon=107.76507966056352&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 463: city name => charyshskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.34075781333138&lon=83.71308764890489&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 464: city name => nsukka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.101108620635202&lon=7.238449635783098&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 465: city name => mataram
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.047368159420003&lon=115.97743818658495&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 466: city name => tabou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.763318187515651&lon=-6.861522563580877&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 467: city name => dingle
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.92128502973068&lon=-16.27696108865618&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 468: city name => mecca
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.451920445958578&lon=40.897296547292825&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 469: city name => kupang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.395676525853517&lon=123.68738213946375&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 470: city name => donskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.34578828937103&lon=19.397622672510096&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 471: city name => sur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.44823368052768&lon=62.00681799158687&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 472: city name => narsimhapur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.11066953891185&lon=79.22261336778251&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 473: city name => palabuhanratu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.502990415566742&lon=106.17532980348415&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 474: city name => itanhem
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.183004993726485&lon=-40.57834524747386&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 475: city name => gornyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.64429081313051&lon=136.18145079437943&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 476: city name => mangalagiri
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.438609859552813&lon=80.57127040984409&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 477: city name => anqing
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.429402544425358&lon=116.90821503523921&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 478: city name => pospelikha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=51.915456954834866&lon=81.6064153191208&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 479: city name => turkistan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.440133653990756&lon=67.4026915334843&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 480: city name => zhanaozen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.196661476386595&lon=53.796415648612026&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 481: city name => atambua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.746875393877602&lon=126.9622065579889&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 482: city name => gull lake
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.17457026061885&lon=-108.55657964445889&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 483: city name => thyolo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.364182519936875&lon=35.00673307919945&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 484: city name => mahwa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.088945411347524&lon=76.96870527226332&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 485: city name => aranos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-24.561129796664105&lon=19.34063204447881&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 486: city name => warmbad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.68605103072055&lon=18.837355438670272&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 487: city name => naziya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.880533370852646&lon=31.271010122393903&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 488: city name => barcelona
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.57812187303449&lon=127.2025087968043&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 489: city name => byron bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.93489261088105&lon=155.7604023970792&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 490: city name => luderitz
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-35.644562423873275&lon=-1.656029438937594&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 491: city name => kiunga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.734782620561916&lon=139.2329673254332&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 492: city name => vicksburg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.46084212667819&lon=-90.72439975747&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 493: city name => saqqez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.84045410181072&lon=46.70560325601244&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 494: city name => yumen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.34684424773327&lon=94.04637781491095&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 495: city name => falealupo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.853615798642267&lon=-174.16435940391153&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 496: city name => babu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.34768349821762&lon=111.70717952832695&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 497: city name => dubbo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.396925447149506&lon=145.5498272062925&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 498: city name => teguldet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.53002286399504&lon=87.80344100959923&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 499: city name => port hardy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.559802079644584&lon=-127.58147180021969&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 500: city name => songjianghe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.42321783063542&lon=127.35911844415642&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 501: city name => sioux lookout
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.60733535460514&lon=-92.55937138243394&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 502: city name => oktyabrskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.346530133394396&lon=66.66704634269783&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 503: city name => san cristobal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.855918027993823&lon=-86.84991854062554&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 504: city name => vuktyl
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.43728997912132&lon=58.5747108954061&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 505: city name => crotone
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.113345794763035&lon=17.455721514544194&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 506: city name => sisimiut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.97533202339014&lon=-59.47884723522314&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 507: city name => kaya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.757668522561744&lon=-0.9704806624450555&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 508: city name => kirakira
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.818289812562995&lon=161.25986407884278&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 509: city name => santo antonio do ica
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.4589588795809334&lon=-67.62276312798208&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 510: city name => gbadolite
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.668601380082336&lon=20.959603373703942&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 511: city name => tecpan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.55804695277304&lon=-102.55087537462288&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 512: city name => dese
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.320158670791486&lon=39.707268722447736&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 513: city name => machico
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.585291030954295&lon=-16.700217817414824&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 514: city name => firminy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.0514511747098&lon=4.106597140071273&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 515: city name => gao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.034568703423034&lon=0.012754951107496026&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 516: city name => coquimbo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.635951310914095&lon=-87.22789742825066&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 517: city name => marataizes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.36231359875157&lon=-40.45324017732514&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 518: city name => baljevac
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.475306896327425&lon=20.55748027399045&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 519: city name => muika
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.16423576796663&lon=139.25294025368953&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 520: city name => lebu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-32.876157348732995&lon=-82.35494533782257&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 521: city name => broken hill
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.854067130672078&lon=142.39733644577416&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 522: city name => korla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.736414823312714&lon=89.70015536041996&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 523: city name => namwala
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.660737832274549&lon=25.920290795798735&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 524: city name => grindavik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.64122987344467&lon=-30.089833056829065&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 525: city name => alexandria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=29.46019210274784&lon=28.71416355530468&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 526: city name => jaisalmer
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=26.90817323637728&lon=70.6607225503598&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 527: city name => barao de melgaco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.158043087859937&lon=-56.20016708530326&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 528: city name => derbent
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.296597851413935&lon=48.42955179432974&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 529: city name => moose factory
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.48547907116378&lon=-80.15666089165502&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 530: city name => ahuimanu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.0626367348092&lon=-151.50446421905582&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 531: city name => cabedelo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.906244807889465&lon=-32.389088758669686&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 532: city name => leshukonskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.86183420910868&lon=47.48034642436534&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 533: city name => urucara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.1722797356750476&lon=-58.87507820300536&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 534: city name => utete
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.21333968533503&lon=38.75005052628427&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 535: city name => georgiyevka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.378827816025506&lon=74.7581094158094&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 536: city name => pinega
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.06742115931314&lon=43.02152456318663&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 537: city name => imaculada
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.432946104750087&lon=-37.42951672342406&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 538: city name => seoni malwa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.305668016701034&lon=77.33374958975867&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 539: city name => schwalmstadt
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.96814712449941&lon=9.274392939904857&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 540: city name => kantunilkin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.285701833512107&lon=-87.48217039263581&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 541: city name => bridlington
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.19587674063354&lon=2.1388690249323474&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 542: city name => kroya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.480617530113918&lon=108.59153216399278&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 543: city name => hearst
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.76100830418335&lon=-83.89818081984265&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 544: city name => north platte
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.813519650753506&lon=-101.2280055066645&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 545: city name => tezu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.282842775995476&lon=97.190523702772&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 546: city name => lincoln
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.7698515551886&lon=-62.79645100828468&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 547: city name => scarborough
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.996256883269055&lon=-60.29686723006452&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 548: city name => solnechnyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.14276335126877&lon=139.68404013660853&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 549: city name => yuzhou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.50607201690306&lon=113.79733172924398&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 550: city name => the pas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.025300838590226&lon=-101.15031511498908&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 551: city name => tilichiki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.35724809020141&lon=165.77933029064963&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 552: city name => bongor
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.927617370950273&lon=15.57792996300364&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 553: city name => hailey
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.96294522005849&lon=-113.36966823964387&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 554: city name => axim
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.372693264407488&lon=-3.8290638822435312&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 555: city name => east retford
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.229291908503285&lon=-0.8114376051706529&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 556: city name => emerald
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.278987207038014&lon=145.87528507087603&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 557: city name => tres arroyos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-38.284310433722275&lon=-59.65588960419831&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 558: city name => sorvag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.068339361183234&lon=-8.989121916001835&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 559: city name => santa gertrudes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.463395886950465&lon=-47.53904128518607&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 560: city name => oksfjord
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.50123866764235&lon=22.143729836592144&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 561: city name => umm lajj
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.68273802073655&lon=36.457452063361046&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 562: city name => biltine
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.346499705494764&lon=19.244057372331895&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 563: city name => krasnoselkup
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.71620645516802&lon=81.52550303591613&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 564: city name => ambanja
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.791413538666887&lon=47.19416753160937&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 565: city name => zyryanka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.17690579542835&lon=149.85578855383085&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 566: city name => kirkwall
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.78694954363178&lon=-2.95842257888836&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 567: city name => thinadhoo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.8942974934532373&lon=64.9859812636279&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 568: city name => saint albans
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.851273046702744&lon=-73.06287507431875&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 569: city name => athabasca
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.156073630449754&lon=-111.84505460918727&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 570: city name => omboue
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.730902690731753&lon=3.1598306351663155&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 571: city name => bergen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.42911872598009&lon=13.683527681586185&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 572: city name => yulara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-25.629993279051206&lon=131.48294169548183&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 573: city name => petropavlovsk-kamchatskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.79324403472745&lon=161.8490170942341&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 574: city name => port-gentil
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.3263336161751198&lon=3.525708216826928&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 575: city name => invermere
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.12542528953293&lon=-116.40314526891032&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 576: city name => porto santo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.466903317469786&lon=-14.6103684575653&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 577: city name => saint-francois
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.147352106252384&lon=-53.70363466632699&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 578: city name => yarada
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.736447059054328&lon=85.73097726550162&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 579: city name => inderborskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.72527559443637&lon=50.466580501051425&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 580: city name => mogadishu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.3357748934352287&lon=50.45285724098639&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 581: city name => malwan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.643531377318013&lon=70.21021189159015&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 582: city name => ust-kamchatsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.833585650229026&lon=162.34856762777162&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 583: city name => emba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.50346337231829&lon=58.39160925669998&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 584: city name => akyab
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.27519500760846&lon=90.40970168432705&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 585: city name => atasu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.212022479381886&lon=71.98849538840471&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 586: city name => gamba
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.5163614036670197&lon=8.358881691612709&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 587: city name => manado
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.5137262961377473&lon=123.55207100905182&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 588: city name => aguie
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.502554661253797&lon=7.733648644027795&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 589: city name => burkburnett
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.212871643758376&lon=-98.4387339763563&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 590: city name => algodones
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.95460431584243&lon=-114.99271507594379&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 591: city name => pindiga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.77925218668949&lon=11.008335923054545&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 592: city name => shima
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=24.037477984572746&lon=117.8130255253136&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 593: city name => ostrovnoy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=75.13799799619744&lon=39.79836768050009&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 594: city name => saldus
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.46648249265917&lon=22.632118761677276&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 595: city name => pasighat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.08862586432673&lon=96.39075195061508&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 596: city name => port-cartier
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.74480550576672&lon=-67.50225798197168&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 597: city name => komsomolskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.00769587376462&lon=173.59764400244165&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 598: city name => kudat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.51694463336257&lon=117.37514331123776&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 599: city name => boende
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.882460920959943&lon=20.196985579959062&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 600: city name => gospic
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.7308998302324&lon=15.42263556107423&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 601: city name => zion
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.43939994281027&lon=-87.30674331383909&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 602: city name => kamenskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.65035576906328&lon=166.22966561223114&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 603: city name => bargal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.94669525936824&lon=55.52899787988602&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 604: city name => camacha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.09009152907409&lon=-15.428154836670132&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 605: city name => shepsi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.73690459842126&lon=38.89032236402386&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 606: city name => arlon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.65389773919131&lon=5.786973748175427&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 607: city name => qixingtai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.033357729586925&lon=111.91441834052245&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 608: city name => ozinki
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.67487247079015&lon=50.38398053903481&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 609: city name => de aar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.396948707409784&lon=23.2619496184754&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 610: city name => ostersund
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.29212850639658&lon=16.34252172248125&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 611: city name => bageshwar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.176087054930875&lon=79.7524444579301&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 612: city name => aksarka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=72.26560197724635&lon=68.51701324454964&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 613: city name => nagornyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.79828501022959&lon=83.52478678222838&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 614: city name => meulaboh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.1659761445182966&lon=90.79648238198803&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 615: city name => murgab
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=37.991371048366915&lon=61.16718623469038&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 616: city name => ca mau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.438442491936271&lon=104.30981763808961&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 617: city name => montepuez
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.807332734248163&lon=39.819440913236036&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 618: city name => broome
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.401054821230375&lon=123.4990600667274&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 619: city name => praia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.488872045762434&lon=-23.32210801580328&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 620: city name => taipu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.47967330848725&lon=-35.585039229911445&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 621: city name => san lorenzo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.05654879248862&lon=-58.90760824437541&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 622: city name => tommot
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.316319700762875&lon=126.15641409722355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 623: city name => lorengau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.398165100314657&lon=149.915265882484&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 624: city name => conceicao da barra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.057046137843685&lon=-36.66238819632994&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 625: city name => ubinskoye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.90432755317596&lon=79.93609424683069&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 626: city name => sulina
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.33153078557831&lon=30.254603932094767&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 627: city name => schwandorf
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.31302512409928&lon=12.057466782255972&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 628: city name => reconquista
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.888833490903117&lon=-60.451736849932416&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 629: city name => maldonado
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-38.83151526421097&lon=-53.33251640748176&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 630: city name => salento
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.7233305681370155&lon=-75.41503786047073&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 631: city name => bardiyah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.19102183515044&lon=25.458788316802668&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 632: city name => bulgan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.032778140589926&lon=103.37224979638523&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 633: city name => rincon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.637565664426745&lon=-67.75355038973493&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 634: city name => the valley
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.84724875228416&lon=-61.65271183283269&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 635: city name => lebedinyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.338168875918086&lon=125.66418585538736&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 636: city name => bolungarvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.20926464612995&lon=-26.387166571074744&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 637: city name => nouadhibou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.20868697992921&lon=-22.51521049356208&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 638: city name => nantucket
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.864864350378525&lon=-69.46197135639962&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 639: city name => borama
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.70830117782559&lon=42.7727048236454&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 640: city name => zabol
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.435421124796463&lon=61.063397537210676&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 641: city name => natal
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.5465434460281955&lon=-27.40817663322798&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 642: city name => gogapur
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.494744177604375&lon=75.738614360378&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 643: city name => umm kaddadah
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.659009675123116&lon=27.42016954242399&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 644: city name => mount pleasant
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.003458129306225&lon=-78.82277768676377&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 645: city name => zomin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.80032034052263&lon=68.3915825074819&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 646: city name => nemuro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.04478484607688&lon=153.59812497608527&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 647: city name => bolobo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.797006607614435&lon=15.678999490842159&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 648: city name => severo-yeniseyskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.71208731740916&lon=92.90258509950581&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 649: city name => lithgow
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-33.92337076854146&lon=149.92552276830622&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 650: city name => acapulco
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.978333638235554&lon=-103.65014575844137&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 651: city name => diffa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.346676144985338&lon=12.833265084565625&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 652: city name => icatu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.7472218769300127&lon=-43.82816764724777&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 653: city name => shellbrook
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.188716284442904&lon=-106.81241033094945&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 654: city name => nyakahanga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.6836629064918185&lon=31.198036824739972&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 655: city name => macau
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.313108425652089&lon=-36.23221944733962&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 656: city name => mafinga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.820043085276637&lon=34.4396812852236&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 657: city name => perth
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.999530411933257&lon=116.13615923830145&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 658: city name => riachao das neves
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.787143489134834&lon=-45.1981683677723&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 659: city name => charters towers
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.542701966578406&lon=144.06860448736973&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 660: city name => turayf
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.456599588377472&lon=38.44958899201234&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 661: city name => shelburne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.505028627153536&lon=-64.45987579106189&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 662: city name => cutzamala
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.466329904460167&lon=-100.5458083535659&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 663: city name => vestmannaeyjar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=58.00660078769113&lon=-21.24074021811569&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 664: city name => gizo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.878919278703179&lon=157.19579309993918&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 665: city name => hambantota
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-5.4902512532212455&lon=86.21575249996539&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 666: city name => waingapu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-10.229271232386253&lon=121.50338566764958&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 667: city name => patiya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.994357657761967&lon=91.75200309783742&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 668: city name => praia da vitoria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.031306087893995&lon=-21.293490053298257&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 669: city name => viligili
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-0.9603891397515412&lon=78.31372857061439&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 670: city name => monroe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.850533277119865&lon=-83.34865527683235&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 671: city name => kayes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.876291675035233&lon=-11.68613729797724&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 672: city name => kamenka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=72.14270397750292&lon=44.767205344854744&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 673: city name => olivet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.57387453777048&lon=1.9927785900019046&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 674: city name => poya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.912973698315213&lon=163.3095059122531&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 675: city name => kuryk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=42.26254995568357&lon=51.46458689875155&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 676: city name => kushiro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.46402353124029&lon=144.4859132339247&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 677: city name => viedma
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-43.186375231379735&lon=-61.013705122150455&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 678: city name => deputatskiy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=69.94455560592283&lon=138.6329200729749&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 679: city name => benguela
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-12.239042159493707&lon=11.748105629398253&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 680: city name => cockburn harbour
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.761451264310708&lon=-71.5409641121267&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 681: city name => chalmette
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.561630353570365&lon=-89.0761561309713&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 682: city name => vostok
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.60598732220876&lon=150.56846757122247&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 683: city name => recreio
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.61247815181804&lon=-42.54769693293733&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 684: city name => kjopsvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=68.11405348396394&lon=16.433451670144024&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 685: city name => hue
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.392231590128404&lon=108.12011110230276&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 686: city name => libertador general san martin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-23.186681248902943&lon=-65.22134231339354&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 687: city name => carutapera
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.142550551449375&lon=-44.06562215490115&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 688: city name => ariquemes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.818192199184423&lon=-62.54848928943538&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 689: city name => santa isabel
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.250251347385685&lon=-113.22456444401527&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 690: city name => port blair
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.925377824269262&lon=89.17593653306255&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 691: city name => estacion coahuila
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=32.22598133106395&lon=-114.94693337287539&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 692: city name => karpathos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.77907904754613&lon=28.269855692914746&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 693: city name => ucluelet
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.984795391826395&lon=-128.0932882491593&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 694: city name => dingtao
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.74282166150256&lon=115.30394710969625&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 695: city name => jazzin
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=33.55084981030558&lon=35.5918975781355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 696: city name => pokanayevka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.67929196745385&lon=97.74272284059504&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 697: city name => zafra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.21651768913739&lon=-6.264724075755453&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 698: city name => marcona
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.386023277058356&lon=-77.83067023785686&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 699: city name => ballina
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-30.9117480230352&lon=158.26821635929656&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 700: city name => santiago del estero
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-28.97604662647742&lon=-64.03754080543037&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 701: city name => vestbygda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.77391048475462&lon=6.381341809250898&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 702: city name => kunyang
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.634746376114137&lon=120.64035592115425&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 703: city name => yenagoa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.8379432050362681&lon=4.011683567732689&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 704: city name => svetlyy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.56733408002381&lon=60.71130445677136&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 705: city name => karaul
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.35967556313369&lon=83.36037780127367&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 706: city name => olafsvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.65163961561879&lon=-27.127210620288224&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 707: city name => kampene
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.701957323977595&lon=26.75317316822887&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 708: city name => jining
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.98366759491498&lon=113.69694083223203&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 709: city name => guerrero negro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=17.886700225524407&lon=-127.01311439882844&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 710: city name => novoagansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.08128494030203&lon=77.93051911347698&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 711: city name => patnos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.52477996934138&lon=42.674860073256895&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 712: city name => xai-xai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.596435284450557&lon=34.09910232450517&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 713: city name => esmeraldas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=2.1648689630631566&lon=-80.91746991592986&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 714: city name => ust-kuyga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=70.67227853205804&lon=135.76617195731149&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 715: city name => klyuchi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=55.60618633190407&lon=161.31931230312125&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 716: city name => hofn
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.31832097606733&lon=-12.565059820736934&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 717: city name => kontagora
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.51592431105803&lon=5.65927649395303&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 718: city name => wabana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.69928722061806&lon=-52.98308307426812&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 719: city name => goalpara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.865905872539344&lon=90.4787598848526&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 720: city name => makakilo city
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.643230000395292&lon=-164.65635255364396&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 721: city name => sechura
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.102187347998822&lon=-85.4516750392607&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 722: city name => najran
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.019531635911335&lon=45.36706477994434&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 723: city name => ornskoldsvik
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.84558108097846&lon=19.244719586836936&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 724: city name => palana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.53088789007418&lon=160.41392641808187&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 725: city name => noyabrsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.91811168176153&lon=75.15885509758922&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 726: city name => hay river
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.02772970962937&lon=-114.76736858326565&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 727: city name => mercedes
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-34.30072329923663&lon=-65.79751712296145&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 728: city name => elbistan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.4672049378151&lon=37.6592868851896&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 729: city name => mallapuram
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.886885963534624&lon=78.37637356146769&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 730: city name => kerteh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.29739281459409&lon=105.86975736331527&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 731: city name => malindi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.862989522522483&lon=39.858315430710434&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 732: city name => saint austell
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.25293202808896&lon=-4.548690934329812&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 733: city name => karauzyak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.45399094427316&lon=59.36174308253075&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 734: city name => roebourne
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-20.946270130030072&lon=117.39875100113557&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 735: city name => poum
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.643334200560858&lon=160.7810502759002&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 736: city name => osypenko
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.985433406627635&lon=36.47432364571853&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 737: city name => mgandu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-6.839729403746546&lon=33.46972599936902&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 738: city name => prokopion
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=38.692853356110305&lon=23.515296960614023&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 739: city name => gushikawa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.179804699755422&lon=134.10095764390297&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 740: city name => banyo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.313801920036383&lon=11.328968782966228&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 741: city name => seddon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-41.3139360155183&lon=174.33850042038472&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 742: city name => floro
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.94178160228091&lon=3.5175422456491106&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 743: city name => tay ninh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.173232044055013&lon=106.28708088531374&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 744: city name => caldas novas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.89271326950707&lon=-48.52250897752742&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 745: city name => corlu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.21585248200674&lon=27.855521826081855&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 746: city name => orshanka
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.79330403829704&lon=48.12272294963918&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 747: city name => raudeberg
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.9352956819557&lon=2.677488674213606&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 748: city name => hauterive
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.05996082730644&lon=-70.51960506648179&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 749: city name => xuanhua
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.84887211198449&lon=115.74553220448337&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 750: city name => barabai
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.5227004470872316&lon=116.41495918370669&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 751: city name => ginda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.367623392129047&lon=39.982769524118254&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 752: city name => sorong
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.34773565953058494&lon=131.5110428593776&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 753: city name => ust-kut
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.17749534332722&lon=106.33248687578617&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 754: city name => palu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.818726168144181&lon=118.77119208800707&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 755: city name => khandyga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.07482952005509&lon=136.38547902092608&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 756: city name => okhotsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=61.94062044631852&lon=142.03723497365132&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 757: city name => seminole
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=27.163837127367557&lon=-84.60689799542585&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 758: city name => liapades
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.24844703555837&lon=19.318119371771928&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 759: city name => uchiza
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.29135853077193&lon=-76.32904151574009&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 760: city name => celestun
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=21.769609301519665&lon=-92.9423395081729&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 761: city name => fayaoue
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.585329866104743&lon=166.27453995260237&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 762: city name => skelleftea
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=64.99998834601328&lon=20.099106467913515&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 763: city name => sokolo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.59793334991997&lon=-6.156787739049122&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 764: city name => port shepstone
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-36.0155475076579&lon=38.23067776240762&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 765: city name => ngukurr
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.210566392637986&lon=137.0564319135355&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 766: city name => san andres
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=12.295044040663342&lon=-80.75295136664235&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 767: city name => diu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.979863663815465&lon=71.00866396355693&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 768: city name => san miguelito
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.520350570174585&lon=-85.33831459836455&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 769: city name => lahat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.33791998980675&lon=103.13059639952951&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 770: city name => strezhevoy
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=59.082139614523896&lon=77.81532752792975&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 771: city name => kuche
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=36.71256588026223&lon=83.55201658661969&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 772: city name => brestovat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.95198278931329&lon=21.67437628728493&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 773: city name => kurilsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.828140193424616&lon=147.3088846911258&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 774: city name => yatou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.8026932162458&lon=124.50680505335362&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 775: city name => gondanglegi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.704009009114884&lon=111.82836159860443&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 776: city name => esso
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.535392899140646&lon=156.78005708974848&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 777: city name => verkhnevilyuysk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=62.0603596327517&lon=121.02858635732565&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 778: city name => monywa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.425466018567505&lon=94.66072359290979&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 779: city name => matagami
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=50.84630284622111&lon=-76.83142987072064&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 780: city name => sarankhola
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.723863129884435&lon=89.76986923918889&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 781: city name => makinsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.69363772544676&lon=69.61620733221844&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 782: city name => itambe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.09036032185979&lon=-40.733139907833305&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 783: city name => uwayl
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.283765091482351&lon=27.097777745144782&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 784: city name => adrar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=25.445469140777192&lon=0.8447515365653828&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 785: city name => tarko-sale
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.24325486360891&lon=78.71057238879393&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 786: city name => kobojango
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-21.85944229538825&lon=29.250560172294797&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 787: city name => cumaribo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.2750886040100795&lon=-70.16551347303624&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 788: city name => isabela
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.626170548414223&lon=-67.02819925174236&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 789: city name => gurgan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.6124667773982&lon=51.84785909954556&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 790: city name => pandan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=14.93082462140734&lon=124.86089705428822&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 791: city name => zhezkazgan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=48.258835856722584&lon=65.38914956987577&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 792: city name => lazaro cardenas
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=10.776825333535982&lon=-106.65058736538694&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 793: city name => nelson bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-35.54461361685235&lon=156.73145119873794&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 794: city name => grand-lahou
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=4.873295328059882&lon=-4.957607703646914&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 795: city name => ilulissat
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=87.13130785216748&lon=-39.62843103418595&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 796: city name => jawa
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.645950386021127&lon=36.56543236772393&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 797: city name => jiazi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=20.086514598348785&lon=117.51421972571524&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 798: city name => champerico
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=11.91098200721045&lon=-93.15258659498737&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 799: city name => opuwo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.577045037177342&lon=11.91281686309398&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 800: city name => sherbakul
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.73972790579572&lon=72.09920334518404&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 801: city name => kutum
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=18.383816625994&lon=24.384889297494766&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 802: city name => fethiye
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=35.703286125185045&lon=29.168969093322858&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 803: city name => mehamn
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=80.82133286961522&lon=27.19562360648527&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 804: city name => neiafu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-18.366269364025314&lon=-176.01311381358207&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 805: city name => talnakh
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=71.81557190833303&lon=92.88534722756282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 806: city name => pecos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.952421963449353&lon=-103.28722517202219&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 807: city name => galiwinku
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-9.768327916542802&lon=135.10415912227847&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 808: city name => cururupu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.6326280518854475&lon=-43.240235728175236&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 809: city name => kloulklubed
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.7018364090965292&lon=133.14040750728753&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 810: city name => balkhash
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.958063952464244&lon=74.79556163869842&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 811: city name => nuevo progreso
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=23.441460752758857&lon=-96.25898044413506&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 812: city name => guaruja
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-26.41348548266057&lon=-45.64543856155501&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 813: city name => aripuana
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-8.948532380841456&lon=-61.258005428559954&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 814: city name => comodoro rivadavia
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-48.156262153546066&lon=-67.76418135903496&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 815: city name => grootfontein
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-19.16367741583025&lon=18.80377412957401&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 816: city name => along
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.731547707509733&lon=95.2672913168168&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 817: city name => zhigansk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=67.67443087593657&lon=123.88654218547623&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 818: city name => sampit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.7652788647464064&lon=113.672313315915&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 819: city name => matara
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.694124974817925&lon=83.36400402660593&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 820: city name => bongandanga
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=0.9793939759242676&lon=20.594010964294824&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 821: city name => barawe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-2.7817873834814435&lon=47.7741214553422&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 822: city name => agadir
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.246227870375208&lon=-10.252933588748874&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 823: city name => teya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=63.31479546215081&lon=92.5908489188991&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 824: city name => camopi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.6218585747178764&lon=-52.60345838879981&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 825: city name => biak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-1.7628578266816959&lon=135.31405828340098&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 826: city name => kununurra
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-15.875603365232095&lon=129.1414316348684&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 827: city name => siuna
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=13.056870199825724&lon=-84.36352569689282&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 828: city name => garowe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.561790577942759&lon=48.50535819899497&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 829: city name => ust-uda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.49130760266317&lon=102.77473126450303&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 830: city name => ozgon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=41.20979976236151&lon=73.10046720335748&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 831: city name => cepu
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-7.280117394118491&lon=111.72798302836298&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 832: city name => valparaiso
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.525723652283865&lon=-74.78330461324454&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 833: city name => bur gabo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.9359706696718604&lon=45.07000589025694&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 834: city name => gavle
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=60.255503717299035&lon=17.16545736297499&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 835: city name => shimanovsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=52.414270450977256&lon=128.33445239232577&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 836: city name => barrhead
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=54.643203134108745&lon=-114.23278185505765&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 837: city name => labuan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=7.416541884740241&lon=113.26157120177766&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 838: city name => fort morgan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.65701925633613&lon=-103.71553813899908&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 839: city name => usta muhammad
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=28.299066362280456&lon=67.71195254782336&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 840: city name => beloha
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-33.11220913098049&lon=42.009389205755014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 841: city name => rumoi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.346642612402576&lon=141.26753963347687&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 842: city name => trofors
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=65.15597230689795&lon=13.839697470371476&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 843: city name => puerto escondido
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=6.597297694879742&lon=-101.34701534767757&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 844: city name => arinos
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-16.6572775297213&lon=-45.67239951515805&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 845: city name => srednekolymsk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=72.83257343290501&lon=154.77433731557431&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 846: city name => pathalgaon
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=22.722677939085486&lon=83.13430628114025&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 847: city name => vetlanda
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=57.495390043294094&lon=15.731745401152779&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 848: city name => yerofey pavlovich
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.55743081872629&lon=122.27043848681933&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 849: city name => micheweni
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-4.551769570814457&lon=39.91074721812731&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 850: city name => bay city
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=43.8793809812577&lon=-83.57233922577356&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 851: city name => sursk
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=53.05284127963719&lon=45.647857122331345&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 852: city name => bida
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=8.953733615584383&lon=6.378339149191788&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 853: city name => coihaique
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-44.629944107815255&lon=-73.44136486808486&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 854: city name => bowen
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-17.301691916023316&lon=152.34052160777264&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 855: city name => teguise
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=30.862498332266696&lon=-12.444219095919067&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 856: city name => ruatoria
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-35.64554828079865&lon=178.08307934443104&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 857: city name => ciudad bolivar
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=5.833648025945152&lon=-64.76648058337193&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 858: city name => port augusta
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-31.844820247912978&lon=139.5248430733041&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 859: city name => kambove
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.324731951353911&lon=26.183331994837886&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 860: city name => nalvo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=16.96439193325743&lon=120.1317162343014&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 861: city name => zharkent
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=44.610169459546825&lon=80.01999206867941&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 862: city name => takoradi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-3.6175143683099833&lon=-0.2230541172226026&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 863: city name => goundi
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=9.676690096690677&lon=17.106840879015607&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 864: city name => hachinohe
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.81972937739994&lon=143.07287082666556&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 865: city name => dzhusaly
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=46.60923625240821&lon=64.148825989948&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 866: city name => neryungri
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=56.84365297363087&lon=123.03961385971206&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 867: city name => sayan
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-11.138232441705227&lon=-77.22010589972692&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 868: city name => nanakuli
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=15.355607479797541&lon=-162.24085380726225&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 869: city name => warqla
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.566728429920786&lon=4.28782699493533&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 870: city name => elmadag
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=39.85438535247354&lon=33.38629087535861&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 871: city name => weyburn
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=49.991864439890065&lon=-104.07326200520528&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 872: city name => mersing
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=3.699337873644282&lon=105.64121510381483&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 873: city name => cap malheureux
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-14.311415053378497&lon=56.50347109794208&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 874: city name => hokitika
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-42.805100765926014&lon=169.53622193489878&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 875: city name => springbok
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-29.628585530632236&lon=18.526977272217408&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 876: city name => menongue
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-13.773663996192596&lon=17.59396884371472&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 877: city name => hit
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.640053245200093&lon=42.60628754003332&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 878: city name => gazojak
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=40.36278862554215&lon=61.319357433615096&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 879: city name => cervo
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=45.81784399405976&lon=-7.099357721304045&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 880: city name => davila
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=19.29933042349316&lon=118.86872439922553&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 881: city name => half moon bay
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=34.61176577454076&lon=-129.3226965090345&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 882: city name => ajdabiya
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=31.154072914559023&lon=19.17710181170696&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 883: city name => mathathane
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=-22.45426411768331&lon=28.79475675489894&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    Now retrieving city number 884: city name => polson
    Requested URL: https://api.openweathermap.org/data/2.5/weather?lat=47.40501394561974&lon=-115.35925683629645&appid=0e837c082d19b7468579902ece2bec6d&units=imperial
    ---------------------------------------------------------------------------------------------------------------
    ---------------Data Retrieval Complete----------------
                                                          
                                                          
    ------------------------------------------------------
    Total Time Elapsed for Loop Completion = 0:21:18
    ------------------------------------------------------
                                                          
    


```python
# Combine data into one df 
city_df2["Temperature"] = temp
city_df2["Humidity"] = humidity
city_df2["Clouds"] = clouds
city_df2["Wind Speed"] = wind
# Save as csv 
city_df2.to_csv("city_df2.csv",encoding="utf-8", index=False)
#city_df2.head()
```

# Fig 1 : Temperature (F) vs. Latitude - 3/13/18


```python
# Temperature vs Latitude
plt.scatter(city_df2["Lat"],city_df2["Temperature"],marker=".")
plt.title("Fig 1 : Temperature (F) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.savefig("1_Temp_v_Lat.png")
plt.show()
```


![png](output_8_0.png)


# Fig 2 : Humidity (%) vs. Latitude - 3/13/18


```python
# Humidity vs Latitude
plt.scatter(city_df2["Lat"],city_df2["Humidity"],marker =".")
plt.title("Fig 2 : Humidity (%) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.savefig("2_Humidity_v_Lat.png")
plt.show()
```


![png](output_10_0.png)


# Fig 3: Cloudiness (%) vs. Latitude - 3/13/18


```python
# Cloudiness vs Latitude 
plt.scatter(city_df2["Lat"],city_df2["Clouds"],marker =".")
plt.title("Fig 3: Cloudiness (%) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.savefig("3_Clouds_v_Lat.png")
plt.show()
```


![png](output_12_0.png)


# Fig 4: Wind Speed (mph) vs. Latitude - 3/13/18


```python
# Wind Speed vs Latitude 
plt.scatter(city_df2["Lat"],city_df2["Clouds"],marker =".")
plt.title("Fig 4: Wind Speed (mph) vs. Latitude - 3/13/18")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
plt.savefig("4_Wind_v_Lat.png")
plt.show()
```


![png](output_14_0.png)

