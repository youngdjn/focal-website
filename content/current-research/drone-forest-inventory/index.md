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

gallery_item:
- album: photogrammetry
  image: 1-dsm.png
  caption: Digital surface model
- album: photogrammetry
  image: 2-chm.png
  caption: Canopy height model
- album: photogrammetry
  image: 3-orthomosaic.png
  caption: Orthomosaic
- album: photogrammetry
  image: 4-stem-map.png
  caption: Treetops, with point size proportional to tree height, overlaid on orthomosaic

---

[<i class="fas fa-circle-exclamation"></i> Call for collaborators](#collaborators)

Our tree mapping work directly supports the [<i class="fas fa-arrow-up-right-from-square"></i> Open Forest Observatory](http://openforestobservatory.org).

We are developing an open-source, automated workflow for producing high-accuracy, spatially extensive (10-300 ha) maps of forest stands at the individual tree level using drone imagery. These maps have many potential applications, including planning post-fire reforestation treatments, develping thinning and prescribed fire prescriptions, and inventorying carbon stocks. Our workflow enables mapping of > 300 ha of forest area in only 5 days of a single technician's time (including collection of drone imagery using a single consumer drone and imagery processing). The workflow has several steps:

### 1. Image collection and photogrammetry

We collect many partially overlapping drone photos across the site. This means any given tree appears in many photos, each from a slightly different angle.

{{< figure library="true" src="photogrammetry.jpg" title="Example of an individual tree viewed from multiple angles in separate drone images." lightbox="true" >}}

Because each tree is visible from multiple angles, it is possible to use methods related to stereo vision (more generally known as *photogrammetry*) to estimate each tree's 3D structure. The photogrammetry algorithm produces a 3D cloud of points that is very similar to lidar data.

{{< figure library="true" src="lidar.PNG" title="Visualization of a 3D point cloud from our Emerald Point site produced via photogrammetry." lightbox="true" >}}

The photogrammetry algorithm is also used to produce an orthomosaic, which is a high-resolution aerial image produced by stitching together the numerous drone photos. It resembles NAIP or Google Earth imagery but is generally much higher resolution (~ 3 cm).

Photogrammetric processing requires parameterization for the specific application. We have thoroughly tested parameterizations for individual tree detection in structurally complex conifer forests and published our results, including recommended parameter values, in [<i class="far fa-file-lines"></i> Methods in Ecology and Evolution](https://besjournals.onlinelibrary.wiley.com/doi/10.1111/2041-210X.13860). We have also built and continue to maintain a [<i class="fab fa-github"></i> software library](https://github.com/ucdavis/metashape) that makes it easy to run multiple photogrammetry workflows with pre-specified parameterizations in a documented, reproducible way.

### 2. Canopy height model (CHM) production

After creation of a point cloud, we process it to compute a digital surface model (DSM). The DSM is a high-resolution (~10-cm) raster indicating the elevation of the vegetation (or ground) surface in each pixel. Then, we subtract elevation values from a high-resolution digital elevation model (DEM). The result is a canopy height model (CHM) indicating the height of the vegetation above the ground in each pixel.

{{< gallery album="photogrammetry" >}}


### 3. Tree detection

Next, using an algorithm such as the variable-window filter implemented in the [ForestTools](https://cran.r-project.org/web/packages/ForestTools/) R package, we detect individual tree tops, and their associated heights, from the canopy height model. The selection and parameterization of the optimal tree detection algorithm is a complex process and can depend on the approach used for drone imagery collection and processing. We have evaluated the interactions between these choices and identified an optimal tree detection method and parameterization. The results are published in [<i class="far fa-file-lines"></i> Methods in Ecology and Evolution](https://besjournals.onlinelibrary.wiley.com/doi/10.1111/2041-210X.13860).

### 4. Tree species identification

After identifying the locations and sizes of individual trees, the next step is to identify each tree taxonomically. We are building an AI-based computer vision approach to classify the drone-derived images of each tree into species categories. We take advantage of the fact that each tree appears in dozens to hundreds of individual drone images, allowing us to use the multiple viewing angles to more confidently identify the tree to species. This approach is known as multi-view computer vision.

{{< figure src="multiview-species-id.png" width="500px" caption="Workflow for programmatically isolating images of individual detected trees for AI-based species identification. (1) The tree is detected using the CHM, orthomosaic, and/or point cloud. (2) The top and bottom of the tree are projected from 3D space onto the drone photo. (3) The tree top and bottom are used to create a bounding box to crop the single tree from the image. (4) The process is repeated for every drone image in which the focal tree appears. The resulting images are supplied to the computer vision image classification algorithm." >}}

## Call for collaborators <a name="collaborators"></a>

We are keen to collaborate with groups or individuals to develop forest mapping methods and tools. In particular, we are interested incorporating any large (> 0.25 ha) field-based forest inventory plot data for development, testing, and training of our forest mapping workflows. If you are interested in collaborating, [contact us](/#contact) or visit the [<i class="fas fa-arrow-up-right-from-square"></i> Open Forest Observatory](http://openforestobservatory.org) for more details on the effort.


## Funding and support

<div class="container text-center align-content-middle">
  <div class="row">
    <div class="col-sm">
{{< figure src="cal_fire_logo_small.png" width="120px" >}}
    </div>
    <div class="col-sm">
{{< figure src="cci_logo_small.png" width="200px" >}}
    </div>
    <div class="col-sm">
{{< figure src="NSF_4-Color_bitmap_Logo.png" width="200px" >}}
    </div>
    <div class="col-sm">
{{< figure src="the-nature-conservancy-vector-logo.png" width="200px" >}}
    </div>
  </div>

</div>