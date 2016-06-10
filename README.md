Neuroglancer is a WebGL-based viewer for volumetric data.  It is capable of displaying arbitrary (non axis-aligned) cross-sectional views of volumetric data, as well as 3-D meshes and line-segment based models (skeletons).

This is not an official Google product.

# Examples

A live demo is hosted at <https://neuroglancer-demo.appspot.com>.  (The prior link opens the viewer without any preloaded dataset.)  Use the viewer links below to open the viewer preloaded with an example dataset.

The four-pane view consists of 3 orthogonal cross-sectional views as well as a 3-D view (with independent orientation) that displays 3-D models (if available) for the selected objects.  All four views maintain the same center position.  The orientation of the 3 cross-sectional views can also be adjusted, although they maintain a fixed orientation relative to each other.  (Try holding the shift key and either dragging with the left mouse button or pressing an arrow key.)

- Kasthuri et al., 2014.  Mouse somatosensory cortex (6x6x30 cubic nanometer resolution). <a href="https://neuroglancer-demo.appspot.com/#!{'layers':{'original-image':{'type':'image'_'source':'precomputed://gs://neuroglancer-public-data/kasthuri2011/image'_'visible':false}_'corrected-image':{'type':'image'_'source':'precomputed://gs://neuroglancer-public-data/kasthuri2011/image_color_corrected'}_'ground_truth':{'type':'segmentation'_'source':'precomputed://gs://neuroglancer-public-data/kasthuri2011/ground_truth'_'selectedAlpha':0.63_'notSelectedAlpha':0.14_'segments':['3208'_'4901'_'13'_'4965'_'4651'_'2282'_'3189'_'3758'_'15'_'4027'_'3228'_'444'_'3207'_'3224'_'3710']}}_'navigation':{'pose':{'position':{'voxelSize':[6_6_30]_'voxelCoordinates':[5523.99072265625_8538.9384765625_1198.0423583984375]}}_'zoomFactor':22.573112129999547}_'perspectiveOrientation':[-0.004047565162181854_-0.9566211104393005_-0.2268827110528946_-0.1827099621295929]_'perspectiveZoom':340.35867907175077}" target="_blank">Open viewer.</a>

  This dataset was copied from <http://openconnecto.me/Kasthurietal2014> and is made available under the [Open Data Common Attribution License](http://opendatacommons.org/licenses/by/1.0/).  Paper: <a href="http://dx.doi.org/10.1016/j.cell.2015.06.054" target="_blank">Kasthuri, Narayanan, et al.  "Saturated reconstruction of a volume of neocortex." Cell 162.3 (2015): 648-661.</a>
  
- Janelia FlyEM FIB-25.  7-column Drosophila medulla (8x8x8 cubic nanometer resolution).  <a href="https://neuroglancer-demo.appspot.com/#!{'layers':{'image':{'type':'image'_'source':'precomputed://gs://neuroglancer-public-data/flyem_fib-25/image'}_'ground-truth':{'type':'segmentation'_'source':'precomputed://gs://neuroglancer-public-data/flyem_fib-25/ground_truth'_'segments':['21894'_'22060'_'158571'_'24436'_'2515']}}_'navigation':{'pose':{'position':{'voxelSize':[8_8_8]_'voxelCoordinates':[2914.500732421875_3088.243408203125_4045]}}_'zoomFactor':30.09748283999932}_'perspectiveOrientation':[0.3143535554409027_0.8142156600952148_0.4843369424343109_-0.06040262430906296]_'perspectiveZoom':443.63404517712684_'showSlices':false}" target="_blank">Open viewer.</a>

  This dataset was copied from <https://www.janelia.org/project-team/flyem/data-and-software-release>, and is made available under the [Open Data Common Attribution License](http://opendatacommons.org/licenses/by/1.0/).  Paper: <a href="http://dx.doi.org/10.1073/pnas.1509820112" target="_blank">Takemura, Shin-ya et al. "Synaptic Circuits and Their Variations within Different Columns in the Visual System of Drosophila."  Proceedings of the National Academy of Sciences of the United States of America 112.44 (2015): 13711-13716.</a>
  
# Supported data sources

Neuroglancer itself is purely a client-side program, but it depends on data being accessible via HTTP in a suitable format.  It is designed to easily support many different data sources, and there is existing support for the following data APIs/formats:

- NDstore/Open Connectome <https://github.com/neurodata/ndstore>
- DVID <https://github.com/janelia-flyem/dvid>
- Precomputed chunk/mesh fragments exposed over HTTP

# Supported browsers

- Chrome >= 51
- Firefox >= 46

# Key bindings

See [src/main.ts](/src/main.ts).

# Mouse bindings

- Click on a layer name to toggle its visibility.

- Double-click on a layer name to edit its properties.

- Left-drag within a slice view to move within that plane.

- Shift-left-drag within a slice view to change the orientation of the slice views.

- Left-drag within the 3-d view to change the orientation.

- Right click to move to the position under the mouse pointer.

- Double click to toggle showing the object under the mouse pointer.

- Hover over a segmentation layer name to see the current list of objects shown.

# Troubleshooting

- Neuroglancer doesn't appear to load properly.

  Neuroglancer requires WebGL (1.0) and the `WEBGL_draw_buffers`, `OES_texture_float`, and `OES_element_index_uint` extensions.
  
  To troubleshoot, check the developer console, which is accessed by the keyboard shortcut `control-shift-i` in Firefox and Chrome.  If there is a message regarding failure to initialize WebGL, you can take the following steps:
  
  - Chrome
  
    Check `chrome://gpu` to see if your GPU is blacklisted.  There may be a flag you can enable to make it work.
    
  - Firefox

    Check `about:support`.  There may be webgl-related properties in `about:config` that you can change to make it work.  Possible settings:
    - `webgl.disable-fail-if-major-performance-caveat = true`
    - `webgl.force-enabled = true`
    - `webgl.msaa-force = true`
    
- Failure to access a data source.

  As a security measure, browsers will in many prevent a webpage from accessing the true error code associated with a failed HTTP request.  It is therefore often necessary to check the developer tools to see the true cause of any HTTP request error.

  There are several likely causes:
  
  - [Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)
  
    Neuroglancer relies on cross-origin requests to retrieve data from third-party servers.  As a security measure, if an appropriate `Access-Control-Allow-Origin` response header is not sent by the server, browsers prevent webpages from accessing any information about the response from a cross-origin request.  In order to make the data accessible to Neuroglancer, you may need to change the cross-origin request sharing (CORS) configuration of the HTTP server.
  
  - Accessing an `http://` resource from a Neuroglancer client hosted at an `https://` URL
    
    As a security measure, recent versions of Chrome and Firefox prohibit webpages hosted at `https://` URLs from issuing requests to `http://` URLs.  As a workaround, you can use a Neuroglancer client hosted at a `http://` URL, e.g. the demo client running at http://neuroglancer-demo.appspot.com, or one running on localhost.  Alternatively, you can start Chrome with the `--disable-web-security` flag, but that should be done only with extreme caution.  (Make sure to use a separate profile, and do not access any untrusted webpages when running with that flag enabled.)
    
# Multi-threaded architecture

In order to maintain a responsive UI and data display even during rapid navigation, work is split between the main UI thread (referred to as the "frontend") and a separate WebWorker thread (referred to as the "backend").  This introduces some complexity due to the fact that current browsers:
 - do not support any form of **shared** memory or standard synchronization mechanism (although they do support relative efficient **transfers** of typed arrays between threads);
 - require that all manipulation of the DOM and the WebGL context happens on the main UI thread.

The "frontend" UI thread handles user actions and rendering, while the "backend" WebWorker thread handle all queuing, downloading, and preprocessing of data needed for rendering.

# Building

node.js is required to build the viewer.

1. First install NVM (node version manager) per the instructions here:

  https://github.com/creationix/nvm

2. Install a version of Node.js >= v5.9.0:

    `nvm install <version>`
    
    `<version>` could be 6.1.0 for example.
    
    Use `nvm ls-remote` to see available versions.

3. Install the dependencies required by this project:

   (From within this directory)

   `npm i`

4. To run a local server for development purposes:

   `npm run dev-server`
  
   This will start a server on <http://localhost:8080>.
   
5. To run the unit test suite on Chrome:

   `npm test`
   
6. See [package.json](/package.json) for other commands available.

# Creating a dependent project

See [examples/dependent-project](/examples/dependent-project).

# License

Copyright 2016 Google Inc.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this software except in compliance with the License.
You may obtain a copy of the License at <http://www.apache.org/licenses/LICENSE-2.0>.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.