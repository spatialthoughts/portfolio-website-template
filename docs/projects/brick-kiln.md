# Mapping Brick Kilns using Satellite Embeddings

![Similarity map showing predicted brick kiln locations](../assets/images/brick-kiln.png)

## Overview

This project demonstrates how Google's AlphaEarth Satellite Embeddings can be used to locate
brick kilns across Gandhinagar district, Gujarat, India — without training a traditional
classifier. A small set of known brick kiln locations is used as reference; their embedding
vectors are compared against every pixel in the region to find visually similar areas.

**Study Area:** Gandhinagar district, Gujarat, India  
**Year:** 2024  
**Role:** Solo project  
**Status:** Completed

---

## Methods & Tools

**Data Sources**

- `GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL` — AlphaEarth annual satellite embeddings (20 m)
- GAUL 2024 Level-2 admin boundaries (`projects/sat-io/open-datasets/FAO/GAUL/GAUL_2024_L2`) for the study area

**Processing Steps**

1. Filter the GAUL dataset to Gandhinagar district and use it as the search region
2. Mosaic the 2024 satellite embedding image collection
3. Digitize a small set of reference brick kiln locations as sample points
4. Extract the 64-band embedding vector at each reference point
5. Compute the dot-product similarity between each reference vector and every pixel in the mosaic (values near 1 = highly similar)
6. Average the per-reference similarity maps into a single mean-similarity image
7. Threshold at 0.97 to select candidate pixels, then vectorize and extract centroids as predicted matches
8. Export predicted matches to a GEE asset for validation

**Tools Used**

| Tool | Purpose |
|------|---------|
| Google Earth Engine | Embedding retrieval, similarity computation, vectorization |
| GAUL 2024 | Administrative boundary for the study area |
| AlphaEarth Satellite Embeddings | Per-pixel visual feature vectors |

---

## Key Findings

- Dot-product similarity in embedding space effectively highlights brick kilns without labeled training data beyond a handful of reference points
- A threshold of 0.97 on mean similarity produced candidate locations with high spatial precision at 20 m scale
- The approach is transferable: swapping the reference points and study area geometry is all that is needed to search for other object types or regions

---

## Code

```javascript
// Similarity Search with Satellite Embeddings
// Mapping Brick Kiln locations

// Select the Search Region
// ****************************************************

// Use GAUL 2024 dataset from GEE Community Catalog
var admin2 = ee.FeatureCollection('projects/sat-io/open-datasets/FAO/GAUL/GAUL_2024_L2');
// Gandhinagar district, Gujarat, India
var filteredAdmin2 = admin2
  .filter(ee.Filter.eq('gaul2_name', 'Gandhinagar'))
  .filter(ee.Filter.eq('gaul1_name', 'Gujarat'));

var geometry = filteredAdmin2.geometry();

Map.centerObject(geometry);
Map.addLayer(geometry, {color: 'red'}, 'Search Area');

// Select Reference Location(s)
// ****************************************************

// Use the satellite basemap
Map.setOptions('SATELLITE');

// Add a few reference locations of Brick Kilns
// in the 'samples' FeatureCollection


// Select a time-period
// ****************************************************
var year = 2024;
var startDate = ee.Date.fromYMD(year, 1, 1);
var endDate = startDate.advance(1, 'year');

// Filter and mosaic the Satellite Embedding dataset
// ****************************************************
var embeddings = ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL');
var mosaic = embeddings
  .filter(ee.Filter.date(startDate, endDate))
  .mosaic();

// Choose the scale
// You may choose a larger value for larger objects
var scale = 20; 

// Extract the embedding vector from the samples
var sampleEmbeddings = mosaic.sampleRegions({
  collection: samples,
  scale: scale
});

// Calculate Similarity
// ****************************************************
// We compute the dot product between two embedding vectors
// Results are interepreted as distances in embedding space
// Values closer to 1 are closer together (more similar)
// Values closer to -1 are further apart (less similar)
var bandNames = mosaic.bandNames();

var sampleDistances = ee.ImageCollection(sampleEmbeddings.map(function (f) {
  var arrayImage = ee.Image(f.toArray(bandNames)).arrayFlatten([bandNames]);
  var dotProduct = arrayImage.multiply(mosaic)
    .reduce('sum')
    .rename('similarity');
  return dotProduct;
}));

// Calculate mean distance from all reference locations
var meanDistance = sampleDistances.mean();

// Visualize the distance image
var palette = [
  '000004', '2C105C', '711F81', 'B63679',
  'EE605E', 'FDAE78', 'FCFDBF', 'FFFFFF'
];
var similarityVis = {palette: palette, min: 0.5, max: 1};
Map.addLayer(meanDistance.clip(geometry), similarityVis,
  'Similarity (bright = close)', false);


// Extract Location Matches
// ****************************************************

// Apply a threshold
var threshold = 0.97;
var similarPixels = meanDistance.gt(threshold);

// Vectorize the results 
// Mask 0 values using selfMask()
// to get polygons only for the matched pixels
var polygons = similarPixels.selfMask().reduceToVectors({
  scale: scale,
  eightConnected: false,
  maxPixels: 1e10,
  geometry: geometry
});

// Extract the centroids of vectorized polygons
var predictedMatches = polygons.map(function(f) {
  return f.centroid({maxError:1});
});

// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
// Export matches to an asset (optional)
// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

// Vectorization operation is a memory and compute-intensive
// operation. To prevent errors, Export the results as an Asset first.

// Once the asset is exported, it can be imported and visualized

// Replace this with your asset folder
// The folder must exist before exporting
var exportFolder = 'projects/spatialthoughts/assets/satellite_embedding/';
var matchesExportFc = 'predicted_brick_kiln_matches';
var matchesExportFcPath = exportFolder + matchesExportFc;

Export.table.toAsset({
  collection: predictedMatches,
  description: 'Predicted_Matches_Export',
  assetId: matchesExportFcPath
});

// Wait for the export to complete and continue with
// the exported asset from here onwards.
//var predictedMatches = ee.FeatureCollection(matchesExportFcPath);

// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
// End optional export section
// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

// Visualize the Matches
// ****************************************************

Map.addLayer(predictedMatches, {color: 'cyan'} , 'Predicted Matches');
```

---

## Links

[View Code on Google Earth Engine](https://code.earthengine.google.co.in/25798991866e518d8c729e0120ee1c50){ .md-button }
