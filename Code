#Within 100-meter buffer from water sources waste dumping is not allowed. Identify possible waste dumping sites.
#*Draw the flowchart
#*Develop a python program to perform this task



                 
#Add  water sources layer 
from qgis.core import QgsVectorLayer, QgsField

file_name = "C:\spatial_data\data\water_sources.shp"

water_sources = QgsVectorLayer(file_name,"water_sources", "ogr")

QgsProject.instance().addMapLayer(water_sources)

#-------------------------------------------------------------------------------

#100m buffer for water sources 
features = water_sources.getFeatures()
print(type(features))
buffers = []
for feature in features:
    buffers.append(feature.geometry().buffer(0.1,5))
    
union_geometry = QgsGeometry.unaryUnion(buffers)

#Create a QgsVectorLayer from the union geometry
buffer_layer = QgsVectorLayer("Polygon?crs=EPSG:4326","Union Geometry", "memory")
pr = buffer_layer.dataProvider()
buffer_layer.startEditing()

#Define the attributes (if needed)
fields = [QgsField("id", QVariant.Int)]
pr.addAttributes (fields)

#Add a feature with the union geometry 
feature = QgsFeature()
feature.setGeometry(union_geometry)
feature.setAttributes([1])
pr.addFeatures([feature])
buffer_layer.commitChanges()

# Specify the path to save the shapefile (replace with your desired file path)
output_shapefile_path = "C:\spatial_data\Result\water_sources_buffer.shp"

# Use QgsVectorFileWriter to save the layer as a shapefile
error = QgsVectorFileWriter.writeAsVectorFormat(buffer_layer, "C:\spatial_data\Result\water_sources_buffer.shp", "UTF-8", buffer_layer.crs(), "ESRI Shapefile")

#Add water_sources_buffer
from qgis.core import QgsVectorLayer, QgsField

file_name = "C:\spatial_data\Result\water_sources_buffer.shp"

water_sources_buffer = QgsVectorLayer(file_name,"water_sources_buffer", "ogr")

QgsProject.instance().addMapLayer(water_sources_buffer)

#---------------------------------------------------------------------------------


#Add waste_dumping_sites 
from qgis.core import QgsVectorLayer, QgsField

file_name = "C:\spatial_data\data\waste_dumping_sites.shp"

waste_dumping_sites = QgsVectorLayer(file_name,"waste_dumping_sites", "ogr")

QgsProject.instance().addMapLayer(waste_dumping_sites)


#-------------------------------------------------------------------------------


#file_name = "C:\spatial_data\data\waste_dumping_sites.shp"

#-------------------------------------------------------------------------------

#Like Union Geometry 
waste_dumping_sites = QgsVectorLayer(file_name,"possible waste dumping sites", "ogr")
is_within = QgsField("is_within", QVariant.String)
waste_dumping_sites.startEditing()
waste_dumping_sites.dataProvider().addAttributes([is_within])
waste_dumping_sites.updateFields()


#Like within union geometry 
waste_dumping_sites_features = waste_dumping_sites.getFeatures()
for wf in waste_dumping_sites_features:
    contains = union_geometry.contains(wf.geometry())
    waste_dumping_sites.changeAttributeValue(wf.id(),waste_dumping_sites.fields().indexFromName("is_within"), f"{str(contains)}")

  
# Commit the changes
waste_dumping_sites.commitChanges()
QgsProject.instance().addMapLayer(buffer_layer)
QgsProject.instance().addMapLayer(waste_dumping_sites)


#-------------------------------------------------------------------------------

