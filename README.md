# Integrate OpenStreetMap with Angular application
Angular application that implements OpenStreetMap using Leaflet renderer.

## Requirements
Angular requires a current, active LTS, or maintenance LTS version of Node.js.

## Setup Angular PWA application
### Install and setup Angular
1. Install Angular CLI globally ```npm install -g @angular/cli```
2. Generate new Angular app using Angular CLI, called AngularPWA ```ng new AngularPwa```

### Start Angular application
In period of development you can run application with command ```ng serve```.   
By default your application will be hosted on http://localhost:4200 .  

If you want to build your Angular application, run command ```ng build```. For production build add production parameter ```ng build --prod```. 
Inside your Angular application folder, ``dist`` folder will be created with compiled files.

---

## OpenStreetMap and LeafletJS
OpenStreetMap is a freem editable map of the world, created by community contributors. Each contributor created some part of map data, such as roads, trails, hospitals, trees...
One of the most popular JavaScript libraries for embedding OSM map is [Leaflet](https://leafletjs.com/).

## Install Leaflet
To use Leaflet in Angular project, you need to install depencency packages:

1. ```npm i --s leaflet``` - Leaflet package.
2. ```npm i --s @asymmetrik/ngx-leaflet``` - Leaflet wrapper for Angular. Provides flexible and extensible components for integrating Leaflet into Angular project.

Because we are using TypeScript inside Angular project, you should install types for Leaflet ```npm i @types\leaflet```.

After the installation proces, you must implement Leaflet inside our application:
1. Add Leaflet style into application, inside ```angular.json``` add ```leaflet.css``` file:
    ```
    {
        ...
        "styles": [
            "styles.scss",
            "./node_modules/leaflet/dist/leaflet.css"
        ],
        ...
    }
    ```
2. Before you are starting to use Leaflet inside application, you must import Leaflet module inside ```app.module.ts``` or inside module you want to use it:
    ```
    import { LeafletModule } from '@my-project/ngx-leaflet';
    ...
    imports: [
        ...
        LeafletModule
    ]
    ...
    ```

## Add and configure map in application
After installation of depencency packages, you should add a map inside your component. Inside template of your component (for demo purposes I will use app.commponent) you must add leaflet attribude directive:
``` html
<div class="map"
     leaflet
     [leafletOptions]="options">
</div>
```
If you want a fullscreen map, you shoud add some style for map (in this case inside ```app.component.scss```):
```css
.map {
    height: 100%;
    padding: 0;
  }
```
Inside your ```app.component.ts``` add options for leaflet:

```javascript
import { Component } from '@angular/core';
import { latLng, tileLayer } from 'leaflet';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'AngularOSM';
  options = {
    layers: [
      tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
      })
    ],
    zoom: 7,
    center: latLng([ 46.879966, -121.726909 ])
  };
}
```
Leaflet render includes multiple [options](https://leafletjs.com/reference-1.7.1.html), such as:
1. **Layers** - you can render multiple layers, from multiple sources. For this sample we are using **OpenStreetMap** tile service, with their attribution.
2. **Zoom** - most tile services offer tiles up to zoom level 18, depending on their coverage.
3. **Center** - specify the  geographic center of the map.

When you start you application with ```ng serve``` command and open http://localhost:4200, you should see a map rendered :)
