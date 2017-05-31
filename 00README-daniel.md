####################################################################
# What we actually need to do to create files for rentmap
make clean
make topo/ch.json PROPERTIES=name,abbr,id

This is easy and created all we need (country, lakes, cantons, municipalities) 
in one file so nothing is shifted against each other.
Also we get 
for cantons PROPERTIES=name,abbr,id
for municipalities PROPERTIES=name,id
this gets simply projected into an 960x500 box which is ok,
alles OHNE REPROJECT=true

==> resulting files are in topo/
####################################################################


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
make topo/ch-cantons.json REPROJECT=true PROPERTIES=name,abbr,id
node_modules/.bin/topo2geo cantons=topo/ch-cantons-geo.json < topo/ch-cantons.json
## Postleitzahlen
make topo/ch-plz.json REPROJECT=true
node_modules/.bin/topo2geo plz=topo/ch-plz-geo.json < topo/ch-plz.json
## Municipalities
make topo/ch-municipalities.json REPROJECT=true PROPERTIES=name,id
node_modules/.bin/topo2geo municipalities=topo/ch-municipalities-geo.json < topo/ch-municipalities.json

## make all 
make all REPROJECT=true PROPERTIES=name,abbr,id


# Simplify parameter
make topo/ch-cantons.json REPROJECT=true PROPERTIES=name,abbr SIMPLIFY=0.00000005




# Examples
Orte mit Namen:
  make topo/ch-municipalities.json REPROJECT=true PROPERTIES=name,id
Zipcode:
  make topo/ch-plz.json REPROJECT=true
Districts (name doesn't work, I guess there is no district name), see here in makefile:
      ##########
      # Currently without names because joining to build/districts.tsv prevents the 'canton' property to be set on districts where it's needed (i.e. districts with BEZIRKSNR = 0)
      build/ch-districts-unmerged.json: build/ch/municipalities-without-lakes.shp
      ##########
  make topo/ch-districts.json REPROJECT=true PROPERTIES=name



# Further Reading
https://medium.com/@mbostock/command-line-cartography-part-1-897aa8f8ca2c
https://gis.stackexchange.com/questions/60928/how-to-insert-a-geojson-polygon-into-a-postgis-table



