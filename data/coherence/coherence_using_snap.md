Coherence Calculation using ESA SNAP - a short guide-line for a possible workflow
---

1. **Coregistration** (S1 TOPS Coregistration):
   First under “Open Product” select the two SLC products as inputs to your interferogram (download e.g. via Copernicus Browser)
   Next, in the product explorer select those two products, then “Radar/Coregistration/S1 TOPS Coregistration/S1 TOPS Coregistration”. In the window that appears,
   ensure the first product is listed under Name in the “Read” tab, and the second in the “Read(2)” tab. In the “TOPSAR-Split” and “TOPSAR-Split(2)” tabs select
   the desired IW subswath and the polarization. Under the tab “Back-Geocoding” check “Output Deramp and Demod Phase” to enable corrections of TOPSAR artefacts.
   Under the “Write” tab, give a name with the dates of your acquisitions. Finally, click “Run”. This creates a registration model connecting the two input products.
2. **Interferogram formation** (using SRTM 3sec DEM to flatten the raw interferogram fringes):
   Select the stack product you generated via coregistration, then “Radar/Interferometric/Products/Interferogram Formation”. In the “Processing Parameters” tab,
   check “Subtract topographic phase”. This ensures that the DEM is used to flatten the raw interferogram fringes rather than a flat earth model.
3. **Goldstein phase filtering**:
   Select “Radar/Interferometric/Filtering/Goldstein Phase Filtering” to generate smoother phase estimates. Accept the default processing parameters and click
   “Run”. The output is a _flt product (filtered).
4. **Debursting**:
   Select “Radar/Sentinel-1 TOPS/S-1 TOPS Deburst”, to generate an image without disjoint azimuth timeline. Accept the default processing parameters and click
   “Run”. The output is a _deb product (debursted).
5. **Geocoding/terrain correction** (from slant range to WGS84 geographic coordinates):
   Select your filtered & debursted phase from the output of the step above, then: Radar/Geometric/Terrain Correction/Range Doppler Terrain Correction. under
   Processing Parameters, highlight the coh_ band. Ensure that under “Image Resampling Method” that “NEAREST_NEIGHBOUR” is selected. With all other parameters
   set to the defaults, press Run. The output is a _TC product (terrain corrected).
6. **Export coherence as GeoTiff and import to GEE for classification**
---

**InSAR Coherence – Explanation and Use:**

InSAR coherence is a measure of how similar the radar signal is between two SAR acquisitions. It ranges from 0 (completely decorrelated) to 1 (perfectly 
correlated) and reflects how consistent the scattering properties of the surface are over time.

Coherence is used to assess data quality and surface stability. High coherence suggests stable surfaces (e.g., bedrock), while low coherence indicates 
changes (e.g., vegetation growth, snow, or glacier movement).

The low coherence over the glacier compared to its surroundings indicates rapid surface changes (like ice flow or melting), which disrupt the radar 
signal consistency. This contrast helps to identify dynamically changing areas like fast-flowing glacier zones. For this work, Sentinel-1 scenes with a 
12-days interval were used. 

InSAR Limitations:
* Low sensitivity to north–south (along-track) displacement.
* Limited ability to detect large displacements between acquisitions.
* Decorrelation due to surface changes or unfavorable imaging geometry.
* Phase unwrapping errors, especially in areas with high deformation rates or steep gradients.
* Atmospheric effects, particularly tropospheric phase delay.
* Uncertainty from coherence estimation, influenced by differing weather, surface conditions, or data sources.
