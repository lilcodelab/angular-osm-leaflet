# Integrate OpenStreetMap using Leaflet with Angular application
Today many clients for Angular applications requier some kind of maps. In this example, I will describe how to implement OpenStreetMap using Leaflet render.

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

### Install Leaflet
To use Leaflet in Angular project, you need to install depencency packages:

1. ```npm i --s leaflet``` - Leaflet package.
2. ```npm i --s @asymmetrik/ngx-leaflet``` - Leaflet wrapper for Angular. Provides flexible and extensible components for integrating Leaflet into Angular project.

Because we are using TypeScript inside Angular project, you should install types for Leaflet ```npm i --s @types\leaflet```.

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
    import { LeafletModule } from '@asymmetrik/ngx-leaflet';
    ...
    imports: [
        ...
        LeafletModule
    ]
    ...
    ```

### Add and configure map in application
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

```typescript
import { Component } from '@angular/core';
import * as Leaflet from 'leaflet';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'AngularOSM';

  options: Leaflet.MapOptions = {
    layers: getLayers(),
    zoom: 12,
    center: new Leaflet.LatLng(43.530147, 16.488932)
  };
}

export const getLayers = (): Leaflet.Layer[] => {
  return [
    new Leaflet.TileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors'
    } as Leaflet.TileLayerOptions),
  ] as Leaflet.Layer[];
};

```
Leaflet render includes multiple [options](https://leafletjs.com/reference-1.7.1.html), such as:
1. **Layers** - you can render multiple layers, from multiple sources. For this sample we are using **OpenStreetMap** tile service, with their attribution.
2. **Zoom** - most tile services offer tiles up to zoom level 18, depending on their coverage.
3. **Center** - specify the geographic center of the map.

When you start your application with ```ng serve``` command and open http://localhost:4200, you should see a map rendered :)

### Show markers on the map
For example, let's add two markers on the map. One marker will represent "Workspace" and second one is "Riva". 

```typescript
export const getMarkers = (): Leaflet.Marker[] => {
  return [
    new Leaflet.Marker(new Leaflet.LatLng(43.5121264, 16.4700729), {
      icon: new Leaflet.Icon({
        iconSize: [50, 41],
        iconAnchor: [13, 41],
        iconUrl: 'assets/blue-marker.svg',
      }),
      title: 'Workspace'
    } as Leaflet.MarkerOptions),
    new Leaflet.Marker(new Leaflet.LatLng(43.5074826, 16.4390046), {
      icon: new Leaflet.Icon({
        iconSize: [50, 41],
        iconAnchor: [13, 41],
        iconUrl: 'assets/red-marker.svg',
      }),
      title: 'Riva'
    } as Leaflet.MarkerOptions),
  ] as Leaflet.Marker[];
};
```
** Inside ```assets``` folder you must add icons for markers, which will be rendered on the map.

Now, inside our layers we must add created markers. After that, check the application and the map with two markers should be rendered.
```typescript

export const getLayers = (): Leaflet.Layer[] => {
  return [
   ...
    ...getMarkers()
  ] as Leaflet.Layer[];
};
```

### Show polylines on the map
We already created markers on the map, now we want to show polyline between those two markers:

```typescript
export const getRoutes = (): Leaflet.Polyline[] => {
  return [
    new Leaflet.Polyline([
      new Leaflet.LatLng(43.5121264, 16.4700729),
      new Leaflet.LatLng(43.5074826, 16.4390046),
    ] as Leaflet.LatLng[], {
      color: '#0d9148'
    } as Leaflet.PolylineOptions)
  ] as Leaflet.Polyline[];
};
```
Of cource, we must include it inside layers:
```typescript
export const getLayers = (): Leaflet.Layer[] => {
  return [
  ...
    ...getRoutes()
  ] as Leaflet.Layer[];
};
```

### Show polygons on the map
Let's try to add some polygone on our current map. Polygone is a two-dimensional plane shape with three or more straight sides. Defining polygone, is done with defining multiple points.

```typescript
export const getPolygons = (): Leaflet.Polygon[] => {
  return [
    new Leaflet.Polygon([
      new Leaflet.LatLng(43.5181349, 16.4537963),
      new Leaflet.LatLng(43.517890, 16.439939),
      new Leaflet.LatLng(43.515599, 16.446556),
      new Leaflet.LatLng(43.518025, 16.463492)
    ] as Leaflet.LatLng[],
      {
        fillColor: '#eb530d',
        color: '#eb780d'
      } as Leaflet.PolylineOptions)
  ] as Leaflet.Polygon[];
};
```

After defining a polygone, we should add it inside our layers:
```typescript
export const getLayers = (): Leaflet.Layer[] => {
  return [
  ...
    ...getPolygons()
  ] as Leaflet.Layer[];
};
```

### Show current location

### Edit map style
If you want to edit style of your map, you can use [MapTiler](https://www.maptiler.com/). After you create your own or choose one of sample designs, you can just change URI of our TileLayer. For example, I will use ```pastel``` style for map:
```typescript
export const getLayers = (): Leaflet.Layer[] => {
  return [
    // Basic style
    new Leaflet.TileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors'
    } as Leaflet.TileLayerOptions),
    // Pastel style, remove if you want basic style.
    new Leaflet.TileLayer('https://api.maptiler.com/maps/pastel/{z}/{x}/{y}.png?key={your_key}', {
      attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">© MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">© OpenStreetMap contributors</a>',
    } as Leaflet.TileLayerOptions),
    ...getMarkers(),
    ...getRoutes(),
    ...getPolygones()
  ] as Leaflet.Layer[];
};
```
In this case, we have two layers of maps. First one is with basic style, second one is pastel styled. The pastel styled map will be rendered above basic one. The idea is that you can have multiple layers. So, each layer will represent some data.

** Be careful. MapTiler is only **free** for non-comercial use and limited on 100 thousand/month. Check the [pricing plan](https://www.maptiler.com/cloud/plans/).
## References
- https://leafletjs.com
- https://github.com/Asymmetrik/ngx-leaflet
- https://medium.com/@nnwabuokei/creating-editing-geo-fences-using-openstreetmap-with-the-leaflet-js-library-in-angular-5-project-44f0abbe2643
- https://www.maptiler.com/
