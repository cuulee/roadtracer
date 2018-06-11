RoadTracer Code
===============

This is the code for ["RoadTracer: Automatic Extraction of Road Networks from Aerial Images"](https://roadmaps.csail.mit.edu/roadtracer/).

There are several components, and each folder has a README with more usage details:

* dataset: code for dataset preparation
* roadtracer: RoadTracer
* roadcnn: our segmentation approach (baseline)
* deeproadmapper: DeepRoadMapper (baseline)

You will need gomapinfer (https://github.com/mitroadmaps/gomapinfer/) as a dependency.

The training/inference code is built on top of TensorFlow.

Applying RoadTracer on a new region
-----------------------------------

First, we need to download the imagery. Update `dataset/lib/regions.go` and put a latitude/longitude bounding box around your region in the regionMap. You can comment out the existing regions. Then, follow instructions in dataset/ for running `1_sat.go`.

Then, update `roadtracer/tileloader.py` and set `TRAINING_REGIONS` and `REGIONS` to just a list with your region label from the regionMap. Also update `REGION`, `TILE_START`, and `TILE_END` in `infer.py` (e.g. if your imagery tiles were saved as `xyz_-1_-1_sat.png` through `xyz_1_1_sat.png`, set `TILE_START = -1, -1` and `TILE_END = 2, 2`.

Finally, manually specify a starting location for RoadTracer in infer.py. Set `USE_TL_LOCATIONS = False`, `MANUAL_RELATIVE = 0, 0`, and set the manual points to the pixel positions of two points on the road network in `xyz_0_0_sat.png`. These two points should be close to each other (around SEGMENT_LENGTH apart) and best to be in the middle of a road.

Now you should be able to get a road network graph by running infer.py.

To convert this to latitude/longitude, you can use `dataset/convertarg.go`:

	go run convertarg.go YOUR_REGION_LABEL frompix out.graph out.lonlat.graph
