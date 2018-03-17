# aquinaomoraninguem

<h2>1. (optional) Pre-processing in QGis</h2>
<p>Start by downloading data from the <a href="http://mapasinterativos.ibge.gov.br/grade/default.html"> Brazilian 2010 Census Statistic Grid </a> (56 shapefiles), and by a simple operation (whether through QGis interface or using python) for selecting only squares in which population is not zero. I've included a python script example used for the batch selection in Qgis in the /py folder. For the rendering to be more efficient in TileMill, we erased every square from a Brazil administrative boundary polygon obtained through a query request in OpenStreetMap (administrative boundary). </p>

<h2>2. TileMill</h2>
<p>Following, we open our resultant shapefile in TileMill. We add a shapefile containing water polygons (obtained through openstreetmap) with the rule comp-op: src-out. If you haven't pre-selected tiles in step 1, just use a rule for CartoCSS symbology along with the water-polygon and admin-boundary shapefile with the <a href="http://tilemill-project.github.io/tilemill/docs/manual/carto/">CSS Carto</a> rule "comp-op: dst-out". It should look something like this:
  
  #water-polygon{polygon-comp-op: dst-out;}
  #admin-bound{polygon-comp-op: dst-in;}

.grade{
  [POP=0] {
  polygon-comp-op: src-over;
  polygon-comp-op: src-in;
  line-width:0;
 polygon-fill:#FEB63E;
 }
  }
  
  Export result in MBTiles format. It should take about 14 hours.</p>

<h2>3. MButil</h2>
<p>The MBTiles file generated has all our imagery packed into it, but now we need to convert them to PNG. To do so, we can use a handy library called mbutil which is capable of converting MBTiles to PNG.

Download and install mbutil: 
git clone git://github.com/mapbox/mbutil.git
sudo python setup.py install
mb-util -h to verify it worked.
Convert the tiles using the following command: 
mb-util --image_format=png aquinaomoraninguem.mbtiles aquinaomoraninguem
It will generate a folder, in my case aquinaomoraninguem, which will contain all of your map tiles in PNG format. 

This should generate a ~400MB sized folder.</p>

<h2>4. Hosting in php server</h2>
<p>If you navigate around your folder of unpacked tiles, you'll notice that the images are extracted into a highly organized structure of level\column\row. This structure is understood by various mapping programs and APIs, so all you have to do at this point is put your tiles onto a web-facing server.</p>

<h2>5. Reference in leaflet</h2>
<p>Now we're going to utilize LeafletJS, an awesome open-source library that will load and render our map tiles on our web page as the user pans and zooms.

var map = L.map('map').setView([51.505, -0.09], 5);  
L.tileLayer('yourwebfacingserver/{z}/{x}/{y}.png', {  
  maxZoom: 8
}).addTo(map);</p>

  
