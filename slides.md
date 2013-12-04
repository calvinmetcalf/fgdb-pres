#Parsing the ESRI File Geo-Database

##(in JavaScript)



#Genesis


## Even Rouault does the hard part and starts specing it out


## In Python


##I must port it to JavaScript



#Previously I have done


#esri2geo
##esrijson -> geojson


#esri2open
## arcpy toolkit to export layers to:

- geojson
- topojson
- sqlite/wkb
- csv/wkt


#shp.js
## JavaScript

shapefile -> geojson


#topojson.py
##port of topojson to python
not actually relevant, just bragging



#Similar internal structures which FGDB shares


#Shape types
##(arcpy, json, and shp)

- Point
- Multi Point
- Line
- Polygon


#Coordinate Types (arcpy and shp)

- XY (normal)
- Z (special)
- M (extra special)


#Polygons as array of arrays (of points)
## (arcpy, json, and shp)


#Inner Rings
##(json and shp)

- outer rings are clockwise
- inner rings are counter-clockwise (anti-clockwise to nick)
- arcpy is horrible


#mini rant about rings in arcpy

- outer ring and inner rings separated by null points
- must literally check every single point to see if it exists
- why?



#Convert it all to GeoJSON

And let God sort them out


#Shape Types

- Point (Same)
- Multi Point (Same)
- LineString
- Multi LineString (same as esri Line)
- Polygon
- Multi Polygon (~ esri Polygon)
- Geometry Collection (makes esri's head asplode)


#Geometry

- x, y (in that order) manditory
- on your own after that


#Inner Rings

- polygon obj is array of LineStrings
- first one is inner
- rest are inner
- this is MUCH easier



#Geoformat type
- esrijson and geojson are transfer formats
- shapefile, fileGDB, sqlite/wkb are storage types


#e.g.

Can turn a shapefile into geojson with just the .shp .dbf & .prj


.shp & .dbf if you don't care about the projection


.shp if you don't care about the attributes. 


#ergo
- we can ignore a good number of the files 



#main attraction
##in which we actually talk about file gdbs


#it's a folder full of weird file names


- a00000001.gdbtable
- a0000000b.gdbtablx
- a00000009.spx


#format

- letter 'a'
- number in hex
- period (full stop to nick)
- file type


#the numbers!

- start at 1
- denote table
- much like a shapefile a00000002.* all go with the same table


#types we care about

- .gdbtable (it has the good stuff)
- .gdbtablx (it has row offsets)
- we need offsets because there can be space between rows


#whats in each table?

1. catalog of all layers
2. config parameters
3. spatial ref data
4. meta data, mostly xml
5. relationships, relationship types, or item types (order varies)
6. relationships, relationship types, or item types (order varies)
7. relationships, relationship types, or item types (order varies)
8. Replica Log, seems to be optional, but always mentioned in teh catalog. 
9. feature classes start here, 
10. I'm actually an 'a'.



#JavaScript


##Binary parsing
- node has 'Buffer' and browser has 'ArrayBuffer'
- browser has DataView to view ArrayBuffers
- ArrayBuffer and DataView are available in node
- DataView(ArrayBuffer) !== Buffer
- DataView(ArrayBuffer) == Buffer


##other reason to use browser version
- shp.js was explicitly browser library
- already was good 


##dates
- JavaScript is ms since 11/1970
- fGDB is days since 12/30/1899


##longs
- JavaScript doesn't have 64bit integers
- Python does
- took me far too long to figure this out


##misc
- just treat m coords as 4th dimension
- coordinates are actually delta encoded (like topojson)
- my target demographic doesn't care about multipatch and other such



#finishing up the library


#loading the files
- easy for node
- not for the browser


##node
- use fs.readdir
- get an array of filenames
- grab the ones we care about
- read those
- dance


##`<input type='file' directory />`
- allows you to select a directory
- perfect
- if your browser supports it (webkit only)


##`<input type='file' multiple />`
- works in all modern browsers
- requires people to ctrl-a in the .gdb directory to select everything
- from a JavaScript perspective looks the same as directory


#async
## this is all async which can be:
- scary
- complicated
- reminiscent of inception


#good thing I wrote a promise lib
- and a tool kit for it
- this project was the final nail in the coffin for me becoming a promise fan-boi
- I digress


#useful formats...
- nobody passes around directory references
- shapefile on the web is only really useful zipped up


#zipfile
- just zip up the filegdb
- can unzip it on the client side or in node
- nice library for that
- this is what I did with shp.js
- can by speed up (sorta) with a web worker
- (I wrote a library for that)


#projections, CRS and bears (oh my)
- geojson/the web & projections get along like a cat on fire
- proj4.js to the rescue
- your welcome


#other stuff
- used browserify to put the library together for the browser
- totally done with AMD for libraries
- I <3 NPM



- @cwmma
- the library: https://github.com/calvinmetcalf/fileGDB.js
- npm install fgdb
- leaflet plugin: https://github.com/calvinmetcalf/leaflet.filegdb
- promise library: https://github.com/calvinmetcalf/lie
- npm install lie
- promise toolkit: https://github.com/calvinmetcalf/liar
- npm install liar
- shp.js: https://github.com/calvinmetcalf/shapefile-js
- web worker library: https://github.com/calvinmetcalf/catiline
- zipfiles: https://github.com/Stukjszip
- npm install jszip
- proj4.js: https://github.com/proj4js/proj4js
- npm install proj4