
# MetroNyam

## Introduction
The following project consist of a telegram bot that reads the user's preferences regarding restaurant's food, location and others and gives 12 restaurants that match the search, whilst also offering the shortest path to get to the restaurant of the user's choice.

## Modules

The project is divided into different interdependent modules: restaurants.py, metro.py, city.py and bot.py. 
### Restaurants.py


This first module (restaurants.py) consists of a set of functions that work with the data frame containing information of the restaurants of Barcelona and their characteristics.
#### Prerequisited libraries: 
```
import pandas as pd
from staticmap import StaticMap, CircleMarker
from typing import *
from dataclasses import dataclass
from typing_extensions import TypeAlias
```

The module restaurants.py contains the following functions:

``def read()-> Restaurants`` : Non-void function that reads the information of the restaurants in the data frame restaurants.csv and returns a list of the restaurants that are contained in said data frame and their attributes.

``def find()-> Restaurants``: Non-void function that receives a query and a list of restaurants and returns a list of restaurants that contain the given query.

``def already_in_list()-> bool``: Boolean function that receives a restaurant and a list of restaurants and determines whether or not the restaurant is already contained in the list or not. We implemented this function to ensure that no restaurant would appear more than once in the find function, since we did enconunter this inconvenience.


### Metro.py
This next module is the metro.py, the objective of which is to store information of the metro system in Barcelona and to reassess the information collected from the data frames restaestacions.csv, accessos.csv into a graph that differenciates between stations, accesses and the edges that connect them in order to later plot and show the remaining graph.
#### Prerequisited libraries: 
```
import pandas as pd
from staticmap import StaticMap, CircleMarker, Line
from typing import *
from dataclasses import dataclass
from typing_extensions import TypeAlias
import networkx as networkx
import matplotlib.pyplot as plt
from haversine import haversine, Unit
```
The module metro.py contains the following functions:

```def read_stations() -> Stations```: This non-void funcion reads the information of the different stations (name, line attributes, id, location and order) that constitute the metro system of Barcelona and returns a list with the stations and their respective attributes.

```def read_accesses() -> Accesses```: This non-void funcion reads the information of the different accesses (name, station they correspond, id, location and accessibility information) that constitute the metro system of Barcelona and returns a list with the accesses and their respective attributes.

```def get_coordinates(location:str) -> Coord```:Given a node location, this function returns a tuple with the coordinates of it's position with the precondition that the location must be a string.

```def get_nodes_dictionary(stations_list: Stations, accesses_list: Accesses) -> dict ```: This function returns a dictionary with node codes as keys and their positions as values.

```def get_metro_graph() -> Metrograph```:This function reads data from stations and accesses in Barcelona and returns an undirected graph based on the information obtained.

```def connect_stations(metro_graph: MetroGraph, stations_list: Stations) -> None ```: This function connects the stations of a subway line following it's order with the precondition that the stations list must be sorted by subway lines.

```def connect_transfers(metro_graph: MetroGraph, stations_list: Stations) -> None ```:This function connects the different lines of the same station.

```def connect_accesses(metro_graph: MetroGraph, accesses_list: Accesses, stations_list: Stations)-> None ```:This function connects accesses with their corresponding stations.

```def get_time(distance: float, type: str) -> float ```:Given a distance and a string that represents our situation when moving around the city, this function returns the time we need to travel this distance.

```def show(g: MetroGraph) -> None```: This void function gets a graph as a parameter and displays it in
    a new window.


![IMG-1200](https://user-images.githubusercontent.com/106911781/172048975-4ab2e98c-98c1-4de9-99a9-d0bcf97a2921.jpg)


```def plot(g: MetroGraph, filename: str) -> None ```: This function saves a given graph as a picture with the Barcelona's
    map behind. The file name where it is saved must be an input parameter.
    
    
![map](https://user-images.githubusercontent.com/106911781/172049006-80e164a7-b560-4384-ae92-8b88ee3cf6a5.png)


### City.py
This following module reads information from the metro module and builds a graph constituted by the metrograph and the graph of the city's streets, which are already built.
#### Prerequisited libraries: 
```
import pandas as pd
from staticmap import StaticMap, CircleMarker
from typing import *
from dataclasses import dataclass
from typing_extensions import TypeAlias
import networkx as networkx
from haversine import haversine, Unit
import matplotlib.pyplot as plt
import osmnx as osmnx
import pickle
import os
from metro import *
```
```get_osmnx_graph()```: This function returns a graph of the streets in Barcelona with the information contained in the data frame barcelona.grf.

```def save_osmnx_graph(g: OsmnxGraph, filename: str) -> None```: Given a graph and the name of a file, this function saves the graph's data in the file.

```def load_osmnx_graph(filename: str) -> OsmnxGraph```: Given the name of a file, this function returns the graph saved in this file.

```def build_city_graph(g1: OsmnxGraph, g2: MetroGraph) -> CityGraph```: This function creates and returns the Barcelona's city graph, which results from the union of Barcelona's street graph and metro graph.

```def add_nodes_and_edges(city_graph: CityGraph, g1: OsmnxGraph) -> None```: This function adds the nodes and edges from the Barcelona's street graph to the city graph.

```def get_access_nodes(metro_graph: MetroGraph) -> Accesses```: This function returns a list with all the access nodes in metro graph.

```def connect_access_and_street_nodes(city_graph: CityGraph, g1: OsmnxGraph, access_nodes: Accesses) -> None```: For each access node, this function searches the nearest street node and connects both of them.

```def find_path(ox_g: OsmnxGraph, g: CityGraph, src: Coord, dst: Coord) -> None```: Given a source and a destination point, these function returns the shortest path in time between these two points.

```def get_nearest_node(g: OsmnxGraph, coordinates: Coord) -> int```: Given a location, these function returns the nearest street node to that location.

```def show(g: CityGraph) -> None```: This void function gets a graph as a parameter and displays it in
    a new window.

![IMG-1199](https://user-images.githubusercontent.com/106911781/172048610-de3a8e55-d866-4df8-955f-8c423c22b813.jpg)

```def get_nodes_positions(city_graph: CityGraph) -> dict```: This function returns a dictionary with node codes as keys and their positions as values.

```def plot(g: CityGraph, filename: str) -> None```: This function saves a given graph as a picture with the Barcelona's
    map behind. The file name where it is saved must be an input parameter.


![city_map](https://user-images.githubusercontent.com/106911781/172049031-9e2e6b0a-86ed-48e2-be6e-f02784d15b5c.png)


```def get_colour(type: str) -> str```: Given a string, this function searches the colour related to the word.

```def plot_path(g: CityGraph, p: Path, filename: str) -> None```: Given a path, this function draws the path with the Barcelona's map
    behind and saves the picture in a specific file.
    
### Bot.py
This following module reads information from the restaurants, the metro and the city module. This module waits for a user to connect and offers help to find a restaurant through some searches. The bot will show the users how to get to the chosen restaurant from their current location, giving the shortest path by metro and walking.

#### Prerequisited libraries: 
```
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters
import restaurants as rest
import random
import os
import city
import metro
from staticmap import StaticMap, CircleMarker
```
```def start(update, context)```: This function iniciates the chat with the bot.

```def help(update, context)```: With this function the bot offers you help. It will tell you the different commands that you can use.


![IMG-1203](https://user-images.githubusercontent.com/106911781/172054575-1de2994f-a76f-4ee8-8350-c7f1290b6cf8.jpg)


```def author(update, context)```: This function shows the name of the project's authors.

```def find(update, context)```: This function searches the restaurants that match the user's request and writes a list with the first 12 restaurants.


![IMG-1204](https://user-images.githubusercontent.com/106911781/172054649-2cdc2901-0d26-44c8-80da-8795478d0aea.jpg)


```def info(update, context)```: This function shows information from one restaurant, which the user specifies with it's number.

```def guide(update, context)```: This function shows a map with the shortest path to get to the chosen restaurant from the user's current location.


![IMG-1205](https://user-images.githubusercontent.com/106911781/172054745-cd5d50f6-65c5-4a17-8098-2d98d8501d5a.jpg)


## Authors

Martina Albà and Blanca Lamarca
