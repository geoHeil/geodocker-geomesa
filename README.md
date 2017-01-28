# building the current images

**step 0: clone the repos**

`git clone https://github.com/geoHeil/geodocker-geomesa.git`

**step 1: choose correct settings**

- choose settings for accumulo / geomesa / ... version which fits your environment
- if there are major changes you might need to recompile geomsa as well (and you need it present in local m2 folder to create the containers)
	```
	git clone https://github.com/locationtech/geomesa.git
	cd geomesa
	git checkout geomesa_2.11-1.3.0
	update ./build.sh to desired version numbers then
	build/mvn clean install -DskipTests=true #test failures otherwise (timeouts on macbook) 
	```
- create the containers by executing `./build.sh`

**step 2: build the image for the tutorial**
```
git clone https://github.com/geoHeil/geomesa-tutorials.git
cd geomesa-tutorials/docker-tutorial/
docker build -t geomesa-tutorial:1.3.0 .
```

**step 3: start all the images**

```
docker-compose up
```

**step 4: run the quickstart**

- connect into the java container which is already running
```
docker ps # shows all the containers
e5b2bb33c0b0        geomesa-tutorial:1.3.0 # I see this one
docker exec -ti <id> bash # id is here e5b2bb33c0b0
```

Now run the quickstart. This requires connection credentials as defined in the `docker-compose.yml` file:

```
java -cp geomesa-quickstart-accumulo/target/geomesa-quickstart-accumulo-1.3.0.jar \
  com.example.geomesa.accumulo.AccumuloQuickStart \
  -instanceId accumulo \
  -zookeepers zookeeper \
  -user root \
  -password GisPwd \
  -tableName quickstart
```
- you can monitor accumulo here http://localhost:9995/accumulo

you should be able to see the following out put

```
Submitting query
1.  Bierce|394|Fri Aug 01 23:55:05 UTC 2014|POINT (-77.42555615743139 -37.26710898726304)|null
2.  Bierce|343|Wed Aug 06 08:59:22 UTC 2014|POINT (-76.66826220670282 -37.44503877750368)|null
3.  Bierce|259|Thu Aug 28 19:59:30 UTC 2014|POINT (-76.90122194030118 -37.148525741002466)|null
4.  Bierce|640|Sun Sep 14 19:48:25 UTC 2014|POINT (-77.36222958792739 -37.13013846773835)|null
5.  Bierce|589|Sat Jul 05 06:02:15 UTC 2014|POINT (-76.88146600670152 -37.40156607152168)|null
6.  Bierce|886|Tue Jul 22 18:12:36 UTC 2014|POINT (-76.59795732474399 -37.18420917493149)|null
7.  Bierce|322|Tue Jul 15 21:09:42 UTC 2014|POINT (-77.01760098223343 -37.30933767159561)|null
8.  Bierce|925|Mon Aug 18 03:28:33 UTC 2014|POINT (-76.5621106573523 -37.34321201566148)|null
9.  Bierce|931|Fri Jul 04 22:25:38 UTC 2014|POINT (-76.51304097832912 -37.49406125975311)|null
Submitting secondary index query
Feature ID Observation.859 | Who: Bierce
Feature ID Observation.355 | Who: Bierce
Feature ID Observation.940 | Who: Bierce
Feature ID Observation.631 | Who: Bierce
Feature ID Observation.817 | Who: Bierce
Submitting secondary index query with sorting (sorted by 'What' descending)
Feature ID Observation.999 | Who: Addams | What: 999
Feature ID Observation.996 | Who: Addams | What: 996
Feature ID Observation.993 | Who: Addams | What: 993
Feature ID Observation.990 | Who: Addams | What: 990
Feature ID Observation.987 | Who: Addams | What: 987
```
