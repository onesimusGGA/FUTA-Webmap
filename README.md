<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
    <link rel="stylesheet" href="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.css" />
    <link rel="stylesheet" href="leaflet-search-master\leaflet-search-master\dist\leaflet-search.src.css" />
    <link rel="stylesheet" href="leaflet-routing-machine-3.2.12\leaflet-routing-machine-3.2.12\dist\leaflet-routing-machine.css" />
    <link rel="stylesheet" href="leaflet-locatecontrol-gh-pages\leaflet-locatecontrol-gh-pages\dist\L.Control.Locate.min.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    <script src="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.js"></script>
    <script src="leaflet-search-master\leaflet-search-master\dist\leaflet-search.src.js"></script>
    <script src="leaflet-routing-machine-3.2.12\leaflet-routing-machine-3.2.12\dist\leaflet-routing-machine.js"></script>
    <script src="leaflet-locatecontrol-gh-pages\leaflet-locatecontrol-gh-pages\dist\L.Control.Locate.min.js"></script>
    <script src="Building.js"></script>
    <script src="Boundary.js"></script>
    <script src="Location.js"></script>
    <script src="Recreationals.js"></script>
    <script src="Road.js"></script>
    <script src="Water_Body.js"></script>

    <style>
        #map {position: absolute; top: 0; bottom: 0; left: 0; right: 0;}
    </style>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id = "map"></div>
    <script>
        var map = L.map('map').setView([7.308056444586103, 5.1364610874240055], 15);
        var osm = L.tileLayer('https://api.maptiler.com/maps/landscape/{z}/{x}/{y}.png?key=vcTAyt42yneFpnPgJeap', {
            attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>',
            minZoom: 0,
            maxZoom: 20,
        }).addTo(map);

        var EsriWorldImagery = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
            attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community',
            minZoom: 0,
            maxZoom: 20,
        });

        var OpenStreetMapMapnik = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
            minZoom: 0,
            maxZoom: 20,
        });

        // Leaflet layer control
        var baseMaps = {
            'Landscape': osm,
            'Esri Imagery': EsriWorldImagery,
            'Mapnik': OpenStreetMapMapnik,
        }

        L.geoJSON(Building).addTo(map)
        L.geoJSON(Boundary).addTo(map)

        const searchLayer = L.geoJSON(Location, {
            onEachFeature: function(feature, layer){
                layer.bindPopup(feature.properties.Name_of_th);
            }
        }).addTo(map)

        L.geoJSON(Recreationals).addTo(map)
        L.geoJSON(Road).addTo(map)
        L.geoJSON(Water_Body).addTo(map)

        var overlayMaps = {
            'Location': Location
        }

        L.control.layers(baseMaps).addTo(map);
        L.Control.geocoder().addTo(map);

        const searchControl = new L.Control.Search({
            layer: searchLayer,
            zoom: '20',
            propertyName: 'Name_of_th'
        })

        map.addControl(searchControl);

        L.control.locate().addTo(map);

    </script>
</body>
</html>
