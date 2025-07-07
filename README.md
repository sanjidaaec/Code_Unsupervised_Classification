# Code_Unsupervised_Classification
//Step 2
// Filter image collection by date and region of interest

var filtered = sentinel2. filterBounds (aoi)
.filterDate( '2022-01-01', '2022-04-30')
.filterMetadata( 'CLOUDY_PIXEL_OVER_LAND_PERCENTAGE', 'less_than', 10)
.median()

print (filtered)
//Step 3
// Select bands for classification
var bands = filtered.select('B2', 'B3', 'B4', 'B8').clip(aoi);

// Step 4: Create a color composite-- map FCC
var vis ={
min: 0,
max: 3600,
};
Map.addLayer(bands.select('B8','B4','B3'),vis,'Las Vegas BRG: G-R-NIR');

// Step 5
// Make the training dataset.
var training = bands.sample({
  region: aoi,
  scale: 10,
  numPixels: 10000
});

// Instantiate the clusterer and train it.

var clusterer10 = ee.Clusterer.wekaKMeans(10).train(training);

// Cluster the input using the trained clusterer.

var result = bands.cluster(clusterer10);
print (result);

//Step 6

// Display the clusters with random colors.
vis = ['#a6cee3', '#1f78b4', '#b2df8a', '#33a02c', '#fb9a99',
'#e31a1c', '#fdbf6f', '#ff7f00', '#cab2d6', '#6a3d9a'];

Map.addLayer(result, {min: 0, max: 9, palette: vis}, 'clusters');

//Step 7
//create histogram of result
var histogramcluster10 = ui.Chart.image.histogram({
  image:result.select('cluster'),
  region:aoi,
  scale:1000,
  minBucketWidth:1
});
histogramcluster10.setOptions({
  title:'cluster Kmeans result'
});
// Display chart
print (histogramcluster10);

//Change the number of cluster to 15

// Step 5
// Make the training dataset.
// var training = bands.sample({
//   region: aoi,
//   scale: 10,
//   numPixels: 10000
// });

// Instantiate the clusterer and train it.

var clusterer15 = ee.Clusterer.wekaKMeans(15).train(training);

// Cluster the input using the trained clusterer.

var result = bands.cluster(clusterer15);
print (result);

//Step 6

// Display the clusters with random colors.
vis = ['#a6cee3', '#1f78b4', '#b2df8a', '#33a02c', '#fb9a99',
'#e31a1c', '#fdbf6f', '#ff7f00', '#cab2d6', '#6a3d9a', '#edf8b1', '#deebf7','#efedf5','#e0f3db'];

Map.addLayer(result, {min: 0, max: 14, palette: vis}, 'clusters15');

//Step 7
//create histogram of result
var histogramcluster15 = ui.Chart.image.histogram({
  image:result.select('cluster'),
  region:aoi,
  scale:1000,
  minBucketWidth:1
});
histogramcluster15.setOptions({
  title:'cluster1 Kmeans result'
});
// Display chart
print (histogramcluster15);

//

// Step 5
// Make the training dataset.
// var training = bands.sample({
//   region: aoi,
//   scale: 10,
//   numPixels: 10000
// });

// Instantiate the clusterer and train it.

var clusterer5 = ee.Clusterer.wekaKMeans(5).train(training);

// Cluster the input using the trained clusterer.

var result = bands.cluster(clusterer5);
print (result);

//Step 6

// Display the clusters with random colors.
vis = ['#a6cee3', '#1f78b4', '#b2df8a', '#33a02c', '#fb9a99'];

Map.addLayer(result, {min: 0, max: 4, palette: vis}, 'clusters5');

//Step 7
//create histogram of result
var histogramcluster5 = ui.Chart.image.histogram({
  image:result.select('cluster'),
  region:aoi,
  scale:1000,
  minBucketWidth:1
});
histogramcluster5.setOptions({
  title:'cluster2 Kmeans result'
});
// Display chart
print (histogramcluster5);
