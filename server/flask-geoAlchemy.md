### Flask SqlAlchemy & GeoAlchemy

1. Using SQLAlchemy 0.8, Flask-SQLAlchemy and Geoalchemy 2:

		from app import db
		from geoalchemy2.types import Geometry
		
		class Point(db.Model):
		
		    """represents an x/y coordinate location."""
		
		    __tablename__ = 'point'
		
		    id = db.Column(db.Integer, primary_key=True)
		    geom = db.Column(Geometry(geometry_type='POINT', srid=4326))

2. Sample query:

		from geoalchemy2.elements import WKTElement
		from app import models
		
		def get_nearest(lat, lon):
		    # find the nearest point to the input coordinates
		    # convert the input coordinates to a WKT point and query for nearest point
		    pt = WKTElement('POINT({0} {1})'.format(lon, lat), srid=4326)
		    return models.Point.query.order_by(models.Point.geom.distance_box(pt)).first()
3. One way of converting the result to x and y coordinates (convert to GeoJSON and extract coordinates):

		import geoalchemy2.functions as func
		import json
		from app import db
		
		def point_geom_to_xy(pt):
		    # extract x and y coordinates from a point geometry
		    geom_json = json.loads(db.session.scalar(func.ST_AsGeoJSON(pt.geom)))
		    return geom_json['coordinates']

4. Better yet, use ST_MakePoint to directly make a geometry object. This is not only faster than ST_GeomFromText, but it is lossless, since you don't need to convert numbers to text to numbers.

		...
		WITH result AS (
		  INSERT INTO dest_pos (coord)
		  SELECT ST_SetSRID(ST_MakePoint(longitude, latitude, altitude), 4326)
		  FROM src_pos
		  RETURNING 1
		)
		SELECT count(*) INTO updated FROM result;
		RETURN updated;
		...

### 连接
- [Flask GeoAlchemy Sample Code](http://stackoverflow.com/questions/4069595/flask-with-geoalchemy-sample-code)
- [long&lat to geo](http://stackoverflow.com/questions/8433513/insert-postgis-object-e-g-st-geomfromtext-from-row-variables-in-plpgsql-scrip)