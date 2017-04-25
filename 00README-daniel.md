# INSTALLATION
I installed this in a separate cloud9 container as there appeared to be conflicts between the gdal installation and the existing postgresql/postgis installation.

* Pick a "Blank" Ubuntu container on cloud9
* sudo add-apt-repository ppa:ubuntugis/ppa
* sudo apt-get update
* sudo apt-get install gdal-bin

* git clone https://github.com/interactivethings/swiss-maps.git
* cd swiss-maps
* possibly also run:
  * npm install topojson
  * npm install topojson-client
  * npm install dbf2dsv


# Generate the files needed
First create topojson files and then geojson files for import.
## Cantons
make topo/ch-cantons.json REPROJECT=true PROPERTIES=name,abbr
node_modules/.bin/topo2geo cantons=topo/ch-cantons-geo.json < topo/ch-cantons.json





# Examples
Orte mit Namen:
  make topo/ch-municipalities.json REPROJECT=true PROPERTIES=name
Zipcode:
  make topo/ch-plz.json REPROJECT=true
Districts (name doesn't work, I guess there is no district name)
  make topo/ch-districts.json REPROJECT=true PROPERTIES=name



# Further Reading
https://medium.com/@mbostock/command-line-cartography-part-1-897aa8f8ca2c
https://gis.stackexchange.com/questions/60928/how-to-insert-a-geojson-polygon-into-a-postgis-table
