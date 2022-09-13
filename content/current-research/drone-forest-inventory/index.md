---
title: Drone-based forest mapping
summary: "Developing methods to map forests at the individual tree level using drones, photogrammetry, and computer vision"

date: 2022-01-01
show_date: false
profile: false
image:
  focal_point: ''
  preview_only: true
  # caption: Automatically detected treetops, with point size indicating tree height, overlaid on drone-derived orthoimagery from the Tahoe National Forest

banner:
  caption: "Emerald Point, Lake Tahoe, USA"
  image: "tahoe.jpg"

---

We are developing an automated workflow for producing high-accuracy, spatially extensive (10-300 ha), species-specific stem maps of forest stands using drone imagery. Such maps have many potential applications: planning post-fire reforestation treatments, develping thinning and prescribed fire prescriptions, inventorying carbon stocks, and more. At its current stage, our workflow enables mapping of > 300 ha of contiguous area in only 5 days of a single technician's time (including collection of drone imagery using a single consumer drone and imagery processing). The workflow has several steps:

### 1. Image collection and photogrammetry

We collect many partially overlapping drone photos across the site. This means any given tree appears in many photos, each from a slightly different angle.

{{< figure library="true" src="photogrammetry.jpg" title="Example of an individual tree viewed from multiple angles in separate drone images." lightbox="true" >}}

Because each tree is visible from multiple angles, it is possible to use methodologies related to stereo vision (more generally known as *photogrammetry*) to estimate each tree's 3D structure. The photogrammetry algorithm produces a 3D cloud of points that is very similar to lidar data.

{{< figure library="true" src="lidar.PNG" title="Visualization of a 3D point cloud from our Emerald Point site produced via photogrammetry." lightbox="true" >}}

{{< gallery album="photogrammetry" >}}

The primary photogrammetry software we use is [Metashape](https://www.agisoft.com/). Photogrammetry is very computationally-intensive, especially when processing large projects consisting of thousands of high-resoluation photos. Processing a single 100-ha area can take over a day on a standard PC. We am collaborating with several researchers at UC Davis to develop a Python library to allow for efficiently running multiple photogrammetry projects in parallel on a high-performance computing cluster at UC Davis.

View our [<i class="fab fa-github"></i>&nbsp;reproducible photogrammetry workflows software repository](https://github.com/ucdavis/metashape).


### 2. Tree detection

After producing a 3D point cloud from drone images, we run an algorithm to detect individual tree canopies based on their shapes. First, we compute a digital surface model from the point cloud data. This process simply involves identifying the highest-elevation point in each grid cell across a grid of a given resolution. We use the [lidR](https://cran.r-project.org/web/packages/lidR/index.html) package in R for this purpose.

Next, to remove the influence of underlying topography, we subtract from the digital surface model elevations the elevations from a high-resolution (10 m) USGS digital elevation model (DEM). The result is a canopy height model.

{{< figure library="true" src="chm.jpg" title="A canopy height model of a portion of our Emerald Point site. The scale on the right indicates canopy height in meters." lightbox="true" >}}

Next, we use a varible-window filter algorithm (implemented in the [ForestTools](https://cran.r-project.org/web/packages/ForestTools/) R package) to detect individual tree canopies from the canopy height model.

{{< figure library="true" src="tree_detection.jpg" title="A *preliminary* stem map of a portion of our Emerald Point site produced using a variable-window filter applied to a canopy height model. Lighter-color points reflect taller trees. This figure depicts the results of canopy detection prior to any substantial parameter tuning." lightbox="true" >}}

The final step is to identify individual trees to species. We are developing a workflow to use the photos of each tree as input to a convolutional neural network (CNN) computer vision model for identification of trees to species.

A core collaborator on this project is [Andrew Latimer](https://www.plantsciences.ucdavis.edu/people/andrew-latimer).

This project is supported by:

{{< figure library="true" src="cal_fire_logo_small.png" lightbox="true" >}}{{< figure library="true" src="cci_logo_small.png" lightbox="true" >}}