# Workflow for using TileMill and Mapbox with Neatline

Neatline has straight-forward, built-in support for using WMS servers and layers within presentations. However, there are other ways to add custom layers into Neatline. [Tilemill](https://www.mapbox.com/tilemill/), created and supported by Mapbox, is a free, native application for creating custom maps. [Mapbox](https://www.mapbox.com) itself is a web application, with free and paid plans, for creating and serving interactive maps. Combined with a geo-referenced map or image, these tools allow you to build a custom map layer that can be used in Neatline.

## TileMill

These directions assume that you have a geo-referenced map, in this case a GeoTIFF.

#### Create new project

To create a new project, click on the "Projects" tab in the left-side menu. From there, click the "New Project button." Within the "New Project" form, fill out "Filename," "Name," and "Description."

For "Image Format," TileMill defaults to "png (24-bit)," which will provide high image quality. However, if you need to be more efficient with file size and storage, especially if hosting the map on Mapbox, you may wish to choose "png (8-bit)" or one of the jpeg options. The "Image Format" can always be changed later in the Project Settings.

By default, TileMill includes a basic world layer as "Default Data." Uncheck this if you do not want this base layer. If you uncheck it, you will need to import your own base layer.

![TileMill New Project](images/tilemill.newproject.png)

#### Editor Screen

The Editor Screen in TileMill has two panes. On the left you'll see the map with standard map controls. On the right will be your style.mss sheet. TileMill uses a language called CartoCSS to style the map. You can find documentation on using this [here](https://www.mapbox.com/tilemill/docs/crashcourse/styling/).

![TileMill Whole Screen](images/tilemill.wholescreen.png)

### Adding your GeoTIFF

To add the GeoTIFF, click the layers icon ![Layers Icon](images/tilemill.layer.icon.png) in the bottom left corner to bring up the layers panel.

![Layers Menu](images/tilemill.addlayer.png)

Click "Add layer." Within the form that pops up, you will need to fill out several fields.

"ID" and "Class" are for use with CartoCSS to style the layer. You will need to provide at least one of these if you wish to make any changes to the style of your layer.

For "Datasource", navigate to your GeoTIFF and hit done.

For SRS (spatial reference system), TileMill requires GeoTIFFS to use the Google Web Mercator projection. If you don't know the SRS of your GeoTIFF, leave this field at "Autodetect" and TileMill may handle it for you. If it doesn't and you know the SRS, enter it in manually. With small GeoTIFFs, TileMill can render the file even if it is not projected to Google Web Mercator. With large GeoTIFFs, it is best to reproject the GeoTIFF to Google Web Mercator before adding it into TileMill. See TileMill's directions [here](https://www.mapbox.com/tilemill/docs/guides/reprojecting-geotiff/) on doing so with GDALwarp.

![Layer Settings](images/tilemill.addlayer.fields.cropped.png)

Hit "Save and Style" to return to the Editor. Your GeoTIFF should now appear on the map. If you wish to style the layer, use the ID or class you assigned to the layer as the selector within the style.mss file.

![Map with Layer](images/tilemill.map.withlayer.png)

### Exporting to MapBox

TileMill has a built in export feature. If you want to only export a section of the map, and not include the background from the world map, you will need to edit the style.mss file. Remove the code for
```
Map {
  background-color: #------
}
```
and for
```
#countries {
  polygon-fill: #------
}
```
If you do want to include the map background, which might be the case if you've imported multiple layers in, then proceed with the export.

To export, click on the arrow beside "Export" in the upper right corner.
Select "MBtiles." Using metatiles increases the efficiency and smoothness of a web map.

![Export](images/tilemill.toexport.png)

After selecting "MBTiles," you'll be presented with a set of options as well as the map itself. First, hold shift and drag to select the part of the map that you want to export. This sets the "Bounds" of the map. In the interest of file size, export the smallest amount of the map that you need/wish to use.

Fill out "Name," "Description," "Attribution," "Version," and "Filename" as desired.

"Zoom" determines at which zoom levels these maptiles, and hence the map, will appear. For example, if you set the range between 6 and 10, then the exported map will only appear once a user has zoomed to level 6, and will disappear again past level 10. The zoom range significantly affects the file size of the exported map. In determining these values, keep in my how you intend to use the exported map and how much space you will have available to host it.

"Center" sets the central position of the map, including the starting zoom level. To set this, zoom and pan to the view you want and click at the center of the map.

"MetaTile size" determines how many tiles will compose each metatile. Changing this value will affect the size of the output file. The default is 2, but you can increase this in order to decrease the file size. For more information on changing the metatile size, see the [TileMill documentation.](https://www.mapbox.com/tilemill/docs/guides/metatiles/)

Click "Save settings to project" if you wish to use these same settings or modify them in the future for this project.

Click "Export."

**A Note on File Size:** This tutorial assumes you will export this map to Mapbox to serve to Neatline. Mapbox has limits based on account type for data storage. For free accounts, you are allowed to have 50 MB of uploaded data. Depending on the resolution of your map, the bounds, zoom level, and metatile size, the size of the file could quickly exceed Mapbox's free limit. If you wish to use Mapbox and want larger storage, see their [paid plans.](https://www.mapbox.com/plans/)

![Export Settings](images/tilemill.export.settings.png)

After export, your map will be available at the location specified in your TileMill Settings.

You are now ready to import the tiles you've created into Mapbox.

## Mapbox

### Uploading the MBtiles created in TileMill

First, you need to upload the layer that you've just created. In the "Data" page of your account, click "Upload data." Choose the file and click "Upload file." The file will appear in your list of data sources. It may take a few minutes for the file to be processed. Once it is done processing it will be available to be used in a map. You will need to refresh the data page in order to update the processing status.

### Creating a new project

In the projects page of Mapbox, click the "Create project" button.

Click on the "Project" tab, then click on "Settings." Give your project a name and description, then hit "Save."

![Mapbox Project Info](images/mapbox.info.name.png)

### Changing the baselayer

Free accounts can choose between two baselayers: streets and terrain. The satellite baselayer is only available to paid users.

To select a baselayer, go to the "Style" tab. Select "Baselayer" and click the desired layer.

![Mapbox Baselayer](images/mapbox.baselayer.png)

The "Style" tab contains a number of other features allowing you to customize your map, changing colors of different features and hiding default layers. For help on this, see the [Mapbox Documentation.](https://www.mapbox.com/foundations/customizing-the-map/)

### Adding your layer from TileMill

In order to add the layer you created in TileMill, click on "Data," then on the layers icon ![Layers Icon](images/mapbox.layer.icon.png). Move the slider to "layers."

![Layers Menu](images/mapbox.layer.png)

Click "Add layers," then select the name of the layer. Click "Save." Your layer will now appear on the map when it is zoomed within the range you specified in the TileMill export.

![Map with Layer](images/mapbox.withlayer.png)

### Using this map in Neatline

Once the map is saved in Mapbox, it is available online and can be pulled into Neatline. In order to make the layer available to Neatline, you'll have to create a JSON file that tells Neatline where to find the layer.

In your favorite text editor, create a file called mapbox.json. Save that file to the following location within your Omeka installation: theme root/neatline/layers/.

For example, if I'm using the Neatlight theme for Omeka, I would save mapbox.json to: omeka folder/themes/neatlight/neatline/layers/. You may need to create the layers folder yourself.

Neatline will detect any layers specified in this file and make them available in exhibits.

The mapbox.json file will look something like this:

```
{

  "Mapbox (Neatlight)": [

    {
      "title": "Project Gemini over Baja California Sur",
      "id": "Mapbox.dclure.hc7a4mo9",
      "type": "XYZ",
      "properties": {
        "urls": [
          "http://a.tiles.mapbox.com/v3/dclure.hc7a4mo9/${z}/${x}/${y}.png",
          "http://b.tiles.mapbox.com/v3/dclure.hc7a4mo9/${z}/${x}/${y}.png",
          "http://c.tiles.mapbox.com/v3/dclure.hc7a4mo9/${z}/${x}/${y}.png",
          "http://d.tiles.mapbox.com/v3/dclure.hc7a4mo9/${z}/${x}/${y}.png"
        ]
      }
    },

    {
      "title": "The Declaration of Independence",
      "id": "Mapbox.dclure.i76fpfoj",
      "type": "XYZ",
      "properties": {
        "urls": [
          "http://a.tiles.mapbox.com/v3/dclure.i76fpfoj/${z}/${x}/${y}.png",
          "http://b.tiles.mapbox.com/v3/dclure.i76fpfoj/${z}/${x}/${y}.png",
          "http://c.tiles.mapbox.com/v3/dclure.i76fpfoj/${z}/${x}/${y}.png",
          "http://d.tiles.mapbox.com/v3/dclure.i76fpfoj/${z}/${x}/${y}.png"
        ]
      }
    }

  ]

}
```
[Source](https://github.com/davidmcclure/neatlight/blob/master/neatline/layers/mapbox.json)

The JSON file has several pieces. First, "Mapbox: (Neatlight)" is the name of the group of layers you are including in the file. This layer name should be descriptive of the layers you are using. In this case, the layers are all hosted at Mapbox and are going to be used within the Neatlight theme for Omeka.

For each layer you are including, you need a "title" for the layer. This will appear in the layer list in Neatline.

For the "id" you will need to enter the unique id of the map from Mapbox. You'll find this by going into your map in Mapbox, and going to the "Project" tab. In the "Info" tab, there is a "Map ID" field. The content of this field is the unique ID you'll put in the "id" field of your mapbox.json file.

For "type", enter "XYZ". This is specifying the type of layer you are importing.

For "properties", you'll be modifying the URL provided for your map by Mapbox. In the same tab where you found the Map ID, you'll find the URL in the "Share" field. Copy this over to your JSON file. You'll notice that the end of the URL is different than that shown in the same JSON file. The URL you're given will look like this: http://a.tiles.mapbox.com/v3/csbailey.idecmm92/page.html#6/38.805/-82.244.

Within the mapbox.json file, delete everything in the link after the unique Map ID (everything from page.html onward). Replace that with "${z}/${x}/${y}.png".

You'll notice that the sample file has four URLs, all the same with just the first letter after "http://" varying from 'a' to 'd.' All four URLs go to the same map, but this variation allows for faster speeds loading the map. However, the map will load just fine with only the one, original URL.

For each layer that you want to make available in Neatline, you'll need to enter all four fields: "title," "id," "type," and "properties." The sample file above specifies two layers which will be available within the Neatlight theme in Omeka. While each layer is being used in a different exhibit, both have to be specified within this one mapbox.json file.

Once you save this file, you can begin using the layer in Neatline. You'll make the layer available through the "Exhibit Settings" of your exhibit.

![Exhibit Options](images/neatline.exhibit.options.png)

In the "Enabled Spatial Layers" field, scroll down and you'll see the name of the group of layers with each layer you specified below it. Select the layer you wish to enable for the exhibit. If you want that layer to be used as the default layer in the exhibit, select it as well for the "Default Spatial Layer" field. Click "Save Exhibit" at the bottom of the page and the layer will now be appear in your Neatline exhibit.

![Exhibit Settings](images/neatline.layers.png)

Two considerations:

- The "id" for each layer must be unique. If an "id" is repeated, Neatline won't break, but it will only use the last layer in the file with that "id."
- Once a layer has been added and used in an exhibit, it is very difficult to change the "id." Neatline stores the "ids" as raw strings in the database, which means that changing the values would have to be done by hand in the database.
