# postgis-plv8

Docker image for use by the IOW team containing a PostGIS database based on mdillon/postgis:11 with the plv8 extension.

You can build and tag it locally with:

```
docker build -f Dockerfile -t usgswma/postgis-plv8:11-2.3.8 .
```

Note that the postgis-plv8:10-2.1.2 version is only here for historical purposes and will not build on Docker Hub
