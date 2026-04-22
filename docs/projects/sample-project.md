<!--
CHECKLIST FOR THIS PAGE (copy this file for each new project):
- [ ] Replace [YOUR PROJECT TITLE] with your project title
- [ ] Replace the hero image with your own (add to docs/assets/images/)
- [ ] Update the Overview section
- [ ] Update the Leaflet map: change coordinates, zoom level, study area box, and markers
- [ ] Update the Methods & Tools section
- [ ] Update the Key Findings section
- [ ] Update the Links section
- [ ] Add a card for this project on docs/projects/index.md
- [ ] Add a nav entry in mkdocs.yml
-->

# Land Cover Classification of Bangalore

![Project overview map showing classified land cover](../assets/images/sample-project.svg)

## Overview

This project maps urban land cover in the Bangalore metropolitan region using multispectral
Landsat-8 imagery and a Random Forest classifier trained in Python. Six land cover classes
were mapped: urban/built-up, vegetation, water bodies, bare soil, cropland, and peri-urban.
The classification achieved an overall accuracy of 89% validated against ground truth points
collected in the field.

**Duration:** June – August 2024  
**Role:** Solo project  
**Status:** Completed

---

## Study Area Map

The map below shows the study area boundary and the sample sites used for training and
validation. Click a marker to see the site label.

<!-- ============================================================
LEAFLET MAP — HOW TO CUSTOMIZE:
1. Change the coordinates in setView([lat, lng], zoom) to center your study area.
2. Change the rectangle corner coordinates to your study area boundary.
3. Edit the 'sites' array: update lat, lng, and label for each sample point.
4. Add or remove site entries as needed.
5. To add a second map on another project page, copy this entire block
   and change 'project-map' to a different unique id (e.g. 'project-map-2')
   in both the <div> and the L.map() call.
============================================================ -->

<div id="project-map" class="leaflet-map-container"></div>

<script>
// The map is initialized on 'load' so that the Leaflet library
// has finished loading before the map code runs.
window.addEventListener('load', function() {
    var map = L.map('project-map').setView([12.97, 77.59], 10);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    // Study area boundary — change these corner coordinates to match your study area
    var studyArea = L.rectangle(
        [[12.83, 77.45], [13.10, 77.75]],
        {color: '#e65100', weight: 2, fillColor: '#ff9800', fillOpacity: 0.08}
    ).addTo(map);
    studyArea.bindPopup('<strong>Study Area</strong><br>Bangalore Metropolitan Region');

    // Sample sites — update lat, lng, and label for your own sites
    var sites = [
        {lat: 12.978, lng: 77.591, label: "City Center (Urban)"},
        {lat: 13.021, lng: 77.650, label: "Yelahanka (Suburban)"},
        {lat: 12.843, lng: 77.526, label: "Electronic City (Industrial)"},
        {lat: 12.952, lng: 77.451, label: "Hesaraghatta Lake (Water Body)"},
        {lat: 13.062, lng: 77.491, label: "Doddaballapur (Cropland)"}
    ];

    sites.forEach(function(site) {
        L.marker([site.lat, site.lng])
            .bindPopup('<strong>' + site.label + '</strong>')
            .addTo(map);
    });
});
</script>

---

## Methods & Tools

**Data Sources**

- Landsat-8 OLI imagery (30 m resolution) from USGS Earth Explorer
- Ground truth points collected via field survey (n = 250)
- OpenStreetMap for reference basemap

**Processing Steps**

1. Cloud masking and atmospheric correction using LEDAPS in Google Earth Engine
2. Computation of spectral indices: NDVI, NDWI, NDBI
3. Random Forest classifier trained with 70% of ground truth points
4. Accuracy assessment using held-out 30% (confusion matrix, kappa coefficient)
5. Final map export and visualization in QGIS

**Tools Used**

| Tool | Purpose |
|------|---------|
| Google Earth Engine | Image acquisition and pre-processing |
| Python + scikit-learn | Random Forest classifier |
| QGIS | Visualization and cartography |
| Matplotlib | Charts and accuracy plots |
| Pandas | Tabular data management |

---

## Key Findings

- Overall accuracy: **89.3%** (Kappa: 0.87)
- Urban/built-up area has expanded by **23%** compared to 2014
- Vegetation cover declined by **18%** in the same period, concentrated in the northern corridor
- Water bodies showed high classification accuracy (F1 = 0.94) due to strong NDWI signal

---

## Links

[View Code on GitHub](https://github.com/[YOUR-GITHUB-USERNAME]/[YOUR-REPO-NAME]){ .md-button }
[Data Source (USGS)](https://earthexplorer.usgs.gov){ .md-button }
