# pagina.ipynb

from bs4 import BeautifulSoup
import requests
import pandas as pd

url = 'https://chile.as.com/resultados/futbol/copa_libertadores/clasificacion/'
page = requests.get(url)
soup = BeautifulSoup(page.content, 'html.parser')


eq = soup.find_all ('span', class_='nombre-equipo')

equipos = list()

count = 0
for i in eq:
    if count < 20:
        equipos.append(i.text)
    else:
        break
    count += 1
    
    
print(equipos, len (equipos))

pt = soup.find_all ('td', class_='destacado')

puntos = list()

count = 0
for i in pt:
    if count < 20:
        puntos.append(i.text)
    else:
        break
    count += 1
    
df = pd.DataFrame({'nombre': equipos,'puntos': puntos}, index=list(range(1,21)))
print(df)
df.to_csv('clasificacion.csv', index=False)
