
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
while len(city_names) < 600:
    for lat, lng in zip(lat_arr, lng_arr):
        city = citipy.nearest_city(lat, lng)
        if city.city_name not in city_names:
            city_names.append(city.city_name)
```

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

    Processing record 0 | isla vista
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=isla%20vista
    Processing record 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vaini
    Processing record 2 | marcona
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marcona
    marcona is not found. Skipping....
    Processing record 3 | lebu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lebu
    Processing record 4 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ushuaia
    Processing record 5 | rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rikitea
    Processing record 6 | labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=labuhan
    Processing record 7 | kendari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kendari
    Processing record 8 | povenets
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=povenets
    Processing record 9 | rincon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rincon
    Processing record 10 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tasiilaq
    Processing record 11 | vostok
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vostok
    Processing record 12 | filadelfia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=filadelfia
    Processing record 13 | ngunguru
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ngunguru
    Processing record 14 | gap
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gap
    Processing record 15 | faanui
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=faanui
    Processing record 16 | jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jamestown
    Processing record 17 | butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=butaritari
    Processing record 18 | san quintin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20quintin
    Processing record 19 | bilma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bilma
    Processing record 20 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=puerto%20ayora
    Processing record 21 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20elizabeth
    Processing record 22 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bredasdorp
    Processing record 23 | khorixas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=khorixas
    Processing record 24 | kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kapaa
    Processing record 25 | simi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=simi
    Processing record 26 | atuona
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=atuona
    Processing record 27 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=taolanaro
    taolanaro is not found. Skipping....
    Processing record 28 | mataura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mataura
    Processing record 29 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yellowknife
    Processing record 30 | mana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mana
    Processing record 31 | pasni
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pasni
    Processing record 32 | boyolangu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=boyolangu
    Processing record 33 | port hardy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20hardy
    Processing record 34 | ewa beach
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ewa%20beach
    Processing record 35 | hem
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hem
    Processing record 36 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saskylakh
    Processing record 37 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=illoqqortoormiut
    illoqqortoormiut is not found. Skipping....
    Processing record 38 | itarema
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=itarema
    Processing record 39 | thompson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=thompson
    Processing record 40 | salalah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salalah
    Processing record 41 | uryupinsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=uryupinsk
    Processing record 42 | chagda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chagda
    chagda is not found. Skipping....
    Processing record 43 | bluff
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bluff
    Processing record 44 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mar%20del%20plata
    Processing record 45 | narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=narsaq
    Processing record 46 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bandarbeyla
    Processing record 47 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ribeira%20grande
    Processing record 48 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=punta%20arenas
    Processing record 49 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=new%20norfolk
    Processing record 50 | mafinga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mafinga
    mafinga is not found. Skipping....
    Processing record 51 | khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=khatanga
    Processing record 52 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=klaksvik
    Processing record 53 | dikson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dikson
    Processing record 54 | hilo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hilo
    Processing record 55 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vila%20franca%20do%20campo
    Processing record 56 | fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fortuna
    Processing record 57 | kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kodiak
    Processing record 58 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=attawapiskat
    attawapiskat is not found. Skipping....
    Processing record 59 | lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lorengau
    Processing record 60 | avarua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=avarua
    Processing record 61 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tuatapere
    Processing record 62 | chirongui
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chirongui
    Processing record 63 | las palmas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=las%20palmas
    Processing record 64 | port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20alfred
    Processing record 65 | te anau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=te%20anau
    Processing record 66 | grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=grindavik
    Processing record 67 | busselton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=busselton
    Processing record 68 | conceicao do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=conceicao%20do%20araguaia
    Processing record 69 | natal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=natal
    Processing record 70 | saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saldanha
    Processing record 71 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=umzimvubu
    umzimvubu is not found. Skipping....
    Processing record 72 | georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=georgetown
    Processing record 73 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barentsburg
    barentsburg is not found. Skipping....
    Processing record 74 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cherskiy
    Processing record 75 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20filipe
    Processing record 76 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mount%20gambier
    Processing record 77 | albany
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=albany
    Processing record 78 | awjilah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=awjilah
    Processing record 79 | santa isabel do rio negro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santa%20isabel%20do%20rio%20negro
    Processing record 80 | araouane
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=araouane
    Processing record 81 | lubao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lubao
    Processing record 82 | hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hobyo
    Processing record 83 | mehamn
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mehamn
    Processing record 84 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mahebourg
    Processing record 85 | bethel
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bethel
    Processing record 86 | san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20patricio
    Processing record 87 | korla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=korla
    korla is not found. Skipping....
    Processing record 88 | hobart
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hobart
    Processing record 89 | baykit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=baykit
    Processing record 90 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ponta%20do%20sol
    Processing record 91 | maniitsoq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maniitsoq
    Processing record 92 | nivala
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nivala
    Processing record 93 | hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hualmay
    Processing record 94 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hithadhoo
    Processing record 95 | bijni
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bijni
    Processing record 96 | honningsvag
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=honningsvag
    Processing record 97 | upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=upernavik
    Processing record 98 | belaya gora
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belaya%20gora
    Processing record 99 | sahrak
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sahrak
    sahrak is not found. Skipping....
    Processing record 100 | ulagan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ulagan
    Processing record 101 | marquette
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marquette
    Processing record 102 | tevriz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tevriz
    Processing record 103 | longlac
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=longlac
    longlac is not found. Skipping....
    Processing record 104 | shubarkuduk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shubarkuduk
    Processing record 105 | bol
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bol
    Processing record 106 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vestmannaeyjar
    Processing record 107 | tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tiksi
    Processing record 108 | ocampo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ocampo
    Processing record 109 | rio gallegos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rio%20gallegos
    Processing record 110 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=severo-kurilsk
    Processing record 111 | hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hamilton
    Processing record 112 | athabasca
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=athabasca
    Processing record 113 | kaduna
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaduna
    Processing record 114 | pout
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pout
    pout is not found. Skipping....
    Processing record 115 | pitimbu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pitimbu
    Processing record 116 | norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=norman%20wells
    Processing record 117 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=palabuhanratu
    palabuhanratu is not found. Skipping....
    Processing record 118 | hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hermanus
    Processing record 119 | matagami
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=matagami
    Processing record 120 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-augustin
    Processing record 121 | bayir
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bayir
    Processing record 122 | okha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=okha
    Processing record 123 | sorvag
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sorvag
    sorvag is not found. Skipping....
    Processing record 124 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-philippe
    Processing record 125 | saryshagan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saryshagan
    saryshagan is not found. Skipping....
    Processing record 126 | vastseliina
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vastseliina
    Processing record 127 | kieta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kieta
    Processing record 128 | savalou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=savalou
    Processing record 129 | zolotyy potik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zolotyy%20potik
    Processing record 130 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nizhneyansk
    nizhneyansk is not found. Skipping....
    Processing record 131 | borogontsy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=borogontsy
    Processing record 132 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=okhotsk
    Processing record 133 | sucua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sucua
    Processing record 134 | bonao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bonao
    Processing record 135 | katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=katsuura
    Processing record 136 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=thinadhoo
    Processing record 137 | amderma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=amderma
    amderma is not found. Skipping....
    Processing record 138 | ukiah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ukiah
    Processing record 139 | pitsunda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pitsunda
    Processing record 140 | mata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mata
    Processing record 141 | new glasgow
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=new%20glasgow
    Processing record 142 | kampene
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kampene
    Processing record 143 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bengkulu
    bengkulu is not found. Skipping....
    Processing record 144 | turukhansk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=turukhansk
    Processing record 145 | kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaeo
    Processing record 146 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=qaanaaq
    Processing record 147 | kadykchan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kadykchan
    kadykchan is not found. Skipping....
    Processing record 148 | ammon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ammon
    Processing record 149 | cape town
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cape%20town
    Processing record 150 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ostrovnoy
    Processing record 151 | havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=havoysund
    Processing record 152 | berlevag
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=berlevag
    Processing record 153 | macaravita
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=macaravita
    Processing record 154 | edd
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=edd
    Processing record 155 | tungor
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tungor
    Processing record 156 | iracoubo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=iracoubo
    Processing record 157 | cortez
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cortez
    Processing record 158 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=longyearbyen
    Processing record 159 | capinzal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=capinzal
    Processing record 160 | odweyne
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=odweyne
    odweyne is not found. Skipping....
    Processing record 161 | aquiraz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aquiraz
    Processing record 162 | alofi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alofi
    Processing record 163 | ginir
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ginir
    Processing record 164 | shelbyville
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shelbyville
    Processing record 165 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belushya%20guba
    belushya guba is not found. Skipping....
    Processing record 166 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nikolskoye
    Processing record 167 | doctor pedro p. pena
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=doctor%20pedro%20p.%20pena
    doctor pedro p. pena is not found. Skipping....
    Processing record 168 | twin falls
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=twin%20falls
    Processing record 169 | eyl
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=eyl
    Processing record 170 | airai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=airai
    Processing record 171 | eureka
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=eureka
    Processing record 172 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=carnarvon
    Processing record 173 | esperance
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=esperance
    Processing record 174 | anadyr
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=anadyr
    Processing record 175 | iskateley
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=iskateley
    Processing record 176 | hit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hit
    Processing record 177 | kazachinskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kazachinskoye
    Processing record 178 | blagoyevo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=blagoyevo
    Processing record 179 | leh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=leh
    Processing record 180 | cururupu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cururupu
    Processing record 181 | kiama
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kiama
    Processing record 182 | yulara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yulara
    Processing record 183 | formoso do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=formoso%20do%20araguaia
    formoso do araguaia is not found. Skipping....
    Processing record 184 | hofn
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hofn
    Processing record 185 | anqiu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=anqiu
    Processing record 186 | carutapera
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=carutapera
    Processing record 187 | goma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=goma
    Processing record 188 | corinto
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=corinto
    Processing record 189 | tiznit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tiznit
    Processing record 190 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=grand%20river%20south%20east
    grand river south east is not found. Skipping....
    Processing record 191 | haibowan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=haibowan
    haibowan is not found. Skipping....
    Processing record 192 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tsihombe
    tsihombe is not found. Skipping....
    Processing record 193 | haimen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=haimen
    Processing record 194 | kungurtug
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kungurtug
    Processing record 195 | barrow
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barrow
    Processing record 196 | bad urach
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bad%20urach
    Processing record 197 | macae
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=macae
    Processing record 198 | constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=constitucion
    Processing record 199 | neiafu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=neiafu
    Processing record 200 | coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=coihaique
    Processing record 201 | nabire
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nabire
    Processing record 202 | tawkar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tawkar
    tawkar is not found. Skipping....
    Processing record 203 | harper
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=harper
    Processing record 204 | rocha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rocha
    Processing record 205 | loandjili
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=loandjili
    Processing record 206 | souillac
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=souillac
    Processing record 207 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sentyabrskiy
    sentyabrskiy is not found. Skipping....
    Processing record 208 | tocopilla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tocopilla
    Processing record 209 | chernyshevskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chernyshevskiy
    Processing record 210 | ruatoria
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ruatoria
    ruatoria is not found. Skipping....
    Processing record 211 | victoria point
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=victoria%20point
    Processing record 212 | salekhard
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salekhard
    Processing record 213 | les herbiers
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=les%20herbiers
    Processing record 214 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ilulissat
    Processing record 215 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=coquimbo
    Processing record 216 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arraial%20do%20cabo
    Processing record 217 | petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=petropavlovsk-kamchatskiy
    Processing record 218 | novo aripuana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=novo%20aripuana
    Processing record 219 | turayf
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=turayf
    Processing record 220 | saint george
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint%20george
    Processing record 221 | rio grande
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rio%20grande
    Processing record 222 | hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hasaki
    Processing record 223 | altamira
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=altamira
    Processing record 224 | acajutla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=acajutla
    Processing record 225 | provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=provideniya
    Processing record 226 | gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gornopravdinsk
    Processing record 227 | adrar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=adrar
    Processing record 228 | chapais
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chapais
    Processing record 229 | touros
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=touros
    Processing record 230 | tateyama
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tateyama
    Processing record 231 | portland
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=portland
    Processing record 232 | kupang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kupang
    Processing record 233 | pisco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pisco
    Processing record 234 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vaitupu
    vaitupu is not found. Skipping....
    Processing record 235 | east london
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=east%20london
    Processing record 236 | payo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=payo
    Processing record 237 | leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=leshukonskoye
    Processing record 238 | lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lompoc
    Processing record 239 | paamiut
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=paamiut
    Processing record 240 | yamada
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yamada
    Processing record 241 | yantikovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yantikovo
    Processing record 242 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lavrentiya
    Processing record 243 | kijang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kijang
    Processing record 244 | tarudant
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tarudant
    tarudant is not found. Skipping....
    Processing record 245 | ojinaga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ojinaga
    Processing record 246 | montgomery
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=montgomery
    Processing record 247 | ngukurr
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ngukurr
    ngukurr is not found. Skipping....
    Processing record 248 | henties bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=henties%20bay
    Processing record 249 | prince rupert
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=prince%20rupert
    Processing record 250 | clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=clyde%20river
    Processing record 251 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chokurdakh
    Processing record 252 | orlik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=orlik
    Processing record 253 | castiglione del lago
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=castiglione%20del%20lago
    Processing record 254 | hertford
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hertford
    Processing record 255 | qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=qasigiannguit
    Processing record 256 | dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dunedin
    Processing record 257 | marawi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marawi
    Processing record 258 | karatau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karatau
    Processing record 259 | namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=namatanai
    Processing record 260 | dalbandin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dalbandin
    Processing record 261 | lifford
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lifford
    Processing record 262 | roma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=roma
    Processing record 263 | biggar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=biggar
    Processing record 264 | mogilno
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mogilno
    Processing record 265 | devils lake
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=devils%20lake
    Processing record 266 | jertih
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jertih
    Processing record 267 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bathsheba
    Processing record 268 | valpoi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=valpoi
    Processing record 269 | ulverstone
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ulverstone
    Processing record 270 | setermoen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=setermoen
    Processing record 271 | torbay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=torbay
    Processing record 272 | dukat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dukat
    Processing record 273 | tibati
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tibati
    Processing record 274 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaitangata
    Processing record 275 | kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kavieng
    Processing record 276 | aleksandrov gay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aleksandrov%20gay
    Processing record 277 | cairns
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cairns
    Processing record 278 | santa rita
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santa%20rita
    Processing record 279 | chuy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chuy
    Processing record 280 | aranos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aranos
    Processing record 281 | prince albert
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=prince%20albert
    Processing record 282 | labuan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=labuan
    Processing record 283 | nemuro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nemuro
    Processing record 284 | lata
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lata
    Processing record 285 | wanxian
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=wanxian
    Processing record 286 | angouleme
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=angouleme
    Processing record 287 | vilyuysk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vilyuysk
    Processing record 288 | rosarito
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rosarito
    Processing record 289 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=olafsvik
    olafsvik is not found. Skipping....
    Processing record 290 | vaitape
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vaitape
    Processing record 291 | akdepe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=akdepe
    Processing record 292 | castro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=castro
    Processing record 293 | ust-kut
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-kut
    Processing record 294 | yanchukan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yanchukan
    yanchukan is not found. Skipping....
    Processing record 295 | dali
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dali
    Processing record 296 | kenai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kenai
    Processing record 297 | mareeba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mareeba
    Processing record 298 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kalmunai
    Processing record 299 | nome
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nome
    Processing record 300 | gongzhuling
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gongzhuling
    Processing record 301 | samusu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=samusu
    samusu is not found. Skipping....
    Processing record 302 | kirando
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kirando
    Processing record 303 | luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=luderitz
    Processing record 304 | witu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=witu
    Processing record 305 | shaunavon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=shaunavon
    Processing record 306 | warqla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=warqla
    warqla is not found. Skipping....
    Processing record 307 | manta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manta
    Processing record 308 | viligili
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=viligili
    viligili is not found. Skipping....
    Processing record 309 | safwah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=safwah
    safwah is not found. Skipping....
    Processing record 310 | vanavara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vanavara
    Processing record 311 | geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=geraldton
    Processing record 312 | minas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=minas
    Processing record 313 | dan khun thot
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dan%20khun%20thot
    Processing record 314 | ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-kuyga
    Processing record 315 | cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cayenne
    Processing record 316 | hounde
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hounde
    Processing record 317 | urdzhar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=urdzhar
    urdzhar is not found. Skipping....
    Processing record 318 | rawson
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rawson
    Processing record 319 | tucumcari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tucumcari
    Processing record 320 | brae
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=brae
    Processing record 321 | richards bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=richards%20bay
    Processing record 322 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mys%20shmidta
    mys shmidta is not found. Skipping....
    Processing record 323 | batticaloa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=batticaloa
    Processing record 324 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nanortalik
    Processing record 325 | meadow lake
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=meadow%20lake
    Processing record 326 | waingapu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=waingapu
    Processing record 327 | jiwani
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jiwani
    Processing record 328 | tidore
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tidore
    tidore is not found. Skipping....
    Processing record 329 | belyy yar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=belyy%20yar
    Processing record 330 | am timan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=am%20timan
    Processing record 331 | barra do garcas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=barra%20do%20garcas
    Processing record 332 | metsavan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=metsavan
    Processing record 333 | yairipok
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yairipok
    Processing record 334 | porto velho
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=porto%20velho
    Processing record 335 | songling
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=songling
    Processing record 336 | aiken
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aiken
    Processing record 337 | vrangel
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vrangel
    Processing record 338 | naze
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=naze
    Processing record 339 | victoria
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=victoria
    Processing record 340 | tukrah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tukrah
    tukrah is not found. Skipping....
    Processing record 341 | saint-georges
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-georges
    Processing record 342 | yueyang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yueyang
    Processing record 343 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tuktoyaktuk
    Processing record 344 | kizukuri
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kizukuri
    Processing record 345 | garden city
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=garden%20city
    Processing record 346 | rumonge
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rumonge
    Processing record 347 | alcaniz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alcaniz
    Processing record 348 | codrington
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=codrington
    Processing record 349 | montanha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=montanha
    Processing record 350 | gravdal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gravdal
    Processing record 351 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bambous%20virieux
    Processing record 352 | hay river
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hay%20river
    Processing record 353 | bonavista
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bonavista
    Processing record 354 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=iqaluit
    Processing record 355 | gayeri
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gayeri
    Processing record 356 | mulchen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mulchen
    Processing record 357 | diamantino
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=diamantino
    Processing record 358 | ilhabela
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ilhabela
    Processing record 359 | vredendal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vredendal
    Processing record 360 | herbertpur
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=herbertpur
    Processing record 361 | manturovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manturovo
    Processing record 362 | labytnangi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=labytnangi
    Processing record 363 | westport
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=westport
    Processing record 364 | kembe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kembe
    kembe is not found. Skipping....
    Processing record 365 | havre-saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=havre-saint-pierre
    Processing record 366 | grand-lahou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=grand-lahou
    Processing record 367 | albury
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=albury
    Processing record 368 | jacareacanga
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jacareacanga
    Processing record 369 | beian
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=beian
    Processing record 370 | huarmey
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=huarmey
    Processing record 371 | itaituba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=itaituba
    Processing record 372 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nouadhibou
    Processing record 373 | pevek
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pevek
    Processing record 374 | yushu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yushu
    Processing record 375 | kedrovyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kedrovyy
    Processing record 376 | tabiauea
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tabiauea
    tabiauea is not found. Skipping....
    Processing record 377 | pangody
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pangody
    Processing record 378 | apricena
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=apricena
    Processing record 379 | riyadh
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=riyadh
    Processing record 380 | birin
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=birin
    Processing record 381 | bara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bara
    Processing record 382 | atar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=atar
    Processing record 383 | godda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=godda
    Processing record 384 | manoel urbano
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=manoel%20urbano
    Processing record 385 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cabo%20san%20lucas
    Processing record 386 | zhaotong
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zhaotong
    Processing record 387 | mombasa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mombasa
    Processing record 388 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=batemans%20bay
    Processing record 389 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=half%20moon%20bay
    Processing record 390 | ambon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ambon
    Processing record 391 | jiazi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jiazi
    Processing record 392 | pervomayskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pervomayskoye
    Processing record 393 | ancud
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ancud
    Processing record 394 | honiara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=honiara
    Processing record 395 | veraval
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=veraval
    Processing record 396 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=los%20llanos%20de%20aridane
    Processing record 397 | port hueneme
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20hueneme
    Processing record 398 | timiryazevskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=timiryazevskiy
    Processing record 399 | krasnoselkup
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=krasnoselkup
    krasnoselkup is not found. Skipping....
    Processing record 400 | nguiu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nguiu
    nguiu is not found. Skipping....
    Processing record 401 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=deputatskiy
    Processing record 402 | itacarambi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=itacarambi
    Processing record 403 | tumannyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tumannyy
    tumannyy is not found. Skipping....
    Processing record 404 | faya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=faya
    Processing record 405 | ondangwa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ondangwa
    Processing record 406 | irbit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=irbit
    Processing record 407 | bolama
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bolama
    Processing record 408 | pochutla
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pochutla
    Processing record 409 | kamenskoye
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kamenskoye
    kamenskoye is not found. Skipping....
    Processing record 410 | pietarsaari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pietarsaari
    pietarsaari is not found. Skipping....
    Processing record 411 | goderich
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=goderich
    Processing record 412 | utran
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=utran
    Processing record 413 | yen bai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yen%20bai
    Processing record 414 | rosita
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rosita
    Processing record 415 | maneadero
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maneadero
    maneadero is not found. Skipping....
    Processing record 416 | evanston
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=evanston
    Processing record 417 | ierapetra
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ierapetra
    Processing record 418 | sovetskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sovetskiy
    Processing record 419 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=comodoro%20rivadavia
    Processing record 420 | teya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=teya
    Processing record 421 | vega de alatorre
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vega%20de%20alatorre
    Processing record 422 | les cayes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=les%20cayes
    Processing record 423 | mitsamiouli
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mitsamiouli
    Processing record 424 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20lincoln
    Processing record 425 | udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=udachnyy
    Processing record 426 | mandalgovi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mandalgovi
    Processing record 427 | coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=coahuayana
    Processing record 428 | burnie
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=burnie
    Processing record 429 | launceston
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=launceston
    Processing record 430 | serang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=serang
    Processing record 431 | beitbridge
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=beitbridge
    Processing record 432 | bay roberts
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bay%20roberts
    Processing record 433 | chlorakas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chlorakas
    chlorakas is not found. Skipping....
    Processing record 434 | mujiayingzi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mujiayingzi
    Processing record 435 | newport
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=newport
    Processing record 436 | rancho palos verdes
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rancho%20palos%20verdes
    Processing record 437 | svetlyy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=svetlyy
    svetlyy is not found. Skipping....
    Processing record 438 | ahipara
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ahipara
    Processing record 439 | umm durman
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=umm%20durman
    umm durman is not found. Skipping....
    Processing record 440 | galiwinku
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=galiwinku
    galiwinku is not found. Skipping....
    Processing record 441 | kaihua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kaihua
    Processing record 442 | faranah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=faranah
    Processing record 443 | hot springs
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hot%20springs
    Processing record 444 | ust-maya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-maya
    Processing record 445 | husavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=husavik
    Processing record 446 | avera
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=avera
    Processing record 447 | ereymentau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ereymentau
    Processing record 448 | vung tau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vung%20tau
    Processing record 449 | san pedro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20pedro
    Processing record 450 | biak
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=biak
    Processing record 451 | ataco
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ataco
    Processing record 452 | charlestown
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=charlestown
    Processing record 453 | cap-chat
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cap-chat
    Processing record 454 | arlit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=arlit
    Processing record 455 | cacu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=cacu
    Processing record 456 | bonthe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bonthe
    Processing record 457 | beyneu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=beyneu
    Processing record 458 | baruun-urt
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=baruun-urt
    Processing record 459 | siguiri
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=siguiri
    Processing record 460 | baijiantan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=baijiantan
    Processing record 461 | mizdah
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mizdah
    Processing record 462 | khomutovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=khomutovo
    Processing record 463 | dapaong
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dapaong
    Processing record 464 | gewane
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gewane
    Processing record 465 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=santa%20cruz
    Processing record 466 | saint-paul
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saint-paul
    Processing record 467 | denau
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=denau
    denau is not found. Skipping....
    Processing record 468 | karabuk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karabuk
    Processing record 469 | college
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=college
    Processing record 470 | alice springs
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alice%20springs
    Processing record 471 | kedougou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kedougou
    Processing record 472 | marsh harbour
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=marsh%20harbour
    Processing record 473 | pangai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pangai
    Processing record 474 | ouadda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ouadda
    Processing record 475 | athmallik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=athmallik
    Processing record 476 | isangel
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=isangel
    Processing record 477 | priboj
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=priboj
    Processing record 478 | tecoanapa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tecoanapa
    Processing record 479 | sao jose da coroa grande
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20jose%20da%20coroa%20grande
    Processing record 480 | caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=caravelas
    Processing record 481 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sinnamary
    Processing record 482 | luena
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=luena
    Processing record 483 | quelimane
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=quelimane
    Processing record 484 | quang ngai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=quang%20ngai
    Processing record 485 | watsa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=watsa
    Processing record 486 | sao sebastiao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20sebastiao
    Processing record 487 | kirgiz-miyaki
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kirgiz-miyaki
    Processing record 488 | chegdomyn
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=chegdomyn
    Processing record 489 | heihe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=heihe
    Processing record 490 | hokitika
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hokitika
    Processing record 491 | meadville
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=meadville
    Processing record 492 | maramba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maramba
    Processing record 493 | kjollefjord
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kjollefjord
    Processing record 494 | sambava
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sambava
    Processing record 495 | yeppoon
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yeppoon
    Processing record 496 | yenagoa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yenagoa
    Processing record 497 | sao desiderio
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20desiderio
    Processing record 498 | mount isa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mount%20isa
    Processing record 499 | fukue
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fukue
    Processing record 500 | camapua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=camapua
    Processing record 501 | sechura
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sechura
    Processing record 502 | alexandria
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alexandria
    Processing record 503 | axim
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=axim
    Processing record 504 | ranong
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ranong
    Processing record 505 | frontera comalapa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=frontera%20comalapa
    Processing record 506 | severo-yeniseyskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=severo-yeniseyskiy
    Processing record 507 | iralaya
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=iralaya
    Processing record 508 | mananjary
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mananjary
    Processing record 509 | sosva
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sosva
    Processing record 510 | broken hill
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=broken%20hill
    Processing record 511 | tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tessalit
    Processing record 512 | bonoua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bonoua
    Processing record 513 | bin qirdan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bin%20qirdan
    Processing record 514 | muisne
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=muisne
    Processing record 515 | jurm
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jurm
    Processing record 516 | sioux lookout
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sioux%20lookout
    Processing record 517 | narwar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=narwar
    Processing record 518 | boralday
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=boralday
    Processing record 519 | lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lagoa
    Processing record 520 | candolim
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=candolim
    Processing record 521 | gorontalo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gorontalo
    Processing record 522 | miles city
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=miles%20city
    Processing record 523 | mao
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mao
    Processing record 524 | nha trang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nha%20trang
    Processing record 525 | novyy urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=novyy%20urengoy
    Processing record 526 | boone
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=boone
    Processing record 527 | sibu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sibu
    Processing record 528 | vila velha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=vila%20velha
    Processing record 529 | san vicente
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20vicente
    Processing record 530 | tirunelveli
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=tirunelveli
    Processing record 531 | macomb
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=macomb
    Processing record 532 | samora correia
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=samora%20correia
    Processing record 533 | majene
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=majene
    Processing record 534 | dezhou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dezhou
    Processing record 535 | la romana
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=la%20romana
    Processing record 536 | emilio carranza
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=emilio%20carranza
    Processing record 537 | karratha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=karratha
    Processing record 538 | halalo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=halalo
    halalo is not found. Skipping....
    Processing record 539 | hohhot
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hohhot
    Processing record 540 | hegang
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hegang
    Processing record 541 | saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=saleaula
    saleaula is not found. Skipping....
    Processing record 542 | rychnov nad kneznou
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=rychnov%20nad%20kneznou
    Processing record 543 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pangnirtung
    Processing record 544 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=umm%20lajj
    Processing record 545 | nioro
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=nioro
    Processing record 546 | kahului
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kahului
    Processing record 547 | salinopolis
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=salinopolis
    Processing record 548 | madimba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=madimba
    Processing record 549 | port blair
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20blair
    Processing record 550 | puerto escondido
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=puerto%20escondido
    Processing record 551 | pierre
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pierre
    Processing record 552 | yanam
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yanam
    Processing record 553 | escanaba
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=escanaba
    Processing record 554 | venado tuerto
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=venado%20tuerto
    Processing record 555 | jalu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=jalu
    Processing record 556 | zhezkazgan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zhezkazgan
    Processing record 557 | svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=svetlogorsk
    Processing record 558 | puerto del rosario
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=puerto%20del%20rosario
    Processing record 559 | palmer
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=palmer
    Processing record 560 | galle
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=galle
    Processing record 561 | perevoz
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=perevoz
    Processing record 562 | sao felix do xingu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sao%20felix%20do%20xingu
    Processing record 563 | port keats
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=port%20keats
    Processing record 564 | maldonado
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maldonado
    Processing record 565 | liverpool
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=liverpool
    Processing record 566 | formosa
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=formosa
    Processing record 567 | kapit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kapit
    Processing record 568 | aras
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aras
    Processing record 569 | fare
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=fare
    Processing record 570 | opuwo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=opuwo
    Processing record 571 | atbasar
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=atbasar
    Processing record 572 | ilobu
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ilobu
    Processing record 573 | taurianova
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=taurianova
    Processing record 574 | dingle
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=dingle
    Processing record 575 | maningrida
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maningrida
    Processing record 576 | griffith
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=griffith
    Processing record 577 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ambilobe
    Processing record 578 | ust-tsilma
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ust-tsilma
    Processing record 579 | ucluelet
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ucluelet
    Processing record 580 | zhigalovo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zhigalovo
    Processing record 581 | ha noi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ha%20noi
    Processing record 582 | lodeynoye pole
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lodeynoye%20pole
    Processing record 583 | lapua
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=lapua
    Processing record 584 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20cristobal
    Processing record 585 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kudahuvadhoo
    Processing record 586 | thap khlo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=thap%20khlo
    Processing record 587 | san carlos
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=san%20carlos
    Processing record 588 | yerbogachen
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=yerbogachen
    Processing record 589 | sinegorskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=sinegorskiy
    Processing record 590 | aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=aklavik
    Processing record 591 | katherine
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=katherine
    Processing record 592 | oktyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=oktyabrskiy
    Processing record 593 | oranjemund
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=oranjemund
    Processing record 594 | kasiri
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kasiri
    Processing record 595 | zhuhai
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=zhuhai
    Processing record 596 | uppsala
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=uppsala
    Processing record 597 | impfondo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=impfondo
    Processing record 598 | bela vista
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bela%20vista
    Processing record 599 | bereda
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=bereda
    Processing record 600 | ler
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=ler
    Processing record 601 | paradwip
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=paradwip
    paradwip is not found. Skipping....
    Processing record 602 | alta floresta
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=alta%20floresta
    Processing record 603 | malanje
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=malanje
    Processing record 604 | savannah bight
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=savannah%20bight
    Processing record 605 | kolimvari
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=kolimvari
    kolimvari is not found. Skipping....
    Processing record 606 | hami
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=hami
    Processing record 607 | mayo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=mayo
    Processing record 608 | el carrizo
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=el%20carrizo
    Processing record 609 | omboue
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=omboue
    Processing record 610 | malwan
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=malwan
    malwan is not found. Skipping....
    Processing record 611 | gigmoto
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=gigmoto
    Processing record 612 | camacha
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=camacha
    Processing record 613 | pont-saint-esprit
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=pont-saint-esprit
    Processing record 614 | inuvik
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=inuvik
    Processing record 615 | maragogi
    http://api.openweathermap.org/data/2.5/weather?appid=7b570103f8d90d61702f430853affd86&units=imperial&q=maragogi
    


```python
city_data_pd = pd.DataFrame({"City":city_name,
            "Cloudiness":cloudiness,
             "Country": country,
             "Date": date,
             "Humidity": humidity,
             "Lat": lat,
             "Lng": lng,
             "Max_Temp": max_temp,
             "Wind_speed": wind_speed})
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
      <td>Isla Vista</td>
      <td>90</td>
      <td>US</td>
      <td>1521694560</td>
      <td>93</td>
      <td>34.41</td>
      <td>-119.86</td>
      <td>60.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Vaini</td>
      <td>0</td>
      <td>IN</td>
      <td>1521694685</td>
      <td>29</td>
      <td>15.34</td>
      <td>74.49</td>
      <td>91.05</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lebu</td>
      <td>0</td>
      <td>ET</td>
      <td>1521695651</td>
      <td>35</td>
      <td>8.96</td>
      <td>38.73</td>
      <td>64.05</td>
      <td>3.94</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ushuaia</td>
      <td>40</td>
      <td>AR</td>
      <td>1521691200</td>
      <td>66</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>55.40</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rikitea</td>
      <td>76</td>
      <td>PF</td>
      <td>1521694683</td>
      <td>99</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>81.06</td>
      <td>15.23</td>
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

# Display the plot
plt.show()
```


![png](output_11_0.png)


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

# Display the plot
plt.show()
```


![png](output_13_0.png)


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

# Display the plot
plt.show()
```


![png](output_15_0.png)


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

# Display the plot
plt.show()
```


![png](output_17_0.png)

