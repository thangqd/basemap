## 0. OpenMapTiles Overview

### OpenMaptiles Schema
https://openmaptiles.org/schema/

### OpenMaptiles Styles
https://openmaptiles.org/styles/


## 1. Clone openmatiles and run make
### Host IP: 10.225.1.252 (/data2/thangqd)
```bash
git clone https://github.com/openmaptiles/openmaptiles.git
cd openmaptiles
# Build the imposm mapping, the tm2source project and collect all SQL scripts
make
```

### Tools used (https://github.com/openmaptiles/openmaptiles-tools/tree/master/bin)
```bash
make list-geofabrik # list actual geofabrik OSM extracts for download
make list-bbbike # list actual BBBike OSM extracts for download (city level)
make download area=india # download OSM data from any source 
make download-geofabrik area=india # download OSM data from geofabrik.de
make download-osmfr area=asia/india  # download OSM data from openstreetmap.fr 
make download-bbbike area=Bombay (or NewDelhi)  # download OSM data from bbbike.org 
make import-data # Import data from OpenStreetMapData, Natural Earth and OSM Lake Labels.
make import-osm # Import OSM data with the mapping rules from build/mapping.yaml (https://imposm.org/docs/imposm3/latest/index.html, Omniscale)
make import-diff # Import OSM updates from data/changes.osc.gz
make import-wikidata # Import labels from Wikidata
make import-sql # Import layers configured in ./build/sql folder

make generate-bbox-file # compute bounding box for tile generation (see ./data/india.bbox)
make generate-tiles-pg # generating MBTiles from PostGIS
```
### Important configurations:
```bash
./Makefile
./.env
./openmaptiles.yaml
./data # folder
./layers # folder
./build # folder
./quickstart.log
```
### Git:  https://git.sovereignsolutions.tech/sovereignsolution/ss-gis/openmaptiles.git 

##  2. Quick Start
### Configure .env for PGPORT, MIN_ZOOM, MAX_ZOOM (14), DIFF_MODE=false (true for update existing osm DB, no testing yet) 
### Run quickstart for India
```bash
./quickstart.sh asia/india # or ./quickstart.sh india
```
### PostgreSQL connection parameters
```bash
PGDATABASE=openmaptiles
PGUSER=openmaptiles
PGPASSWORD=openmaptiles
PGHOST=postgres
PGPORT=5433 (defaut: 5432)
```
### OSM and MBTiles data folder:
```bash
./openmaptiles/data
```
## 3. Test and serve
### Install AWS CLI (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
### Install vtiles (https://pypi.org/project/vtiles/)
```bash
pip install vtiles
```
### show tiles.mbtiles info
```bash
cd data
mbtiles info tiles.mbtiles
```
### Start tileserver to host mbtiles
```bash
make start-tileserver # (make stop-tileserver to stop)
```
Tileserver URL: http://10.225.1.252:8080
Tile URL: http://10.225.1.252:8080/data/openmaptiles/{z}/{x}/{y}.pbf 


### or start postserve to serve tiles from PostGIS
```bash
make start-postserve # (make stop-postserve to stop)
```
Postserver Tile URL: http://10.225.1.252:8090/tiles/{z}/{x}/{y}.pbf

### Start maputnik to edit styles
```bash
make start-maputnik  # (make stop-maputnik to stop)
```
Access:  http://10.225.1.252:8088/


## 4. Upload to S3
### Convert tiles.mbtiles into XYZ folder
```bash
 mbtiles2folder tiles.mbtiles (the default openmaptiles tiling scheme is TMS, set -flipy 1 to convert to XYZ tiling scheme if needed)
```
### Upload tiles folder to S3 
```bash
 folder2s3 tiles -format pbf
 S3 Bucket name: ss-vector-tile-data
 S3 prefix: sovereign/v20251111/india
 AWS Access Key ID: ******
 AWS Secret Access Key: ******** 
```
S3 URL: https://ap-south-1.console.aws.amazon.com/s3/buckets/ss-vector-tile-data?region=ap-south-1&prefix=sovereign%2Fv20251111%2Findia%2F&showversions=false&tab=objects

XYZ Tile URL: https://map-api-new.sovereignsolutions.net/sovereign/v20251111/india/{z}/{x}/{y}.pbf


## Carto Utils:
### self-host fonts
### sprite generator and self-host sprite 