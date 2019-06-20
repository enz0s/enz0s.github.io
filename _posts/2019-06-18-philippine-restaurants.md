Have you ever wondered where most restaurants are located in the Philippines? Using [Open Street Map (OSM)](https://www.openstreetmap.org/#map=5/13.019/121.767) data, I was able to figure out the number of restaurants in different provinces of the Philippines. 

The figure below shows the map of the Philippines with different shades of red to indicate number of restaurants per province. The darker the shade, the more restaurants in that province.


![png](/assets/images/20190618/restos_ph.png)


Out of 9,485 restaurants all over the Philippines, not surprisingly, Metropolitan Manila (Metro Manila) holds the most number of restaurants with 3,047. This is followed by Cebu with 641 and Davao del Sur with 479. This makes sense because Metro Manila is the most densed location in the Philippines. More people, more food places to go.


I used the SQL statement below to extract data from the OSM Philippines database.

```python
pd.read_sql('''
SELECT DISTINCT g.name_1, COUNT(*) as cnt
FROM gadm AS g
JOIN points AS p
ON st_within(p.wkb_geometry, g.geom)
WHERE p.amenity='restaurant'
GROUP BY g.name_1
ORDER BY cnt DESC
''', conn_psql)
```