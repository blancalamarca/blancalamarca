
# Metronyam

The following project consist of a telegram bot that reads the user's preferences regarding restaurant's food, location and others and gives 12 restaurants that match the search, whilst also offering the shortest path to get to the restaurant of the user's choice.

## MODULES

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

```def connect_accesses(metro_graph: MetroGraph, accesses_list: Accesses, stations_list: Stations)-> None ```:This function connects accesses with their corresponding stations. Once we have connected the access with one node from the station, we stop checking the rest of the station nodes.

```def get_time(distance: float, type: str) -> float ```:Given a distance and a string that represents our situation when moving around the city, this function returns the time we need to travel this distance.

```def show(g: MetroGraph) -> None```: Given a graph of the type Metrograph (networkx.graph), this void function displays the nodes and edges that take part of this graph.
[x-special/nautilus-clipboard
copy
file:///home/martina/Documents/AP2/Pr%C3%A0ctica2/IMG-1199.jpg](https://github.com/martinaalba21/fotos/issues/1)

```def plot(g: MetroGraph, filename: str) -> None ```: Given a graph of the type Metrograph (networkx.graph) and a filename (string), this void function representsthe nodes and edges that take part of this graph imprinted in a representation of Barcelona.
### City.py
This following module reads information from the metro module and builds a graph constituted by the metrograph and the graph of the city's streets, which is already built.
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

```def show(g: CityGraph) -> None```: Void function that  displays the citygraph that it reads in a new window.

```def get_nodes_positions(city_graph: CityGraph) -> dict```: This function returns a dictionary with node codes as keys and their positions as values.

```def plot(g: CityGraph, filename: str) -> None```: This void function saves the citygraph g that it receives as a parameter against Barcelona's map in the file filename(string).

```def get_colour(type: str) -> str```: This non-void function receives a string parameter and returns the colour of a given element of the correspondin graph.

```def plot_path(g: CityGraph, p: Path, filename: str) -> None```: This void function saves the corresponding path that takes place in the citygraph g according to Path, which is a list of node's id in the file filename(string).
   


### Prerequisites

What things you need to install the software and how to install them

```
Give examples
```

### Installing

A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

Martina Albà and Blanca Lamarca

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
