6 - Geography
=============

Creation
--------

Create a geography from lat / lon text datas 

```SQL
SELECT ST_SetSRID(ST_MakePoint(longitude_wgs84, latitude_wgs84), 4326)::geography AS geog
FROM rff.station;
```

Same one viewable in QGIS DBManager:


```SQL
SELECT (row_number() OVER ()) AS gid,
       (ST_SetSRID(ST_MakePoint(longitude_wgs84, latitude_wgs84), 4326)::geography)::geometry AS geom
FROM rff.station;
```

About the query :
- geometry vs geography
- geography only for EPSG:4326
- cast 
- DB Manager not (yet) handle geography rightly

Distance
--------

Compute (brute) distance beetween geography

```
WITH station AS (
  SELECT  row_number() OVER() AS id,
	  nom, dept,
          ST_SetSRID(ST_MakePoint(longitude_wgs84, latitude_wgs84), 4326)::geography AS geog
  FROM rff.station
  WHERE dept = 77
)

SELECT s1.nom, 
       s2.nom, 
       ST_Distance(s1.geog, s2.geog)

FROM   station AS s1, 
       station AS s2 

WHERE  s1.id > s2.id;  -- to avoid to compute dist(a,b) and dist(b,a)
```


About the query :
- Distance and measure functions can deal with geography
- Look at distance result if you do not cast to geography


