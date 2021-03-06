### PostGis + Postgresql
1. Install Postgis

		apt-get install postgis*

1. Login

		psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432

2. To get the 5 closest

		SELECT * FROM your_table 
		ORDER BY ST_Distance(your_table.geom, ST_Geomfromtext(your point as wkt)) 
		limit 5;
3. If you have a big dataset and know that you don't want to search further than , say 1 km, the query will be more efficient if you do:
	
		SELECT * FROM your_table 
		WHERE ST_DWithin(your_table.geom, ST_Geomfromtext(your point as wkt, 1000)
		ORDER BY ST_Distance(your_table.geom, ST_Geomfromtext(your point as wkt))  
		limit 5;

### 坐标转换
The simplest way to transform coordinates in Python is pyproj, i.e. the Python interface to PROJ.4 library. In fact:

	from pyproj import Proj, transform
	
	inProj = Proj(init='epsg:3857')
	outProj = Proj(init='epsg:4326')
	x1,y1 = -11705274.6374,4826473.6922
	x2,y2 = transform(inProj,outProj,x1,y1)
	print x2,y2
	returns -105.150271116 39.7278572773

### Links
1. Install postgresql first, also postgresql-dev-x.y
2. [PostGis](http://postgis.net/install/)
2. [各种方案比较](http://openlife.cc/blogs/2012/august/comparing-open-source-gis-implementations)
4. [Postgresql新手入门](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html)
5. **[GeoAlchemy](https://geoalchemy-2.readthedocs.org/en/0.2.6/orm_tutorial.html)**
6. [WKT、SRID、EPSG概念](http://www.cnblogs.com/xiashengwang/p/3897536.html)
7. [EPSG website](http://spatialreference.org/ref/epsg/nad83-utm-zone-10n/)