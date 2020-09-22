# Integrate OpenStreetMap with Angular application
Angular application that implements Open Street Map

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
        LeafletModule.forRoot()
    ]
    ...
    ```

## Add and configure map in application
After installation of depencency packages, you should add a map inside your component. Inside template of your component (for demo purposes i will use app.commponent) you must add leaflet attribude directive:
``` html
<div class="map" leaflet>
...
</div>
```

