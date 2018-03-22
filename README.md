
# WeatherPy

Analysis

Observed trend 1: Cities with latitude around degree 0 (i.e., equator), have the highest temperature. As the latitude increases toward degree 90 (i.e., north pole), the temperature decreases.

Observed trend 2: The distribution of humidity versus latitude is approximately uniform, which indicates that most probably there is no strong correlation between humidity and latitude.
    
Observed trend 3: There is no significant difference in wind speed of cities around equator.


```python
# Import Dependencies
import requests
import json
import openweathermapy.core as owm
from config import api_key
from pprint import pprint
from citipy import citipy
import seaborn as sns
import random
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from datetime import datetime
```


```python
# Getting the base URL and units
base_url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

# Build a URL
url = f"{base_url}appid={api_key}&units={units}&q="
```

# Generate Cities List


```python
# Create an empty list to hold the city names
city_names=[]

# Generate random latitude and longitude values
lat_arr = random.sample(list(np.arange(-90, 90,0.1)), 1500)
lng_arr = random.sample(list(np.arange(-180, 180,0.1)), 1500)

# loop through each combination of lat and lon to get the city names from citipy
for lat, lng  in zip(lat_arr, lng_arr):
    city = citipy.nearest_city(lat, lng)
    if city.city_name not in city_names:
        city_names.append(city.city_name)
```


```python
len(city_names)
```




    622



# Perform API Calls


```python
# Create empty lists to hold city names, cloudiness, country, date, humidity, lat, lng, max temperature and wind speed
city_name = []
cloudiness = []
country = []
date = []
humidity = []
lat = []
lng = []
max_temp = []
wind_speed = []

# Loop through each city in city names list to get the response from open weathermapy
row_count = 0
for city in city_names:
    print(f"Processing record {row_count} | {city}")
    row_count+=1
    response = requests.get(url+city)
    print(response.url)
    response = response.json()
    # Error handling to skip the cities that are not found in OWM
    try:   
        city_name.append(response['name'])
        cloudiness.append(response['clouds']['all'])
        country.append(response['sys']['country'])
        date.append(response['dt'])
        humidity.append(response['main']['humidity'])
        lat.append(response['coord']['lat'])
        lng.append(response['coord']['lon'])
        max_temp.append(response['main']['temp_max'])
        wind_speed.append(response['wind']['speed'])            
    except:
        print(f"{city} is not found. Skipping....")
        continue
```

    Processing record 0 | salalah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salalah
    Processing record 1 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=taolanaro
    taolanaro is not found. Skipping....
    Processing record 2 | bluff
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bluff
    Processing record 3 | sioux lookout
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sioux%20lookout
    Processing record 4 | hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hamilton
    Processing record 5 | kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kodiak
    Processing record 6 | camara de lobos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=camara%20de%20lobos
    Processing record 7 | vaini
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vaini
    Processing record 8 | tuggurt
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tuggurt
    tuggurt is not found. Skipping....
    Processing record 9 | cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cidreira
    Processing record 10 | albany
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=albany
    Processing record 11 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chokurdakh
    Processing record 12 | fuente de oro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fuente%20de%20oro
    Processing record 13 | poum
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=poum
    Processing record 14 | ikornnes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ikornnes
    Processing record 15 | ust-nera
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-nera
    Processing record 16 | castro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=castro
    Processing record 17 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=punta%20arenas
    Processing record 18 | atuona
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=atuona
    Processing record 19 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=carnarvon
    Processing record 20 | tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tiksi
    Processing record 21 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arraial%20do%20cabo
    Processing record 22 | rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rikitea
    Processing record 23 | san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20patricio
    Processing record 24 | korla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=korla
    korla is not found. Skipping....
    Processing record 25 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ushuaia
    Processing record 26 | mangit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mangit
    Processing record 27 | amderma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=amderma
    amderma is not found. Skipping....
    Processing record 28 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=illoqqortoormiut
    illoqqortoormiut is not found. Skipping....
    Processing record 29 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port-gentil
    Processing record 30 | padang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=padang
    Processing record 31 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saskylakh
    Processing record 32 | agua branca
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=agua%20branca
    Processing record 33 | tumannyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tumannyy
    tumannyy is not found. Skipping....
    Processing record 34 | avera
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=avera
    Processing record 35 | suao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=suao
    suao is not found. Skipping....
    Processing record 36 | olga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=olga
    Processing record 37 | bethel
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bethel
    Processing record 38 | zelenoborskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zelenoborskiy
    Processing record 39 | khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=khatanga
    Processing record 40 | jieshi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jieshi
    Processing record 41 | dikson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dikson
    Processing record 42 | lebu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lebu
    Processing record 43 | enumclaw
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=enumclaw
    Processing record 44 | farso
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=farso
    Processing record 45 | udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=udachnyy
    Processing record 46 | hobart
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hobart
    Processing record 47 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tuktoyaktuk
    Processing record 48 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=severo-kurilsk
    Processing record 49 | fujin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fujin
    Processing record 50 | samusu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=samusu
    samusu is not found. Skipping....
    Processing record 51 | talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=talnakh
    Processing record 52 | ekhabi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ekhabi
    Processing record 53 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barentsburg
    barentsburg is not found. Skipping....
    Processing record 54 | shenkursk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shenkursk
    Processing record 55 | amazar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=amazar
    Processing record 56 | issum
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=issum
    Processing record 57 | chuy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chuy
    Processing record 58 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tasiilaq
    Processing record 59 | vao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vao
    Processing record 60 | busselton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=busselton
    Processing record 61 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=new%20norfolk
    Processing record 62 | bayan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bayan
    Processing record 63 | middletown
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=middletown
    Processing record 64 | coos bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=coos%20bay
    Processing record 65 | alofi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alofi
    Processing record 66 | ngunguru
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ngunguru
    Processing record 67 | east london
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=east%20london
    Processing record 68 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hithadhoo
    Processing record 69 | drayton valley
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=drayton%20valley
    Processing record 70 | ariquemes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ariquemes
    Processing record 71 | kiama
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kiama
    Processing record 72 | villaviciosa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=villaviciosa
    Processing record 73 | okato
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=okato
    Processing record 74 | sevsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sevsk
    Processing record 75 | constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=constitucion
    Processing record 76 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20elizabeth
    Processing record 77 | chagda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chagda
    chagda is not found. Skipping....
    Processing record 78 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-joseph
    Processing record 79 | krasnyy chikoy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=krasnyy%20chikoy
    Processing record 80 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bredasdorp
    Processing record 81 | naze
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=naze
    Processing record 82 | nioki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nioki
    Processing record 83 | nenjiang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nenjiang
    Processing record 84 | mataura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mataura
    Processing record 85 | isangel
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=isangel
    Processing record 86 | norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=norman%20wells
    Processing record 87 | sao geraldo do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20geraldo%20do%20araguaia
    Processing record 88 | hilo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hilo
    Processing record 89 | aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aklavik
    Processing record 90 | yulara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yulara
    Processing record 91 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cap%20malheureux
    Processing record 92 | casian
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=casian
    Processing record 93 | cabedelo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cabedelo
    Processing record 94 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tsihombe
    tsihombe is not found. Skipping....
    Processing record 95 | sora
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sora
    Processing record 96 | hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hermanus
    Processing record 97 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mount%20gambier
    Processing record 98 | meiganga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=meiganga
    Processing record 99 | vega de alatorre
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vega%20de%20alatorre
    Processing record 100 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bengkulu
    bengkulu is not found. Skipping....
    Processing record 101 | juneau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=juneau
    Processing record 102 | verkhovyna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=verkhovyna
    Processing record 103 | bolshiye uki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bolshiye%20uki
    bolshiye uki is not found. Skipping....
    Processing record 104 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kavaratti
    Processing record 105 | bardiyah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bardiyah
    bardiyah is not found. Skipping....
    Processing record 106 | brekstad
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=brekstad
    Processing record 107 | lasa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lasa
    Processing record 108 | thompson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=thompson
    Processing record 109 | mount isa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mount%20isa
    Processing record 110 | high level
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=high%20level
    Processing record 111 | manggar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manggar
    Processing record 112 | cockburn harbour
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cockburn%20harbour
    cockburn harbour is not found. Skipping....
    Processing record 113 | manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manokwari
    Processing record 114 | virginia beach
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=virginia%20beach
    Processing record 115 | torbay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=torbay
    Processing record 116 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=srednekolymsk
    Processing record 117 | anadyr
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=anadyr
    Processing record 118 | saveh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saveh
    Processing record 119 | barrow
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barrow
    Processing record 120 | dingle
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dingle
    Processing record 121 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=puerto%20ayora
    Processing record 122 | cockburn town
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cockburn%20town
    Processing record 123 | lishu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lishu
    Processing record 124 | lensk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lensk
    Processing record 125 | gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gornopravdinsk
    Processing record 126 | lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lompoc
    Processing record 127 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nanortalik
    Processing record 128 | gao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gao
    Processing record 129 | sao luiz gonzaga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20luiz%20gonzaga
    Processing record 130 | lumeje
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lumeje
    Processing record 131 | malwan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=malwan
    malwan is not found. Skipping....
    Processing record 132 | andros town
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=andros%20town
    Processing record 133 | cape town
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cape%20town
    Processing record 134 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=qaanaaq
    Processing record 135 | evensk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=evensk
    Processing record 136 | matagami
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=matagami
    Processing record 137 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belushya%20guba
    belushya guba is not found. Skipping....
    Processing record 138 | bardsir
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bardsir
    Processing record 139 | faanui
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=faanui
    Processing record 140 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santa%20cruz
    Processing record 141 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tuatapere
    Processing record 142 | shevchenko
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shevchenko
    Processing record 143 | hsinchu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hsinchu
    Processing record 144 | camocim
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=camocim
    Processing record 145 | upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=upernavik
    Processing record 146 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pacific%20grove
    Processing record 147 | yendi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yendi
    Processing record 148 | vostok
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vostok
    Processing record 149 | billings
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=billings
    Processing record 150 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20cristobal
    Processing record 151 | olovyannaya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=olovyannaya
    Processing record 152 | butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=butaritari
    Processing record 153 | nuuk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nuuk
    Processing record 154 | labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=labuhan
    Processing record 155 | kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaeo
    Processing record 156 | viedma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=viedma
    Processing record 157 | almas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=almas
    Processing record 158 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sisimiut
    Processing record 159 | urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=urengoy
    Processing record 160 | kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kapaa
    Processing record 161 | banda aceh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=banda%20aceh
    Processing record 162 | dzhebariki-khaya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dzhebariki-khaya
    Processing record 163 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vestmannaeyjar
    Processing record 164 | bolungarvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bolungarvik
    bolungarvik is not found. Skipping....
    Processing record 165 | jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jamestown
    Processing record 166 | san carlos de bariloche
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20carlos%20de%20bariloche
    Processing record 167 | marcona
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marcona
    marcona is not found. Skipping....
    Processing record 168 | matara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=matara
    Processing record 169 | bull savanna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bull%20savanna
    Processing record 170 | arusha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arusha
    Processing record 171 | apastovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=apastovo
    Processing record 172 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=umzimvubu
    umzimvubu is not found. Skipping....
    Processing record 173 | kuusamo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kuusamo
    Processing record 174 | nome
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nome
    Processing record 175 | mutsamudu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mutsamudu
    mutsamudu is not found. Skipping....
    Processing record 176 | udimskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=udimskiy
    Processing record 177 | maungaturoto
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maungaturoto
    Processing record 178 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ribeira%20grande
    Processing record 179 | esperance
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=esperance
    Processing record 180 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=olafsvik
    olafsvik is not found. Skipping....
    Processing record 181 | jalesar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jalesar
    Processing record 182 | farmington
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=farmington
    Processing record 183 | spenge
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=spenge
    Processing record 184 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-philippe
    Processing record 185 | camabatela
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=camabatela
    Processing record 186 | hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hualmay
    Processing record 187 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yar-sale
    Processing record 188 | meyungs
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=meyungs
    meyungs is not found. Skipping....
    Processing record 189 | anshun
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=anshun
    Processing record 190 | ust-kamchatsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-kamchatsk
    ust-kamchatsk is not found. Skipping....
    Processing record 191 | changping
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=changping
    Processing record 192 | lata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lata
    Processing record 193 | namibe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=namibe
    Processing record 194 | borovoy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=borovoy
    Processing record 195 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bambous%20virieux
    Processing record 196 | thessalon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=thessalon
    Processing record 197 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=longyearbyen
    Processing record 198 | fengzhen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fengzhen
    Processing record 199 | lancaster
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lancaster
    Processing record 200 | arkhara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arkhara
    Processing record 201 | aswan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aswan
    Processing record 202 | husavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=husavik
    Processing record 203 | port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20alfred
    Processing record 204 | loandjili
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=loandjili
    Processing record 205 | pevek
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pevek
    Processing record 206 | hami
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hami
    Processing record 207 | south lyon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=south%20lyon
    Processing record 208 | qaqortoq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=qaqortoq
    Processing record 209 | batagay-alyta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=batagay-alyta
    Processing record 210 | roma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=roma
    Processing record 211 | richards bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=richards%20bay
    Processing record 212 | vardo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vardo
    Processing record 213 | souillac
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=souillac
    Processing record 214 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20filipe
    Processing record 215 | katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=katsuura
    Processing record 216 | pisco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pisco
    Processing record 217 | hella
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hella
    Processing record 218 | moose factory
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=moose%20factory
    Processing record 219 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cherskiy
    Processing record 220 | west bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=west%20bay
    Processing record 221 | tagusao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tagusao
    Processing record 222 | araouane
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=araouane
    Processing record 223 | roquetas de mar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=roquetas%20de%20mar
    Processing record 224 | kendari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kendari
    Processing record 225 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mahebourg
    Processing record 226 | suksun
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=suksun
    Processing record 227 | laguna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=laguna
    Processing record 228 | teahupoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=teahupoo
    Processing record 229 | sur
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sur
    Processing record 230 | zaozerne
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zaozerne
    Processing record 231 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=meulaboh
    Processing record 232 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kalmunai
    Processing record 233 | talara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=talara
    Processing record 234 | urumqi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=urumqi
    urumqi is not found. Skipping....
    Processing record 235 | burnie
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=burnie
    Processing record 236 | bilma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bilma
    Processing record 237 | gamovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gamovo
    Processing record 238 | paradwip
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=paradwip
    paradwip is not found. Skipping....
    Processing record 239 | asau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=asau
    asau is not found. Skipping....
    Processing record 240 | ondjiva
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ondjiva
    Processing record 241 | hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hasaki
    Processing record 242 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=quatre%20cocos
    Processing record 243 | seia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=seia
    Processing record 244 | cape canaveral
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cape%20canaveral
    Processing record 245 | novomyshastovskaya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=novomyshastovskaya
    Processing record 246 | sao joaquim
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20joaquim
    Processing record 247 | northam
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=northam
    Processing record 248 | inuvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=inuvik
    Processing record 249 | zunyi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zunyi
    Processing record 250 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fairbanks
    Processing record 251 | zlotoryja
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zlotoryja
    Processing record 252 | manakara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manakara
    Processing record 253 | elvas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=elvas
    Processing record 254 | sulangan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sulangan
    Processing record 255 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yellowknife
    Processing record 256 | san quintin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20quintin
    Processing record 257 | katakwi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=katakwi
    Processing record 258 | avarua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=avarua
    Processing record 259 | taltal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=taltal
    Processing record 260 | narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=narsaq
    Processing record 261 | lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lagoa
    Processing record 262 | zemio
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zemio
    Processing record 263 | maunabo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maunabo
    Processing record 264 | luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=luderitz
    Processing record 265 | le vauclin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=le%20vauclin
    Processing record 266 | sindor
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sindor
    Processing record 267 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20joao%20da%20barra
    Processing record 268 | poddorye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=poddorye
    Processing record 269 | mayumba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mayumba
    Processing record 270 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=taoudenni
    Processing record 271 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=plettenberg%20bay
    Processing record 272 | conceicao do mato dentro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=conceicao%20do%20mato%20dentro
    Processing record 273 | kijang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kijang
    Processing record 274 | kalat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kalat
    Processing record 275 | sambava
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sambava
    Processing record 276 | matamoros
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=matamoros
    Processing record 277 | airai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=airai
    Processing record 278 | bisho
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bisho
    bisho is not found. Skipping....
    Processing record 279 | teknaf
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=teknaf
    Processing record 280 | buala
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=buala
    Processing record 281 | guaiba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=guaiba
    Processing record 282 | angren
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=angren
    Processing record 283 | conakry
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=conakry
    Processing record 284 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaitangata
    Processing record 285 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=okhotsk
    Processing record 286 | ancud
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ancud
    Processing record 287 | union de reyes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=union%20de%20reyes
    Processing record 288 | timra
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=timra
    Processing record 289 | port hedland
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20hedland
    Processing record 290 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cabo%20san%20lucas
    Processing record 291 | bielsk podlaski
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bielsk%20podlaski
    Processing record 292 | lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lorengau
    Processing record 293 | mildura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mildura
    Processing record 294 | cochabamba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cochabamba
    Processing record 295 | shache
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shache
    Processing record 296 | cremona
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cremona
    Processing record 297 | belovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belovo
    Processing record 298 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nikolskoye
    Processing record 299 | ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-kuyga
    Processing record 300 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=leningradskiy
    Processing record 301 | georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=georgetown
    Processing record 302 | ganzhou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ganzhou
    Processing record 303 | bajil
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bajil
    Processing record 304 | sarakhs
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sarakhs
    Processing record 305 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=umm%20lajj
    Processing record 306 | tucuman
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tucuman
    Processing record 307 | tunxi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tunxi
    tunxi is not found. Skipping....
    Processing record 308 | samarai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=samarai
    Processing record 309 | kuala selangor
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kuala%20selangor
    Processing record 310 | abalak
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=abalak
    Processing record 311 | kahului
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kahului
    Processing record 312 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=guerrero%20negro
    Processing record 313 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nizhneyansk
    nizhneyansk is not found. Skipping....
    Processing record 314 | lima
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lima
    Processing record 315 | bitung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bitung
    Processing record 316 | alyangula
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alyangula
    Processing record 317 | sayat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sayat
    Processing record 318 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ketchikan
    Processing record 319 | sitka
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sitka
    Processing record 320 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zhigansk
    Processing record 321 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=palabuhanratu
    palabuhanratu is not found. Skipping....
    Processing record 322 | usinsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=usinsk
    Processing record 323 | keetmanshoop
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=keetmanshoop
    Processing record 324 | sao borja
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20borja
    Processing record 325 | takab
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=takab
    Processing record 326 | toliary
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=toliary
    toliary is not found. Skipping....
    Processing record 327 | sechura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sechura
    Processing record 328 | padre bernardo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=padre%20bernardo
    padre bernardo is not found. Skipping....
    Processing record 329 | beyla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=beyla
    Processing record 330 | warqla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=warqla
    warqla is not found. Skipping....
    Processing record 331 | ondorhaan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ondorhaan
    ondorhaan is not found. Skipping....
    Processing record 332 | gunjur
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gunjur
    Processing record 333 | tambul
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tambul
    tambul is not found. Skipping....
    Processing record 334 | acuna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=acuna
    acuna is not found. Skipping....
    Processing record 335 | lolodorf
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lolodorf
    Processing record 336 | maragogi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maragogi
    Processing record 337 | tambun
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tambun
    Processing record 338 | sembakung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sembakung
    Processing record 339 | tautira
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tautira
    Processing record 340 | geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=geraldton
    Processing record 341 | savalou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=savalou
    Processing record 342 | birin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=birin
    Processing record 343 | rio gallegos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rio%20gallegos
    Processing record 344 | alice springs
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alice%20springs
    Processing record 345 | calabozo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=calabozo
    Processing record 346 | khorixas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=khorixas
    Processing record 347 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mar%20del%20plata
    Processing record 348 | pavino
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pavino
    Processing record 349 | cabra
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cabra
    Processing record 350 | jawhar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jawhar
    Processing record 351 | tabou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tabou
    Processing record 352 | alekseyevka
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alekseyevka
    Processing record 353 | karaul
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karaul
    karaul is not found. Skipping....
    Processing record 354 | hofn
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hofn
    Processing record 355 | cognac
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cognac
    Processing record 356 | acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=acapulco
    Processing record 357 | berlevag
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=berlevag
    Processing record 358 | pitimbu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pitimbu
    Processing record 359 | autazes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=autazes
    Processing record 360 | saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saleaula
    saleaula is not found. Skipping....
    Processing record 361 | am timan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=am%20timan
    Processing record 362 | provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=provideniya
    Processing record 363 | mocajuba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mocajuba
    Processing record 364 | kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kavieng
    Processing record 365 | liverpool
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=liverpool
    Processing record 366 | itarema
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=itarema
    Processing record 367 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=grand%20river%20south%20east
    grand river south east is not found. Skipping....
    Processing record 368 | north bend
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=north%20bend
    Processing record 369 | shenjiamen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shenjiamen
    Processing record 370 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-pierre
    Processing record 371 | saint anthony
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint%20anthony
    Processing record 372 | obukhiv
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=obukhiv
    Processing record 373 | erzin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=erzin
    Processing record 374 | brandon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=brandon
    Processing record 375 | luena
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=luena
    Processing record 376 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ostrovnoy
    Processing record 377 | opuwo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=opuwo
    Processing record 378 | nordby
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nordby
    Processing record 379 | itoman
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=itoman
    Processing record 380 | gejiu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gejiu
    Processing record 381 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vaitupu
    vaitupu is not found. Skipping....
    Processing record 382 | reconquista
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=reconquista
    Processing record 383 | la baneza
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=la%20baneza
    Processing record 384 | marsh harbour
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marsh%20harbour
    Processing record 385 | suluq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=suluq
    Processing record 386 | santa cruz de tenerife
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santa%20cruz%20de%20tenerife
    Processing record 387 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kudahuvadhoo
    Processing record 388 | puerto escondido
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=puerto%20escondido
    Processing record 389 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=egvekinot
    Processing record 390 | seara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=seara
    Processing record 391 | odweyne
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=odweyne
    odweyne is not found. Skipping....
    Processing record 392 | arman
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arman
    Processing record 393 | balikpapan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=balikpapan
    Processing record 394 | amapa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=amapa
    Processing record 395 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sinnamary
    Processing record 396 | maues
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maues
    Processing record 397 | buin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=buin
    Processing record 398 | derby
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=derby
    Processing record 399 | road town
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=road%20town
    Processing record 400 | dakoro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dakoro
    Processing record 401 | zhezkazgan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zhezkazgan
    Processing record 402 | caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=caravelas
    Processing record 403 | cap-aux-meules
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cap-aux-meules
    Processing record 404 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=klaksvik
    Processing record 405 | fez
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fez
    Processing record 406 | atikokan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=atikokan
    Processing record 407 | vizinga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vizinga
    Processing record 408 | coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=coahuayana
    Processing record 409 | rapid valley
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rapid%20valley
    Processing record 410 | karasjok
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karasjok
    Processing record 411 | margate
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=margate
    Processing record 412 | pitangui
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pitangui
    Processing record 413 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bandarbeyla
    Processing record 414 | salihorsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salihorsk
    Processing record 415 | altay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=altay
    Processing record 416 | bhuj
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bhuj
    Processing record 417 | victoria
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=victoria
    Processing record 418 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=attawapiskat
    attawapiskat is not found. Skipping....
    Processing record 419 | falealupo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=falealupo
    falealupo is not found. Skipping....
    Processing record 420 | hvide sande
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hvide%20sande
    Processing record 421 | bolobo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bolobo
    Processing record 422 | santiago del estero
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santiago%20del%20estero
    Processing record 423 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=grand%20gaube
    Processing record 424 | oranjestad
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=oranjestad
    Processing record 425 | codrington
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=codrington
    Processing record 426 | gornyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gornyy
    Processing record 427 | palmer
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=palmer
    Processing record 428 | savonlinna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=savonlinna
    Processing record 429 | luganville
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=luganville
    Processing record 430 | pangkalanbuun
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pangkalanbuun
    Processing record 431 | show low
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=show%20low
    Processing record 432 | teluk nibung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=teluk%20nibung
    teluk nibung is not found. Skipping....
    Processing record 433 | tecpan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tecpan
    Processing record 434 | sijunjung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sijunjung
    Processing record 435 | bonavista
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bonavista
    Processing record 436 | chiredzi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chiredzi
    Processing record 437 | belmonte
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belmonte
    Processing record 438 | volgodonsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=volgodonsk
    Processing record 439 | tual
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tual
    Processing record 440 | skalistyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=skalistyy
    skalistyy is not found. Skipping....
    Processing record 441 | swellendam
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=swellendam
    Processing record 442 | alappuzha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alappuzha
    alappuzha is not found. Skipping....
    Processing record 443 | ornskoldsvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ornskoldsvik
    Processing record 444 | carutapera
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=carutapera
    Processing record 445 | arlit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arlit
    Processing record 446 | krasnoarmeysk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=krasnoarmeysk
    Processing record 447 | kifri
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kifri
    Processing record 448 | saint-leu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-leu
    Processing record 449 | lumphat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lumphat
    Processing record 450 | norman
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=norman
    Processing record 451 | touros
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=touros
    Processing record 452 | tehachapi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tehachapi
    Processing record 453 | kanniyakumari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kanniyakumari
    Processing record 454 | catamarca
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=catamarca
    catamarca is not found. Skipping....
    Processing record 455 | champerico
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=champerico
    Processing record 456 | maceio
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maceio
    Processing record 457 | portland
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=portland
    Processing record 458 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20lincoln
    Processing record 459 | mareeba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mareeba
    Processing record 460 | ye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ye
    ye is not found. Skipping....
    Processing record 461 | la ronge
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=la%20ronge
    Processing record 462 | la grande
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=la%20grande
    Processing record 463 | gurskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gurskoye
    gurskoye is not found. Skipping....
    Processing record 464 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sentyabrskiy
    sentyabrskiy is not found. Skipping....
    Processing record 465 | nyurba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nyurba
    Processing record 466 | brae
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=brae
    Processing record 467 | waipawa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=waipawa
    Processing record 468 | gwadar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gwadar
    Processing record 469 | bambanglipuro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bambanglipuro
    Processing record 470 | san jeronimo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20jeronimo
    Processing record 471 | manzhouli
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manzhouli
    Processing record 472 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ilulissat
    Processing record 473 | alberton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alberton
    Processing record 474 | kaohsiung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaohsiung
    Processing record 475 | san luis
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20luis
    Processing record 476 | jatiroto
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jatiroto
    Processing record 477 | coolum beach
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=coolum%20beach
    Processing record 478 | mangan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mangan
    Processing record 479 | rawson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rawson
    Processing record 480 | qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=qasigiannguit
    Processing record 481 | abnub
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=abnub
    Processing record 482 | libenge
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=libenge
    Processing record 483 | tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tessalit
    Processing record 484 | puerto carreno
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=puerto%20carreno
    Processing record 485 | saint-georges
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-georges
    Processing record 486 | haines junction
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=haines%20junction
    Processing record 487 | napier
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=napier
    Processing record 488 | ayagoz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ayagoz
    Processing record 489 | vila velha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vila%20velha
    Processing record 490 | solnechnyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=solnechnyy
    Processing record 491 | cahors
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cahors
    Processing record 492 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ponta%20do%20sol
    Processing record 493 | stokmarknes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=stokmarknes
    Processing record 494 | mtimbira
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mtimbira
    Processing record 495 | port blair
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20blair
    Processing record 496 | homer
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=homer
    Processing record 497 | khandyga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=khandyga
    Processing record 498 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=thinadhoo
    Processing record 499 | kathmandu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kathmandu
    Processing record 500 | sao felix do xingu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20felix%20do%20xingu
    Processing record 501 | gangotri
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gangotri
    gangotri is not found. Skipping....
    Processing record 502 | valley station
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=valley%20station
    Processing record 503 | boyolangu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=boyolangu
    Processing record 504 | san felipe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20felipe
    Processing record 505 | galle
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=galle
    Processing record 506 | sahrak
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sahrak
    sahrak is not found. Skipping....
    Processing record 507 | evora
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=evora
    Processing record 508 | mitu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mitu
    Processing record 509 | cheuskiny
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cheuskiny
    cheuskiny is not found. Skipping....
    Processing record 510 | godfrey
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=godfrey
    Processing record 511 | muncar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=muncar
    Processing record 512 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=deputatskiy
    Processing record 513 | beloha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=beloha
    Processing record 514 | bar harbor
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bar%20harbor
    Processing record 515 | at-bashi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=at-bashi
    Processing record 516 | linchuan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=linchuan
    linchuan is not found. Skipping....
    Processing record 517 | argelia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=argelia
    Processing record 518 | iskateley
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=iskateley
    Processing record 519 | malanje
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=malanje
    Processing record 520 | shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shimoda
    Processing record 521 | asfi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=asfi
    asfi is not found. Skipping....
    Processing record 522 | moron
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=moron
    Processing record 523 | saint george
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint%20george
    Processing record 524 | lisala
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lisala
    Processing record 525 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vila%20franca%20do%20campo
    Processing record 526 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mys%20shmidta
    mys shmidta is not found. Skipping....
    Processing record 527 | tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tilichiki
    Processing record 528 | sorvag
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sorvag
    sorvag is not found. Skipping....
    Processing record 529 | camacha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=camacha
    Processing record 530 | chubbuck
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chubbuck
    Processing record 531 | villa bruzual
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=villa%20bruzual
    Processing record 532 | ayan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ayan
    Processing record 533 | karkaralinsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karkaralinsk
    karkaralinsk is not found. Skipping....
    Processing record 534 | eureka
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=eureka
    Processing record 535 | reforma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=reforma
    Processing record 536 | derzhavinsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=derzhavinsk
    Processing record 537 | sabang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sabang
    Processing record 538 | natal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=natal
    Processing record 539 | kyshtovka
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kyshtovka
    Processing record 540 | sterling
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sterling
    Processing record 541 | manyana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manyana
    Processing record 542 | najran
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=najran
    Processing record 543 | la paz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=la%20paz
    Processing record 544 | mogocha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mogocha
    Processing record 545 | inyonga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=inyonga
    Processing record 546 | fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fortuna
    Processing record 547 | gulshat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gulshat
    gulshat is not found. Skipping....
    Processing record 548 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sobolevo
    Processing record 549 | opobo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=opobo
    opobo is not found. Skipping....
    Processing record 550 | samana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=samana
    Processing record 551 | koslan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=koslan
    Processing record 552 | kinanah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kinanah
    kinanah is not found. Skipping....
    Processing record 553 | harper
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=harper
    Processing record 554 | pembroke
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pembroke
    Processing record 555 | vibo valentia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vibo%20valentia
    Processing record 556 | krasnogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=krasnogorsk
    Processing record 557 | longavi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=longavi
    longavi is not found. Skipping....
    Processing record 558 | aykhal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aykhal
    Processing record 559 | hecun
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hecun
    Processing record 560 | monterey
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=monterey
    Processing record 561 | paamiut
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=paamiut
    Processing record 562 | indian head
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=indian%20head
    Processing record 563 | louisbourg
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=louisbourg
    louisbourg is not found. Skipping....
    Processing record 564 | ituni
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ituni
    ituni is not found. Skipping....
    Processing record 565 | alakurtti
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alakurtti
    Processing record 566 | kainantu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kainantu
    Processing record 567 | yermakovskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yermakovskoye
    Processing record 568 | saint-francois
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-francois
    Processing record 569 | belaya gora
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belaya%20gora
    Processing record 570 | adrar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=adrar
    Processing record 571 | samalaeulu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=samalaeulu
    samalaeulu is not found. Skipping....
    Processing record 572 | pirgos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pirgos
    Processing record 573 | basco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=basco
    Processing record 574 | salvador
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salvador
    Processing record 575 | viloco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=viloco
    Processing record 576 | mezen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mezen
    Processing record 577 | college
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=college
    Processing record 578 | pijijiapan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pijijiapan
    Processing record 579 | karla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karla
    Processing record 580 | tabiauea
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tabiauea
    tabiauea is not found. Skipping....
    Processing record 581 | chom bung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chom%20bung
    chom bung is not found. Skipping....
    Processing record 582 | tulun
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tulun
    Processing record 583 | sulurpeta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sulurpeta
    sulurpeta is not found. Skipping....
    Processing record 584 | clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=clyde%20river
    Processing record 585 | matola
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=matola
    Processing record 586 | milingimbi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=milingimbi
    milingimbi is not found. Skipping....
    Processing record 587 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fort%20nelson
    Processing record 588 | mungwi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mungwi
    Processing record 589 | ojinaga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ojinaga
    Processing record 590 | westport
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=westport
    Processing record 591 | tarija
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tarija
    Processing record 592 | nchelenge
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nchelenge
    Processing record 593 | fukue
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fukue
    Processing record 594 | sabha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sabha
    Processing record 595 | bubaque
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bubaque
    Processing record 596 | gizo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gizo
    Processing record 597 | kulhudhuffushi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kulhudhuffushi
    Processing record 598 | butterworth
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=butterworth
    Processing record 599 | barra do garcas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barra%20do%20garcas
    Processing record 600 | papetoai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=papetoai
    Processing record 601 | santa maria da vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santa%20maria%20da%20vitoria
    Processing record 602 | salinas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salinas
    Processing record 603 | petatlan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=petatlan
    Processing record 604 | myitkyina
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=myitkyina
    Processing record 605 | san policarpo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20policarpo
    Processing record 606 | pundaguitan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pundaguitan
    Processing record 607 | chitrakonda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chitrakonda
    Processing record 608 | kamina
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kamina
    Processing record 609 | mananjary
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mananjary
    Processing record 610 | burica
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=burica
    burica is not found. Skipping....
    Processing record 611 | kununurra
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kununurra
    Processing record 612 | ola
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ola
    Processing record 613 | ijaki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ijaki
    ijaki is not found. Skipping....
    Processing record 614 | kerki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kerki
    kerki is not found. Skipping....
    Processing record 615 | marfino
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marfino
    Processing record 616 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kruisfontein
    Processing record 617 | alugan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alugan
    Processing record 618 | barao de melgaco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barao%20de%20melgaco
    Processing record 619 | nioro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nioro
    Processing record 620 | yatou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yatou
    Processing record 621 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nouadhibou
    


```python
# Create a city data dataframe
city_data_pd = pd.DataFrame({"City":city_name,
            "Cloudiness":cloudiness,
             "Country": country,
             "Date": date,
             "Humidity": humidity,
             "Lat": lat,
             "Lng": lng,
             "Max_Temp": max_temp,
             "Wind_speed": wind_speed})

# Writing the city data to csv file
city_data_pd.to_csv("City Weather Data.csv")
city_data_pd.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max_Temp</th>
      <th>Wind_speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Salalah</td>
      <td>20</td>
      <td>OM</td>
      <td>1521697800</td>
      <td>61</td>
      <td>17.01</td>
      <td>54.10</td>
      <td>80.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bluff</td>
      <td>92</td>
      <td>AU</td>
      <td>1521701459</td>
      <td>85</td>
      <td>-23.58</td>
      <td>149.07</td>
      <td>72.87</td>
      <td>14.12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sioux Lookout</td>
      <td>5</td>
      <td>CA</td>
      <td>1521698400</td>
      <td>66</td>
      <td>50.10</td>
      <td>-91.92</td>
      <td>24.80</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hamilton</td>
      <td>40</td>
      <td>CA</td>
      <td>1521698400</td>
      <td>54</td>
      <td>43.26</td>
      <td>-79.87</td>
      <td>32.00</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kodiak</td>
      <td>1</td>
      <td>US</td>
      <td>1521697980</td>
      <td>80</td>
      <td>39.95</td>
      <td>-94.76</td>
      <td>33.80</td>
      <td>10.09</td>
    </tr>
  </tbody>
</table>
</div>



# Latitude vs Temperature Plot


```python
# Set the grey background
sns.set()

# Plot the scatter
plt.scatter(city_data_pd.Lat, city_data_pd.Max_Temp, color="Blue", edgecolors="black", alpha= 0.7, linewidths=1)

# Add Title, xlabel and ylabel
plt.title(f"City Latitude vs. Max Temperatur ({datetime.today().strftime('%m/%d/%y')})")
plt.xlabel("Latitude")
plt.ylabel("Max Temperature (F)")

# Save the figure
plt.savefig("Latitude vs Temperature.png")
# Display the plot
plt.show()
```


![png](output_12_0.png)


# Latitude vs. Humidity Plot


```python
# Set the grey background
sns.set()

# The scatter plot
plt.scatter(city_data_pd.Lat,city_data_pd.Humidity, color="Blue",edgecolors="black", alpha=0.7, linewidths=1)

# Add title, xlabel and ylabel
plt.title(f"City Latitude vs. Humidity ({datetime.today().strftime('%m/%d/%y')})")
plt.xlabel("Latitude")
plt.ylabel("Humidity(%)")

# Save the figure
plt.savefig("Latitude vs Humidity.png")
# Display the plot
plt.show()
```


![png](output_14_0.png)


# Latitude vs. Cloudiness Plot


```python
# Set the grey background
sns.set()

# The Scatter plot
plt.scatter(city_data_pd.Lat,city_data_pd.Cloudiness, color="Blue",edgecolors="black", alpha=0.7, linewidths=1)

# Add title, xlabel, ylabel
plt.title(f"City Latitude vs. Cloudiness ({datetime.today().strftime('%m/%d/%y')})")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness(%)")

# Save the figure
plt.savefig("Latitude vs Cloudiness.png")
# Display the plot
plt.show()
```


![png](output_16_0.png)


# Latitude vs. Wind Speed Plot


```python
# Set the grey background
sns.set()

# The scatter plot
plt.scatter(city_data_pd.Lat, city_data_pd.Wind_speed, color="Blue",edgecolors="black", alpha=0.7, linewidths=1)

# Add title, xlabel, ylabel
plt.title(f"City Latitude vs. Wind Speed ({datetime.today().strftime('%m/%d/%y')})")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed(mph)")

# Save the figure
plt.savefig("Latitude vs Wind Speed.png")

# Display the plot
plt.show()
```


![png](output_18_0.png)

