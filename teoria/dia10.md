# Día 10


### Dudas

- [código 418](http://www.nytimes.com/418) un cásico [April Fools' Day](https://es.wikipedia.org/wiki/D%C3%ADa_de_las_bromas_de_abril)
![google_418](https://i1.wp.com/fat.gfycat.com/EquatorialAjarBlackfootedferret.gif)

- [Más bromas del Internet Engineering Task Force (IETF)](https://es.wikipedia.org/wiki/RFC_del_1_de_abril)



### AJAX

4 - Desarrolla una versión mejorada de [MovieFire](https://github.com/arvindr21/movieFire) (JS puro) incluyendo llamadas AJAX a la base de datos de IMBD para enriquecer los datos, usando [OMDb API](http://omdbapi.com/). 

- [Solución](https://github.com/UlisesGascon/Curso-in-comany-BQ/tree/master/otros/movieFire/jsFire)


### HTML5 API

### Geolocalización

- [Espeficicación](http://dev.w3.org/geo/api/spec-source.html)
- [Compatibildiad](http://caniuse.com/#feat=geolocation)

- Precisión:
    - Wi-fi (MAC)
    - Ethernet (IP)
    - Triangulación (3G y 4G)    
    - GPS (máxima precisión, pero tardará más en cargar)

- Métodos de *geolocation*
    - getCurrentPosition():
    ```javascript
        // Posición Actual
        navigator.geolocation.getCurrentPosition();
    ```
    - watchPosition():
    ```javascript
        // Seguimiento como un GPS 
        navigator.geolocation.watchPosition();
    ```
    - clearWatch():
    ```javascript
        // Para el seguimiento
        navigator.geolocation.clearWatch();
    ```
    
- Propiedades de *geolocation*
    - Latitud (en base 10):
    ```javascript
        console.info(position.coords.latitude);
    ```
    - Longitud (en base 10):
    ```javascript
        console.info(position.coords.longitude);
    ```    
    - Precisión en posicionamiento:
    ```javascript
        console.info(position.coords.accuracy);
    ```    
    - Altitud (metros por encima del nivel del mar):
    ```javascript
        console.info(position.coords.altitude);
    ```    
    - Precisión de altitud:
    ```javascript
        console.info(position.coords.altitudeAccuracy);
    ```     
    - Rumbo (Grados respectos al norte):
    ```javascript
        console.info(position.coords.heading);
    ```     
    - Velocidad (m/s):
    ```javascript
        console.info(position.coords.speed);
    ``` 
    - Timestamp:
    ```javascript
        console.info(position.timestamp);
    ``` 


- Ajustes de *geolocation*

    - enableHighAccuracy:
        - Mejora los datos para conexiones no GPS, pero aumenta drásticamente el consumo de batería del dispositivo.
        - *False por defecto*
    
    - timeout:
        - Tiempo (ms) de espera antes de lanzar el error.
        - *0 por defecto*
    
    - maximumAge:
        - Tiempo (ms) para el almacenamiento en memoria cache de la posición actual
        - *0 por defecto*
    
    - Ejemplo:
    ```javascript
        var opciones = {
            enableHighAccuracy: true,
            maximumAge: 1000, // 1s
            timeout: 2000 // 2s
        }
    ```


- Trabajando con *geolocation*

    - Comprobando la compatibildiad de *geolocation*
    ```javascript
        if ("geolocation" in navigator) {
          console.info("Podemos usar Geolocation! :-) ");
        } else {
          console.warn("Geolocation no soportado :-( ");
        }
    ```
    
    - Probando la geolocalización:
    ```javascript
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(function(position) {
                // Consola
                console.info("Latitud: " + position.coords.latitude + "\nLongitud: "+ position.coords.longitude);
                // HTML
                var datos = "<h1>Te pille!</h1>"
                datos += "Latitud: " + position.coords.latitude.toFixed(4) + "<br>"
                datos += "Longitud: "+ position.coords.longitude.toFixed(4)
                document.body.innerHTML = datos;
            });
        } else {
          console.warn("Geolocation no soportado :-( ");
        }
    ```
    - Mostrar la localización en una imagen estatica usando Gogole Maps:
    ```javascript
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(function(position) {

                var latlon = position.coords.latitude + "," + position.coords.longitude;
            
                var img_url = "http://maps.googleapis.com/maps/api/staticmap?center="+latlon+"&zoom=14&size=400x300&sensor=false";
            
                document.body.innerHTML = "<img src='"+img_url+"'>";
            });
        } else {
          console.warn("Geolocation no soportado :-( ");
        }    
    ```
    
    - Gestionar los Errores y rechazos:
    ```javascript
    navigator.geolocation.getCurrentPosition(geo_success, geo_error);
    
    function geo_success(position) {
      console.info(position.coords.latitude+","+ position.coords.longitude);
    }
    
    function geo_error(error) {
        switch(error.code) {
            case error.PERMISSION_DENIED:
                document.body.innerHTML = "El usuario ha rechazado la petición.";
                console.warn(error);
                break;
            case error.POSITION_UNAVAILABLE:
                document.body.innerHTML = "La posición de usuario no es correcta.";
                console.warn(error);
                break;
            case error.TIMEOUT:
                document.body.innerHTML = "No hay respuesta en el tiempo limite previsto.";
                console.warn(error);
                break;
            case error.UNKNOWN_ERROR:
                document.body.innerHTML = "Error Desconocido";
                console.warn(error);
                break;
        }
    }
    ```   
    
    - Trabajando con ajustes personalizados:
    ```javascript
    navigator.geolocation.getCurrentPosition(geo_exito, geo_error, opciones);
    
    var opciones = {
        enableHighAccuracy: true,
        maximumAge: 1000, // 1s
        timeout: 2000 // 2s
    }
    
    function geo_exito(position) {
        console.info(position.coords.latitude+","+ position.coords.longitude);
    }
    
    function geo_error(error) {
        console.warn("Error! - "+error);
    }
    ```
    
    - Convirtiendo las coordenadas a hexadecimal:
    ```javascript
    
    /**
     * From Isabel Castillo
     * http://isabelcastillo.com/convert-latitude-longitude-decimal-degrees
     * Convert longitude/latitude decimal degrees to degrees and minutes
     * DDD to DMS, no seconds
     * @param lat, latitude degrees decimal
     * @param lng, longitude degrees decimal
     */
            
    function convertDMS( lat, lng ) {
     
            var convertLat = Math.abs(lat);
            var LatDeg = Math.floor(convertLat);
            var LatSec = (Math.floor((convertLat - LatDeg) * 60));
            var LatCardinal = ((lat > 0) ? "n" : "s");
             
            var convertLng = Math.abs(lng);
            var LngDeg = Math.floor(convertLng);
            var LngSec = (Math.floor((convertLng - LngDeg) * 60));
            var LngCardinal = ((lng > 0) ? "e" : "w");
             
            return LatDeg + LatCardinal + LatSec  + "<br />" + LngDeg + LngCardinal + LngSec;
    }
    ```
    
    - Sigue a un usuario:
    ```javascript
    
        navigator.geolocation.watchPosition(geo_exito, geo_error);

        function geo_exito(position) {
            console.info(position.coords.latitude +", "+ position.coords.longitude);
        }
        
        function geo_error(error) {
            console.warn("Error! - "+error);
        }

    ```

**Google Maps**
- Librería
```html 
<script type='text/javascript' src="http://maps.googleapis.com/maps/api/js?sensor=false&extension=.js&output=embed"></script>
```

- Centrar el mapa
```javascript
function initMap() {
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 8,
    center: {lat: -3.8199647, lng: 40.4381307}
  });
}
```

- Markers ( [Demo](https://developers.google.com/maps/documentation/javascript/examples/marker-labels) )
```javascript
// In the following example, markers appear when the user clicks on the map.
// Each marker is labeled with a single alphabetical character.
var labels = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
var labelIndex = 0;

function initialize() {
  var bangalore = { lat: 12.97, lng: 77.59 };
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 12,
    center: bangalore
  });

  // This event listener calls addMarker() when the map is clicked.
  google.maps.event.addListener(map, 'click', function(event) {
    addMarker(event.latLng, map);
  });

  // Add a marker at the center of the map.
  addMarker(bangalore, map);
}

// Adds a marker to the map.
function addMarker(location, map) {
  // Add the marker at the clicked location, and add the next-available label
  // from the array of alphabetical characters.
  var marker = new google.maps.Marker({
    position: location,
    label: labels[labelIndex++ % labels.length],
    map: map
  });
}

google.maps.event.addDomListener(window, 'load', initialize);
```

- Markers Personalizados ( [Demo](https://developers.google.com/maps/documentation/javascript/examples/icon-simple) )
```javascript
// This example adds a marker to indicate the position of Bondi Beach in Sydney,
// Australia.
function initMap() {
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 4,
    center: {lat: -33, lng: 151}
  });

  var image = 'images/beachflag.png';
  var beachMarker = new google.maps.Marker({
    position: {lat: -33.890, lng: 151.274},
    map: map,
    icon: image
  });
}
```

- InfoWindows ( [Demo](https://developers.google.com/maps/documentation/javascript/examples/infowindow-simple) )
```javascript
// This example displays a marker at the center of Australia.
// When the user clicks the marker, an info window opens.

function initMap() {
  var uluru = {lat: -25.363, lng: 131.044};
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 4,
    center: uluru
  });

  var contentString = '<div id="content">'+
      '<div id="siteNotice">'+
      '</div>'+
      '<h1 id="firstHeading" class="firstHeading">Uluru</h1>'+
      '<div id="bodyContent">'+
      '<p><b>Uluru</b>, also referred to as <b>Ayers Rock</b>, is a large ' +
      'sandstone rock formation in the southern part of the '+
      'Northern Territory, central Australia. It lies 335&#160;km (208&#160;mi) '+
      'south west of the nearest large town, Alice Springs; 450&#160;km '+
      '(280&#160;mi) by road. Kata Tjuta and Uluru are the two major '+
      'features of the Uluru - Kata Tjuta National Park. Uluru is '+
      'sacred to the Pitjantjatjara and Yankunytjatjara, the '+
      'Aboriginal people of the area. It has many springs, waterholes, '+
      'rock caves and ancient paintings. Uluru is listed as a World '+
      'Heritage Site.</p>'+
      '<p>Attribution: Uluru, <a href="https://en.wikipedia.org/w/index.php?title=Uluru&oldid=297882194">'+
      'https://en.wikipedia.org/w/index.php?title=Uluru</a> '+
      '(last visited June 22, 2009).</p>'+
      '</div>'+
      '</div>';

  var infowindow = new google.maps.InfoWindow({
    content: contentString
  });

  var marker = new google.maps.Marker({
    position: uluru,
    map: map,
    title: 'Uluru (Ayers Rock)'
  });
  marker.addListener('click', function() {
    infowindow.open(map, marker);
  });
}
```


- Notas sobre GMaps:
    - [Documentación](https://developers.google.com/maps/documentation/javascript/)
    - [Ejemplos](https://developers.google.com/maps/documentation/javascript/examples/)
    - [Soporte Stackoverflow](https://stackoverflow.com/questions/tagged/google-maps-api-3)