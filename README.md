# WeatherPy


```python
#Import Dependencies
import pandas as pd
import requests as req
from citipy import citipy 
import matplotlib.pyplot as plt
import random
import seaborn as sns
import time as time
key="f2a9a6283d7386b40fb4317c3f545a0a"
```


```python
#create list of random latitudes and longitudes
city_data= pd.DataFrame(columns =['Lat',"Lng","City","Temperature","Humidity","Clouds","Wind Speed"])
lat = []
lng = []
for x in range(0,1100):
    lat.append(random.uniform(-90,90))
    lng.append(random.uniform(-180,180))
city_data['Lat']=lat
city_data['Lng']=lng

#find cities related to coordinates
cities = []
for index, row in data.iterrows():
    city=citipy.nearest_city(row["Lat"],row["Lng"])
    cities.append(city.city_name)
city_data['City']=cities

#check for and remove duplicates
city_data_2 = data.drop_duplicates("City",keep="first")

city_data_2.head()
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
      <th>Lat</th>
      <th>Lng</th>
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
      <td>24.255733</td>
      <td>-36.110380</td>
      <td>ponta do sol</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.277629</td>
      <td>-159.102792</td>
      <td>hilo</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.653160</td>
      <td>-76.704095</td>
      <td>marsh harbour</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>55.418722</td>
      <td>129.920305</td>
      <td>fevralsk</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-73.119245</td>
      <td>-157.569374</td>
      <td>mataura</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
len(city_data_2)
```




    504




```python
temp=[]
humidity =[]
clouds = []
wind = []

counter = 0
url = "https://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

#iterate through the rows to pull data from the api 
try:
    for index, row in city_data_2.iterrows():
        counter +=1
        city = row["City"]
        target_url = url+"lat="+str(row["Lat"])+"&lon="+str(row["Lng"])+"&appid="+key+"&units="+units
        print("we are now on city number "+str(counter))
        print("The name of the city is "+row["City"])
        print(target_url)
        print("__________________________________________________________________________________________")
        info = req.get(target_url).json()
        temp.append(info['main']['temp'])
        humidity.append(info['main']['humidity'])
        clouds.append(info['clouds']['all'])
        wind.append(info['wind']['speed'])
        time.sleep(1)
except:
    pass
#assign the values from the arrays to the data frame
city_data_2["Temperature"]=temp
city_data_2["Humidity"]=humidity
city_data_2["Clouds"]=clouds
city_data_2["Wind Speed"]=wind
```

    we are now on city number 1
    The name of the city is ponta do sol
    https://api.openweathermap.org/data/2.5/weather?lat=24.255733239127878&lon=-36.11037984094634&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 2
    The name of the city is hilo
    https://api.openweathermap.org/data/2.5/weather?lat=6.277628621664164&lon=-159.1027920434618&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 3
    The name of the city is marsh harbour
    https://api.openweathermap.org/data/2.5/weather?lat=26.653160136336894&lon=-76.70409518052617&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 4
    The name of the city is fevralsk
    https://api.openweathermap.org/data/2.5/weather?lat=55.418722328912&lon=129.92030526006698&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 5
    The name of the city is mataura
    https://api.openweathermap.org/data/2.5/weather?lat=-73.11924543003965&lon=-157.5693744015229&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 6
    The name of the city is kapaa
    https://api.openweathermap.org/data/2.5/weather?lat=15.542206394627186&lon=-168.68725081825247&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 7
    The name of the city is hermanus
    https://api.openweathermap.org/data/2.5/weather?lat=-71.96649882734474&lon=1.672993988083931&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 8
    The name of the city is viedma
    https://api.openweathermap.org/data/2.5/weather?lat=-46.061357129784064&lon=-58.95390431598845&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 9
    The name of the city is tsiroanomandidy
    https://api.openweathermap.org/data/2.5/weather?lat=-18.46530674136298&lon=46.20590804500557&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 10
    The name of the city is georgetown
    https://api.openweathermap.org/data/2.5/weather?lat=-18.933588879544246&lon=-17.597547960999577&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 11
    The name of the city is qasigiannguit
    https://api.openweathermap.org/data/2.5/weather?lat=67.40221010322117&lon=-50.121311785082554&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 12
    The name of the city is nikolskoye
    https://api.openweathermap.org/data/2.5/weather?lat=35.509057775849556&lon=173.70382261452505&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 13
    The name of the city is tornio
    https://api.openweathermap.org/data/2.5/weather?lat=67.25269534868872&lon=23.45968271096953&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 14
    The name of the city is mehamn
    https://api.openweathermap.org/data/2.5/weather?lat=72.85093454774642&lon=27.738206511149002&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 15
    The name of the city is santa cruz
    https://api.openweathermap.org/data/2.5/weather?lat=14.797986379796953&lon=-88.99631987025319&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 16
    The name of the city is barra do bugres
    https://api.openweathermap.org/data/2.5/weather?lat=-15.59259459578422&lon=-57.12773630678922&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 17
    The name of the city is rikitea
    https://api.openweathermap.org/data/2.5/weather?lat=-55.92257897425207&lon=-112.25478159557389&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 18
    The name of the city is saskylakh
    https://api.openweathermap.org/data/2.5/weather?lat=84.65767286227617&lon=109.04160452429227&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 19
    The name of the city is clyde river
    https://api.openweathermap.org/data/2.5/weather?lat=72.04146757748475&lon=-82.07966715546398&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 20
    The name of the city is maues
    https://api.openweathermap.org/data/2.5/weather?lat=-4.379403755276229&lon=-58.052898631108704&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 21
    The name of the city is lianran
    https://api.openweathermap.org/data/2.5/weather?lat=25.046128271280608&lon=102.03664434215568&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 22
    The name of the city is nova bana
    https://api.openweathermap.org/data/2.5/weather?lat=48.32311943669518&lon=18.7424508696258&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 23
    The name of the city is puerto ayora
    https://api.openweathermap.org/data/2.5/weather?lat=-12.342702816869263&lon=-96.03954158109276&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 24
    The name of the city is saint-philippe
    https://api.openweathermap.org/data/2.5/weather?lat=-57.847953255935096&lon=68.41372929021139&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 25
    The name of the city is ushuaia
    https://api.openweathermap.org/data/2.5/weather?lat=-86.39696461617238&lon=-16.934706835401556&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 26
    The name of the city is vaitupu
    https://api.openweathermap.org/data/2.5/weather?lat=-4.481661816325683&lon=-174.82368954815672&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 27
    The name of the city is puerto leguizamo
    https://api.openweathermap.org/data/2.5/weather?lat=-1.3486608130679087&lon=-73.57084556485401&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 28
    The name of the city is punta arenas
    https://api.openweathermap.org/data/2.5/weather?lat=-70.02676177593383&lon=-108.72313352643906&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 29
    The name of the city is mar del plata
    https://api.openweathermap.org/data/2.5/weather?lat=-42.98586053617187&lon=-49.6385869164927&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 30
    The name of the city is sumbawa
    https://api.openweathermap.org/data/2.5/weather?lat=-7.56820351826056&lon=116.82099917238679&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 31
    The name of the city is barrow
    https://api.openweathermap.org/data/2.5/weather?lat=89.85581496721915&lon=-168.26293616733537&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 32
    The name of the city is dongning
    https://api.openweathermap.org/data/2.5/weather?lat=43.904131392105825&lon=130.58724114787384&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 33
    The name of the city is buala
    https://api.openweathermap.org/data/2.5/weather?lat=-3.4863120529993097&lon=162.39279040713996&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 34
    The name of the city is nizhnyaya tavda
    https://api.openweathermap.org/data/2.5/weather?lat=57.375669419292336&lon=66.12486985555631&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 35
    The name of the city is yellowknife
    https://api.openweathermap.org/data/2.5/weather?lat=73.64630116172435&lon=-105.3833425001216&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 36
    The name of the city is east london
    https://api.openweathermap.org/data/2.5/weather?lat=-34.444284780586564&lon=28.412973808299768&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 37
    The name of the city is saldanha
    https://api.openweathermap.org/data/2.5/weather?lat=-37.935971151123894&lon=6.607101883463173&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 38
    The name of the city is hobart
    https://api.openweathermap.org/data/2.5/weather?lat=-85.86909126548748&lon=132.11500160703253&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 39
    The name of the city is inhambane
    https://api.openweathermap.org/data/2.5/weather?lat=-27.967110144456697&lon=38.10123712521164&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 40
    The name of the city is astoria
    https://api.openweathermap.org/data/2.5/weather?lat=45.81451314552538&lon=-124.75035469743437&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 41
    The name of the city is juifang
    https://api.openweathermap.org/data/2.5/weather?lat=26.38727897575994&lon=123.36971332427919&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 42
    The name of the city is souillac
    https://api.openweathermap.org/data/2.5/weather?lat=-25.34617405434014&lon=59.989329303404816&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 43
    The name of the city is raga
    https://api.openweathermap.org/data/2.5/weather?lat=7.5955966724486075&lon=25.883495089794735&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 44
    The name of the city is alice springs
    https://api.openweathermap.org/data/2.5/weather?lat=-23.38305280837335&lon=134.3129816421668&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 45
    The name of the city is arraial do cabo
    https://api.openweathermap.org/data/2.5/weather?lat=-40.283219269794756&lon=-20.502939447553615&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 46
    The name of the city is pinawa
    https://api.openweathermap.org/data/2.5/weather?lat=49.96077766938379&lon=-95.42023560723446&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 47
    The name of the city is salalah
    https://api.openweathermap.org/data/2.5/weather?lat=13.393186765809943&lon=56.255863597068696&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 48
    The name of the city is port lincoln
    https://api.openweathermap.org/data/2.5/weather?lat=-34.86204221042383&lon=136.62062654220995&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 49
    The name of the city is portales
    https://api.openweathermap.org/data/2.5/weather?lat=34.21783867592502&lon=-103.46102532750987&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 50
    The name of the city is taolanaro
    https://api.openweathermap.org/data/2.5/weather?lat=-77.67614815244829&lon=73.28158536004338&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 51
    The name of the city is kodiak
    https://api.openweathermap.org/data/2.5/weather?lat=50.24366967764715&lon=-158.17276297667522&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 52
    The name of the city is guerrero negro
    https://api.openweathermap.org/data/2.5/weather?lat=14.797927657707561&lon=-128.05713193910807&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 53
    The name of the city is severo-kurilsk
    https://api.openweathermap.org/data/2.5/weather?lat=30.469804547634&lon=169.16966605255118&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 54
    The name of the city is tazovskiy
    https://api.openweathermap.org/data/2.5/weather?lat=68.17290222631996&lon=78.65734383509044&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 55
    The name of the city is vaini
    https://api.openweathermap.org/data/2.5/weather?lat=-47.681811838760595&lon=-175.93494357921958&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 56
    The name of the city is vangaindrano
    https://api.openweathermap.org/data/2.5/weather?lat=-23.160751113638867&lon=47.66445655512669&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 57
    The name of the city is cherskiy
    https://api.openweathermap.org/data/2.5/weather?lat=85.43665531807989&lon=162.3489272711023&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 58
    The name of the city is scarborough
    https://api.openweathermap.org/data/2.5/weather?lat=56.25670267944116&lon=0.4217561516956607&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 59
    The name of the city is atuona
    https://api.openweathermap.org/data/2.5/weather?lat=-1.495771081222415&lon=-130.63543967984958&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 60
    The name of the city is clyde
    https://api.openweathermap.org/data/2.5/weather?lat=-45.03201533927052&lon=169.06511757647547&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 61
    The name of the city is seoul
    https://api.openweathermap.org/data/2.5/weather?lat=34.546864697841826&lon=125.22627860326503&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 62
    The name of the city is umzimvubu
    https://api.openweathermap.org/data/2.5/weather?lat=-39.57977100985068&lon=35.00229777928601&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 63
    The name of the city is bredasdorp
    https://api.openweathermap.org/data/2.5/weather?lat=-85.49424278995234&lon=23.869920988331017&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 64
    The name of the city is tutoia
    https://api.openweathermap.org/data/2.5/weather?lat=2.8641611110096648&lon=-41.66195175370956&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 65
    The name of the city is busselton
    https://api.openweathermap.org/data/2.5/weather?lat=-73.74700041635445&lon=92.32222531511007&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 66
    The name of the city is alta floresta
    https://api.openweathermap.org/data/2.5/weather?lat=-10.772777507932389&lon=-55.34295511893099&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 67
    The name of the city is namibe
    https://api.openweathermap.org/data/2.5/weather?lat=-16.00072785084636&lon=10.891390406310961&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 68
    The name of the city is khatanga
    https://api.openweathermap.org/data/2.5/weather?lat=82.73974075956266&lon=107.10841907013133&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 69
    The name of the city is caravelas
    https://api.openweathermap.org/data/2.5/weather?lat=-18.680961628798244&lon=-36.64766206898659&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 70
    The name of the city is nara
    https://api.openweathermap.org/data/2.5/weather?lat=17.112748974321903&lon=-8.290136891458445&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 71
    The name of the city is longyearbyen
    https://api.openweathermap.org/data/2.5/weather?lat=80.2882632999987&lon=21.62438647745003&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 72
    The name of the city is aksarka
    https://api.openweathermap.org/data/2.5/weather?lat=66.53945306196513&lon=68.70697724454038&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 73
    The name of the city is tilichiki
    https://api.openweathermap.org/data/2.5/weather?lat=59.10984742786641&lon=172.89820744583852&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 74
    The name of the city is nome
    https://api.openweathermap.org/data/2.5/weather?lat=61.926549182425106&lon=-165.1251130805199&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 75
    The name of the city is abiy adi
    https://api.openweathermap.org/data/2.5/weather?lat=13.220204248711099&lon=38.43562223638585&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 76
    The name of the city is caucaia
    https://api.openweathermap.org/data/2.5/weather?lat=1.6921162088832915&lon=-34.7672086614387&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 77
    The name of the city is hirara
    https://api.openweathermap.org/data/2.5/weather?lat=19.683454373448683&lon=127.96635621149136&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 78
    The name of the city is zarya
    https://api.openweathermap.org/data/2.5/weather?lat=60.074979409086666&lon=48.08984503249064&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 79
    The name of the city is olafsvik
    https://api.openweathermap.org/data/2.5/weather?lat=61.07173935235224&lon=-27.376838350798636&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 80
    The name of the city is albany
    https://api.openweathermap.org/data/2.5/weather?lat=-89.99089915639374&lon=116.1837377440853&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 81
    The name of the city is aripuana
    https://api.openweathermap.org/data/2.5/weather?lat=-10.34104674899568&lon=-59.969463821115895&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 82
    The name of the city is kawalu
    https://api.openweathermap.org/data/2.5/weather?lat=-12.6791182755031&lon=106.82281081700216&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 83
    The name of the city is rosarito
    https://api.openweathermap.org/data/2.5/weather?lat=30.11766731491676&lon=-119.3621123229403&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 84
    The name of the city is guinguineo
    https://api.openweathermap.org/data/2.5/weather?lat=14.139389921988013&lon=-15.818266018078674&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 85
    The name of the city is cape town
    https://api.openweathermap.org/data/2.5/weather?lat=-46.57782896730098&lon=5.070614215651972&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 86
    The name of the city is prince rupert
    https://api.openweathermap.org/data/2.5/weather?lat=54.696735157463905&lon=-130.08432962978472&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 87
    The name of the city is kafanchan
    https://api.openweathermap.org/data/2.5/weather?lat=9.834302785045097&lon=8.227406854432274&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 88
    The name of the city is bagamoyo
    https://api.openweathermap.org/data/2.5/weather?lat=-6.509186472733191&lon=39.141500499981646&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 89
    The name of the city is jamestown
    https://api.openweathermap.org/data/2.5/weather?lat=-33.220061956759785&lon=-2.099859574050299&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 90
    The name of the city is kaitangata
    https://api.openweathermap.org/data/2.5/weather?lat=-67.46378434562277&lon=177.33671923990846&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 91
    The name of the city is saint-prosper
    https://api.openweathermap.org/data/2.5/weather?lat=46.22744292133177&lon=-70.25954780398254&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 92
    The name of the city is roald
    https://api.openweathermap.org/data/2.5/weather?lat=64.78723451725119&lon=5.033490603688705&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 93
    The name of the city is lorengau
    https://api.openweathermap.org/data/2.5/weather?lat=-1.7210658303979613&lon=145.08518185616003&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 94
    The name of the city is pochutla
    https://api.openweathermap.org/data/2.5/weather?lat=13.638091255749487&lon=-96.50529594932537&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 95
    The name of the city is pedernales
    https://api.openweathermap.org/data/2.5/weather?lat=16.410384793140835&lon=-71.1288167513864&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 96
    The name of the city is saint-georges
    https://api.openweathermap.org/data/2.5/weather?lat=5.434405446759868&lon=-50.055808195921486&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 97
    The name of the city is qaanaaq
    https://api.openweathermap.org/data/2.5/weather?lat=85.59984416987473&lon=-93.3797737899988&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 98
    The name of the city is cerveteri
    https://api.openweathermap.org/data/2.5/weather?lat=41.99984672294269&lon=12.005399643081972&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 99
    The name of the city is puerto narino
    https://api.openweathermap.org/data/2.5/weather?lat=-2.9285481694787308&lon=-70.2809217220079&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 100
    The name of the city is tasiilaq
    https://api.openweathermap.org/data/2.5/weather?lat=67.65307330083695&lon=-43.360433365048266&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 101
    The name of the city is port elizabeth
    https://api.openweathermap.org/data/2.5/weather?lat=-46.877216328730896&lon=27.8670002710694&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 102
    The name of the city is pisco
    https://api.openweathermap.org/data/2.5/weather?lat=-13.921070320413364&lon=-76.6798801120997&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 103
    The name of the city is katsuura
    https://api.openweathermap.org/data/2.5/weather?lat=20.369467227331427&lon=154.60542408384288&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 104
    The name of the city is san diego
    https://api.openweathermap.org/data/2.5/weather?lat=10.292055170090833&lon=-73.1200590738529&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 105
    The name of the city is new norfolk
    https://api.openweathermap.org/data/2.5/weather?lat=-64.81163790602224&lon=130.00807865981693&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 106
    The name of the city is xining
    https://api.openweathermap.org/data/2.5/weather?lat=33.25512642602523&lon=100.31891884035372&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 107
    The name of the city is mahebourg
    https://api.openweathermap.org/data/2.5/weather?lat=-40.34783405769484&lon=76.39542456496429&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 108
    The name of the city is thompson
    https://api.openweathermap.org/data/2.5/weather?lat=68.72572291074383&lon=-89.30303175867968&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 109
    The name of the city is hammerfest
    https://api.openweathermap.org/data/2.5/weather?lat=70.60354683605505&lon=24.173362151668613&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 110
    The name of the city is ribeira grande
    https://api.openweathermap.org/data/2.5/weather?lat=40.45015843464779&lon=-32.11772235038444&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 111
    The name of the city is saint george
    https://api.openweathermap.org/data/2.5/weather?lat=37.75851988305848&lon=-61.36014626515673&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 112
    The name of the city is tiksi
    https://api.openweathermap.org/data/2.5/weather?lat=76.31965985151254&lon=129.89449246776292&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 113
    The name of the city is cheremisinovo
    https://api.openweathermap.org/data/2.5/weather?lat=51.81954299176053&lon=37.33254329203953&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 114
    The name of the city is goba
    https://api.openweathermap.org/data/2.5/weather?lat=7.08405263891062&lon=40.171415031342235&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 115
    The name of the city is zhigansk
    https://api.openweathermap.org/data/2.5/weather?lat=73.3049238710505&lon=121.73955612409856&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 116
    The name of the city is waseca
    https://api.openweathermap.org/data/2.5/weather?lat=44.19527145764147&lon=-93.62301548842578&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 117
    The name of the city is lebu
    https://api.openweathermap.org/data/2.5/weather?lat=-35.748347621828&lon=-89.27630990133625&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 118
    The name of the city is coruripe
    https://api.openweathermap.org/data/2.5/weather?lat=-10.349152814099071&lon=-35.78685477566907&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 119
    The name of the city is port macquarie
    https://api.openweathermap.org/data/2.5/weather?lat=-32.44057091253076&lon=156.03897783270264&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 120
    The name of the city is chokurdakh
    https://api.openweathermap.org/data/2.5/weather?lat=75.51363314155478&lon=145.12286085040432&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 121
    The name of the city is vao
    https://api.openweathermap.org/data/2.5/weather?lat=-22.718363873119443&lon=168.48793611352193&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 122
    The name of the city is belushya guba
    https://api.openweathermap.org/data/2.5/weather?lat=77.62990060554847&lon=45.67744772178486&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 123
    The name of the city is pevek
    https://api.openweathermap.org/data/2.5/weather?lat=74.7684236527997&lon=167.86801578583214&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 124
    The name of the city is faanui
    https://api.openweathermap.org/data/2.5/weather?lat=-10.232955001120843&lon=-151.0379450642452&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 125
    The name of the city is katangli
    https://api.openweathermap.org/data/2.5/weather?lat=52.565906637492475&lon=145.97052945282996&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 126
    The name of the city is palmer
    https://api.openweathermap.org/data/2.5/weather?lat=57.02692975586314&lon=-146.72785756932245&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 127
    The name of the city is oga
    https://api.openweathermap.org/data/2.5/weather?lat=40.83553559514229&lon=138.39066676698013&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 128
    The name of the city is mount isa
    https://api.openweathermap.org/data/2.5/weather?lat=-20.343028528573285&lon=140.6702909010528&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 129
    The name of the city is sao felix do xingu
    https://api.openweathermap.org/data/2.5/weather?lat=-6.211494970657441&lon=-53.190241303049234&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 130
    The name of the city is mandalgovi
    https://api.openweathermap.org/data/2.5/weather?lat=43.058231380561466&lon=105.80923924307268&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 131
    The name of the city is avarua
    https://api.openweathermap.org/data/2.5/weather?lat=-37.87200040295917&lon=-161.55102079538142&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 132
    The name of the city is klyuchi
    https://api.openweathermap.org/data/2.5/weather?lat=56.62040035439395&lon=159.81800868569127&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 133
    The name of the city is kloulklubed
    https://api.openweathermap.org/data/2.5/weather?lat=6.89885724791732&lon=130.85712963953085&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 134
    The name of the city is sao jose da coroa grande
    https://api.openweathermap.org/data/2.5/weather?lat=-11.451522456933773&lon=-27.708050047335774&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 135
    The name of the city is berlevag
    https://api.openweathermap.org/data/2.5/weather?lat=85.65954357558999&lon=32.46233156006247&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 136
    The name of the city is lucapa
    https://api.openweathermap.org/data/2.5/weather?lat=-8.441782142588579&lon=21.278187748554814&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 137
    The name of the city is alpena
    https://api.openweathermap.org/data/2.5/weather?lat=44.91430563229653&lon=-83.27025789395506&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 138
    The name of the city is chuy
    https://api.openweathermap.org/data/2.5/weather?lat=-52.43093912706663&lon=-36.663763702471556&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 139
    The name of the city is hithadhoo
    https://api.openweathermap.org/data/2.5/weather?lat=-4.443035527351597&lon=66.44966167103792&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 140
    The name of the city is nantucket
    https://api.openweathermap.org/data/2.5/weather?lat=38.877450493207334&lon=-70.69270640727537&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 141
    The name of the city is sumkino
    https://api.openweathermap.org/data/2.5/weather?lat=57.971997178115686&lon=68.06915136489786&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 142
    The name of the city is klaksvik
    https://api.openweathermap.org/data/2.5/weather?lat=72.71480854457835&lon=-2.036709156541889&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 143
    The name of the city is upernavik
    https://api.openweathermap.org/data/2.5/weather?lat=84.56202409394649&lon=-55.10091755101938&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 144
    The name of the city is nanortalik
    https://api.openweathermap.org/data/2.5/weather?lat=52.881442367990985&lon=-41.529964408498074&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 145
    The name of the city is barentsburg
    https://api.openweathermap.org/data/2.5/weather?lat=74.5053282275093&lon=3.123720849654177&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 146
    The name of the city is mantua
    https://api.openweathermap.org/data/2.5/weather?lat=24.319977247956146&lon=-85.21723744882917&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 147
    The name of the city is sinazongwe
    https://api.openweathermap.org/data/2.5/weather?lat=-17.046262729757075&lon=27.463819790222487&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 148
    The name of the city is katastarion
    https://api.openweathermap.org/data/2.5/weather?lat=37.57060845003281&lon=20.193713031888052&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 149
    The name of the city is oktyabrskoye
    https://api.openweathermap.org/data/2.5/weather?lat=63.6262766149504&lon=67.22709965809935&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 150
    The name of the city is castro
    https://api.openweathermap.org/data/2.5/weather?lat=-45.69195309356228&lon=-92.45752596533416&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 151
    The name of the city is bengkulu
    https://api.openweathermap.org/data/2.5/weather?lat=-7.951883445610093&lon=96.51064218561851&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 152
    The name of the city is te anau
    https://api.openweathermap.org/data/2.5/weather?lat=-40.175065472663995&lon=160.1153249032651&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 153
    The name of the city is mecca
    https://api.openweathermap.org/data/2.5/weather?lat=22.646077375710036&lon=39.86609954854694&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 154
    The name of the city is illoqqortoormiut
    https://api.openweathermap.org/data/2.5/weather?lat=85.37669017813849&lon=-14.530199437316554&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 155
    The name of the city is provideniya
    https://api.openweathermap.org/data/2.5/weather?lat=41.6687353061663&lon=-176.3081627020325&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 156
    The name of the city is riyadh
    https://api.openweathermap.org/data/2.5/weather?lat=21.537078557916516&lon=48.70457264114464&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 157
    The name of the city is malangali
    https://api.openweathermap.org/data/2.5/weather?lat=-7.946659761514681&lon=34.89354135550863&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 158
    The name of the city is dukat
    https://api.openweathermap.org/data/2.5/weather?lat=62.28116592960055&lon=154.75866375557882&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 159
    The name of the city is nizhneyansk
    https://api.openweathermap.org/data/2.5/weather?lat=88.81748397164691&lon=133.0826489235268&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 160
    The name of the city is mopti
    https://api.openweathermap.org/data/2.5/weather?lat=15.486908485566588&lon=-4.131980300609371&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 161
    The name of the city is fevik
    https://api.openweathermap.org/data/2.5/weather?lat=57.947348244632934&lon=9.150563747818069&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 162
    The name of the city is novoseleznevo
    https://api.openweathermap.org/data/2.5/weather?lat=55.80406364865931&lon=69.12990518182406&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 163
    The name of the city is bawku
    https://api.openweathermap.org/data/2.5/weather?lat=11.12941678855671&lon=-0.22554539349164315&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 164
    The name of the city is shingu
    https://api.openweathermap.org/data/2.5/weather?lat=28.18612699054512&lon=139.08292346478254&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 165
    The name of the city is west odessa
    https://api.openweathermap.org/data/2.5/weather?lat=32.08036885656492&lon=-102.46737460280232&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 166
    The name of the city is attawapiskat
    https://api.openweathermap.org/data/2.5/weather?lat=60.224050884568044&lon=-79.87151831532944&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 167
    The name of the city is knysna
    https://api.openweathermap.org/data/2.5/weather?lat=-36.41712231119258&lon=22.60717102942229&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 168
    The name of the city is pacific grove
    https://api.openweathermap.org/data/2.5/weather?lat=34.05570688743701&lon=-124.29891886848158&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 169
    The name of the city is ancud
    https://api.openweathermap.org/data/2.5/weather?lat=-39.60035894575242&lon=-83.77911933352631&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 170
    The name of the city is beringovskiy
    https://api.openweathermap.org/data/2.5/weather?lat=50.43707646049947&lon=178.4660231460408&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 171
    The name of the city is lompoc
    https://api.openweathermap.org/data/2.5/weather?lat=25.197221473382953&lon=-127.87615618747844&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 172
    The name of the city is kavieng
    https://api.openweathermap.org/data/2.5/weather?lat=13.761807621198457&lon=155.5217770413356&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 173
    The name of the city is gumushane
    https://api.openweathermap.org/data/2.5/weather?lat=40.54808765131608&lon=39.25743113031663&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 174
    The name of the city is bambous virieux
    https://api.openweathermap.org/data/2.5/weather?lat=-34.812500175948564&lon=84.00654136969604&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 175
    The name of the city is vallenar
    https://api.openweathermap.org/data/2.5/weather?lat=-28.229561249957754&lon=-70.11709414640703&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 176
    The name of the city is katherine
    https://api.openweathermap.org/data/2.5/weather?lat=-14.116485836018768&lon=131.95046917714052&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 177
    The name of the city is urucui
    https://api.openweathermap.org/data/2.5/weather?lat=-7.725714361043742&lon=-44.27604981417281&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 178
    The name of the city is lavrentiya
    https://api.openweathermap.org/data/2.5/weather?lat=73.35204553679498&lon=-167.96148001580448&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 179
    The name of the city is ardakan
    https://api.openweathermap.org/data/2.5/weather?lat=32.52490954048916&lon=53.49104573554723&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 180
    The name of the city is labuhan
    https://api.openweathermap.org/data/2.5/weather?lat=-14.05874262687503&lon=99.58486288333842&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 181
    The name of the city is benton
    https://api.openweathermap.org/data/2.5/weather?lat=34.58334071345841&lon=-92.69290134454057&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 182
    The name of the city is itarema
    https://api.openweathermap.org/data/2.5/weather?lat=5.533336464074026&lon=-38.77439697156723&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 183
    The name of the city is grand river south east
    https://api.openweathermap.org/data/2.5/weather?lat=-19.345626732693674&lon=76.80204515159096&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 184
    The name of the city is sinnamary
    https://api.openweathermap.org/data/2.5/weather?lat=15.913933208677733&lon=-45.82681491692995&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 185
    The name of the city is luzilandia
    https://api.openweathermap.org/data/2.5/weather?lat=-3.5021139155265786&lon=-42.155001111463775&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 186
    The name of the city is bluff
    https://api.openweathermap.org/data/2.5/weather?lat=-55.62616818104658&lon=162.99399506447105&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 187
    The name of the city is dalinghe
    https://api.openweathermap.org/data/2.5/weather?lat=41.50566646914646&lon=121.23803324130546&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 188
    The name of the city is bacuit
    https://api.openweathermap.org/data/2.5/weather?lat=11.855835310955598&lon=117.70943679838672&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 189
    The name of the city is aparecida do taboado
    https://api.openweathermap.org/data/2.5/weather?lat=-19.891873743379122&lon=-50.92908911687172&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 190
    The name of the city is taoudenni
    https://api.openweathermap.org/data/2.5/weather?lat=22.3096256357439&lon=-1.344483461038294&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 191
    The name of the city is torbay
    https://api.openweathermap.org/data/2.5/weather?lat=48.57944179698757&lon=-40.905085406497335&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 192
    The name of the city is cabo rojo
    https://api.openweathermap.org/data/2.5/weather?lat=16.887049122913197&lon=-67.81348247952303&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 193
    The name of the city is isangel
    https://api.openweathermap.org/data/2.5/weather?lat=-18.856444018636765&lon=172.18761110831872&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 194
    The name of the city is deyang
    https://api.openweathermap.org/data/2.5/weather?lat=31.203026052673394&lon=104.53362239578422&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 195
    The name of the city is lundazi
    https://api.openweathermap.org/data/2.5/weather?lat=-12.081988057174371&lon=32.98850133231966&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 196
    The name of the city is cabedelo
    https://api.openweathermap.org/data/2.5/weather?lat=-6.041238685149878&lon=-32.78604057302459&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 197
    The name of the city is moindou
    https://api.openweathermap.org/data/2.5/weather?lat=-23.98736540980842&lon=164.40874802912066&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 198
    The name of the city is pyshma
    https://api.openweathermap.org/data/2.5/weather?lat=57.19213154654582&lon=63.24043268619789&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 199
    The name of the city is zihuatanejo
    https://api.openweathermap.org/data/2.5/weather?lat=17.517060280375702&lon=-101.53061485765099&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 200
    The name of the city is arinos
    https://api.openweathermap.org/data/2.5/weather?lat=-15.720878739249457&lon=-46.01482197849151&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 201
    The name of the city is mlonggo
    https://api.openweathermap.org/data/2.5/weather?lat=-6.433653614736414&lon=110.46986433502121&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 202
    The name of the city is airai
    https://api.openweathermap.org/data/2.5/weather?lat=7.915437710199953&lon=141.81253368729426&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 203
    The name of the city is hasaki
    https://api.openweathermap.org/data/2.5/weather?lat=35.72037095475429&lon=144.085813928815&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 204
    The name of the city is verkhoyansk
    https://api.openweathermap.org/data/2.5/weather?lat=67.7908840578678&lon=133.18648629261884&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 205
    The name of the city is liberty
    https://api.openweathermap.org/data/2.5/weather?lat=39.55744672699271&lon=-94.48559081844797&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 206
    The name of the city is moree
    https://api.openweathermap.org/data/2.5/weather?lat=-28.824620452181037&lon=148.56192965698062&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 207
    The name of the city is rawson
    https://api.openweathermap.org/data/2.5/weather?lat=-50.161378949163044&lon=-58.64298917905431&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 208
    The name of the city is kankon
    https://api.openweathermap.org/data/2.5/weather?lat=14.776734225917025&lon=73.47630195553677&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 209
    The name of the city is mys shmidta
    https://api.openweathermap.org/data/2.5/weather?lat=88.14338998739404&lon=-175.0704472021055&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 210
    The name of the city is kruisfontein
    https://api.openweathermap.org/data/2.5/weather?lat=-47.415509121669515&lon=24.601416825489707&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 211
    The name of the city is poum
    https://api.openweathermap.org/data/2.5/weather?lat=-19.01354278624875&lon=156.8154380491642&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 212
    The name of the city is mikhaylovka
    https://api.openweathermap.org/data/2.5/weather?lat=42.908513555689126&lon=71.58837591956043&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 213
    The name of the city is evensk
    https://api.openweathermap.org/data/2.5/weather?lat=63.84864519424306&lon=160.8813211217473&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 214
    The name of the city is dikson
    https://api.openweathermap.org/data/2.5/weather?lat=72.41572736610377&lon=78.05724713329778&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 215
    The name of the city is alice town
    https://api.openweathermap.org/data/2.5/weather?lat=25.808642508550392&lon=-79.36441124016507&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 216
    The name of the city is zvishavane
    https://api.openweathermap.org/data/2.5/weather?lat=-20.614100267561454&lon=30.310351212960995&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 217
    The name of the city is fortuna
    https://api.openweathermap.org/data/2.5/weather?lat=40.48423223229335&lon=-130.92177640017468&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 218
    The name of the city is am timan
    https://api.openweathermap.org/data/2.5/weather?lat=10.639464835121743&lon=19.663195222824555&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 219
    The name of the city is tuktoyaktuk
    https://api.openweathermap.org/data/2.5/weather?lat=88.97329459366938&lon=-132.3517579455529&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 220
    The name of the city is hami
    https://api.openweathermap.org/data/2.5/weather?lat=38.941658869760914&lon=91.31446006661344&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 221
    The name of the city is galich
    https://api.openweathermap.org/data/2.5/weather?lat=58.64451227497841&lon=42.049688187285&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 222
    The name of the city is minsk
    https://api.openweathermap.org/data/2.5/weather?lat=54.64447767727714&lon=27.648624647594715&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 223
    The name of the city is los zacatones
    https://api.openweathermap.org/data/2.5/weather?lat=23.179505793343594&lon=-101.75468383662907&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 224
    The name of the city is hofn
    https://api.openweathermap.org/data/2.5/weather?lat=67.65827550797206&lon=-10.66548691492082&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 225
    The name of the city is paradwip
    https://api.openweathermap.org/data/2.5/weather?lat=18.985971670406883&lon=87.14735548667863&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 226
    The name of the city is falealupo
    https://api.openweathermap.org/data/2.5/weather?lat=-10.993449596420035&lon=-174.25382498880347&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 227
    The name of the city is saleaula
    https://api.openweathermap.org/data/2.5/weather?lat=1.1831307417050425&lon=-168.66499672539442&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 228
    The name of the city is victoria
    https://api.openweathermap.org/data/2.5/weather?lat=-3.296580567389398&lon=55.06989618784212&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 229
    The name of the city is faya
    https://api.openweathermap.org/data/2.5/weather?lat=17.263355365407378&lon=16.04990285142847&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 230
    The name of the city is pokhara
    https://api.openweathermap.org/data/2.5/weather?lat=28.650814426914195&lon=84.8843850846194&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 231
    The name of the city is ziyang
    https://api.openweathermap.org/data/2.5/weather?lat=30.23432855586684&lon=104.6940301102477&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 232
    The name of the city is yulara
    https://api.openweathermap.org/data/2.5/weather?lat=-23.91187096974643&lon=128.44480229891576&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 233
    The name of the city is krasnoselkup
    https://api.openweathermap.org/data/2.5/weather?lat=65.07315222923995&lon=80.71949999318093&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 234
    The name of the city is talnakh
    https://api.openweathermap.org/data/2.5/weather?lat=81.08082475888577&lon=92.06586903112043&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 235
    The name of the city is matasari
    https://api.openweathermap.org/data/2.5/weather?lat=44.833867600717724&lon=23.112776304046093&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 236
    The name of the city is kapiri mposhi
    https://api.openweathermap.org/data/2.5/weather?lat=-14.132604963122347&lon=28.83949957977711&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 237
    The name of the city is dengzhou
    https://api.openweathermap.org/data/2.5/weather?lat=32.65563799870273&lon=111.97184857749448&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 238
    The name of the city is urumqi
    https://api.openweathermap.org/data/2.5/weather?lat=43.699721044882125&lon=90.18582345932924&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 239
    The name of the city is palabuhanratu
    https://api.openweathermap.org/data/2.5/weather?lat=-14.779449219450171&lon=104.85810644587355&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 240
    The name of the city is la ronge
    https://api.openweathermap.org/data/2.5/weather?lat=60.26097805939156&lon=-103.74768649070022&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 241
    The name of the city is waipawa
    https://api.openweathermap.org/data/2.5/weather?lat=-44.94290845025728&lon=176.88905221773797&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 242
    The name of the city is tchaourou
    https://api.openweathermap.org/data/2.5/weather?lat=8.723449138916365&lon=2.4655919058217535&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 243
    The name of the city is domoni
    https://api.openweathermap.org/data/2.5/weather?lat=-11.37221448903098&lon=45.04918250514041&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 244
    The name of the city is bandarbeyla
    https://api.openweathermap.org/data/2.5/weather?lat=7.087445875282555&lon=54.84470954028319&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 245
    The name of the city is jaciara
    https://api.openweathermap.org/data/2.5/weather?lat=-15.718807619718731&lon=-54.51537194301193&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 246
    The name of the city is usinsk
    https://api.openweathermap.org/data/2.5/weather?lat=68.71913697505502&lon=57.037363750444314&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 247
    The name of the city is karlstad
    https://api.openweathermap.org/data/2.5/weather?lat=60.52460910775068&lon=13.767450318499158&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 248
    The name of the city is goderich
    https://api.openweathermap.org/data/2.5/weather?lat=3.238738296945087&lon=-18.544633690865453&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 249
    The name of the city is shu
    https://api.openweathermap.org/data/2.5/weather?lat=44.072786774087035&lon=74.08381270600245&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 250
    The name of the city is sentyabrskiy
    https://api.openweathermap.org/data/2.5/weather?lat=36.88502198753093&lon=158.36011203933805&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 251
    The name of the city is bhind
    https://api.openweathermap.org/data/2.5/weather?lat=26.638213051825176&lon=78.67455159888289&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 252
    The name of the city is muli
    https://api.openweathermap.org/data/2.5/weather?lat=2.661724354561059&lon=76.05490102586441&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 253
    The name of the city is tsihombe
    https://api.openweathermap.org/data/2.5/weather?lat=-46.16302469955932&lon=45.00865338999975&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 254
    The name of the city is laguna
    https://api.openweathermap.org/data/2.5/weather?lat=-41.27303131130609&lon=-33.37641951448941&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 255
    The name of the city is cidreira
    https://api.openweathermap.org/data/2.5/weather?lat=-63.292610220010815&lon=-18.036284099590915&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 256
    The name of the city is diu
    https://api.openweathermap.org/data/2.5/weather?lat=19.509145170311285&lon=71.35102532743787&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 257
    The name of the city is saint-paul
    https://api.openweathermap.org/data/2.5/weather?lat=-21.12724770205358&lon=54.29249115519329&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 258
    The name of the city is maragogi
    https://api.openweathermap.org/data/2.5/weather?lat=-10.938332089880433&lon=-33.19978093833123&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 259
    The name of the city is leningradskiy
    https://api.openweathermap.org/data/2.5/weather?lat=77.98709718014172&lon=177.947510221039&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 260
    The name of the city is hamilton
    https://api.openweathermap.org/data/2.5/weather?lat=29.636098014070868&lon=-65.85484240521235&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 261
    The name of the city is shimoda
    https://api.openweathermap.org/data/2.5/weather?lat=31.942985325922663&lon=139.26957131868363&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 262
    The name of the city is kholodnyy
    https://api.openweathermap.org/data/2.5/weather?lat=62.25654951239355&lon=147.59345999266128&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 263
    The name of the city is krasnyy chikoy
    https://api.openweathermap.org/data/2.5/weather?lat=50.55772685182572&lon=108.32105190638265&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 264
    The name of the city is bonnyville
    https://api.openweathermap.org/data/2.5/weather?lat=54.81149982100723&lon=-111.10575704714358&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 265
    The name of the city is le port
    https://api.openweathermap.org/data/2.5/weather?lat=-19.777262091399393&lon=55.03331071379128&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 266
    The name of the city is otofuke
    https://api.openweathermap.org/data/2.5/weather?lat=43.488687340809065&lon=143.1801168669632&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 267
    The name of the city is norman wells
    https://api.openweathermap.org/data/2.5/weather?lat=61.90345024593353&lon=-130.1606218315264&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 268
    The name of the city is kyra
    https://api.openweathermap.org/data/2.5/weather?lat=49.78690120714941&lon=111.81050699614548&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 269
    The name of the city is fort-shevchenko
    https://api.openweathermap.org/data/2.5/weather?lat=44.97824591850639&lon=49.41974143278719&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 270
    The name of the city is aban
    https://api.openweathermap.org/data/2.5/weather?lat=56.50489981183168&lon=95.80488702003726&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 271
    The name of the city is baruun-urt
    https://api.openweathermap.org/data/2.5/weather?lat=47.709903754759694&lon=112.36385671639027&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 272
    The name of the city is llano de piedra
    https://api.openweathermap.org/data/2.5/weather?lat=7.465877437697685&lon=-80.82196734522581&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 273
    The name of the city is yar-sale
    https://api.openweathermap.org/data/2.5/weather?lat=68.15885340550716&lon=70.769654914368&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 274
    The name of the city is kristiinankaupunki
    https://api.openweathermap.org/data/2.5/weather?lat=61.96794111475961&lon=20.864159022504225&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 275
    The name of the city is bolungarvik
    https://api.openweathermap.org/data/2.5/weather?lat=67.15752319515681&lon=-24.2691124591758&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 276
    The name of the city is voznesenye
    https://api.openweathermap.org/data/2.5/weather?lat=60.23135560009271&lon=35.885562684560455&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 277
    The name of the city is jabiru
    https://api.openweathermap.org/data/2.5/weather?lat=-13.669826952801714&lon=133.09978818625734&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 278
    The name of the city is rodionovo-nesvetayskaya
    https://api.openweathermap.org/data/2.5/weather?lat=47.72308656879363&lon=39.54518965820114&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 279
    The name of the city is tierra colorada
    https://api.openweathermap.org/data/2.5/weather?lat=17.055263562969415&lon=-99.38961704464546&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 280
    The name of the city is egvekinot
    https://api.openweathermap.org/data/2.5/weather?lat=60.47478328345056&lon=-178.5341908864317&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 281
    The name of the city is biograd na moru
    https://api.openweathermap.org/data/2.5/weather?lat=44.074890172004444&lon=15.439005421769508&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 282
    The name of the city is tashla
    https://api.openweathermap.org/data/2.5/weather?lat=51.456746929198374&lon=52.46352229648414&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 283
    The name of the city is mayo
    https://api.openweathermap.org/data/2.5/weather?lat=65.88223809977322&lon=-140.47959997111502&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 284
    The name of the city is kahului
    https://api.openweathermap.org/data/2.5/weather?lat=29.219142125750025&lon=-153.57901020010286&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 285
    The name of the city is kalmunai
    https://api.openweathermap.org/data/2.5/weather?lat=6.928290481797902&lon=85.29415484506012&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 286
    The name of the city is yellandu
    https://api.openweathermap.org/data/2.5/weather?lat=17.665133685099065&lon=80.23990266482991&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 287
    The name of the city is borogontsy
    https://api.openweathermap.org/data/2.5/weather?lat=63.658851705730626&lon=131.86842827925926&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 288
    The name of the city is kaduna
    https://api.openweathermap.org/data/2.5/weather?lat=10.087611989111466&lon=7.118674317019725&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 289
    The name of the city is bocas del toro
    https://api.openweathermap.org/data/2.5/weather?lat=10.244299049550648&lon=-81.98955249758976&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 290
    The name of the city is carnarvon
    https://api.openweathermap.org/data/2.5/weather?lat=-19.300623602296014&lon=109.67795631261868&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 291
    The name of the city is korla
    https://api.openweathermap.org/data/2.5/weather?lat=36.41498614526209&lon=88.82044324007069&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 292
    The name of the city is safford
    https://api.openweathermap.org/data/2.5/weather?lat=33.65531736665274&lon=-109.11967011636415&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 293
    The name of the city is kamaishi
    https://api.openweathermap.org/data/2.5/weather?lat=35.48241741877354&lon=148.66258563956637&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 294
    The name of the city is ayan
    https://api.openweathermap.org/data/2.5/weather?lat=56.33685201894983&lon=137.79934858422075&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 295
    The name of the city is coihaique
    https://api.openweathermap.org/data/2.5/weather?lat=-47.1600482323208&lon=-70.72448382146364&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 296
    The name of the city is alofi
    https://api.openweathermap.org/data/2.5/weather?lat=-17.40937734646012&lon=-170.12114072486378&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 297
    The name of the city is disna
    https://api.openweathermap.org/data/2.5/weather?lat=55.26240549456395&lon=27.90638201056322&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 298
    The name of the city is husavik
    https://api.openweathermap.org/data/2.5/weather?lat=78.42669768201111&lon=-7.237584241602633&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 299
    The name of the city is ruteng
    https://api.openweathermap.org/data/2.5/weather?lat=-8.83158764335974&lon=119.9959597873787&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 300
    The name of the city is opuwo
    https://api.openweathermap.org/data/2.5/weather?lat=-19.96278506445492&lon=8.346292909935869&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 301
    The name of the city is kutum
    https://api.openweathermap.org/data/2.5/weather?lat=18.06992359612265&lon=24.42836900251328&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 302
    The name of the city is yarkovo
    https://api.openweathermap.org/data/2.5/weather?lat=57.645807491581365&lon=67.38708475036123&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 303
    The name of the city is teknaf
    https://api.openweathermap.org/data/2.5/weather?lat=20.433126987723483&lon=90.76606790293215&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 304
    The name of the city is bela
    https://api.openweathermap.org/data/2.5/weather?lat=26.806078896153565&lon=66.41585979834602&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 305
    The name of the city is hervey bay
    https://api.openweathermap.org/data/2.5/weather?lat=-20.910333731138095&lon=156.50326462387147&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 306
    The name of the city is inuvik
    https://api.openweathermap.org/data/2.5/weather?lat=68.0762222869696&lon=-133.4518538973707&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 307
    The name of the city is porto novo
    https://api.openweathermap.org/data/2.5/weather?lat=16.572841873683657&lon=-27.765461413011792&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 308
    The name of the city is atbasar
    https://api.openweathermap.org/data/2.5/weather?lat=52.92782675048139&lon=68.5524718101988&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 309
    The name of the city is avera
    https://api.openweathermap.org/data/2.5/weather?lat=-26.539602434037718&lon=-153.2842829911591&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 310
    The name of the city is bethel
    https://api.openweathermap.org/data/2.5/weather?lat=61.18636875850828&lon=-162.8160318682908&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 311
    The name of the city is necochea
    https://api.openweathermap.org/data/2.5/weather?lat=-51.1474845241833&lon=-52.73716457263828&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 312
    The name of the city is marcona
    https://api.openweathermap.org/data/2.5/weather?lat=-20.835733189497745&lon=-78.04826892721829&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 313
    The name of the city is dharchula
    https://api.openweathermap.org/data/2.5/weather?lat=30.66093702322202&lon=81.614691120683&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 314
    The name of the city is coahuayana
    https://api.openweathermap.org/data/2.5/weather?lat=17.003884825865597&lon=-104.52630021526349&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 315
    The name of the city is saint-pierre
    https://api.openweathermap.org/data/2.5/weather?lat=45.57479072586324&lon=-57.49911460586739&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 316
    The name of the city is iqaluit
    https://api.openweathermap.org/data/2.5/weather?lat=63.50545293897483&lon=-74.87551806743978&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 317
    The name of the city is alyangula
    https://api.openweathermap.org/data/2.5/weather?lat=-13.716283861754647&lon=136.85673730697584&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 318
    The name of the city is kalevala
    https://api.openweathermap.org/data/2.5/weather?lat=65.27399457501727&lon=31.187920240418833&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 319
    The name of the city is simao
    https://api.openweathermap.org/data/2.5/weather?lat=23.747691600652757&lon=100.42267316768408&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 320
    The name of the city is lata
    https://api.openweathermap.org/data/2.5/weather?lat=-8.381896790781681&lon=168.15040970133083&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 321
    The name of the city is tyrma
    https://api.openweathermap.org/data/2.5/weather?lat=50.067224876232785&lon=131.85737830998357&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 322
    The name of the city is port alfred
    https://api.openweathermap.org/data/2.5/weather?lat=-83.65260447225171&lon=48.726197827307004&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 323
    The name of the city is forrest city
    https://api.openweathermap.org/data/2.5/weather?lat=35.33104535730034&lon=-90.57376396336605&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 324
    The name of the city is cayenne
    https://api.openweathermap.org/data/2.5/weather?lat=13.213466733403578&lon=-44.66552827892414&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 325
    The name of the city is bilibino
    https://api.openweathermap.org/data/2.5/weather?lat=68.80715630360459&lon=167.80029442622475&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 326
    The name of the city is high level
    https://api.openweathermap.org/data/2.5/weather?lat=58.94937521714513&lon=-117.75541332809156&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 327
    The name of the city is shelburne
    https://api.openweathermap.org/data/2.5/weather?lat=38.93362759497279&lon=-64.03242653458183&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 328
    The name of the city is yima
    https://api.openweathermap.org/data/2.5/weather?lat=33.89356303679449&lon=111.82321930211441&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 329
    The name of the city is deputatskiy
    https://api.openweathermap.org/data/2.5/weather?lat=71.25354875094143&lon=141.6137603193473&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 330
    The name of the city is kamyshlov
    https://api.openweathermap.org/data/2.5/weather?lat=56.70226966215816&lon=62.70048669007744&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 331
    The name of the city is hobyo
    https://api.openweathermap.org/data/2.5/weather?lat=1.974157293968247&lon=52.14507203036396&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 332
    The name of the city is sao filipe
    https://api.openweathermap.org/data/2.5/weather?lat=5.389544678499533&lon=-28.08539159783885&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 333
    The name of the city is butaritari
    https://api.openweathermap.org/data/2.5/weather?lat=7.673417395244499&lon=175.91035885759754&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 334
    The name of the city is maniitsoq
    https://api.openweathermap.org/data/2.5/weather?lat=63.20139846249643&lon=-54.46818161837905&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 335
    The name of the city is khani
    https://api.openweathermap.org/data/2.5/weather?lat=56.480880868868155&lon=121.08503770754118&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 336
    The name of the city is awjilah
    https://api.openweathermap.org/data/2.5/weather?lat=27.068332664198252&lon=19.355554605412493&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 337
    The name of the city is ambodifototra
    https://api.openweathermap.org/data/2.5/weather?lat=-18.526468220980973&lon=52.583655422397925&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 338
    The name of the city is taybad
    https://api.openweathermap.org/data/2.5/weather?lat=34.40044703758642&lon=59.53070780969392&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 339
    The name of the city is reconquista
    https://api.openweathermap.org/data/2.5/weather?lat=-28.971580945859863&lon=-59.90987002653196&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 340
    The name of the city is tunduru
    https://api.openweathermap.org/data/2.5/weather?lat=-11.425709898948071&lon=36.48849842178217&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 341
    The name of the city is camacha
    https://api.openweathermap.org/data/2.5/weather?lat=37.42347575892444&lon=-15.654839566719176&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 342
    The name of the city is garowe
    https://api.openweathermap.org/data/2.5/weather?lat=7.6748022390627&lon=47.30688472306366&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 343
    The name of the city is ondjiva
    https://api.openweathermap.org/data/2.5/weather?lat=-16.18357703624315&lon=15.219316854027852&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 344
    The name of the city is kidal
    https://api.openweathermap.org/data/2.5/weather?lat=17.091050608642135&lon=1.7656757276210442&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 345
    The name of the city is dawlatabad
    https://api.openweathermap.org/data/2.5/weather?lat=36.033013236242056&lon=65.13270820634546&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 346
    The name of the city is cabo san lucas
    https://api.openweathermap.org/data/2.5/weather?lat=11.5718293745378&lon=-124.07153709594314&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 347
    The name of the city is ozinki
    https://api.openweathermap.org/data/2.5/weather?lat=50.80890176707766&lon=50.22585104693189&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 348
    The name of the city is tual
    https://api.openweathermap.org/data/2.5/weather?lat=-8.688594221556187&lon=132.6440295295145&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 349
    The name of the city is galesong
    https://api.openweathermap.org/data/2.5/weather?lat=-5.314263100283156&lon=117.2843602128761&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 350
    The name of the city is belmonte
    https://api.openweathermap.org/data/2.5/weather?lat=-17.587926865853973&lon=-30.899892737100686&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 351
    The name of the city is rimbey
    https://api.openweathermap.org/data/2.5/weather?lat=52.55367013968848&lon=-114.14924315970487&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 352
    The name of the city is tautira
    https://api.openweathermap.org/data/2.5/weather?lat=-15.67697965256157&lon=-145.5532034570585&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 353
    The name of the city is mount gambier
    https://api.openweathermap.org/data/2.5/weather?lat=-50.765918816883676&lon=131.48443981615685&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 354
    The name of the city is yenotayevka
    https://api.openweathermap.org/data/2.5/weather?lat=46.80214249274533&lon=47.00861489880603&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 355
    The name of the city is kerrobert
    https://api.openweathermap.org/data/2.5/weather?lat=52.12553754777622&lon=-109.01043447749075&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 356
    The name of the city is juneau
    https://api.openweathermap.org/data/2.5/weather?lat=59.39825672799745&lon=-133.2873414137993&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 357
    The name of the city is pasighat
    https://api.openweathermap.org/data/2.5/weather?lat=32.15418956004871&lon=95.93185815771534&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 358
    The name of the city is port hedland
    https://api.openweathermap.org/data/2.5/weather?lat=-23.700740722923612&lon=123.32355513215305&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 359
    The name of the city is brae
    https://api.openweathermap.org/data/2.5/weather?lat=62.88072825307191&lon=-0.29591337557701536&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 360
    The name of the city is oriximina
    https://api.openweathermap.org/data/2.5/weather?lat=0.3580351031859408&lon=-55.291660284987515&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 361
    The name of the city is byron bay
    https://api.openweathermap.org/data/2.5/weather?lat=-29.263232958179287&lon=160.79981456735914&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 362
    The name of the city is khandagayty
    https://api.openweathermap.org/data/2.5/weather?lat=50.71306363905967&lon=92.43297043117002&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 363
    The name of the city is novoorsk
    https://api.openweathermap.org/data/2.5/weather?lat=51.24046392121383&lon=58.875635454032164&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 364
    The name of the city is gardez
    https://api.openweathermap.org/data/2.5/weather?lat=33.59154422788198&lon=69.44014648900168&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 365
    The name of the city is santiago
    https://api.openweathermap.org/data/2.5/weather?lat=-27.093282416208375&lon=-56.91506156010149&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 366
    The name of the city is okha
    https://api.openweathermap.org/data/2.5/weather?lat=55.34318457697776&lon=144.3156392297949&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 367
    The name of the city is kungurtug
    https://api.openweathermap.org/data/2.5/weather?lat=49.995250997709206&lon=98.23010908930632&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 368
    The name of the city is amazar
    https://api.openweathermap.org/data/2.5/weather?lat=54.054136672622974&lon=121.03698971852413&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 369
    The name of the city is ostrovnoy
    https://api.openweathermap.org/data/2.5/weather?lat=78.9402267594815&lon=42.75392565542495&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 370
    The name of the city is sola
    https://api.openweathermap.org/data/2.5/weather?lat=-13.108579978344736&lon=174.36017765320895&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 371
    The name of the city is praia
    https://api.openweathermap.org/data/2.5/weather?lat=12.146678507130702&lon=-22.913556989114767&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 372
    The name of the city is sistranda
    https://api.openweathermap.org/data/2.5/weather?lat=64.29500467946445&lon=9.221141208621617&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 373
    The name of the city is kaohsiung
    https://api.openweathermap.org/data/2.5/weather?lat=22.23422696696244&lon=119.91536906351206&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 374
    The name of the city is mocorito
    https://api.openweathermap.org/data/2.5/weather?lat=25.463361218205847&lon=-107.73422680911189&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 375
    The name of the city is buraydah
    https://api.openweathermap.org/data/2.5/weather?lat=27.47878461980966&lon=42.12038025025751&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 376
    The name of the city is ebensee
    https://api.openweathermap.org/data/2.5/weather?lat=47.83542779097348&lon=13.768073490904044&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 377
    The name of the city is helong
    https://api.openweathermap.org/data/2.5/weather?lat=39.84612995462689&lon=129.58343564984614&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 378
    The name of the city is ilo
    https://api.openweathermap.org/data/2.5/weather?lat=-19.3623789503564&lon=-73.35871553569814&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 379
    The name of the city is ulladulla
    https://api.openweathermap.org/data/2.5/weather?lat=-37.64660966321322&lon=154.05433474601085&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 380
    The name of the city is guaymas
    https://api.openweathermap.org/data/2.5/weather?lat=27.68040967653306&lon=-110.9964713209618&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 381
    The name of the city is san cristobal
    https://api.openweathermap.org/data/2.5/weather?lat=-8.88743333578472&lon=-88.80328789238112&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 382
    The name of the city is padang
    https://api.openweathermap.org/data/2.5/weather?lat=-8.306224969371854&lon=91.48356135671077&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 383
    The name of the city is tlaxiaco
    https://api.openweathermap.org/data/2.5/weather?lat=17.024543787131464&lon=-97.68904656030998&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 384
    The name of the city is hovd
    https://api.openweathermap.org/data/2.5/weather?lat=45.117851205145485&lon=103.45832025252452&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 385
    The name of the city is nioro
    https://api.openweathermap.org/data/2.5/weather?lat=16.568802639197855&lon=-10.319975428252746&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 386
    The name of the city is ouadda
    https://api.openweathermap.org/data/2.5/weather?lat=8.060384291152985&lon=22.363836037280635&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 387
    The name of the city is batetskiy
    https://api.openweathermap.org/data/2.5/weather?lat=58.691794647048056&lon=30.40572243100945&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 388
    The name of the city is wenling
    https://api.openweathermap.org/data/2.5/weather?lat=27.17367883445651&lon=123.83088020342689&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 389
    The name of the city is xingyi
    https://api.openweathermap.org/data/2.5/weather?lat=24.118531593532296&lon=105.31293727340102&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 390
    The name of the city is monte carmelo
    https://api.openweathermap.org/data/2.5/weather?lat=-19.205457432953537&lon=-47.648184478493164&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 391
    The name of the city is taraclia
    https://api.openweathermap.org/data/2.5/weather?lat=45.832520772760574&lon=28.68457687404097&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 392
    The name of the city is raymond
    https://api.openweathermap.org/data/2.5/weather?lat=43.892699541142804&lon=-70.40990198211576&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 393
    The name of the city is nuuk
    https://api.openweathermap.org/data/2.5/weather?lat=59.698791190956484&lon=-57.39911448450712&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 394
    The name of the city is portland
    https://api.openweathermap.org/data/2.5/weather?lat=-54.3308359276624&lon=132.94750907071398&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 395
    The name of the city is hokitika
    https://api.openweathermap.org/data/2.5/weather?lat=-41.471264592199454&lon=168.07210586935878&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 396
    The name of the city is amahai
    https://api.openweathermap.org/data/2.5/weather?lat=-4.631757755829042&lon=130.27531885072858&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 397
    The name of the city is petropavlovka
    https://api.openweathermap.org/data/2.5/weather?lat=49.66471314310505&lon=105.24369023979904&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 398
    The name of the city is road town
    https://api.openweathermap.org/data/2.5/weather?lat=21.83216510076342&lon=-63.55916486548675&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 399
    The name of the city is stony mountain
    https://api.openweathermap.org/data/2.5/weather?lat=50.015010008038786&lon=-97.0913885215198&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 400
    The name of the city is milledgeville
    https://api.openweathermap.org/data/2.5/weather?lat=33.03994308728009&lon=-83.22400543568827&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 401
    The name of the city is karkaralinsk
    https://api.openweathermap.org/data/2.5/weather?lat=49.477689646883704&lon=77.84276442263945&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 402
    The name of the city is heihe
    https://api.openweathermap.org/data/2.5/weather?lat=51.067528158200844&lon=126.27237547450915&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 403
    The name of the city is narsaq
    https://api.openweathermap.org/data/2.5/weather?lat=86.77568739695664&lon=-60.320628697594515&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 404
    The name of the city is labuan
    https://api.openweathermap.org/data/2.5/weather?lat=6.975271912809461&lon=113.1707698029835&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 405
    The name of the city is strathmore
    https://api.openweathermap.org/data/2.5/weather?lat=51.021314399941645&lon=-113.2871863938952&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 406
    The name of the city is matameye
    https://api.openweathermap.org/data/2.5/weather?lat=13.383620011699364&lon=8.252902002827767&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 407
    The name of the city is tabuk
    https://api.openweathermap.org/data/2.5/weather?lat=28.055868283611815&lon=38.1419477456175&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 408
    The name of the city is muzaffargarh
    https://api.openweathermap.org/data/2.5/weather?lat=30.102316206776564&lon=71.03428299075836&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 409
    The name of the city is puerto escondido
    https://api.openweathermap.org/data/2.5/weather?lat=7.041159728398256&lon=-101.05621191483985&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 410
    The name of the city is ballina
    https://api.openweathermap.org/data/2.5/weather?lat=57.95566919118116&lon=-11.839722585217118&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 411
    The name of the city is paso de carrasco
    https://api.openweathermap.org/data/2.5/weather?lat=-36.25635245453357&lon=-56.057402745061836&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 412
    The name of the city is aasiaat
    https://api.openweathermap.org/data/2.5/weather?lat=70.89129659712654&lon=-53.99629347638441&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 413
    The name of the city is lerik
    https://api.openweathermap.org/data/2.5/weather?lat=38.725107465768616&lon=47.72509258793747&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 414
    The name of the city is honningsvag
    https://api.openweathermap.org/data/2.5/weather?lat=70.708190097178&lon=26.448512611610795&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 415
    The name of the city is huilong
    https://api.openweathermap.org/data/2.5/weather?lat=33.78590259922065&lon=124.42173347024936&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 416
    The name of the city is vestmannaeyjar
    https://api.openweathermap.org/data/2.5/weather?lat=54.31413398005341&lon=-22.464931561373817&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 417
    The name of the city is chippewa falls
    https://api.openweathermap.org/data/2.5/weather?lat=44.72839916193735&lon=-91.0494356278854&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 418
    The name of the city is bintulu
    https://api.openweathermap.org/data/2.5/weather?lat=2.9631880607889656&lon=112.43777098435714&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 419
    The name of the city is springfield
    https://api.openweathermap.org/data/2.5/weather?lat=39.88742959744491&lon=-83.59776672308564&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 420
    The name of the city is antalaha
    https://api.openweathermap.org/data/2.5/weather?lat=-14.15350389179359&lon=56.183569011131084&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 421
    The name of the city is madang
    https://api.openweathermap.org/data/2.5/weather?lat=-4.219852824725464&lon=145.87083418208482&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 422
    The name of the city is ilulissat
    https://api.openweathermap.org/data/2.5/weather?lat=78.64660182394073&lon=-44.49131798856257&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 423
    The name of the city is nacala
    https://api.openweathermap.org/data/2.5/weather?lat=-14.527892226325434&lon=42.46287735708498&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 424
    The name of the city is laibin
    https://api.openweathermap.org/data/2.5/weather?lat=23.81930759778014&lon=109.3168003494539&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 425
    The name of the city is esperance
    https://api.openweathermap.org/data/2.5/weather?lat=-31.871230970300665&lon=120.18185579968349&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 426
    The name of the city is arrecife
    https://api.openweathermap.org/data/2.5/weather?lat=28.28902566078142&lon=-11.597148022579916&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 427
    The name of the city is toliary
    https://api.openweathermap.org/data/2.5/weather?lat=-24.412277853844643&lon=43.050049405103465&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 428
    The name of the city is chapais
    https://api.openweathermap.org/data/2.5/weather?lat=54.87701856930738&lon=-75.98623356581297&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 429
    The name of the city is atasu
    https://api.openweathermap.org/data/2.5/weather?lat=48.24215813723629&lon=72.07245390656311&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 430
    The name of the city is monrovia
    https://api.openweathermap.org/data/2.5/weather?lat=5.608408521462863&lon=-11.088803339404507&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 431
    The name of the city is iwanai
    https://api.openweathermap.org/data/2.5/weather?lat=43.29557791149031&lon=138.7022649686234&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 432
    The name of the city is muisne
    https://api.openweathermap.org/data/2.5/weather?lat=1.2085439686937463&lon=-83.21757210218807&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 433
    The name of the city is chameza
    https://api.openweathermap.org/data/2.5/weather?lat=5.193000560546437&lon=-72.86464902467665&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 434
    The name of the city is borba
    https://api.openweathermap.org/data/2.5/weather?lat=-4.347678072547637&lon=-59.22918682212962&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 435
    The name of the city is kedrovyy
    https://api.openweathermap.org/data/2.5/weather?lat=56.55010550739843&lon=79.81888053530736&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 436
    The name of the city is kalabo
    https://api.openweathermap.org/data/2.5/weather?lat=-15.232050429110231&lon=22.38450043422762&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 437
    The name of the city is kirkuk
    https://api.openweathermap.org/data/2.5/weather?lat=35.60885887613843&lon=43.786060690974836&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 438
    The name of the city is aksarayskiy
    https://api.openweathermap.org/data/2.5/weather?lat=47.19711742489562&lon=48.31058500544475&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 439
    The name of the city is puerto carreno
    https://api.openweathermap.org/data/2.5/weather?lat=7.195045313345048&lon=-68.52711961189216&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 440
    The name of the city is port blair
    https://api.openweathermap.org/data/2.5/weather?lat=14.003566577861832&lon=91.34411843782289&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 441
    The name of the city is opobo
    https://api.openweathermap.org/data/2.5/weather?lat=3.8420841298972164&lon=7.4211183126492415&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 442
    The name of the city is samusu
    https://api.openweathermap.org/data/2.5/weather?lat=-12.15180362662386&lon=-162.63312897200188&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 443
    The name of the city is wadena
    https://api.openweathermap.org/data/2.5/weather?lat=52.4469547305909&lon=-104.03102429106588&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 444
    The name of the city is andra
    https://api.openweathermap.org/data/2.5/weather?lat=63.477387490057936&lon=65.55672616618813&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 445
    The name of the city is rawlins
    https://api.openweathermap.org/data/2.5/weather?lat=42.12204505131274&lon=-107.35800977783786&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 446
    The name of the city is niles
    https://api.openweathermap.org/data/2.5/weather?lat=42.038864566657395&lon=-86.15863517307639&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 447
    The name of the city is auburn
    https://api.openweathermap.org/data/2.5/weather?lat=32.670036505749266&lon=-85.68878359100813&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 448
    The name of the city is kadaya
    https://api.openweathermap.org/data/2.5/weather?lat=50.41919329123414&lon=120.04320085180149&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 449
    The name of the city is rocha
    https://api.openweathermap.org/data/2.5/weather?lat=-48.85973065936509&lon=-42.06607174279338&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 450
    The name of the city is islamkot
    https://api.openweathermap.org/data/2.5/weather?lat=24.472135031995748&lon=70.13728739220221&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 451
    The name of the city is nishihara
    https://api.openweathermap.org/data/2.5/weather?lat=23.9359483190419&lon=130.83813482549874&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 452
    The name of the city is tessalit
    https://api.openweathermap.org/data/2.5/weather?lat=23.674404978225866&lon=2.5660136215452383&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 453
    The name of the city is tumannyy
    https://api.openweathermap.org/data/2.5/weather?lat=89.32714925634451&lon=39.84818025919958&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 454
    The name of the city is codrington
    https://api.openweathermap.org/data/2.5/weather?lat=18.27672609262372&lon=-61.519577014111874&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 455
    The name of the city is pangnirtung
    https://api.openweathermap.org/data/2.5/weather?lat=60.105789105394905&lon=-60.65001493216239&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 456
    The name of the city is yumen
    https://api.openweathermap.org/data/2.5/weather?lat=37.84969778048165&lon=96.4272133000548&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 457
    The name of the city is yenisehir
    https://api.openweathermap.org/data/2.5/weather?lat=40.303486626542735&lon=29.513295364655278&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 458
    The name of the city is tuatapere
    https://api.openweathermap.org/data/2.5/weather?lat=-55.17670634769849&lon=157.85075937620724&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 459
    The name of the city is shcholkine
    https://api.openweathermap.org/data/2.5/weather?lat=45.678328487730795&lon=35.94121985353442&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 460
    The name of the city is lagoa
    https://api.openweathermap.org/data/2.5/weather?lat=43.907405088263374&lon=-29.958224335561738&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 461
    The name of the city is saint-denis
    https://api.openweathermap.org/data/2.5/weather?lat=-17.903914362905326&lon=54.83064152497647&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 462
    The name of the city is piacabucu
    https://api.openweathermap.org/data/2.5/weather?lat=-14.44415275634185&lon=-32.69769304135181&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 463
    The name of the city is kalofer
    https://api.openweathermap.org/data/2.5/weather?lat=42.81346266177482&lon=25.043173447636917&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 464
    The name of the city is san jeronimo
    https://api.openweathermap.org/data/2.5/weather?lat=9.570885353046563&lon=-104.26836033102629&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 465
    The name of the city is moses lake
    https://api.openweathermap.org/data/2.5/weather?lat=46.95103963877065&lon=-118.97129283448744&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 466
    The name of the city is bayangol
    https://api.openweathermap.org/data/2.5/weather?lat=51.18410369195112&lon=103.04453291708711&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 467
    The name of the city is lolua
    https://api.openweathermap.org/data/2.5/weather?lat=-8.683443908165174&lon=175.4706267711922&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 468
    The name of the city is boyuibe
    https://api.openweathermap.org/data/2.5/weather?lat=-20.348629269747505&lon=-62.3045047014743&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 469
    The name of the city is togur
    https://api.openweathermap.org/data/2.5/weather?lat=58.79458126836772&lon=82.52444986141768&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 470
    The name of the city is axim
    https://api.openweathermap.org/data/2.5/weather?lat=1.3578779831532728&lon=-3.708673160786077&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 471
    The name of the city is visnes
    https://api.openweathermap.org/data/2.5/weather?lat=58.57656968206595&lon=2.1285515439047344&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 472
    The name of the city is karaul
    https://api.openweathermap.org/data/2.5/weather?lat=69.00216562299644&lon=84.42191688483945&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 473
    The name of the city is kuhdasht
    https://api.openweathermap.org/data/2.5/weather?lat=33.6768192888657&lon=47.54745585295211&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 474
    The name of the city is srimushnam
    https://api.openweathermap.org/data/2.5/weather?lat=11.347749078272003&lon=79.33497894315377&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 475
    The name of the city is sitka
    https://api.openweathermap.org/data/2.5/weather?lat=53.045972562212455&lon=-140.07389812264597&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 476
    The name of the city is cheyenne
    https://api.openweathermap.org/data/2.5/weather?lat=41.45363807184066&lon=-104.81173255223482&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 477
    The name of the city is ola
    https://api.openweathermap.org/data/2.5/weather?lat=58.46004605702507&lon=153.89179219761797&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 478
    The name of the city is college
    https://api.openweathermap.org/data/2.5/weather?lat=67.1093550568059&lon=-152.4381304463708&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 479
    The name of the city is nabire
    https://api.openweathermap.org/data/2.5/weather?lat=-4.7482058404170004&lon=138.10415325940204&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 480
    The name of the city is zhuzhou
    https://api.openweathermap.org/data/2.5/weather?lat=27.95122802742644&lon=113.32992244217786&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 481
    The name of the city is sungaipenuh
    https://api.openweathermap.org/data/2.5/weather?lat=-2.0152787468958735&lon=100.96829769169307&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 482
    The name of the city is acapulco
    https://api.openweathermap.org/data/2.5/weather?lat=10.473637787754711&lon=-101.39484236038331&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 483
    The name of the city is sokoni
    https://api.openweathermap.org/data/2.5/weather?lat=-6.635858983399231&lon=42.39341275142601&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 484
    The name of the city is kampene
    https://api.openweathermap.org/data/2.5/weather?lat=-3.2758619876912576&lon=26.318100590674817&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 485
    The name of the city is kupang
    https://api.openweathermap.org/data/2.5/weather?lat=-12.670430493300671&lon=122.75719008465057&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 486
    The name of the city is broome
    https://api.openweathermap.org/data/2.5/weather?lat=-19.331169990320348&lon=122.31563867524682&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 487
    The name of the city is antanifotsy
    https://api.openweathermap.org/data/2.5/weather?lat=-19.43262833672182&lon=47.8512850218919&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 488
    The name of the city is erenhot
    https://api.openweathermap.org/data/2.5/weather?lat=43.5803020894478&lon=114.44159583824677&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 489
    The name of the city is el carmen
    https://api.openweathermap.org/data/2.5/weather?lat=-18.02748964039145&lon=-63.127586065836795&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 490
    The name of the city is warmbad
    https://api.openweathermap.org/data/2.5/weather?lat=-29.2179998449477&lon=19.77159260242047&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 491
    The name of the city is luziania
    https://api.openweathermap.org/data/2.5/weather?lat=-16.182851366844346&lon=-47.47134839120153&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 492
    The name of the city is bako
    https://api.openweathermap.org/data/2.5/weather?lat=6.235851108914488&lon=36.90057422272025&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 493
    The name of the city is manbij
    https://api.openweathermap.org/data/2.5/weather?lat=36.34910650776649&lon=37.87558046087125&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 494
    The name of the city is yaan
    https://api.openweathermap.org/data/2.5/weather?lat=31.39585407959663&lon=101.09264263898217&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 495
    The name of the city is anadyr
    https://api.openweathermap.org/data/2.5/weather?lat=63.0514218979761&lon=176.22068995316567&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 496
    The name of the city is oranjemund
    https://api.openweathermap.org/data/2.5/weather?lat=-32.03640595952963&lon=10.688132198750424&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 497
    The name of the city is mangai
    https://api.openweathermap.org/data/2.5/weather?lat=-3.2317664188052646&lon=19.18714455205557&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 498
    The name of the city is arman
    https://api.openweathermap.org/data/2.5/weather?lat=59.027834955754486&lon=147.30732170082302&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 499
    The name of the city is marawi
    https://api.openweathermap.org/data/2.5/weather?lat=20.530258109812806&lon=27.4580901317793&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 500
    The name of the city is cabras
    https://api.openweathermap.org/data/2.5/weather?lat=39.67414541766544&lon=7.563951127823287&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 501
    The name of the city is baiao
    https://api.openweathermap.org/data/2.5/weather?lat=-2.943056472508445&lon=-50.24737600214843&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 502
    The name of the city is basmat
    https://api.openweathermap.org/data/2.5/weather?lat=19.33673450999504&lon=77.23314511955209&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 503
    The name of the city is ormara
    https://api.openweathermap.org/data/2.5/weather?lat=24.31523759790842&lon=64.20863726015759&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________
    we are now on city number 504
    The name of the city is coquimbo
    https://api.openweathermap.org/data/2.5/weather?lat=-28.992852052828617&lon=-81.63548560482826&appid=f2a9a6283d7386b40fb4317c3f545a0a&units=imperial
    __________________________________________________________________________________________


    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:29: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:30: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:31: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:32: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy



```python
city_data_2.head()
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
      <th>Lat</th>
      <th>Lng</th>
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
      <td>24.255733</td>
      <td>-36.110380</td>
      <td>ponta do sol</td>
      <td>70.23</td>
      <td>100</td>
      <td>92</td>
      <td>13.09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.277629</td>
      <td>-159.102792</td>
      <td>hilo</td>
      <td>74.51</td>
      <td>100</td>
      <td>92</td>
      <td>10.63</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.653160</td>
      <td>-76.704095</td>
      <td>marsh harbour</td>
      <td>71.85</td>
      <td>100</td>
      <td>56</td>
      <td>7.72</td>
    </tr>
    <tr>
      <th>3</th>
      <td>55.418722</td>
      <td>129.920305</td>
      <td>fevralsk</td>
      <td>-1.28</td>
      <td>51</td>
      <td>36</td>
      <td>2.13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-73.119245</td>
      <td>-157.569374</td>
      <td>mataura</td>
      <td>28.43</td>
      <td>100</td>
      <td>92</td>
      <td>32.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
len(city_data_2)
```




    504




```python
# Save as a csv
# Note to avoid any issues later, use encoding="utf-8"
city_data_2.to_csv("city_data_2.csv", encoding="utf-8", index=False)
```

# Temperature vs Latitude Plot


```python
#Temperature vs Latitude
plt.scatter(city_data_2["Lat"],city_data_2["Temperature"],marker ="o")
plt.title("Temperature vs Latitude")
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
sns.set()

plt.savefig("Temperature_vs_Latitude.png")
plt.show()
```


![png](output_8_0.png)


# Humidity vs Latitude Plot


```python
#Humidity Vs. Latitude
plt.scatter(city_data_2["Lat"],city_data_2["Humidity"],marker ="o")
plt.title("Humidity vs Latitude")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
sns.set()

plt.savefig("Humidity_vs_Latitude.png")
plt.show()
```


![png](output_10_0.png)


# Cloudiness vs Latitude Plot


```python
#Cloudiness Vs. Latitude 
plt.scatter(city_data_2["Lat"],city_data_2["Clouds"],marker ="o")
plt.title("Cloudiness vs Latitude")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
sns.set()

plt.savefig("Cloudiness_vs_Latitude.png")
plt.show()
```


![png](output_12_0.png)


# Wind Speed vs Latitude Plot


```python
#Wind Speed Vs. Latitude 
plt.scatter(city_data_2["Lat"],city_data_2["Clouds"],marker ="o")
plt.title("Wind Speed vs Latitude")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
sns.set()

plt.savefig("Windspeed_vs_Latitude.png")
plt.show()
```


![png](output_14_0.png)

