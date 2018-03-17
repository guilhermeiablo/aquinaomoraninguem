# aquinaomoraninguem

<h2>1. Pre-processing in QGis</h2>
<p>After downloading data from the <a href="ftp://geoftp.ibge.gov.br/recortes_para_fins_estatisticos/grade_estatistica/censo_2010/"></a>Brazilian 2010 Census Statistic Grid (56 shapefiles), simple operation using python for selecting only squares in which population is not zero. For the rendering to be more efficient in TileMill, we erased every square from a Brazil administrative boundary polygon. </p>

<h2>2. TileMill</h2>
<p>Following, we open our resultant shapefile in TileMill with the CSS Carto rule comp-op: dst-out. We add a shapefile containing water polygons (obtained through openstreetmap) with the rule comp-op: src-out. Export result in MBTiles format.</p>

<h2>3. MButil</h2>
<p>The MBTiles file generated has all our imagery packed into it, but now we need to convert them to PNG. To do so, we can use a handy library called mbutil which is capable of converting MBTiles to PNG.

Download and install mbutil: 
git clone git://github.com/mapbox/mbutil.git
sudo python setup.py install
mb-util -h to verify it worked.
Convert the tiles using the following command: 
mb-util --image_format=png aquinaomoraninguem.mbtiles aquinaomoraninguem
It will generate a folder, in my case aquinaomoraninguem, which will contain all of your map tiles in PNG format. 

This process took about 14 hours and generated a ~400MB sized folder.</p>

<h2>4. Hosting in php server</h2>
<p>If you navigate around your folder of unpacked tiles, you'll notice that the images are extracted into a highly organized structure of level\column\row. This structure is understood by various mapping programs and APIs, so all you have to do at this point is put your tiles onto a web-facing server.</p>

<h2>5. Reference in leaflet</h2>
<p>Now we're going to utilize LeafletJS, an awesome open-source library that will load and render our map tiles on our web page as the user pans and zooms.

var map = L.map('map').setView([51.505, -0.09], 5);  
L.tileLayer('yourwebfacingserver/{z}/{x}/{y}.png', {  
  maxZoom: 8
}).addTo(map);</p>

  
