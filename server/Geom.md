### Geom

1. Search by distance

		select *, ST_Distance(geom, ST_GeomFromText('POINT(32 122)', 26910)) as Distance from voice order by Distance;
2. Geom Orm

		lake = Lake()
		lake.geom = 'SRID=4326;POINT(120 45)'

3. Curl insert voice

		curl -i -X POST -u steven:hello -F "voice_data=@/home/steven/Downloads/girl.jpg"  -F "image_name=dog.jpg"  -F "description=this is a description" -F "longitude=33.044" -F "latitude=24.304" http://127.0.0.1:5000/voices/add
5. Search and by distance

		db.session.query(func.ST_Distance(Voice.geom, pt)).order_by(func.ST_Distance(Voice.geom, pt)).all()
































### 相关链接
[WKTElement always gives a Geometry](https://github.com/geoalchemy/geoalchemy2/issues/61#issuecomment-27144712)
[gps](http://www.gpsspg.com/maps.htm)