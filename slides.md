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
## JavaScript shapefile -> geojson


#topojson.py
##not actually relevent, just braggin



#Similar internal structures which FGDB shares


#Shape types (arcpy, json, and shp)

- Point
- Multi Point
- Line
- Polygone


#Coordinate Types (json and shp)

- XY (normal)
- Z (special)
- M (extra special)


#Inner Rings (json and shp)

- outer rings are clockwide
- inner rings are counter-clockwise (anti-clockwise to nick)



#Convert it all to GeoJSON

And let God sort them out


#Shape Types

- Point (Same)
- Multi Point (Same)
- LineString
- Multi LineString (same as esri Line)
- Polygon
- Multi Polygon (same as esri Polygon)
- Geometry Collection (makes esri's head asplode)


#Geometry

- x, y (in that order) manditory
- on your own after that


#Inner Rings

- polygon obj is array of LineStrings
- first one is other
- rest are inner
- this is MUCH easier



#Geoformat type
- esrijson and geojson are transfer formats
- shapefile, fileGDB, sqlite/wkb are storage types


#e.g.

Can turn a shapefile into geojson with just the .shp .dbf & .prj


.shp & .dbf if you don't care about the projection


.shp if you don't care about the attributes. 



#main attraction
##in which we actually talk about file gdbs


#it's a folder full of weird file names


- a00000001.gdbtable
- a0000000b.gdbtablx
- a0000009.spx


#format

- letter a
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
5. relationships, relationship types, or item types (order varries)
6. relationships, relationship types, or item types (order varries)
7. relationships, relationship types, or item types (order varries)
8. Replica Log, seems to be optional, but always mentioned in teh catalog. 
9. feature classes start here, 
10. I'm actually a.



