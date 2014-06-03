# Workflow for using TileMill and Mapbox with Neatline

Neatline has straight-forward, built-in support for using WMS servers and layers within presentations. However, there are other ways to add custom layers into Neatline. [Tilemill](https://www.mapbox.com/tilemill/), created and supported by Mapbox, is a free, native application for creating custom maps. [Mapbox](https://www.mapbox.com) itself is a web application, with free and paid plans, for creating and serving interactive maps. Combined with a geo-referenced map or image, these tools allow you to build a custom map layer that can be used in Neatline.

## TileMill

These directions assume that you have a geo-referenced map, in this case a GeoTIFF.

#### Create new project - settings screen

Filename
Default Data - Need to explain this?

![TileMill New Project](images/tilemill.newproject.png)

#### Default Editor screen

![TileMill Whole Screen](images/tilemill.wholescreen.png)

explain map on left, style.mss on right

to add the GeoTIFF, click the layers icon ![Layers Icon](images/tilemill.layer.icon.png)

*maybe use:* ![Layers Menu](images/tilemill.addlayer.png)

![Layer Settings](images/tilemill.addlayer.fields.cropped.png)

click add layer button

for datasource, navigate to your GeoTIFF

For SRS, TileMill may autodetect settings. If not, need to specify SRS. (Do we need to document how to determine this with GDALinfo?)

if you want to style the layer you are creating, give it an id or class to be used in the mss file

When working with GeoTIFFs, you may need to reproject your file to Google Web Mercator. TileMill docs have instructions on doing this with gdalwarp [here](https://www.mapbox.com/tilemill/docs/guides/reprojecting-GeoTIFF/).

Hit save and style

Your GeoTIFF should now appear on the map

To push a section of this map to MapBox

hit the little arrow beside export in the upper right corner
Select MBtiles (need to explain why tiles)

Name
Attribution - creator of map or data
explain zoom - determines at which zoom levels these maptiles will appear
center - click on map to reset center at whichever zoom level
bounds - in the map pane, hold shift and drag to specify the section of the map that you wish to export
metatile size - changing this can affect how large the output file is. default is 2, but you can increase this in order to decrease the file size
click save settings to project if you wish to do so
Click export

Write something about keeping the filesize small if using free mapbox (50MB upload storage)

If sucessful, your map will now be availab eat whichever location you have specified in your TileMill Settings.

You are now ready to import the tiles you've created into mapbox

## Mapbox

### Uploading the MBtiles created in TileMill

First, we need to upload the layer that we've just created. In the data page of your account, click upload data. Choose the file and click upload. The free account limits you to only 50 mb of space. For more space, for larger files, you would need a paid plan.

After you've clicked upload, the file will appear in your list of data sources. It may take a few minutes for the file to processed. Once it is done processing it will be available to be used in a map. You will need to refresh the data page in order to update the processing status.

## Creating a new project

in the projects page of mapbox, hit the create project button

click on the project tab, then click on settings. Go ahead and give your project a name and description, then hit save.

Free accounts can choose between two baselayers: streets and terrain. Satellite baselayer is only available to paid users

to select baselayer, go to style, then baselayer and click desired layer

briefly address other things in style tab

In order to add the new layer, click on data, then on the layers icon. Move the slider to layers.

click add layers, then select the name of the layer you created in TileMill

Your layer will now appear on the map when it is zoomed within the range you specified in the TileMill export

click save

## Using this map in Neatline

Once the map is saved in Mapbox, it is available online and can be pulled into Neatline.

In order to make the layer available to Neatline, you'll have to create a JSON file that tells Neatline where to find the layer.

create a file called mapbox.json in your favorite text editor. Save that file to the following location within your omeka installation: <theme root>/neatline/layers/.

For example, if I'm using the Neatlight theme for Omeka, I would save mapbox.json to <omeka folder>/themes/neatlight/neatline/layers/. You may need to create the layers folder yourself.

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

For the "id" you will need to enter the unique id of the map from Mapbox. You'll find this by going into your map in Mapbox, and going to the "Project" tab. In the "Info" tab, there is a "Map ID" field. The contents of this field is the unique ID you'll put in the "id" field of your mapbox.json file.

For "Type", enter "XYZ". This is specifying the type of layer you are importing.

For "properties", you'll be modifying the URL provided for your map by Mapbox. In the same tab where you found the Map ID, you'll find the URL in the "Share" field. Copy this over to your JSON file. You'll notice that the end of the URL is different than that shown in the same JSON file. The URL you're given will look like this: https://a.tiles.mapbox.com/v3/csbailey.idecmm92/page.html#6/38.805/-82.244

Within the mapbox.json file, delete everything in the link after the unique Map ID (everything from page.html onward). Replace that with "${z}/${x}/${y}.png".

You'll notice that the sample file has four URLs, all the same with just the first letter after "http://" varying from 'a' to 'd.' All four URLs go to the same map, but this variation allows for faster speeds loading the map. However, the map will load just find with only the one, original URL.

For each layer that you want to make available to in Neatline, you'll need to enter all four fields: "title," "id," "type," and properties. The sample file above specifies two layers which will be available within the Neatlight theme in Omeka. While each layer is being used in a different exhibit, both have to be specified within this one mapbox.json file.

Once you save this file, you can begin using the layer in Neatline. You'll make the layer available through the "Exhibit Settings" of your exhibit. In the "Enabled Spatial Layers" field, scroll down and you'll see the name of the group of layers with each layer you specified below it. Select the layer you wish to enable for the exhibit. If you want that layer to be used as the default layer in the exhibit, select it as well for the "Default Spatial Layer" field.

Two considerations:

- The "id" for each layer must be unique. If an "id" is repeated, Neatline won't break, but it will only use the last layer in the file with that "id."
- Once a layer has been added and used in an exhibit, it is very difficult to change the "id." Neatline stores the "ids" as raw strings in the database, which mens that changing the values would have to be done by hand in the database.