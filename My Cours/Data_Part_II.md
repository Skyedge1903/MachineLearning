# Acquisition des données

## Le Scraping

### Utiliser une librairie pour parser html

Il y a plusieurs bibliothèques Python qui peuvent être utilisées pour faire du scraping, comme BeautifulSoup, Scrapy et Selenium. Voici un exemple de scraping de données à partir d'une page web en utilisant la bibliothèque BeautifulSoup :

1. Commencez par installer BeautifulSoup en utilisant pip : `pip install beautifulsoup4`
2. Utilisez la bibliothèque requests pour récupérer le contenu HTML de la page web :

```python
import requests

url = 'https://www.example.com'
response = requests.get(url)
content = response.content
```

3. Utilisez BeautifulSoup pour extraire les données de la page :

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(content, 'html.parser')
```

4. Utilisez les méthodes de BeautifulSoup pour naviguer et sélectionner les éléments de la page HTML que vous souhaitez extraire :

```python
title = soup.title
print(title.string)
```

### Utiliser les expressions régulières

Il est possible d'utiliser des expressions régulières pour extraire des informations à partir de contenu HTML en utilisant la bibliothèque Python re. Cependant, il est généralement conseillé d'utiliser des bibliothèques spécialisées pour le parsing de HTML, comme BeautifulSoup, car les expressions régulières peuvent être difficiles à maintenir et à gérer pour des pages web complexes.

Voici un exemple simple d'utilisation d'expressions régulières pour extraire un lien à partir d'une page HTML :

```python
import re
import requests

url = 'https://www.example.com'
response = requests.get(url)
content = response.content

link_pattern = re.compile(r'<a href="(.*?)">')
links = link_pattern.findall(content)
print(links)
```

Ce code utilise la méthode findall de la bibliothèque re pour rechercher tous les liens (c'est-à-dire tous les groupes de caractères entre les balises `<a href=" et ">`dans le contenu HTML de la page. Il est important de noter que cette méthode n'est pas aussi efficace que d'autres méthodes de parsing de HTML, il est donc préférable d'utiliser des bibliothèques spécialisées pour des projets de scraping plus importants.

#### Exercice Scraping

**Enoncé :** 

L'objectif de cet exercice est de télécharger des images de chats à partir de la page Wikipedia sur les chats. Pour ce faire, nous allons utiliser diverses bibliothèques Python pour faire du scraping de sites web.

Nous commencerons par définir des fonctions pour récupérer le contenu d'une page web, extraire les liens des balises `<a>` et télécharger un fichier.

Ensuite, nous spécifierons l'URL de la page Wikipedia sur les chats et nous récupérerons les liens de cette page. Nous filtrerons les liens qui contiennent le mot "chat" ou "cat" et qui ont une extension d'image.

Nous récupérerons ensuite les liens des images sur les pages de chaque lien filtré en utilisant à nouveau les fonctions pour récupérer le contenu d'une page web et extraire les liens des balises `<a>`. Nous stockerons ensuite ces liens dans une liste unique en enlevant les doublons.

Enfin, nous téléchargerons chaque image à partir des liens stockés en utilisant la fonction de téléchargement de fichiers. Chaque téléchargement sera suivi d'une pause de 1 seconde pour éviter de surcharger le site web.

**Exemple de solution :** 

```python
import re
import requests
import shutil
import os
import gzip
import urllib.request
import time


# function that retrieves the content of a web page
def get_page(url):
    # we get the content of the page
    page = requests.get(url).text
    # we return the content
    return page


# function that retrieves the content of all <a> tags of a web page
def get_links(page):
    # we get the links
    links = re.findall(r'<a href="([^"]+)".*>', page)
    # we return the links
    return links


# function that downloads a file
def download_file(url, p):
    # we download the picture
    urllib.request.urlretrieve(url, p)
    # we write the file
    with open(p, "wb") as f:
        f.write(urllib.request.urlopen(url).read())


wiki = "https://fr.wikipedia.org/wiki/Chat"

download = "https://fr.wikipedia.org"

links = get_links(get_page(wiki))

cats = []

for link in links:
    if "Chat" in link or "Cat" in link or "chat" in link or "cat" in link:
        if "jpg" in link or "png" in link or "jpeg" in link:
            cats.append(download + link)

cats_links = set()

# we get the links of the images
for i, cat in enumerate(cats):
    content = get_page(cat)
    links = get_links(content)
    for link in links:
        if "//upload.wikimedia.org/" in link:
            if "jpg" in link:
                cats_links.add(link)
            if "png" in link:
                cats_links.add(link)
            if "jpeg" in link:
                cats_links.add(link)

cats_links = list(cats_links)
cats_links = ["https:" + link for link in cats_links]

for i, link in enumerate(cats_links):
    print(link)
    # delais de 1 seconde entre chaque téléchargement
    time.sleep(1)
    download_file(link, "mes_petits_chats/" + str(i) + ".jpg")
```


### Utiliser un navigateur téléguidé

Il est possible d'utiliser un navigateur téléguidé, comme Selenium, pour faire du scraping en Python. Selenium permet de simuler une interaction avec un navigateur web en utilisant un script Python, ce qui peut être utile pour accéder à des pages qui nécessitent une interaction utilisateur (comme des formulaires de connexion, des pages de résultats de recherche dynamiques, etc.).

Voici un exemple simple d'utilisation de Selenium pour extraire des informations à partir d'une page web :

1. Commencez par installer Selenium en utilisant pip : pip install selenium
2. Téléchargez et installez également le driver pour votre navigateur, comme ChromeDriver pour Chrome.
3. Utilisez Selenium pour ouvrir une session de navigateur et accéder à la page web :

```python
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("https://www.example.com")
```

4. Utilisez les méthodes de Selenium pour interagir avec la page web, comme la recherche d'éléments et la simulation de clics :

```python
title = driver.find_element_by_tag_name("title")
print(title.get_attribute("innerHTML"))

driver.quit()
```

Il est important de noter que l'utilisation de Selenium pour faire du scraping peut être plus lente que d'autres méthodes, car il doit charger et exécuter une instance de navigateur. Il est également important de respecter les politiques de confidentialité et de respect de la propriété intellectuelle des sites web que vous scrapez.

#### Exercice Scraping avec Sélénum

**Exercice :**

1. Installez les bibliothèques Selenium et re si ce n'est pas déjà fait.
2. Créez un fichier Python et importez les bibliothèques nécessaires.
3. Configurez l'emplacement de votre navigateur Firefox et de son pilote Gecko.
4. Ouvrez une session de navigateur Firefox avec Selenium en utilisant les options définies.
5. Accédez à Google.com en utilisant la méthode "get".
6. Cliquez sur le bouton "J'accepte" pour accepter les conditions d'utilisation de Google.
7. Recherchez le mot "chat" en utilisant la méthode "send_keys" avec l'élément de nom "q".
8. Validez la recherche en appuyant sur la touche entrée avec la méthode "send_keys" et la constante "Keys.ENTER".
9. Chargez le contenu de la page résultante dans une variable en utilisant la propriété "page_source".
10. Utilisez la fonction "re.findall" pour récupérer les liens contenant le mot "wikipedia".
11. Parcourez les liens pour trouver celui qui contient les mots "https" et "Chat".
12. Ouvrez le lien correspondant en utilisant la méthode "get".
13. Attendez 5 secondes pour charger le contenu de la page.
14. Fermez la session de navigateur en utilisant la méthode "quit".


**Exemple de solution :**

```python
from selenium import webdriver
from selenium.webdriver import Keys
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.common.by import By
import re
import time

options = Options()
options.binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
driver = webdriver.Firefox(executable_path=r'C:\WebDrivers\geckodriver.exe', options=options)
time.sleep(10)

driver.get('http://google.com/')

'''
# on veux accepter les conditions d'utilisation de google
# puis rechercher le mot "chat"
# puis cliquer sur le premier lien
# puis charger le contenu de la page dans une variable
# puis fermer le navigateur
'''

# on accepte les conditions d'utilisation de google
driver.find_element(By.ID, 'L2AGLb').click()

# on recherche le mot "chat"
driver.find_element(By.NAME, 'q').send_keys('chat')

# on valide la recherche en appuyant sur la touche entrée
driver.find_element(By.NAME, 'q').send_keys(Keys.ENTER)

# on charge le contenu de la page dans une variable
time.sleep(5)
page = driver.page_source

# on récupère le lien qui contient le mot "wikipedia"
links = re.findall(r'href="([^"]+)"', page)
for link in links:
    if "wikipedia" in link:
        if "https" in link:
            if "Chat" in link:
                # on ouvre le lien
                driver.get(link)

time.sleep(5)
# on ferme le navigateur
driver.quit()
```

## Les DataSets

Un jeu de données (en anglais, dataset) est un ensemble de données structurées qui peut être utilisé pour alimenter des algorithmes d'apprentissage automatique. Un jeu de données peut comprendre des données de différentes sources, comme des bases de données, des fichiers CSV (Comma Separated Values), des fichiers Excel, des images, des vidéos, etc.

Voici quelques exemples de formats de fichier couramment utilisés pour les jeux de données :

### CSV

- **CSV (Comma Separated Values)**: Le format CSV est un format de fichier texte qui est utilisé pour stocker des données structurées sous forme de tableau. Les données sont séparées par des virgules et chaque ligne représente une observation ou un enregistrement.

Voici un exemple de fichier CSV contenant des données sur des films :

```python
id,titre,année,réalisateur,note
1,Le Parrain,1972,Francis Ford Coppola,8.9
2,La Liste de Schindler,1993,Steven Spielberg,8.6
3,Le Seigneur des Anneaux: Le Retour du Roi,2003,Peter Jackson,8.9
4,Forrest Gump,1994,Robert Zemeckis,8.7
5,Pulp Fiction,1994,Quentin Tarantino,8.9
```

Chaque ligne du fichier représente un film, avec les colonnes **id**, **titre**, **année**, **réalisateur** et **note** contenant les informations correspondantes.

Pour lire ce fichier avec Pandas, vous pouvez utiliser la fonction **read_csv** :

```python
import pandas as pd

df = pd.read_csv('films.csv')
print(df)
```

Cela imprimera le contenu du fichier sous forme de tableau, avec chaque colonne représentée par une variable et chaque ligne correspondant à une observation :

Vous pouvez accéder à une colonne spécifique en utilisant la syntaxe `df['nom_colonne']`, par exemple :

```python
titres = df['titre']
print(titres)
```

Vous pouvez également sélectionner plusieurs colonnes en utilisant la syntaxe `df[['nom_colonne_1', 'nom_colonne_2']]`, ou accéder à une ligne spécifique en utilisant `df.loc[index]`. Consultez la documentation de Pandas pour en savoir plus sur les différentes options de sélection de données dans un DataFrame.

#### Exercice ouverture CSV

**Exercice :**

- Téléchargez un dataset contenant des dates à partir de Kaggle.
- Importez le dataset dans votre code en utilisant la fonction pd.read_csv().
- Convertir les dates en timestamp en utilisant la fonction time.mktime().
- Définir une date de comparaison en utilisant la fonction time.mktime().
- Utilisez un boucle pour afficher toutes les dates qui sont supérieures à la date de comparaison.
- Remplacer les dates originales dans le dataframe par les timestamps.
- Enregistrez le dataframe sous forme de fichier CSV en utilisant la fonction df.to_csv().

**Exemple de solution :**

```python
import pandas as pd
import time

df = pd.read_csv('Tesla Deaths - Deaths.csv')

dates = df['Date'].tolist()

dates2 = []

# on convertit les dates (1/17/2023) en timestamp
for i in range(len(dates)):
    try:
        dates2.append(time.mktime(time.strptime(dates[i], '%m/%d/%Y')))
    except Exception as e:
        # on supprime la ligne si la date n'est pas valide
        df.drop(i, inplace=True)

ma_date = time.mktime(time.strptime("8/14/2022", "%m/%d/%Y"))

# on affiche toutes les dates qui sont supérieures à la date du 14/08/2022

for i in range(len(dates)):
    if dates[i] < ma_date:
        print(df['Date'][i])


# on remplace les dates par des timestamps dans le dataframe
df['Date'] = dates2

# on enregistre le dataframe dans un fichier csv
df.to_csv('Tesla Deaths - Deaths.csv', index=False)

```

### Excel

- **Excel**: Le format Excel est un format de fichier couramment utilisé pour stocker des données structurées sous forme de tableaux. Il est facile à utiliser et permet de visualiser et de manipuler facilement les données.

Voici un exemple de fichier Excel contenant des données sur des films, avec une feuille de calcul nommée "films" :

Comme pour le fichier CSV, chaque ligne du fichier représente un film, avec les colonnes id, titre, année, réalisateur et note contenant les informations correspondantes.


| id | titre | année | réalisateur | note |
| -------- | -------- | -------- | --- | --- |
| 1     | Le Parrain     | 1972     | Francis Ford Coppola | 8.9 |
|2|	La Liste de Schindler	|1993	|Steven Spielberg|	8.6|
|3|	Le Seigneur des Anneaux|	2003	|Peter Jackson|	8.9|
|4|	Forrest Gump	|1994	|Robert Zemeckis	|8.7|
|5|	Pulp Fiction	|1994	|Quentin Tarantino	|8.9|

Pour lire ce fichier avec Pandas, vous pouvez utiliser la fonction read_excel :

```python
import pandas as pd

df = pd.read_excel('films.xlsx', sheet_name='films')
print(df)

```

Cela imprimera le contenu de la feuille de calcul "films" sous forme de tableau, avec chaque colonne représentée par une variable et chaque ligne correspondant à une observation.

Les instructions que vous pouvez utiliser sur le dataframe Pandas sont les mêmes que pour le CSV.

### JSON

- **JSON (JavaScript Object Notation)**: Le format JSON est un format de fichier de données qui est utilisé pour stocker et échanger des données structurées. Il est souvent utilisé pour stocker des données en ligne de commande et est facile à lire et à écrire pour les humains.

Pour lire des fichiers au format JSON en Python, vous pouvez utiliser la librairie built-in json. Voici un exemple de code qui lit un fichier JSON et imprime son contenu :

```python
import json

# Ouvrir le fichier en lecture
with open('data.json', 'r') as f:
    # Charger le contenu du fichier en utilisant json.load
    data = json.load(f)

# Afficher le contenu du fichier
print(data)
```

Le fichier JSON doit être structuré de manière à ce que les données puissent être converties en objets Python. Par exemple, si le fichier contient un dictionnaire avec des paires clé-valeur, vous pourrez accéder aux valeurs en utilisant la syntaxe `data['nom_clé']`. Si le fichier contient une liste d'objets, vous pourrez accéder aux éléments de la liste en utilisant la syntaxe `data[index]`. Consultez la documentation de la librairie `json` pour en savoir plus sur la façon de travailler avec des données au format JSON en Python.

#### GeoJSON :

GeoJSON est un format de données géospatiales basé sur le format de données JSON (JavaScript Object Notation). Il est utilisé pour représenter des données géographiques, telles que des points, des lignes et des polygones, sous forme de géométries codées en JSON.

Voici un exemple de données GeoJSON représentant un point géographique avec les coordonnées (longitude, latitude) :

```python
{
  "type": "Feature",
  "properties": {},
  "geometry": {
    "type": "Point",
    "coordinates": [
      -73.97,
      40.77
    ]
  }
}
```

Et voici un exemple de données GeoJSON représentant une ligne géographique avec plusieurs points :

```python
{
  "type": "Feature",
  "properties": {},
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [-73.97, 40.77],
      [-73.96, 40.76],
      [-73.95, 40.75],
      [-73.94, 40.74]
    ]
  }
}
```

GeoJSON est souvent utilisé pour stocker et échanger des données géographiques sur Internet, en particulier dans le contexte de développement d'applications Web de cartographie. Vous pouvez utiliser la librairie geojson en Python pour travailler avec des données au format GeoJSON.

#### Exercice

Téléchargez les fichiers geojson pour les communes à partir de l'adresse suivante : https://cadastre.data.gouv.fr/data/etalab-cadastre/2023-01-01/geojson/communes/

Modifiez le code pour afficher uniquement la commune de votre choix à l'aide de Folium en remplaçant les fichiers "cadastre-78646-batiments.json" et "cadastre-78646-parcelles.json" par les fichiers geojson pour votre commune sélectionnée.

Exécutez le code pour afficher la carte de votre commune avec les bâtiments en rouge et les parcelles en bleu.

```python
import json
import folium
import webbrowser

# load cadastre-01-batiments.json
with open('cadastre-78646-batiments.json') as f:
    data = json.load(f)

# load cadastre-78646-parcelles.json
with open('cadastre-78646-parcelles.json') as f:
    data2 = json.load(f)

# create a map
m = folium.Map(location=[48.85, 2.35], zoom_start=12)
# print the first building

for feature in data2['features']:
    # ajoute à la carte le polygone en bleu
    folium.GeoJson(feature['geometry'], style_function=lambda x: {'color': 'blue'}).add_to(m)

for feature in data["features"]:
    # ajoute à la carte le polygone en rouge
    folium.GeoJson(feature["geometry"], style_function=lambda x: {'color': 'red'}).add_to(m)

# save the map
m.save('map.html')
# launch the map
webbrowser.open('map.html')
```

![](https://md.picasoft.net/uploads/upload_1c71a6fb91282deaf50d7020f9bb0fef.png)


### XML

- **XML (Extensible Markup Language)** : Le format XML est un format de fichier de données qui est utilisé pour stocker et échanger des données structurées. Il est souvent utilisé pour stocker des données en ligne de commande et est facile à lire et à écrire pour les humains.

Pour lire des fichiers au format XML en Python, vous pouvez utiliser la librairie `lxml`. Voici un exemple de code qui lit un fichier XML et imprime le contenu de l'élément racine :

```python
import lxml.etree as ET

# Ouvrir le fichier en lecture
with open('data.xml', 'r') as f:
    # Charger le contenu du fichier en utilisant ET.parse
    tree = ET.parse(f)

# Afficher le contenu de l'élément racine
root = tree.getroot()
print(root.tag)
print(root.attrib)
```

Le fichier XML doit être structuré de manière à ce que les données puissent être converties en arborescence d'éléments. Vous pouvez alors accéder aux éléments enfants en utilisant la méthode `getchildren()`, et accéder aux attributs en utilisant la syntaxe `element.attrib['nom_attribut']`. Consultez la documentation de la librairie `lxml` pour en savoir plus sur la façon de travailler avec des données au format XML en Python.

### A partir d'une base de donnée

Avec Python, vous pouvez utiliser des bibliothèques telles que Pandas pour lire des données à partir d'une base de données (par exemple, MySQL, SQLite, etc.) et les stocker dans un objet DataFrame. Vous pouvez ensuite travailler avec ces données en utilisant les fonctionnalités de Pandas, telles que la manipulation de données, le traitement et l'analyse statistique. Finalement, vous pouvez enregistrer le DataFrame en tant que dataset dans un fichier (par exemple, CSV, Excel, etc.).

## Les API

Une API (Application Programming Interface) est un ensemble de règles et de méthodes pour accéder à un système ou à une application. Elle permet à des développeurs de créer des applications qui communiquent avec d'autres applications ou services en utilisant des fonctions spécifiques et en transmettant des données. Les API peuvent être utilisées pour accéder à des bases de données, des systèmes de fichiers, des services Web et bien plus encore.

### C'est quoi une route API ?

Une route API est un chemin d'accès défini pour accéder à une ressource spécifique sur un serveur via une API. Une API peut définir plusieurs routes, chacune associée à une URL différente et décrivant une fonctionnalité spécifique de l'API. Les requêtes envoyées à l'API sont dirigées vers une route particulière en fonction de l'URL spécifiée.

Par exemple, une API pour une application de gestion des tâches peut définir les routes suivantes :

- `/tasks` pour obtenir une liste de toutes les tâches.
- `/tasks/<task_id>` pour obtenir les détails d'une tâche particulière en fonction de son identifiant task_id.
- `/tasks/new` pour ajouter une nouvelle tâche.
- `/tasks/<task_id>/edit` pour modifier une tâche existante.

Lorsqu'une requête est envoyée à l'API, elle est dirigée vers la route appropriée en fonction de l'URL spécifiée, et la fonctionnalité associée à cette route est exécutée pour fournir une réponse à la requête.

### Récupérer des données depuis un API

Pour communiquer avec une API en Python, vous pouvez utiliser la bibliothèque Requests. Elle permet de faire des requêtes HTTP (GET, POST, PUT, DELETE, etc.) pour interagir avec l'API.

Exemple de code pour envoyer une requête GET à une API :

```python
import requests

url = "https://api.example.com/data"

response = requests.get(url)

if response.status_code == 200:
    data = response.json()
    print(data)
else:
    print("Erreur avec le code d'état :", response.status_code)
```

Dans ce code, nous envoyons une requête GET à l'API en appelant la méthode get de la bibliothèque Requests avec l'URL de l'API en tant qu'argument. La réponse est stockée dans l'objet response et nous pouvons vérifier le code d'état de la réponse pour savoir si la requête a réussi ou échoué. Si la requête réussit (code d'état 200), nous pouvons extraire les données de la réponse en appelant la méthode json.

La plupart des API retournent des données au format JSON, qui est un format de données léger et facile à lire pour les ordinateurs. En Python, vous pouvez utiliser la bibliothèque json pour parser les données JSON.

Voici un exemple d'appel API en Python avec des paramètres :

```python
import requests

url = "https://api.example.com/data"

payload = {
    "param1": "value1",
    "param2": "value2"
}

response = requests.get(url, params=payload)

if response.status_code == 200:
    data = response.json()
    print(data)
else:
    print("Erreur avec le code d'état :", response.status_code)
```

Dans ce code, nous définissons un dictionnaire `payload` qui contient les paramètres que nous voulons envoyer avec notre requête API. Nous les passons à la requête en utilisant le paramètre `params`. La réponse de l'API est ensuite stockée dans l'objet `response` et nous pouvons travailler avec les données de la même manière que dans l'exemple précédent.

#### Exercice sur la récupération de données depuis une API

**Exercice :**

Objectif : Afficher un graphique représentant la variation de la quantité de ETH en USD dans un noeud sur les derniers jours.

**Instructions :**

1. Importer les librairies requests, json, matplotlib.pyplot et datetime.
2. Créer une fonction get_page qui permet de récupérer le contenu d'une page web à partir d'une URL.
3. Initialiser une variable node avec la valeur "412204".
4. Récupérer le contenu de la page web "https://beaconcha.in/api/v1/validator/stats/" + node et le stocker dans une variable page.
5. Transformer la variable page en format JSON.
6. Récupérer le taux de conversion ETH/USD à l'aide de la page web "https://min-api.cryptocompare.com/data/price?fsym=ETH&tsyms=ETH,USD" et le stocker dans une variable convertio.
7. Transformer la variable convertio en format JSON.
8. Initialiser une liste vide days pour stocker les jours.
9. Boucler sur les données de la variable page pour ajouter chaque jour à la liste days.
10. Initialiser une liste vide eth pour stocker les quantités de ETH.
11. Boucler sur les données de la variable page pour ajouter à la liste eth la quantité de ETH à la fin de chaque jour.
12. Boucler sur les données de la variable page pour soustraire la quantité de ETH au début de chaque jour et la convertir en USD.
13. Initialiser une liste vide dates pour stocker les dates.
14. Boucler sur la longueur de la liste days (sans le dernier élément) pour ajouter à la liste dates la date d'aujourd'hui décrémentée de i jours.
15. Afficher un graphique en barres avec les données de la liste dates en abscisse et les données de la liste eth (sans le dernier élément) en ordonnée.

**Solution :**

```python
import requests
import json
import matplotlib.pyplot as plt
from datetime import datetime, timedelta


# function that retrieves the content of a web page
def get_page(url):
    # we get the content of the page
    page = requests.get(url).text
    # we return the content
    return page


today = datetime.now()
print(today)

node = "412204"
page = get_page("https://beaconcha.in/api/v1/validator/stats/" + node)
page = json.loads(page)

convertio = get_page("https://min-api.cryptocompare.com/data/price?fsym=ETH&tsyms=ETH,USD")
convertio = json.loads(convertio)

# 1 - les jours

days = []

for day in page['data']:
    days.append(day['day'])

# 2 - ETH à la fin

eth = []

for day in page['data']:
    eth.append(day['end_balance'])

# 3 - ETH au début

for i, day in enumerate(page['data']):
    eth[i] -= day['start_balance']
    eth[i] /= 1000000000
    eth[i] *= convertio['USD']

# 4 - Date du jour

dates = []

for i in range(len(days[:-1])):
    dates.append(today.date())
    today -= timedelta(days=1)

plt.bar(dates, eth[:-1])
plt.show()

```

![](https://md.picasoft.net/uploads/upload_58fa3d283b56d821b79ce24205b752b2.png)


### Exposer une API

Voici un exemple simple d'exposition d'une API avec Flask en Python :

```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route("/")
def index():
    return "Bienvenue sur l'API Flask"

@app.route("/data")
def data():
    response = {
        "status": "success",
        "data": [1, 2, 3, 4, 5]
    }
    return jsonify(response)

if __name__ == "__main__":
    app.run(debug=True)
```

Dans ce code, nous créons d'abord une instance de Flask en utilisant `app = Flask(__name__)`. Nous définissons ensuite deux routes pour notre API en utilisant les décorateurs `@app.route("/")` et `@app.route("/data")`. Les fonctions associées à ces routes renvoient simplement une chaîne de caractères pour la première route et un dictionnaire JSON pour la seconde. Enfin, nous exécutons notre application Flask en utilisant `app.run(debug=True)`.

Lorsque vous exécutez ce code, votre API sera disponible à l'adresse `http://localhost:5000/`. Vous pouvez accéder à la première route en accédant à `http://localhost:5000/`, et à la seconde en accédant à `http://localhost:5000/data`.

## Les Capteurs (IoT)

### Les Types de capteurs

Il existe de nombreux types de capteurs qui peuvent être utilisés pour récupérer des données. Voici quelques exemples courants :

#### Capteurs de température

Les capteurs de température mesurent la température en utilisant différents principes physiques tels que la résistance thermique, la thermoconductivité, la thermopile, la thermistance, etc.

Les capteurs de température à résistance thermique (RTD) mesurent la variation de résistance d'un matériau conducteur en fonction de la température. La résistance est mesurée en utilisant un circuit électrique.

Les capteurs de température à thermistance (NTC) utilisent le même principe que les RTD, mais avec des matériaux qui changent de résistance plus rapidement en réponse à des variations de température.

Les capteurs de température à thermocouple mesurent la différence de potentiel générée par la jonction de deux métaux différents en réponse à une variation de température.

Les capteurs infrarouges mesurent la radiance infrarouge émise par un objet pour en déduire sa température.

Dans l'ensemble, le signal mesuré par le capteur de température est généralement converti en une tension ou un courant électrique, qui peut être ensuite traité par une unité de traitement pour produire une mesure numérique de la température.

![](https://md.picasoft.net/uploads/upload_68a3c8db6aefa3abd9189a66c9e8cb09.png)

#### Capteurs d'humidité

Les capteurs d'humidité mesurent la quantité d'humidité relative ou absolue dans l'air. Il existe plusieurs types de capteurs d'humidité, chacun utilisant des principes physiques différents pour mesurer l'humidité.

Les capteurs d'humidité relative utilisent un matériau hygroscopique, qui absorbe ou libère de l'humidité en fonction de l'humidité relative de l'air. Cette variation d'humidité peut être mesurée en utilisant une variété de techniques, telles que la capacitance, la conductance, la résistance, etc.

Les capteurs d'humidité absolue utilisent un élément chauffant pour évaporer une petite quantité d'eau à partir d'un film mince d'eau. La quantité d'eau évaporée est directement proportionnelle à la quantité d'humidité absolue dans l'air. La température de l'élément chauffant est mesurée pour déterminer la quantité d'humidité dans l'air.

Les capteurs infrarouges mesurent également la quantité d'humidité en mesurant les variations dans l'absorption infrarouge d'un gaz hygrosensible.

Comme pour les capteurs de température, le signal mesuré par le capteur d'humidité est généralement converti en une tension ou un courant électrique, qui peut être ensuite traité par une unité de traitement pour produire une mesure numérique de l'humidité.

![](https://md.picasoft.net/uploads/upload_8a5a971125f2588a0e9e87882005c2a1.png)

#### Capteurs de mouvement

Les capteurs de mouvement mesurent la présence ou le mouvement dans une zone donnée. Il existe plusieurs types de capteurs de mouvement, chacun utilisant des principes physiques différents pour détecter le mouvement.

Les capteurs infrarouges passifs (PIR) utilisent la détection de la chaleur dégagée par des objets en mouvement pour déterminer la présence de mouvement dans une zone donnée. Les PIR utilisent un capteur de température infrarouge sensible à la température pour détecter les variations de température associées à des objets en mouvement.

Les capteurs ultrasoniques utilisent des ondes ultrasoniques pour mesurer la distance à un objet en mouvement. Les capteurs envoient des impulsions ultrasoniques et mesurent le temps nécessaire à l'onde pour rebondir sur un objet en mouvement et revenir au capteur.

Les capteurs de mouvement à la lumière utilisent la détection de la lumière réfléchie par un objet en mouvement pour déterminer la présence de mouvement dans une zone donnée. Les capteurs utilisent une source lumineuse et un détecteur pour mesurer les variations de la lumière réfléchie par un objet en mouvement.

Le signal mesuré par le capteur de mouvement est généralement converti en une tension ou un courant électrique, qui peut être ensuite traité par une unité de traitement pour déterminer la présence ou le mouvement dans une zone donnée. Les capteurs de mouvement peuvent être utilisés pour déclencher une alarme, un éclairage, un enregistrement vidéo, etc. en réponse à la détection de mouvement.

![](https://md.picasoft.net/uploads/upload_93d6323af1ada4c86a82b81baa939cfe.png)

#### Capteurs de lumière

Les capteurs de lumière mesurent la quantité de lumière dans une zone donnée. Il existe plusieurs types de capteurs de lumière, chacun utilisant des principes physiques différents pour mesurer la quantité de lumière.

Les photorésistances sont des composants électroniques qui modifient leur résistance en fonction de la quantité de lumière incidente. La variation de résistance peut être utilisée pour mesurer la quantité de lumière.

Les photodiodes sont des composants électroniques qui produisent un courant électrique en réponse à la lumière incidente. La quantité de courant produit est proportionnelle à la quantité de lumière.

Les phototransistors sont des transistors qui modifient leur gain en fonction de la quantité de lumière incidente. La variation de gain peut être utilisée pour mesurer la quantité de lumière.

Le signal mesuré par le capteur de lumière est généralement converti en une tension ou un courant électrique, qui peut être ensuite traité par une unité de traitement pour produire une mesure numérique de la quantité de lumière. Les capteurs de lumière peuvent être utilisés pour contrôler la luminosité d'un écran, ajuster la luminosité d'un éclairage, déterminer l'heure du jour, etc.

![](https://md.picasoft.net/uploads/upload_c2c64a8481dc6af590d600498c966622.png)

#### GPS

Le GPS (Global Positioning System) est un système de navigation par satellite qui permet de déterminer la position géographique d'un objet sur Terre. Les capteurs GPS mesurent la distance entre l'objet et plusieurs satellites GPS en orbite autour de la Terre. En utilisant ces mesures, le capteur GPS peut déterminer sa position géographique en trois dimensions (latitude, longitude et altitude).

Les satellites GPS transmettent des signaux contenant des informations de temps et de position. Le capteur GPS reçoit ces signaux et mesure le temps nécessaire pour les signaux de parvenir à partir des satellites. En utilisant la vitesse de la lumière connue, le capteur peut déterminer la distance à chaque satellite.

Le capteur GPS utilise ensuite une technique de triangulation pour déterminer sa position géographique. En utilisant les mesures de distance à plusieurs satellites, le capteur peut déterminer sa position géographique avec une très grande précision.

En général, les capteurs GPS intégrés dans les téléphones portables et les systèmes de navigation automobile utilisent une antenne GPS pour recevoir les signaux des satellites et un module de traitement pour déterminer la position géographique. Les données de position peuvent ensuite être utilisées pour afficher des cartes, des itinéraires de navigation, etc.

![](https://md.picasoft.net/uploads/upload_624c67ede643452dc5b5273fcc757867.png)

#### Sonar

Le sonar est un système qui utilise les ondes sonores pour mesurer la distance ou la profondeur d'objets sous l'eau. Le sonar envoie une onde sonore à partir d'un émetteur, puis mesure le temps nécessaire pour que l'onde rebondisse sur un objet et revienne à un récepteur. La vitesse connue du son dans l'eau permet de déterminer la distance à l'objet.

Les systèmes de sonar peuvent être actifs ou passifs. Les systèmes actifs envoient une onde sonore et mesurent le temps de retour, tandis que les systèmes passifs écoutent simplement les signaux sonores naturels présents dans l'environnement sous-marin.

Les systèmes de sonar peuvent être utilisés pour mesurer la profondeur de l'eau, détecter les obstacles sous-marins, localiser des objets sous-marins, surveiller les mouvements des animaux marins, etc. Les systèmes de sonar sont largement utilisés dans les navires de recherche scientifique, les sous-marins militaires, les bateaux de pêche et les systèmes de surveillance de la faune marine.

![](https://md.picasoft.net/uploads/upload_1398310a7521ec1c8598cad12859d863.png)

#### Lidar

Le LiDAR (Light Detection and Ranging) est un système qui utilise des faisceaux laser pour mesurer la distance et créer des cartes en 3D de l'environnement. Le LiDAR envoie un faisceau laser rapide et mesure le temps nécessaire pour que le faisceau rebondisse sur un objet et retourne au récepteur. La vitesse connue de la lumière permet de déterminer la distance à l'objet.

Les systèmes LiDAR peuvent être montés sur des véhicules terrestres, aériens ou spatiaux, et peuvent être utilisés pour créer des cartes en 3D de l'environnement, détecter et éviter les obstacles, surveiller les mouvements des animaux, etc.

Les systèmes LiDAR modernes utilisent souvent des lasers à haute fréquence pour envoyer des milliers de faisceaux laser par seconde. Les données de distance collectées peuvent être utilisées pour créer des images 3D en temps réel de l'environnement, ce qui en fait un outil utile pour la cartographie, la géologie, la géodésie, la reconnaissance des formes et la reconnaissance de la scène.

![](https://md.picasoft.net/uploads/upload_8ca2ee80b3cbad0dd1dc3f294318e8cf.png)

#### Caméra

Une caméra est un appareil qui convertit des images en électricité pour les enregistrer ou les transmettre. La plupart des caméras modernes fonctionnent en captant la lumière avec un objectif et en la focalisant sur un capteur d'image.

Le capteur d'image dans une caméra est généralement composé de millions de pixels (ou photosites), qui sont responsables de convertir la lumière en signaux électriques. Plus il y a de pixels sur un capteur, plus l'image sera détaillée. Les caméras peuvent avoir des capteurs de différentes tailles et résolutions, ce qui peut affecter la qualité de l'image finale.

Lorsqu'une image est capturée, la lumière est focalisée sur le capteur par l'objectif, où elle est convertie en signaux électriques. Les signaux sont ensuite traités par le processeur d'image intégré à la caméra pour produire une image numérique. Les caméras peuvent être conçues pour capturer des images fixes ou en mouvement, en couleur ou en noir et blanc, et peuvent être utilisées pour diverses applications, telles que la photographie, la vidéo, la surveillance et la reconnaissance de la scène.

![](https://md.picasoft.net/uploads/upload_fa88d5726ea5a21390adb364c6605625.png)

#### Conclusion

Le choix du type de capteur dépend de vos besoins en matière de collecte de données et de la nature de l'application que vous voulez créer.

### Récupérer les données

Il existe plusieurs manières de récupérer des données à partir de capteurs. Voici quelques méthodes courantes :

#### Communication série

Si le capteur est connecté à un ordinateur en utilisant un câble série, vous pouvez utiliser un logiciel ou une bibliothèque de programmation pour communiquer avec le capteur et récupérer les données.

Voici un exemple de script Python pour lire des données à partir d'un port série sur un système Windows:

```python
import serial

# Ouvrir un port série
ser = serial.Serial('COM3', 9600)

while True:
    # Lire les données en attente sur le port série
    data = ser.readline().decode().strip()
    print(data)

# Fermer le port série
ser.close()
```

Dans ce script, nous utilisons la bibliothèque `serial` pour communiquer avec le port série. La méthode `serial.Serial` ouvre un port série en spécifiant son nom et sa vitesse de transmission (9600 bauds dans ce cas).

La boucle `while` lit les données reçues sur le port série avec la méthode `readline`, qui lit les données jusqu'au caractère de nouvelle ligne. Nous décodons les données en utilisant `.decode()` et supprimons les espaces inutiles avec `.strip()` pour les rendre plus faciles à lire.

Enfin, nous fermons le port série en utilisant la méthode `close()`.

Notez que vous devrez peut-être ajuster le nom du port série (`COM3` dans l'exemple) pour refléter le nom du port sur votre système. Vous pouvez trouver le nom de votre port série en utilisant l'application "Gestionnaire de périphériques" de Windows.

Voici un exemple de script Python pour lire des données à partir d'un port série sur un système Linux:

```python
import serial

# Ouvrir un port série
ser = serial.Serial('/dev/ttyACM0', 9600)

while True:
    # Lire les données en attente sur le port série
    data = ser.readline().decode().strip()
    print(data)

# Fermer le port série
ser.close()
```

Ce script est similaire à l'exemple pour Windows, mais nous utilisons un nom de port différent pour refléter la convention de nommage des ports série sur Linux (`/dev/ttyACM0` dans ce cas).

Notez que vous devrez peut-être ajuster le nom du port série (`/dev/ttyACM0` dans l'exemple) pour refléter le nom du port sur votre système. Vous pouvez trouver le nom de votre port série en utilisant la commande `ls /dev/tty*` dans un terminal.

Sur Linux, l'accès aux ports série peut être limité par les autorisations d'accès.

#### Communication en réseau

Si le capteur est connecté à un réseau local, vous pouvez utiliser une requête HTTP pour récupérer les données à partir du capteur en utilisant son adresse IP.

Voici un exemple de code Python pour récupérer des données à partir d'un capteur connecté à un réseau local en utilisant une requête HTTP:

```python
import requests

url = "http://<ip-address>/data"

response = requests.get(url)

if response.status_code == 200:
    data = response.text
    print("Data received:", data)
else:
    print("Failed to retrieve data, status code:", response.status_code)
```

Remplacez `<ip-address>` par l'adresse IP du capteur sur le réseau local.

Ce code utilise la bibliothèque `requests` pour envoyer une requête GET à l'adresse URL du capteur. La réponse de la requête est vérifiée pour s'assurer qu'elle a réussi (code de statut 200) et les données reçues sont stockées dans la variable `data`.

Notez que la structure de l'URL et les données reçues dépendent de la façon dont le capteur a été configuré pour communiquer sur le réseau. Il est important de consulter la documentation du capteur pour savoir comment accéder aux données à partir d'une requête HTTP.

#### Récupération à partir d'un service cloud

Certains capteurs sont connectés à des services cloud, tels que AWS IoT ou Google Cloud IoT, qui peuvent être utilisés pour récupérer les données du capteur.

Voici un exemple de code Python pour récupérer des données à partir d'un capteur connecté à AWS IoT:

```python
import boto3

client = boto3.client("iot-data")

response = client.get_thing_shadow(thingName="<thing-name>")

if response["ResponseMetadata"]["HTTPStatusCode"] == 200:
    shadow = response["payload"].read().decode("utf-8")
    print("Data received:", shadow)
else:
    print("Failed to retrieve data, status code:", response["ResponseMetadata"]["HTTPStatusCode"])
```

Remplacez `<thing-name>` par le nom de la chose (device) du capteur dans AWS IoT.

Ce code utilise la bibliothèque `boto3` pour accéder à AWS IoT Data Plane et récupérer les données du capteur via la méthode `get_thing_shadow`. La réponse de la requête est vérifiée pour s'assurer qu'elle a réussi (code de statut 200) et les données reçues sont stockées dans la variable `shadow`.

Pour utiliser ce code, vous devez disposer d'une clé d'accès AWS valide et avoir configuré AWS IoT pour votre compte. Il est important de consulter la documentation d'AWS IoT pour savoir comment configurer les things (devices) et les autorisations nécessaires pour accéder aux données à partir d'AWS IoT Data Plane.

Notez que la méthode pour récupérer les données à partir d'autres services cloud, tels que Google Cloud IoT, peut varier et nécessite également de consulter leur documentation respective.

#### Protocoles de communication sans fil

Certaines technologies sans fil, telles que Zigbee, BLE et Z-Wave, peuvent être utilisées pour récupérer les données à partir de capteurs sans fil.

Voici un exemple de code Python pour récupérer des données à partir d'un capteur Bluetooth Low Energy (BLE) en utilisant la bibliothèque bluepy:

```python
from bluepy.btle import Peripheral, UUID

class BLEClient(Peripheral):
    def __init__(self, addr):
        Peripheral.__init__(self, addr)

    def read_data(self, service_uuid, characteristic_uuid):
        service = self.getServiceByUUID(service_uuid)
        characteristic = service.getCharacteristics(characteristic_uuid)[0]
        return characteristic.read()

# Connect to the BLE device
device = BLEClient("<device-address>")

# Read data from the BLE device
data = device.read_data(UUID("<service-uuid>"), UUID("<characteristic-uuid>"))
print("Data received:", data)

# Disconnect from the BLE device
device.disconnect()
```

Remplacez `<device-address>` par l'adresse physique du capteur BLE, `<service-uuid>` par l'UUID du service sur le capteur BLE et `<characteristic-uuid>` par l'UUID de la caractéristique sur le capteur BLE.

Ce code crée une classe `BLEClient` qui hérite de la classe `Peripheral` de la bibliothèque `bluepy`, ce qui permet de se connecter et de communiquer avec un périphérique BLE. La méthode `read_data` de la classe `BLEClient` est utilisée pour lire les données du capteur BLE à partir du service et de la caractéristique spécifiées. Les données reçues sont stockées dans la variable `data` et affichées à l'écran.

Notez que cet exemple n'est que pour la communication avec un capteur BLE et que les protocoles de communication sans fil tels que Zigbee et Z-Wave peuvent nécessiter des bibliothèques différentes et des implémentations de code différentes pour communiquer avec les capteurs associés. Il est important de consulter la documentation des protocoles de communication sans fil pour savoir comment communiquer avec les capteurs.

#### Conclusion

Le choix de la méthode dépend de la configuration de votre capteur et de vos besoins en matière de sécurité et de performance.

En utilisant une des méthodes ci-dessus, vous pouvez récupérer les données à partir de capteurs et les utiliser pour des analyses, des visualisations et des applications en temps réel. Par exemple, vous pouvez utiliser Python et les bibliothèques de traitement de données pour analyser les données et générer des graphiques en temps réel.

### Mise en application

Dans ce tutoriel, nous allons explorer deux exercices de visualisation de données en utilisant des capteurs connectés à une carte Arduino et en utilisant le langage de programmation Python pour enregistrer les données reçues via le port série dans un fichier CSV. Pour le premier exercice, nous utiliserons un capteur lidar pour récupérer la vue en coupe d'une pièce. Pour le second exercice, nous utiliserons un accéléromètre X, Y et Z pour mesurer les mouvements et déduire une position au cours du temps. Nous allons ensuite utiliser des bibliothèques Python telles que Matplotlib et Pandas pour visualiser les données de manière conviviale et informative.

#### Le Lidar

Ce code est un exemple de programme Arduino qui utilise un capteur LIDAR (LIDARLite) pour mesurer la distance à un objet. Il utilise la bibliothèque `LIDARLite` pour communiquer avec le capteur et calcule la distance en utilisant la méthode `distance()`. Le résultat est affiché via le port série à une vitesse de 9600 bauds. Le code comporte également un compteur de lecture qui permet d'effectuer une correction de biais du récepteur toutes les 100 lectures.

```c=
#include <Wire.h>
#include <LIDARLite.h>

// Instance de la classe LIDARLite pour le capteur lidar
LIDARLite lidarLite;

void setup()
{
  Serial.begin(9600); // Initialisation de la connexion série pour afficher les lectures de distance

  lidarLite.begin(0, true); // Mise en place de la configuration par défaut et I2C à 400 kHz
  lidarLite.configure(0); // Modifier ce nombre pour essayer des configurations alternatives

  // Initialisation du compteur de lecture
  cal_cnt = 0;
}

void loop()
{
  int dist;

  // Au début de chaque 100 lectures, effectuer une mesure avec correction de biais du récepteur
  if (cal_cnt == 0) {
    dist = lidarLite.distance(); // Avec correction de biais
  } else {
    dist = lidarLite.distance(false); // Sans correction de biais
  }

  // Incrémenter le compteur de lecture
  cal_cnt++;
  cal_cnt = cal_cnt % 100;

  // Affichage de la distance
  Serial.println(dist);

  delay(1);
}
```

Passons côté PC, pour enregistrer les 1800 premières valeurs du lidar dans un fichier CSV horodaté, vous pouvez utiliser le code suivant en Python:

```python
import serial
import csv
import datetime

ser = serial.Serial('COM3', 9600) # Initialize serial connection with the same baud rate as in the Arduino code

values = [] # List to store the LIDAR readings

for i in range(1800):
    value = int(ser.readline().decode().strip()) # Read the value from the serial connection and store it in the list
    values.append([datetime.datetime.now(), value]) # Store the current datetime and the value in the list

with open('data-lidar.csv', 'w', newline='') as csvfile: # Open a CSV file to store the data
    writer = csv.writer(csvfile, delimiter=',')
    writer.writerow(['Timestamp', 'Distance']) # Write the header of the CSV file
    writer.writerows(values) # Write the data to the CSV file

ser.close() # Close the serial connection
```

Ce code utilise la bibliothèque `serial` pour initialiser la connexion série avec l'Arduino et la bibliothèque `csv` pour écrire les données dans un fichier CSV. Le code lit 1800 lignes à partir de la connexion série, décode les valeurs et les stocke dans une liste `values` avec le timestamp actuel. Ensuite, le code ouvre un fichier CSV (ou le crée s'il n'existe pas) et écrit les données dans le fichier sous la forme de deux colonnes: Timestamp et Distance. Enfin, le code ferme la connexion série.

Pour installer la bibliothèque `serial` en Python, vous pouvez utiliser `pip` en exécutant la commande suivante dans votre terminal ou invite de commande:

```cmd=
pip install pyserial
```

Cela installera la bibliothèque `serial` et vous pourrez l'utiliser dans vos projets Python.


Pour finir, nous allons utiliser ce code pour ouvrir et visualiser le dataset précédement enregistré : 

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from shapely.geometry import Polygon

# Lire le fichier data-lidar.csv, séparateur = ",", encodage = utf-8
df = pd.read_csv('data-lidar.csv', sep=',', encoding='utf-8')

# Supprimer les 110 premières lignes car les données se chevauchent
df = df[110:]

# Convertir la colonne Temps en objet datetime
df['Time'] = pd.to_datetime(df['Time'])

# Calculer le temps en considérant un mouvement continu
total_time = (df['Time'].iloc[-1] - df['Time'].iloc[0]).total_seconds()
nb_lines = len(df.index)
frequency = total_time / nb_lines
range_ = list(np.arange(0, total_time, frequency))
df['Time'] = range_

# Calculer l'angle en considérant un mouvement continu
frequency_angle = 360 / nb_lines
angle = list(np.arange(0, 360, frequency_angle))
df['Angle'] = angle

# Tracer les données (Temps, Distance)
plt.plot(df['Time'], df['Distance'])
plt.xlabel('Temps (s)')
plt.ylabel('Distance (m)')
plt.show()

# Convertir l'angle, la distance en x, y
df['X'] = df['Distance'] * np.cos(df['Angle'] * np.pi / 180)
df['Y'] = df['Distance'] * np.sin(df['Angle'] * np.pi / 180)

# Tracer les données
plt.plot(df['X'], df['Y'])
plt.xlabel('X (cm)')
plt.ylabel('Y (cm)')
plt.show()

# Calculer la surface
tab_y = df['Y'].tolist()
tab_x = df['X'].tolist()
poly = Polygon(zip(tab_x, tab_y))

# Afficher la surface en cm2
print(str(poly.area) + " cm2")
# Conversion cm2 en m2
print(str(poly.area / 10000) + " m2")
```
Ce code utilise les bibliothèques `pandas`, `numpy`, `matplotlib` et `shapely` pour lire, manipuler et visualiser les données d'un fichier CSV contenant les données du lidar.

Tout d'abord, le fichier CSV est lu en utilisant `pd.read_csv`, les premières 110 lignes sont supprimées car elles sont en double, la colonne "Time" est convertie en un objet `datetime` en utilisant `pd.to_datetime` et un temps continu est calculé en utilisant les dates de début et de fin. De même, un angle continu est calculé en divisant 360 par le nombre de lignes. Les données (temps, distance) sont tracées à l'aide de `matplotlib`.

![](https://md.picasoft.net/uploads/upload_4086ca08394c8c2fa22e8f7a6cbff3cf.png)



Ensuite, les coordonnées (x, y) sont calculées à partir de l'angle et de la distance en utilisant les formules trigonométriques et les données sont de nouveau tracées.

![](https://md.picasoft.net/uploads/upload_2d1e4fd342bd85994f3a320f49baac6f.png)


Enfin, une surface est calculée en utilisant la bibliothèque `shapely` qui calcule la surface d'un polygone formé par les coordonnées (x, y). La surface est affichée en cm2 et en m2.


#### L'accéléromètre

Pour ce cas d'utilisation, nous pouvons utiliser une centrale inertielle et la relier à une carte Arduino pour récupérer les données dans un jeu de données. Cependant, pour simplifier les choses et rendre l'expérience plus facilement reproductible, nous allons utiliser l'application Android "Physics Toolbox Accelerometer" pour enregistrer nos données.

1. Installez l'application en cherchant Physics Toolbox Accelerometer
2. Ouvrez l'application et lancez un enregistrement en appuyant sur le bouton orange "+" quand vous êtes prêt
3. Rappuyez au même endroit quand vous avez finit
4. Enregistrez le fichier de sortie sur votre appareil et envoyez le par mail pour le récupérer sur votre ordinateur

![](https://md.picasoft.net/uploads/upload_f0e9425c57ccd31c665da03f7e865d69.png)

Le code suivant devras en suite être utilisé pour exploiter les données : 

```python
import pandas as pd
import matplotlib.pyplot as plt

# Lire les données séparateur : , et encodage : utf-8
data = pd.read_csv('2023-02-0715.28.33.csv', sep=';', encoding='utf-8')

# Calcule de la durée du mouvement
data['time'] = [float(i.replace(',', '.')) for i in data['time']]
duration = data['time'].iloc[-1] - data['time'].iloc[0]
print('Duration of the movement : ', duration, 's')

# renommer les colonnes
data.columns = ['time', 'X', 'Y', 'Z', 'Total']

# Convertissez les colonnes X, Y, Z, Total en colonnes flottantes.
data['X'] = [float(i.replace(',', '.')) for i in data['X']]
data['Y'] = [float(i.replace(',', '.')) for i in data['Y']]
data['Z'] = [float(i.replace(',', '.')) for i in data['Z']]
data['Total'] = [float(i.replace(',', '.')) for i in data['Total']]

# On traite les données d'accélération pour obtenir une vitesse et une position
# On affiche les données d'accélération, vitesse et position sur le même graphique
# Puis un graphe 3D de la position en fonction du temps

# On commence par supprimer la dérive et la gravité

data['X'] = data['X'] - data['X'].mean()
data['Y'] = data['Y'] - data['Y'].mean()
data['Z'] = data['Z'] - data['Z'].mean()

# On utilise la méthode de simpson pour intégrer les données d'accélération

# On crée une liste de 0 pour stocker les valeurs de vitesse et de position
Vx = [0] * len(data['time'])
Vy = [0] * len(data['time'])
Vz = [0] * len(data['time'])
Px = [0] * len(data['time'])
Py = [0] * len(data['time'])
Pz = [0] * len(data['time'])

# On calcule la vitesse et la position
for i in range(1, len(data['time'])):
    Vx[i] = Vx[i - 1] + (data['X'][i] + data['X'][i - 1]) * (data['time'][i] - data['time'][i - 1]) / 2
    Vy[i] = Vy[i - 1] + (data['Y'][i] + data['Y'][i - 1]) * (data['time'][i] - data['time'][i - 1]) / 2
    Vz[i] = Vz[i - 1] + (data['Z'][i] + data['Z'][i - 1]) * (data['time'][i] - data['time'][i - 1]) / 2
    Px[i] = Px[i - 1] + (Vx[i] + Vx[i - 1]) * (data['time'][i] - data['time'][i - 1]) / 2
    Py[i] = Py[i - 1] + (Vy[i] + Vy[i - 1]) * (data['time'][i] - data['time'][i - 1]) / 2
    Pz[i] = Pz[i - 1] + (Vz[i] + Vz[i - 1]) * (data['time'][i] - data['time'][i - 1]) / 2


data['Vx'] = Vx
data['Vy'] = Vy
data['Vz'] = Vz
data['Px'] = Px
data['Py'] = Py
data['Pz'] = Pz

# Tracez les données sur la même page avec 3 sous-graphes.
fig, (ax1, ax2, ax3) = plt.subplots(3, 1, sharex=True)
ax1.plot(data['time'], data['X'], label='X')
ax1.plot(data['time'], data['Y'], label='Y')
ax1.plot(data['time'], data['Z'], label='Z')
ax1.plot(data['time'], data['Total'], label='Total')
ax1.set_ylabel('Acceleration (m/s²)')
ax1.legend()
ax2.plot(data['time'], Vx, label='Vx')
ax2.plot(data['time'], Vy, label='Vy')
ax2.plot(data['time'], Vz, label='Vz')
ax2.set_ylabel('Speed (m/s)')
ax2.legend()
ax3.plot(data['time'], Px, label='Px')
ax3.plot(data['time'], Py, label='Py')
ax3.plot(data['time'], Pz, label='Pz')
ax3.set_ylabel('Position (m)')
ax3.set_xlabel('Time (s)')
ax3.legend()
plt.show()

# Affiche le déplacement dans un graphe à 3 dimentions
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.plot(Px, Py, Pz)
ax.set_xlabel('X (m)')
ax.set_ylabel('Y (m)')
ax.set_zlabel('Z (m)')
plt.show()
```

Ce code analyse les données d'accélération enregistrées dans un fichier CSV et effectue des opérations mathématiques pour en déduire la vitesse et la position à partir des données d'accélération. Le code utilise les bibliothèques Python Pandas pour lire les données à partir du fichier CSV et Matplotlib pour tracer les données et les visualiser graphiquement.

1. Importe des bibliothèques Pandas et Matplotlib sous les noms de "pd" et "plt".
2. Lit des données à partir d'un fichier CSV en utilisant la fonction read_csv de Pandas. Le fichier CSV utilise un séparateur ';' et une encodage UTF-8.
3. Le temps total du mouvement est calculé en soustrayant la première entrée de temps de la dernière entrée de temps.
4. Renomme les colonnes dans le jeu de données.
5. Convertit des colonnes X, Y, Z, Total en flottants.
6. Supprime de la dérive et de la gravité en soustrayant la moyenne de chaque colonne.
7. Intègre les données d'accélération en utilisant la méthode de Simpson pour calculer la vitesse et la position.
8. Trace les données d'accélération, vitesse et position sur le même graphique à l'aide de la fonction plot de Matplotlib.
![](https://md.picasoft.net/uploads/upload_0db317bf8d0ed73de8148841494b7e0b.png)
10. Trace un graphique 3D de la position en fonction du temps.
![](https://md.picasoft.net/uploads/upload_3172e5c154d4f91a8780fde00ce55b1a.png)

# Traitement des données

Il existe plusieurs bibliothèques Python qui peuvent être utilisées pour le prétraitement des données, notamment :

## Pandas

Pandas est une bibliothèque de manipulation de données qui permet de lire, manipuler et analyser facilement des données tabulaires. Elle offre de nombreuses fonctionnalités pour la manipulation de données, telles que la sélection, le filtrage, l'agrégation et l'élimination des doublons.

### Créer, lire et écrire

Voici les principales fonctionnalités de Pandas pour créer, lire et écrire des données :

1. Créer des données avec Pandas

    Pandas offre plusieurs moyens pour créer un DataFrame, tels que la conversion de données en mémoire (listes, dictionnaires, ndarrays, etc.) en un DataFrame.

    ```python
    import pandas as pd

    # Création d'un DataFrame à partir d'une liste
    data = [1,2,3,4,5]
    df = pd.DataFrame(data)
    print(df)

    # Création d'un DataFrame à partir d'un dictionnaire
    data = {'col1': [1,2,3,4,5],
        'col2': ['A', 'B', 'C', 'D', 'E']}
    df = pd.DataFrame(data)
    print(df)
    ```

2. Lire des données avec Pandas

    Pandas facilite la lecture de données à partir de fichiers tels que des fichiers CSV ou Excel.

    ```python
    # Lecture de données à partir d'un fichier CSV
    df = pd.read_csv('data.csv')
    print(df)

    # Lecture de données à partir d'un fichier Excel
    df = pd.read_excel('data.xlsx')
    print(df)
    ```

3. Écrire des données avec Pandas

    Pandas offre également des fonctionnalités pour écrire des données dans des fichiers tels que des fichiers CSV ou Excel.

    ```python
    # Écriture de données dans un fichier CSV
    df.to_csv('data_out.csv', index=False)
    
    # Écriture de données dans un fichier Excel
    df.to_excel('data_out.xlsx', index=False)
    ```

Ceci est une introduction aux bases de la création, de la lecture et de l'écriture de données avec Pandas. Il existe de nombreuses autres fonctionnalités pour manipuler et analyser les données, que nous couvrirons dans les prochaines parties de ce cours. Pandas offre une puissance et une flexibilité exceptionnelles pour travailler avec des données de différentes formes et tailles, ce qui en fait un outil incontournable pour les scientifiques des données et les développeurs.

### Indexation, sélection et affectation

Pandas est un outil puissant pour gérer les données structurées. Dans cette section, nous allons explorer les différentes méthodes pour faire de l'indexation, de la sélection et de l'affectation de données avec Pandas.

1. Indexation de données avec Pandas

    L'indexation est un processus consistant à sélectionner des lignes et des colonnes dans un DataFrame. Il existe plusieurs façons de faire de l'indexation dans Pandas. Nous pouvons sélectionner une ou plusieurs colonnes dans un DataFrame en utilisant la syntaxe suivante :
    
    ```python
    # Sélectionner une colonne dans un DataFrame
    df['col1']

    # Sélectionner plusieurs colonnes dans un DataFrame
    df[['col1', 'col2']]
    ```
    
    Pour sélectionner des lignes dans un DataFrame, nous pouvons utiliser les méthodes `loc` ou `iloc`. La méthode `loc` utilise les étiquettes d'index pour sélectionner des lignes tandis que la méthode `iloc` utilise les positions d'index :

    ```python 
    # Sélectionner des lignes dans un DataFrame en utilisant des étiquettes d'index
    df.loc[0]
    df.loc[0:2]

    # Sélectionner des lignes dans un DataFrame en utilisant des positions d'index
    df.iloc[0]
    df.iloc[0:2]
    ```

2. Sélection de données avec Pandas

    La sélection de données consiste à filtrer les données à partir d'un DataFrame en utilisant des conditions. Nous pouvons utiliser la syntaxe de l'opérateur de comparaison pour sélectionner des lignes en fonction d'une ou plusieurs conditions :
    
    ```python
    # Sélectionner des lignes à partir d'un DataFrame en utilisant une condition
    df[df['col1'] > 2]

    # Sélectionner des lignes à partir d'un DataFrame en utilisant plusieurs conditions
    df[(df['col1'] > 2) & (df['col2'] == 'A')]
    ```

3. Affectation de données avec Pandas

    Modifier des valeurs dans un DataFrame. Il peut y avoir plusieurs façons d'affecter des données dans un DataFrame, et cela dépend de la situation. Par exemple, vous pouvez affecter une valeur à une seule cellule ou à plusieurs cellules en utilisant une condition. Voici quelques exemples d'affectation de données avec Pandas :
    
    ```python 
    # Affecter une valeur à une seule cellule
    df.at[0, 'col1'] = 100
    
    # Affecter une valeur à plusieurs cellules en utilisant une condition
    df.loc[df['col1'] > 2, 'col2'] = 'F'
    
    # Affecter une nouvelle colonne avec des valeurs spécifiques
    df['col3'] = [10, 20, 30, 40, 50]
    
    # Affecter une nouvelle colonne en utilisant une fonction sur une colonne existante
    df['col4'] = df['col1'].apply(lambda x: x**2)
    ```

En utilisant ces méthodes d'affectation de données, vous pouvez facilement travailler avec des données dans un DataFrame et les transformer selon vos besoins.

### Fonctions de synthèse et cartes

Voici comment utiliser les principales fonctions de synthèse et les cartes de Pandas.

1. Fonctions de synthèse `Statistique` avec Pandas

    Les fonctions de synthèse sont des fonctions statistiques simples qui permettent de résumer les données d'un DataFrame. Avec Pandas, vous pouvez facilement calculer des statistiques telles que la moyenne, la somme, la somme cumulée, la déviation standard, etc. sur une colonne d'un DataFrame.

    Par exemple, pour calculer la moyenne d'une colonne nommée "col1", vous pouvez utiliser la fonction `mean()` comme ceci :
    
    ```python 
    # Calculer la moyenne d'une colonne
    df['col1'].mean()
    ```
    De même, pour calculer la somme d'une colonne nommée "col1", vous pouvez utiliser la fonction `sum()` comme ceci :
    
    ```python
    # Calculer la somme d'une colonne
    df['col1'].sum()
    ```
    
    Pour calculer la somme cumulée d'une colonne, vous pouvez utiliser la fonction `cumsum()` comme ceci :
    
    ```python
    # Calculer la somme cumulée d'une colonne
    df['col1'].cumsum()
    ```
    Et pour calculer la déviation standard d'une colonne, vous pouvez utiliser la fonction std() comme ceci :
    
    ```python 
    # Calculer la déviation standard d'une colonne
    df['col1'].std()
    ```

2. Fonctions de synthèse `Classique` avec Pandas

    Les fonctions de synthèse servent également à synthétiser les données d'un DataFrame ou d'une Series en fournissant des statistiques et des informations utiles. Voici une explication de trois fonctions de synthèse courantes :
    
    `describe()`: Cette fonction génère des statistiques descriptives pour chaque colonne d'un DataFrame ou d'une Series, telles que la moyenne, l'écart-type, le minimum, le maximum et les quartiles. Elle est utile pour avoir une vue d'ensemble rapide des données, notamment pour détecter les valeurs aberrantes ou les données manquantes.
    
    Exemple d'utilisation :
    ```python
    import pandas as pd

    df = pd.read_csv('data.csv')

    # Afficher les statistiques descriptives pour chaque colonne du DataFrame
    print(df.describe())
    ```
    
    `unique()`: Cette fonction renvoie un tableau des valeurs uniques d'une Series. Elle est utile pour identifier les valeurs uniques dans une colonne et les utilisations qui leur sont associées.
    
    Exemple d'utilisation :
    
    ```python 
    import pandas as pd

    df = pd.read_csv('data.csv')

    # Afficher les valeurs uniques de la colonne 'ville'
    print(df['ville'].unique())
    ```
    
    `value_counts()`: Cette fonction renvoie un tableau des valeurs uniques d'une Series et le nombre de fois qu'elles apparaissent dans la Series. Elle est utile pour identifier la distribution des valeurs dans une colonne.
    
    Exemple d'utilisation :
    
    ```python
    import pandas as pd

    df = pd.read_csv('data.csv')

    # Afficher le décompte des valeurs uniques dans la colonne 'ville'
    print(df['ville'].value_counts())
    ```

3. Cartes avec Pandas

    Les cartes sont une fonction de Pandas qui permet d'appliquer une fonction à chaque élément d'un DataFrame. Cela peut être utile pour effectuer des transformations sur les données d'un DataFrame.

    Par exemple, pour appliquer une fonction à chaque élément d'une colonne nommée "col1", vous pouvez utiliser la fonction `apply()` comme ceci :
    
    ```python
    # Appliquer une fonction à chaque élément d'une colonne
    df['col1'].apply(lambda x: x**2)
    ```
    
    Pour appliquer une fonction à chaque élément d'un DataFrame complet, vous pouvez utiliser la fonction `applymap()` comme ceci :
    
    ```python
    # Appliquer une fonction à chaque élément d'un DataFrame
    df.applymap(lambda x: x**2)
    ```

Notez que la fonction `applymap()` s'applique à toutes les cellules du DataFrame, tandis que la fonction `apply()` s'applique uniquement à une colonne ou une série spécifique.

### Groupement et tri

Le traitement des données peut parfois nécessiter le regroupement et le tri des données en fonction de critères spécifiques. Pandas, offre des fonctionnalités puissantes pour effectuer ces opérations.

1. Regroupement de données avec Pandas

    Le regroupement de données consiste à regrouper des enregistrements similaires en fonction de certaines colonnes d'un DataFrame. En utilisant la méthode `groupby` de Pandas, vous pouvez regrouper des données en utilisant une ou plusieurs colonnes d'un DataFrame.

    Par exemple, pour regrouper des données en utilisant une colonne, vous pouvez utiliser la syntaxe suivante :
    
    ```python
    # Regrouper des données en utilisant une colonne
    df.groupby('col1').sum()
    ```
    
    Où `col1` représente la colonne à utiliser pour regrouper les données et `sum` est l'agrégation à utiliser pour combiner les valeurs regroupées.

    De même, pour regrouper des données en utilisant plusieurs colonnes, vous pouvez utiliser la syntaxe suivante :
    
    ```python
    # Regrouper des données en utilisant plusieurs colonnes
    df.groupby(['col1', 'col2']).sum()
    ```

2. Tri de données avec Pandas

    Le tri de données consiste à classer les enregistrements dans un ordre spécifique en fonction de certaines colonnes d'un DataFrame. En utilisant la méthode `sort_values` de Pandas, vous pouvez trier les données en utilisant une ou plusieurs colonnes d'un DataFrame.

    Par exemple, pour trier les données en utilisant une colonne, vous pouvez utiliser la syntaxe suivante :
    
    ```python
    # Trier des données en utilisant une colonne
    df.sort_values(by='col1')
    ```
    Où `col1` représente la colonne à utiliser pour trier les données.

    De même, pour trier les données en utilisant plusieurs colonnes, vous pouvez utiliser la syntaxe suivante :
    ```python 
    
    # Trier des données en utilisant plusieurs colonnes
    df.sort_values(by=['col1', 'col2'])
    ```
    

Il est important de noter que lorsque vous triez les données en utilisant plusieurs colonnes, la priorité sera donnée à la première colonne, puis à la seconde, et ainsi de suite.

### Types de données et valeurs manquantes

En suite voici comment manipuler les types de données et les valeurs manquantes dans Pandas.

1. Types de données dans Pandas

    Pandas permet de déterminer et de changer facilement les types de données des colonnes dans un DataFrame. Pour obtenir le type de données d'une colonne, utilisez `.dtype` :
    ```python
    # Afficher le type de données d'une colonne
    df['col1'].dtype
    ```
    Pour changer le type de données d'une colonne, vous pouvez utiliser la méthode 
    ```python
    # Changer le type de données d'une colonne
    df['col1'] = df['col1'].astype(float)
    ```
    
2. Valeurs manquantes dans Pandas
    Il est fréquent de trouver des valeurs manquantes dans les données. Pandas offre plusieurs méthodes pour gérer ces valeurs manquantes.

    Pour compter le nombre de valeurs manquantes dans un DataFrame, vous pouvez utiliser la méthode `isna` combinée avec `sum` :
    ```python
    # Comptez les valeurs manquantes dans un DataFrame
    df.isna().sum()
    ```
    Pour supprimer les lignes contenant des valeurs manquantes, vous pouvez utiliser la méthode `dropna` :
    ```python
    # Supprimer les lignes contenant des valeurs manquantes
    df.dropna()
    ```
    Pour remplir les valeurs manquantes avec une valeur spécifique, vous pouvez utiliser la méthode `fillna` :
    ```python
    # Remplir les valeurs manquantes avec une valeur spécifique
    df.fillna(value=0)
    ```

Il est important de comprendre les différentes méthodes pour gérer les valeurs manquantes car cela peut avoir un impact significatif sur les analyses et les modèles futurs basés sur ces données.

### Renommer et combiner

Lorsque vous travaillez avec des données, il est souvent nécessaire de modifier les noms de colonnes et de fusionner des données à partir de différentes sources. Pandas offre des moyens faciles de faire cela.

1. Renommer des colonnes dans Pandas

    Pour renommer une ou plusieurs colonnes dans un DataFrame, vous pouvez utiliser la méthode `rename`. Il vous suffit de spécifier les noms actuels et les noms souhaités des colonnes dans un dictionnaire et d'utiliser la méthode `rename` comme suit:
    
    ```python 
    # Renommer une colonne
    df.rename(columns={'col1': 'column1'}, inplace=True)

    # Renommer plusieurs colonnes
    df.rename(columns={'col1': 'column1', 'col2': 'column2'}, inplace=True)
    ```

2. Fusionner des données avec Pandas

    Pour fusionner des données provenant de différents DataFrames, vous pouvez utiliser la fonction `merge` de Pandas. La fonction `merge` vous permet de fusionner des DataFrames en utilisant une ou plusieurs colonnes en tant que clés pour déterminer les correspondances entre les enregistrements. Il est également possible de spécifier le type de jointure à utiliser, telles que les jointures gauches, droites, intérieures ou extérieures. Voici un exemple:
    
    ```python 
    # Fusionner deux DataFrames en utilisant une colonne en tant que clé
    pd.merge(df1, df2, on='key_column')
    
    # Fusionner deux DataFrames en utilisant plusieurs colonnes en tant que clés
    pd.merge(df1, df2, on=['key_column1', 'key_column2'])
    
    # Fusionner deux DataFrames en utilisant l'opération de jointure appropriée (gauche, droite, intérieure ou extérieure)
    pd.merge(df1, df2, how='left')
    ```
    



### Exercices 

Pour accéder aux exercices Kaggle sur Pandas, il suffit de se rendre sur la rubrique "Learn" du site Kaggle (https://www.kaggle.com/learn), qui propose une grande variété de cours en ligne pour apprendre différentes compétences en science des données, dont Pandas. Une fois sur la page d'accueil de la rubrique "Learn", il suffit de rechercher le cours intitulé "Pandas" ou de naviguer dans la liste des cours proposés jusqu'à atteindre celui-ci. Le cours sur Pandas est gratuit et comprend plusieurs modules, chacun avec des exercices pratiques qui permettent d'appliquer les concepts étudiés dans le cours. Les exercices se présentent sous forme de notebooks Jupyter, qui sont exécutables directement dans l'environnement de Kaggle. Il est possible de suivre le cours à son propre rythme et de recommencer les exercices autant de fois que nécessaire pour consolider ses connaissances en Pandas.

## Nettoyage des données

### Traitement des valeurs manquantes

Pandas est un bibliothèque puissante pour le traitement des données en Python. Il propose plusieurs méthodes pour traiter les valeurs manquantes dans les données. Voici les principales méthodes que vous pouvez utiliser :

- `dropna` : Cette méthode permet de supprimer les lignes ou les colonnes qui contiennent des valeurs manquantes. Par exemple, si vous voulez supprimer toutes les lignes qui contiennent des valeurs manquantes, vous pouvez utiliser la méthode dropna comme suit :

    ```python
    import pandas as pd
    
    df = pd.read_csv("data.csv")
    df = df.dropna()
    ```
- `fillna` : Cette méthode permet de remplacer les valeurs manquantes par une valeur spécifique. Par exemple, si vous voulez remplacer toutes les valeurs manquantes par 0, vous pouvez utiliser la méthode fillna comme suit :

    ```python
    import pandas as pd
    
    df = pd.read_csv("data.csv")
    df = df.fillna(0)
    ```
    
- `interpolate` : Cette méthode permet de remplacer les valeurs manquantes en utilisant une interpolation linéaire. Par exemple, si vous voulez remplacer les valeurs manquantes en utilisant une interpolation linéaire, vous pouvez utiliser la méthode interpolate comme suit :

    ```python
    import pandas as pd
    
    df = pd.read_csv("data.csv")
    df = df.interpolate()
    ```
    
- `bfill` et `ffill` : Les méthodes bfill et ffill permettent de remplacer les valeurs manquantes en utilisant la valeur de la cellule précédente ou suivante respectivement. Par exemple, si vous voulez remplacer les valeurs manquantes en utilisant la valeur de la cellule suivante, vous pouvez utiliser la méthode ffill comme suit :

    ```python
    import pandas as pd
    
    df = pd.read_csv("data.csv")
    df = df.ffill()
    ```
### Mise à l'échelle et normalisation

La mise à l'échelle et la normalisation sont des techniques couramment utilisées pour préparer les données pour l'apprentissage automatique. Elles permettent de normaliser les valeurs des différentes variables pour qu'elles aient des distributions similaires et des échelles similaires. Cela peut aider les algorithmes d'apprentissage automatique à mieux fonctionner en réduisant les biais introduits par les différences d'échelle et de distribution des variables.

Il existe plusieurs techniques courantes pour la mise à l'échelle et la normalisation des données, notamment :

Mise à l'échelle min-max : Cette méthode consiste à réduire les valeurs d'une colonne à une plage spécifique, généralement entre 0 et 1. Cela peut être accompli en soustrayant la valeur minimale de la colonne à chaque valeur et en divisant le résultat par la différence entre la valeur minimale et la valeur maximale.

Standardisation : Cette méthode consiste à centrer les valeurs d'une colonne sur 0 en soustrayant la moyenne de la colonne à chaque valeur et en divisant le résultat par l'écart type de la colonne.

Normalisation L2 : Cette méthode consiste à normaliser les valeurs d'une colonne pour qu'elles aient une norme L2 (somme des carrés) égale à 1.

Avec pandas, vous pouvez facilement mettre à l'échelle et normaliser vos données en utilisant les fonctions de mise à l'échelle et de normalisation de scikit-learn. Par exemple, pour mettre à l'échelle les valeurs de la colonne 'col' à l'aide de la méthode min-max, vous pouvez faire :

```python
from sklearn.preprocessing import MinMaxScaler
import pandas as pd

df = pd.read_csv("data.csv")
scaler = MinMaxScaler()
df['col'] = scaler.fit_transform(df[['col']])
```

De manière similaire, vous pouvez utiliser les fonctions `StandardScaler` et `Normalizer` pour effectuer la standardisation et la normalisation L2 respectivement. Il est important de noter que vous devez normaliser ou mettre à l'échelle toutes les variables qui seront utilisées pour entraîner un algorithme d'apprentissage automatique, pas seulement une seule colonne.

### Analyse des dates

L'analyse des dates peut être un aspect important de la préparation des données pour l'apprentissage automatique. Les dates peuvent souvent être utilisées pour faire des prévisions, telles que la prévision des ventes en fonction des saisons ou des jours de la semaine.

Avec pandas, vous pouvez facilement travailler avec des données de dates en les convertissant en objets de type `datetime` en utilisant la fonction `pd.to_datetime`. Une fois que vous avez converti vos dates en objets `datetime`, vous pouvez extraire des informations telles que le jour de la semaine, le mois, l'année, etc. et les utiliser pour créer de nouvelles variables à inclure dans vos modèles d'apprentissage automatique.

Par exemple, pour convertir une colonne 'date' de chaînes de caractères en objets `datetime`, vous pouvez faire :

```python
import pandas as pd

df = pd.read_csv("data.csv")
df['date'] = pd.to_datetime(df['date'])
```

Une fois que vous avez converti vos dates en objets `datetime`, vous pouvez extraire des informations telles que le jour de la semaine en utilisant la méthode `dt.weekday`, comme ceci :

```python
df['weekday'] = df['date'].dt.weekday
```

Vous pouvez également créer des variables de saison en utilisant la méthode `dt.quarter` :

```python
df['season'] = df['date'].dt.quarter
```

Il est important de noter que, lorsque vous travaillez avec des dates, vous devez souvent traiter les données manquantes de manière spécifique pour garantir que vos modèles d'apprentissage automatique ne soient pas affectés par les données manquantes. Il est également souvent utile de faire un traitement des données de dates en utilisant des techniques telles que l'encodage de cyclage pour capturer les relations temporelles dans vos données.

### Encodage des caractères

Lorsque vous ouvrez un jeu de données avec pandas, il peut parfois y avoir des problèmes d'encodage de caractères qui peuvent affecter la qualité des données. Par exemple, certains caractères peuvent être mal codés et apparaître comme des symboles étranges ou des erreurs dans les données.

Pour gérer ces problèmes d'encodage de caractères lors de l'ouverture de jeux de données, vous pouvez spécifier l'encodage de caractères attendu lors de l'utilisation de la fonction `pd.read_csv` :

```python
df = pd.read_csv("data.csv", encoding="UTF-8")
```

Si vous n'êtes pas sûr de l'encodage de caractères utilisé dans votre jeu de données, vous pouvez utiliser la bibliothèque `chardet` pour détecter automatiquement l'encodage de caractères:

```python
import chardet

with open("data.csv", "rb") as f:
    result = chardet.detect(f.read())

df = pd.read_csv("data.csv", encoding=result['encoding'])
```

En spécifiant l'encodage attendu ou en utilisant la détection automatique de l'encodage de caractères, vous pouvez minimiser les erreurs d'encodage de caractères et garantir la qualité des données lors de l'ouverture de jeux de données avec pandas.

### Exercices

Pour accéder aux exercices Kaggle sur le Data Cleaning, il faut se rendre sur la rubrique "Learn" du site Kaggle, à l'adresse https://www.kaggle.com/learn. Ensuite, il faut sélectionner la section "Data Cleaning" qui propose plusieurs cours en ligne sur le nettoyage de données en utilisant Pandas et d'autres bibliothèques Python. Ces cours sont divisés en modules et chaque module contient plusieurs leçons avec des vidéos, des exercices pratiques, des quiz et des projets. Les utilisateurs peuvent accéder gratuitement aux cours et aux exercices en créant un compte Kaggle. En outre, les utilisateurs peuvent obtenir des certificats de réussite pour chaque cours et module en passant des évaluations finales.

# Visualisation des données

## Graphiques et Diagrammes

Voici quelques exemples de graphiques et de diagrammes couramment utilisés en apprentissage automatique :

- **Graphiques en barres** : Un graphique en barres est un type de graphique utilisé pour comparer les valeurs de différentes catégories. Chaque barre représente une catégorie et sa hauteur indique la valeur associée à cette catégorie.

![](https://md.picasoft.net/uploads/upload_1a5bc5ad7ed3159c9de991b5296fcb4f.png)

- **Diagrammes à secteurs** : Un diagramme à secteurs, également appelé camembert, est un type de diagramme qui permet de représenter la répartition des proportions d'un ensemble de valeurs. Chaque secteur du cercle représente une valeur et sa taille est proportionnelle à cette valeur.

![](https://md.picasoft.net/uploads/upload_83274aed153cfe03b76fe3b7d82a1110.png)

- **Diagrammes en boîte** : Un diagramme en boîte, également appelé boxplot, est un type de diagramme utilisé pour représenter la distribution d'une variable numérique. Il met en évidence les quartiles de la distribution, c'est-à-dire les valeurs qui séparent la distribution en quatre parties égales.

![](https://md.picasoft.net/uploads/upload_e33e93d12d9628bce08aff94e1843a86.png)

La médiane d'une distribution est la valeur qui sépare la distribution en deux parties égales. Si la distribution a un nombre pair d'observations, la médiane est la valeur moyenne de ces observations. Si la distribution a un nombre impair d'observations, la médiane est la valeur de l'observation du milieu.

Les quartiles d'une distribution sont les valeurs qui séparent la distribution en quatre parties égales. Le premier quartile (Q1) est la valeur qui sépare la distribution en deux parties égales, tout comme la médiane, mais en ne prenant en compte que la moitié inférieure de la distribution. Le troisième quartile (Q3) est la valeur qui sépare la distribution en deux parties égales, tout comme la médiane, mais en ne prenant en compte que la moitié supérieure de la distribution.

Les valeurs aberrantes, également appelées valeurs extrêmes, sont les valeurs qui se trouvent en dehors de l'intervalle des quartiles dans une distribution. Elles peuvent indiquer une anomalie ou une erreur de mesure dans les données.

Dans un diagramme en boîte, la boîte indique l'intervalle des quartiles, tandis que les "moustaches" indiquent les valeurs extrêmes (hors de l'intervalle des quartiles). Les points individuels représentent les valeurs aberrantes.

- **Nuages de points** : Un nuage de points est un type de graphique qui permet de représenter deux variables numériques en plaçant chaque point à l'emplacement correspondant à ses valeurs sur les deux axes.

![](https://md.picasoft.net/uploads/upload_0983822c7b85faf9958c6fa11cf90d39.png)

La position de chaque point sur l'axe des x (coûts de production) et l'axe des y (ventes) indique les valeurs correspondantes pour chaque produit.

- **Courbes de régression** : Une courbe de régression est un type de graphique utilisé pour représenter la relation entre une variable prédictive (également appelée variable explicative) et une variable cible (également appelée variable à prédire). La courbe de régression essaie de "suivre" le nuage de points en dessinant une ligne qui minimise l'erreur de prédiction.

![](https://md.picasoft.net/uploads/upload_ecb33a1fc2ed2627fa4ef9be31549a38.png)

- **Matrices de confusion** : Une matrice de confusion est un tableau utilisé pour visualiser les performances d'un classifieur en comparant les valeurs prédites aux valeurs réelles. Chaque ligne de la matrice représente les valeurs réelles, tandis que chaque colonne représente les valeurs prédites.

```python
# valeurs réelles
y_true = [0, 0, 0, 1, 1, 1, 1, 1, 1]

# valeurs prédites
y_pred = [0, 0, 1, 1, 1, 0, 1, 1, 1]
```

![](https://md.picasoft.net/uploads/upload_09a6c495eb715d5c74ac2727258976f2.png)

Les valeurs de la matrice indiquent le nombre de fois où une valeur réelle a été prédite comme telle (diagonale) ou comme une valeur différente (hors de la diagonale). Par exemple, dans l'exemple de code ci-dessus, la matrice indique que la valeur 0 a été prédite correctement 2 fois, mais a également été prédite comme une valeur 1 une fois. De même, la valeur 1 a été prédite correctement 4 fois, mais a également été prédite comme une valeur 0 deux fois.


Il existe de nombreux autres types de graphiques et de diagrammes qui peuvent être utiles en apprentissage automatique, en fonction des données et des objectifs de l'analyse.

## Matplotlib

Matplotlib est un module de tracé de graphiques en Python qui peut être utilisé pour afficher des statistiques en temps réel ou en fin de partie, comme le nombre de points en fonction du temps de jeu, le remplissage de la grille au cours du temps, la proportion de haut, bas, droite, gauche, les combinaisons de touches consécutives préférées du joueur, etc.

Elle permet de générer des figures de différents types (courbes, histogrammes, barres, etc.) de manière interactive ou en enregistrant les figures sous différents formats (PNG, PDF, etc.).

Voici quelques exemples de ce que Matplotlib peut faire :

**Tracer une courbe simple** :

```python
import matplotlib.pyplot as plt

# Définition des données
x = [1, 2, 3, 4]
y = [1, 4, 9, 16]

# Création de la figure
plt.plot(x, y)

# Affichage de la figure
plt.show()
```

![](https://md.picasoft.net/uploads/upload_815281ef542a18b980a4abec73d3f1f6.png)


**Créer un histogramme** :

```python
import matplotlib.pyplot as plt
import numpy as np

# Génération de données aléatoires
data = np.random.normal(5, 3, 1000)

# Création de la figure
plt.hist(data, bins=50)

# Affichage de la figure
plt.show()
```

![](https://md.picasoft.net/uploads/upload_7aaff39351cdf929835764021efdd09e.png)


**Afficher plusieurs courbes sur un même graphe** :

```python
import matplotlib.pyplot as plt

# Définition des données
x = [1, 2, 3, 4]
y1 = [1, 4, 9, 16]
y2 = [2, 8, 18, 32]

# Création de la figure
plt.plot(x, y1, label="y = x^2")
plt.plot(x, y2, label="y = 2*x^2")

# Ajout d'une légende
plt.legend()

# Affichage de la figure
plt.show()
```

![](https://md.picasoft.net/uploads/upload_1bcb1c54147b6435f7a898dc2180a05b.png)

**Afficher un diagramme en toile d'araignée**:

Un radar chart, également appelé "diagramme en toile d'araignée", est un type de graphique utilisé pour afficher les valeurs d'une série de données pour plusieurs variables dans un même graphique. Les variables sont représentées sur les rayons d'un diagramme en forme de cercle, avec les valeurs indiquées sur la longueur de chaque rayon.

```python
import matplotlib.pyplot as plt
import numpy as np

# Crée un radar chart avec 4 axes
categories = ['A', 'B', 'C', 'D']
N = len(categories)

# Crée les valeurs pour chaque axe
values = [3, 1, 2, 4]

# Crée un angle pour chaque axe
angles = [n / float(N) * 2 * np.pi for n in range(N)]
angles += angles[:1]

# Initialise le graphique
ax = plt.subplot(111, polar=True)

# Dessine les axes
plt.xticks(angles[:-1], categories)

# Dessine les yticks
ax.set_rlabel_position(0)
plt.yticks([1, 2, 3, 4], ["1", "2", "3", "4"], color="grey", size=7)
plt.ylim(0, 4)

# Dessine les données
values += values[:1]
ax.plot(angles, values, linewidth=1, linestyle='solid')

# Rempli l'interieur
ax.fill(angles, values, 'b', alpha=0.1)

# Affiche le graphique
plt.show()
```
![](https://md.picasoft.net/uploads/upload_d0bf1ec1ebba5607db8f515189b6ecfe.png)


Le radar chart est utile pour visualiser les différences et les similitudes entre plusieurs éléments sur plusieurs dimensions. Cependant, il peut être difficile de comparer les valeurs précises sur un radar chart, car la distance entre les rayons n'est pas linéaire.

### Reproduire les graphes précédents

- **Diagrammes à secteurs** :

```python
import matplotlib.pyplot as plt

# données de ventes de produits (en milliers d'unités)
ventes = [55, 45, 70, 60, 50]

# noms des produits
produits = ['Produit 1', 'Produit 2', 'Produit 3', 'Produit 4', 'Produit 5']

# création du diagramme à secteurs
plt.pie(ventes, labels=produits)

# ajout d'un titre
plt.title('Répartition des ventes de produits')

# affichage du graphique
plt.show()
```

Ce code génère un diagramme à secteurs avec cinq secteurs, chacun représentant les ventes d'un produit différent. La taille de chaque secteur est proportionnelle au nombre d'unités vendues.

- **Diagrammes en boîte** :

```python
import matplotlib.pyplot as plt

# données de ventes de produits (en milliers d'unités)
ventes = [55, 45, 70, 60, 50, 40, 30, 20, 10]

# création du diagramme en boîte
plt.boxplot(ventes)

# ajout d'un titre
plt.title('Distribution des ventes de produits')

# affichage du graphique
plt.show()
```

Ce code génère un diagramme en boîte qui représente la distribution des ventes de produits. La boîte indique les quartiles de la distribution, tandis que les "moustaches" indiquent les valeurs extrêmes (hors de l'intervalle des quartiles). Les points individuels représentent les valeurs aberrantes.

- **Nuage de points**:

```python
import matplotlib.pyplot as plt

# données de ventes de produits (en milliers d'unités)
ventes = [55, 45, 70, 60, 50]

# coûts de production des produits (en milliers d'euros)
couts = [50, 40, 65, 55, 45]

# création du nuage de points
plt.scatter(couts, ventes)

# ajout d'un titre
plt.title('Ventes de produits en fonction des coûts de production')

# ajout de légendes aux axes
plt.xlabel('Coûts de production (en milliers d'euros)')
plt.ylabel('Ventes (en milliers d'unités)')

# affichage du graphique
plt.show()
```

Ce code génère un nuage de points avec cinq points, chacun représentant les ventes et les coûts de production d'un produit différent. La position de chaque point sur l'axe des x (coûts de production) et l'axe des y (ventes) indique les valeurs correspondantes pour chaque produit.

- **Courbes de régression** :

```python
import matplotlib.pyplot as plt
import numpy as np
import random

# génération de données aléatoires pour les ventes et les coûts de production
ventes = []
couts = []
for i in range(20):
  vente = 50 + random.randint(-1, 1) + i
  cout = 50 + random.randint(-1, 1) + i
  ventes.append(vente)
  couts.append(cout)

# utilisation de la fonction polyfit de NumPy pour ajuster une courbe de régression aux données
coeffs = np.polyfit(couts, ventes, 1)

# génération de valeurs prédites pour chaque coût de production
predictions = np.polyval(coeffs, couts)

# création du nuage de points
plt.scatter(couts, ventes)

# ajout de la courbe de régression au graphique
plt.plot(couts, predictions, color='red')

# ajout d'un titre
plt.title('Ventes de produits en fonction des coûts de production')

# ajout de légendes aux axes
plt.xlabel('Coûts de production (en milliers deuros)')
plt.ylabel('Ventes (en milliers dunités)')

# affichage du graphique
plt.show()
```

Ce code génère un nuage de points avec 20 points aléatoires, chacun représentant les ventes et les coûts de production d'un produit différent. La position de chaque point sur l'axe des x (coûts de production) et l'axe des y (ventes) indique les valeurs correspondantes pour chaque produit. La courbe de régression en rouge essaie de suivre le nuage de points en minimisant l'erreur de prédiction, mais elle est perturbée par le bruit dans les données.

- **Matrice de confusion** :

```python
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.metrics import confusion_matrix

# valeurs réelles
y_true = [0, 0, 0, 1, 1, 1, 1, 1, 1]

# valeurs prédites
y_pred = [0, 0, 1, 1, 1, 0, 1, 1, 1]

# utilisation de la fonction confusion_matrix de Scikit-learn pour générer la matrice de confusion
matrix = confusion_matrix(y_true, y_pred)

# utilisation de la fonction heatmap de Seaborn pour visualiser la matrice de confusion
sns.heatmap(matrix, annot=True, fmt='d', cmap='Blues')

# ajout de légendes aux axes
plt.xlabel('Valeurs prédites')
plt.ylabel('Valeurs réelles')

# affichage de la matrice de confusion
plt.show()
```

Ce code génère une matrice de confusion qui compare les valeurs réelles (0 ou 1) aux valeurs prédites par un classifieur.

Matplotlib est une bibliothèque très puissante qui permet de créer des figures de très haute qualité. Elle est particulièrement utile pour la visualisation de données scientifiques ou pour l'intégration de graphes dans des documents ou des présentations.

## Seaborn

Seaborn est une bibliothèque de visualisation de données basée sur Matplotlib qui offre de nombreux styles et options de personnalisation pour les graphiques. Elle est particulièrement utile pour l'analyse de données statistiques.

Voici quelques exemples d'utilisation de Seaborn pour la visualisation de données en Python :

1. Distribution univariée : pour visualiser la distribution d'une variable individuelle, vous pouvez utiliser un histogramme ou un violin plot :

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Charger un jeu de données
titanic = sns.load_dataset("titanic")

# Histogramme
sns.histplot(titanic["fare"])

# Violin Plot
sns.violinplot(x=titanic["fare"])
```

2. Distribution bivariée : pour visualiser la relation entre deux variables, vous pouvez utiliser un nuage de points, un scatter plot ou une heatmap :

```python
# Nuage de points
sns.jointplot(x="age", y="fare", data=titanic)

# Scatter Plot
sns.scatterplot(x="age", y="fare", data=titanic)

# Heatmap
sns.heatmap(titanic.corr(), annot=True)
```

3. Distribution multivariée : pour visualiser les relations entre plusieurs variables, vous pouvez utiliser un scatter plot pair, un pair plot ou un facets plot :

```python
# Scatter Plot pair
sns.pairplot(titanic)

# Pair Plot
sns.pairplot(titanic, hue="survived")

# Facets Plot
g = sns.FacetGrid(titanic, col="survived")
g.map(sns.histplot, "age")
```

4. Vous pouvez utiliser la bibliothèque `seaborn` pour afficher une `heatmap` des corrélations entre vos variables. Voici comment faire:

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Importation de votre jeu de données
df = pd.read_csv("path/to/your/data.csv")

# Calcul des corrélations entre les variables
corr = df.corr()

# Création de la heatmap
sns.heatmap(corr, annot=True, cmap='coolwarm')

# Affichage de la heatmap
plt.show()
```

Ces exemples vous montrent comment utiliser Seaborn pour visualiser différents types de distributions de données en utilisant différentes techniques de visualisation. Vous pouvez personnaliser ces graphiques en utilisant les nombreuses options de Seaborn pour ajuster les couleurs, les styles, les étiquettes et d'autres aspects de la visualisation.

## Visualisation de données en ligne

Dash est une bibliothèque open source pour la création d'applications web interactives en Python. Il est basé sur la bibliothèque Flask et utilise Plotly pour la visualisation de données.

Avec Dash, vous pouvez créer des tableaux de bord, des applications de données interactives, des interfaces utilisateur complexes avec des graphiques, des tableaux et d'autres éléments de visualisation. Dash vous permet également de connecter des modèles de machine learning et de les intégrer directement dans des applications web interactives.

L'un des points forts de Dash est sa facilité d'utilisation. Il utilise une syntaxe simple pour décrire les éléments de l'interface utilisateur et les interactions avec les données, ce qui permet de développer rapidement des applications web interactives. De plus, Dash offre une forte intégration avec d'autres bibliothèques de visualisation telles que Plotly, ce qui vous permet de créer facilement des graphiques riches et interactifs.

En résumé, Dash est une bibliothèque puissante pour la création d'applications web interactives en Python, offrant une facilité d'utilisation, une forte intégration avec d'autres bibliothèques et une grande flexibilité pour la personnalisation des graphiques et des interfaces utilisateur.

Voici un exemple de code pour afficher une visualisation de données en temps réel avec Dash :

```python
import dash
import dash_core_components as dcc
import dash_html_components as html
import plotly.express as px
import random
import pandas as pd
import time

app = dash.Dash(__name__)

# Créer un DataFrame avec des données aléatoires pour simuler des données en temps réel
df = pd.DataFrame({'x': [0], 'y': [0]})

app.layout = html.Div([
    dcc.Graph(id='live-graph', animate=True),
    dcc.Interval(
        id='graph-update',
        interval=1000, # Mettre à jour la visualisation toutes les 1 secondes
        n_intervals=0
    ),
])

@app.callback(Output('live-graph', 'figure'),
              [Input('graph-update', 'n_intervals')])
def update_graph(n):
    # Ajouter des données aléatoires au DataFrame
    df.loc[n+1] = [n+1, random.randint(0, 100)]
    fig = px.line(df, x='x', y='y')
    return fig

if __name__ == '__main__':
    app.run_server(debug=True)
```

Ce code crée un graphique en temps réel qui met à jour la visualisation toutes les 1 secondes avec des données aléatoires. La visualisation utilise Plotly Express pour tracer une ligne en fonction de `x` et `y`, où `x` représente le temps et `y` représente les données aléatoires.

L'interface utilisateur est définie en utilisant la syntaxe Dash HTML, qui vous permet de décrire les éléments de l'interface utilisateur en utilisant du code HTML simple. La mise à jour de la visualisation est implémentée en utilisant un `callback` de Dash, qui est déclenché par l'intervalle de temps défini avec `dcc.Interval`.

Voici un autre exemple qui affiche des point avec une coordonnée x, y aléatoire :

```python
import dash
from dash import Output, Input
import dash_core_components as dcc
import dash_html_components as html
import plotly.express as px
import random

app = dash.Dash(__name__)
x = []
y = []

# on créé la page web avec Dash
app.layout = html.Div([
    html.H1("Dash Graphique"),
    html.Div([
        html.Button("Ajouter un point", id="button"),
    ]),
    dcc.Graph(id="graph"),
])


# on ajoute un point au graphique
@app.callback(
    Output("graph", "figure"),
    Input("button", "n_clicks"),
)
def add_point(n_clicks):
    x.append(random.randint(0, 100))
    y.append(random.randint(0, 100))
    return px.scatter(x=x, y=y)


if __name__ == "__main__":
    app.run_server(debug=True)

```

Tout d'abord, les modules nécessaires sont importés, notamment la bibliothèque Dash et les composants d'interface utilisateur (UI) de base tels que `html` et `dcc`. En outre, la bibliothèque Plotly Express est également importée pour créer le graphique.

Ensuite, un objet Dash est créé, qui est stocké dans la variable `app`.

Le code crée une liste vide `x` et une autre liste vide `y`. Ensuite, l'interface utilisateur est créée en utilisant les composants `html.Div` et `dcc.Graph`. L'interface utilisateur se compose d'un titre (`html.H1`), d'un bouton (`html.Button`) et d'un graphique (`dcc.Graph`). Le graphique est initialisé avec des données vides.

Enfin, la fonction `add_point` est définie en utilisant la décoration `@app.callback`. Cette fonction est appelée chaque fois que le bouton est cliqué (`Input("button", "n_clicks")`). La fonction ajoute un point aléatoire (`random.randint(0, 100)`) à la fin des listes `x` et `y`, et renvoie un nouveau graphique mis à jour avec le nouveau point ajouté (`px.scatter(x=x, y=y)`). L'objet de figure renvoyé est alors mis à jour dans le graphique en temps réel, sans avoir besoin de recharger la page.

Enfin, l'application est exécutée en utilisant `app.run_server(debug=True)`, ce qui lance l'application web sur un serveur local et active le mode de débogage.



Ces deux codes vous donne un aperçu de la façon dont vous pouvez utiliser Dash pour créer des visualisations de données en temps réel en Python. Vous pouvez personnaliser cette visualisation en utilisant les nombreuses fonctionnalités de Plotly et Dash pour créer des graphiques riches et interactifs.

### Exemple géographique

Pour afficher un polygone géojson sur une carte Leaflet avec Dash, vous pouvez utiliser le composant `dash-leaflet` qui fournit des composants pour créer des cartes interactives avec Leaflet en utilisant Dash. Voici un exemple de code pour afficher un polygone géojson sur une carte Leaflet avec Dash :

```python
import json
import dash
import dash_leaflet as dl
from dash import html, dcc
import random

app = dash.Dash(__name__)
# Charger le geojson
with open("cadastre-78646-batiments.json") as f:
    geojson = json.load(f)

# on coupe le geojson en 4 morceaux
nb_features = len(geojson["features"])
geojson1 = geojson["features"][0:nb_features // 4]
geojson2 = geojson["features"][nb_features // 4:nb_features // 2]
geojson3 = geojson["features"][nb_features // 2:nb_features * 3 // 4]
geojson4 = geojson["features"][nb_features * 3 // 4:]

# on remet l'en-tête du geojson
geojson1 = {"type": "FeatureCollection", "features": geojson1}
geojson2 = {"type": "FeatureCollection", "features": geojson2}
geojson3 = {"type": "FeatureCollection", "features": geojson3}
geojson4 = {"type": "FeatureCollection", "features": geojson4}

# on crée la carte et on met nos polygones de couleur rouge, vert, bleu et jaune
map = dl.Map(
    children=[
        # DuplicateIdError
        dl.TileLayer(),
        dl.GeoJSON(data=geojson1, id="geojson1", options=dict(style=dict(color="red"))),
        dl.GeoJSON(data=geojson2, id="geojson2", options=dict(style=dict(color="green"))),
        dl.GeoJSON(data=geojson3, id="geojson3", options=dict(style=dict(color="blue"))),
        dl.GeoJSON(data=geojson4, id="geojson4", options=dict(style=dict(color="yellow"))),
    ],
    center=[48.80483661413611, 2.120409041231981],
    zoom=13,
    style={"width": "100%", "height": "100vh", "margin": "auto", "display": "block"},
)

# rouge -> vert -> bleu -> jaune
# batiments1 -> batiments2 -> batiments3 -> batiments4

legend = html.Div(
    [
        html.Div(
            [
                html.Div(
                    style={
                        "width": "50px",
                        "height": "50px",
                        "backgroundColor": "red",
                        "border": "1px solid black",
                    }
                ),
                html.Div("Batiments 1"),
            ],
            style={"display": "flex", "align-items": "center"},
        ),
        html.Div(
            [
                html.Div(
                    style={
                        "width": "50px",
                        "height": "50px",
                        "backgroundColor": "green",
                        "border": "1px solid black",
                    }
                ),
                html.Div("Batiments 2"),
            ],
            style={"display": "flex", "align-items": "center"},
        ),
        html.Div(
            [
                html.Div(
                    style={
                        "width": "50px",
                        "height": "50px",
                        "backgroundColor": "blue",
                        "border": "1px solid black",
                    }
                ),
                html.Div("Batiments 3"),
            ],
            style={"display": "flex", "align-items": "center"},
        ),
        html.Div(
            [
                html.Div(
                    style={
                        "width": "50px",
                        "height": "50px",
                        "backgroundColor": "yellow",
                        "border": "1px solid black",
                    }
                ),
                html.Div("Batiments 4"),
            ],
            style={"display": "flex", "align-items": "center"},
        ),
    ],
    style={
        "position": "absolute",
        "top": "10px",
        "right": "10px",
        "background-color": "white",
        "padding": "10px",
        "border": "1px solid black",
    },
)

# on crée la page web avec Dash
# on ajoute la légende sur le côté droit
app.layout = html.Div(
        children=[
            html.Div(
                children=[map, legend],
                style={"width": "80%", "float": "left", "display": "inline-block"},
            ),
        ]
    )

if __name__ == "__main__":
    app.run_server(debug=True)
```

Ce code est un exemple de l'utilisation de la bibliothèque Dash pour créer une application web interactive en Python. Plus précisément, il s'agit d'une carte interactive utilisant le module Dash Leaflet pour afficher des données géographiques sous forme de polygones colorés.

Le code commence par importer les modules nécessaires (json, Dash, Dash Leaflet, html et dcc) et créer une instance de l'application Dash.

Ensuite, le code charge un fichier GeoJSON (un format de données géographiques) contenant des informations sur des bâtiments dans une zone donnée. Le GeoJSON est ensuite divisé en quatre parties égales, chacune représentant un quart des bâtiments. Chaque partie est mise en forme pour être utilisée par Dash Leaflet.

Ensuite, le code crée une carte Dash Leaflet avec les polygones de chaque quartier colorés en rouge, vert, bleu et jaune. La carte est centrée sur les coordonnées géographiques données et affichée en plein écran avec une légende sur le côté droit.

Enfin, l'application est lancée avec app.run_server(debug=True), ce qui permet de la visualiser dans un navigateur web.

Cet exemple vous montre comment utiliser Dash et dash-leaflet pour créer une application qui affiche une carte avec un polygone géojson. Vous pouvez personnaliser cet exemple en utilisant d'autres composants de dash-leaflet pour ajouter des marqueurs, des couches de données, etc.

## Exercices

Pour accéder aux exercices Kaggle sur la visualisation de données, vous pouvez vous rendre sur la rubrique "Data Visualization" du site Kaggle Learn, en suivant ce lien: https://www.kaggle.com/learn/data-visualization. Cette rubrique propose plusieurs cours interactifs et projets pratiques pour vous aider à maîtriser les techniques de visualisation de données avec des bibliothèques telles que Matplotlib et Seaborn en Python. Les exercices sont organisés de manière progressive, avec des exemples concrets et des instructions détaillées pour vous aider à développer vos compétences en visualisation de données. Vous pourrez également accéder à des ensembles de données pertinents pour chaque projet, et vous pourrez partager votre travail avec la communauté Kaggle pour obtenir des commentaires et des conseils.

