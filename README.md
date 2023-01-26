# Global glacier ice volume - Code & Data

Code & Data for *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2023: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.*

Release: v1.1 (2022-12-29)

## Licence 

The code is released under BSD3. It's all coded in python 3. You'll need to install pandas, geopandas, rioxarray, xarray, matplotlib, and seaborn to run the notebooks.

The data is licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Some of the data given in the the [data](data) folder are taken from previous publications. They are included in table format here (`.csv`) since they provided input to our calculations or were used in the figures. If you use these data, please refer to the original studies: Millan et al., 2022; Farinotti et al., 2019; Radić et al., 2014; Grinsted, 2013; Huss and Farinotti, 2012; Marzeion et al., 2012; Radić and Hock, 2010; Raper and Braithwaite 2005; Dyurgerov and Meier, 2005; Ohmura, 2004; Meier and Bahr, 1996.

**If you use the plots in the [figures](figures) folder or the data in the [tables_regional_glacier_volume](tables_regional_glacier_volume) folder, please refer to** *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2023: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.* 

## Note on the publication of a correction by Millan et al. in December 2022

On December 12, 2022, Millan et al. [published a correction](https://www.nature.com/articles/s41561-022-01106-x) to their original paper. **Our code repository takes this correction into account, and so do our figures and tables**. The correction addressed inconsistencies between the data provided by Millan et al. and the numbers reported in their paper. As a result, our recalculations of regional volumes are now identical to the numbers reported in the Millan et al. paper (with the exception of region 03, see below).

## Code

See the [code](code) folder for the notebooks producing figures and tables.

## Figures

Used in the paper:
- [Figure 1: maps](figures/plot_maps_bright.png)
- [Figure 2: volume comparison](figures/plot_global_and_reg_log.png) ([pdf](figures/plot_global_and_reg_log.pdf))

## Corrected regional and global volumes tables

In the [tables_regional_glacier_volume](tables_regional_glacier_volume) folder, you will find three CSV files. The main purpose of these files is to provide corrected regional volume estimates to account for incomplete spatial coverage of the velocity and thickness data in Millan et al., (2022). See the Supplementary Material in Hock et al., (2023) for details.

For all decriptions below, the "original M22 dataset" refers to the dataset as downloaded from [sedoo.fr](https://www.sedoo.fr/theia-publication-products/?uuid=55acbdd5-3982-4eac-89b2-46703557938c) as of 2022-07-20. This dataset is used by M22 for their computations, with the exception of RGI region 03, where their repository changelog says: *"Concerning the ice thickness for region RGI-3 (Arctic Canada North), we noticed artefacts at some of marine glacier termini, with abnormally high thickness values. For the sake of constant improvement of the data set, we decided to follow a stronger outlier filtering procedure and updated the thickness layer"*. Our table values therefore are slightly different than reported in M22 (they report 25.4, we compute 25.3 10³ km³).

### Glacier per glacier volume

[all_glaciers_m22_f19.csv](tables_regional_glacier_volume/all_glaciers_m22_f19.csv): volume estimates **for each individual glacier** recalculated from the original ice thickness data given by [Millan et al. (2022)](https://www.nature.com/articles/s41561-021-00885-z) (M22) and by [Farinotti et al. (2019)](https://www.nature.com/articles/s41561-019-0300-3) (F19). The original M22 thickness data are available from [sedoo.fr](https://www.sedoo.fr/theia-publication-products/?uuid=55acbdd5-3982-4eac-89b2-46703557938c), the original F19 data from [research-collection.ethz.ch](https://www.research-collection.ethz.ch/handle/20.500.11850/315707). To derive the M22 estimates, reprojection and subsetting of the original M22 data files was necessary - we did this using the OGGM framework ([Maussion et al., 2019](https://gmd.copernicus.org/articles/12/909/2019/)).

Details of the columns:
- `rgi_id`: unique glacier identifier from the Randolf Glacier Inventory (RGI, version 6.0)
- `rgi_area_km2`: area of the glacier as provided by the RGI 6.0. 
- `millan_vol_km3`: volume of the glacier as computed from the original M22 dataset reprojected and cropped for each glacier (the original dataset does not provide glacier per glacier estimates, only regional files). 
- `millan_area_km2`: area of the glacier as computed from the original M22 dataset. The differences to RGI6 originate from: (1) the raster representation at 50 m resolution in the M22 data which results in different areas than the vector RGI outlines, (2) map projection differences and (3) incomplete data coverage in the M22 dataset. Points 1 and 2 lead to small differences and are not cause for concern. Point 3 requires a correction for some regions, which is our main objective here.
- `millan_perc_cov`: percentage of the glacier area covered by the ice thickness dataset in M22 (recalculated in this study: column `millan_area_km2`) relative to area in the RGI.
- `f19_vol_km3`: volume of the glacier as recalculated from the original F19 dataset.
- `millan_vol_adj`: volume of the glacier as recalculated from the original M22 dataset, but adjusted for regional undercatch (pixels in the original M22 dataset non-attributable to a specific glacier because of reprojection issues). Since this regional undercatch is not attributable to single glaciers, we compute the ratio of the total regional volume in the original M22 files to the regional sum of individual glaciers (`millan_vol_km3`), yielding regional correction factors of 2.5 ± 1.5% depending on the region. This regional correction factor is applied to all glaciers in a region, ensuring that our regional totals of glacier per glacier volumes are uequivalent to the regional volume reported by M22.
- `vas_millan_vol`: volume as computed by volume area scaling (Eq. S1 in Hock et al., 2023), with the RGI area as reference. Scaling parameters are computed for each RGI region separately. They are fitted regionally to the glacier per glacier M22 dataset (`millan_vol_adj` column) for all glaciers with >95% area coverage (see Hock et al., 2013 for details and the regional tables below for the parameter values).

### Corrected regional volumes

[volume_table_corrected.csv](tables_regional_glacier_volume/volume_table_corrected.csv): recalcuated and upscaled regional glacier volume based on M22 compared to original M22 and F19 data, as well as additional data used in the re-calculation. See Hock et al. (2023) Supplementary Material for a full description and the code in [millan_regional_volume_scaling.ipynb](code/millan_regional_volume_scaling.ipynb) for implementation details.

[volume_table_corrected_rounded.csv](tables_regional_glacier_volume/volume_table_corrected_rounded.csv): same as [volume_table_corrected.csv](tables_regional_glacier_volume/volume_table_corrected.csv) but rounded for readability.

Details of the columns:

0. `RGI Region`: Region number of the Randolf Glacier Inventory (RGI, version 6.0). M22 added some RGI regions together - we do the same for consistency.
1. `RGI area (km²)`: total glacier area in the region as provided by the RGI 6.0 dataset.
2. `M22 area uncorrected (km²)`: regional glacier area covered by ice volume estimates in M22; recalculated in this study based on the original gridded dataset provided by M22 (note that in some cases the area differs from those reported in M22, Table 1, because M22 reports velocity data coverage, not thickness data).
3. `M22 coverage (%)`: ratio of of glacier area covered by M22 (recalculated in this study, column 2) to RGI area (column 1).
4. `M22 volume uncorrected (km³)`: regional glacier volume recalculated in this study based on the original gridded dataset provided by M22 (volume refers to areas in column 2).
5. `M22 volume as reported (km³)`: regional volume as reported in the original M22 study. Should be similar to column 4: differences arise from rounding in M22 (except for Region 03, as explained above).
6. `F19 volume as computed (km³)`: regional glacier volume recalculated in this study based on the original gridded dataset provided by F19. We recompute them here for consistency with the methodology applied to the original M22 dataset and to avoid using the rounded values from the published table in F19.
7. `F19 volume as reported (km³)`: regional volume as reported in the original F19 study. Should be similar to column 6: differences arise from rounding errors and floating point accuracy.
8. `M22-F19 volume difference uncorrected (%)`: (column 4 - column 6) / (column 6) * 100. This is equivalent to the differences reported by M22 in their paper (column 18), based on the original M22 data with incomplete coverage in several regions.
9. `M22 volume on subset (km³)`: "subset" refers to a subset of glaciers in this region with M22 ice thickness data coverage > 95%. The volume is recalculated as in column 4, but only for this subset.
10. `F19 volume on subset (km³)`: glacier volume from F19 on the same subset as column 9. Columns 9 and 10 allow a "true" comparison, comparing the two datasets over a domain where both dataset have a good coverage (> 95%).
11. `M22-F19 volume difference on subset (%)`: (column 9 - column 10) / (column 10) * 100. Fair comparison of the two datasets.
12. `VAS parameter c`: the regional parameter `c` in the volume area scaling `V` = `c A^gamma`. Fitted on M22 data processed by us over all glaciers in the subset.
13. `VAS parameter gamma`: the regional parameter `gamma` in the volume area scaling `V` = `c A^gamma`. Fitted on M22 data processed by us over all glaciers in the subset.
14. `M22 volume corrected method 1 (km³)`: regional glacier volume by M22 (column 9) upscaled to the total RGI area using method 1 (V-A scaling) and the parameters in columns 12 and 13.  See Hock et al., (2023) for details.
15. `M22 volume corrected method 2 (km³)`: regional glacier volume (column 9) upscaled to the RGI area using method 2 (based on F19 data). See Hock et al., (2023) for details.
16. `M22-F19 volume difference method 1 (%)`: (column 14 - column 6) / (column 6) * 100.
17. `M22-F19 volume difference method 2 (%)`: (column 15 - column 6) / (column 6) * 100. By design, this should be the same as column 11 and is used for verification.
18. `M22-F19 volume difference as reported (%)`: same as columns 16 and 17 but as reported in M22, Table 1.

Note that for RGI Region 19 we use the values as reported by M22 without any correction. The original M22 dataset does not provide thickness values for much of the glaciated area in this region (they rely on BedMachine v.1; Morlighem et al., 2019). Furthermore, they report a coverage of 100% which makes the regional upscaling obsolete.

## Data folder

Files used as input to our calculations or for the figures. 

- `.csv` files: table data from the original studies. If you use these data, please refer to the original studies: Millan et al., 2022; Farinotti et al., 2019; Radić et al., 2014; Grinsted, 2013; Huss and Farinotti, 2012; Marzeion et al., 2012; Radić and Hock, 2010; Raper and Braithwaite 2005; Dyurgerov and Meier, 2005; Ohmura, 2004; Meier and Bahr, 1996.
- other files need to be downloaded and are indicated in the respective code notebooks.

## Changelog

- v1.0 (2022-12-29): initial release
- v1.1 (2022-12-29): removed large input files to reduce repository size (they can be downloaded)

## DOI

Permanent Zenodo link: https://doi.org/10.5281/zenodo.7492034

## References

- Dyurgerov MB and Meier MF (2005) Glaciers and the changing Earth system: a 2004 snapshot (Vol. 58,). Boulder: Institute of Arctic and Alpine Research, University of Colorado.
- Farinotti, D., Huss, M., Fürst, J. J., Landmann, J., Machguth, H., Maussion, F. and Pandit, A. (2019): A consensus estimate for the ice thickness distribution of all glaciers on Earth, Nat. Geosci., 12(3), 168–173, https://doi.org/10.1038/s41561-019-0300-3
- Grinsted A (2013) An estimate of global glacier volume. The Cryosphere 7(1), 141-151. https://doi.org/10.5194/tc-7-141-2013 
- Huss M and Farinotti D (2012) Distributed ice thickness and volume of all glaciers around the globe. Journal of Geophysical Research: Earth Surface 117(F4).  https://doi.org/10.1029/2012JF002523
- Maussion, F., Butenko, A., Champollion, N., Dusch, M., Eis, J., Fourteau, K., Gregor, P., Jarosch, A. H., Landmann, J., Oesterle, F., Recinos, B., Rothenpieler, T., Vlug, A., Wild, C. T. and Marzeion, B. (2019): The Open Global Glacier Model (OGGM) v1.1, Geosci. Model Dev., 12(3), 909–931, https://doi.org/10.5194/gmd-12-909-2019.
- Marzeion B, Jarosch AH and Hofer M (2012) Past and future sea-level change from the surface mass balance of glaciers. The Cryosphere 6(6), 1295-1322. https://doi.org/10.5194/tc-6-1295-2012 
- Meier MF and Bahr DB (1996) Counting Glaciers: Use of Scaling Methods to Estimate the Number and Size Distribution of the Glaciers of the World,. In Colbeck SC (ed.) Glaciers, ice sheets and volcanoes: a tribute to Mark F. Meier,. U.S. Army Corps of Engineers Cold Regions Research & Engineering Laboratory, Special Report 96(27), 89-94.
- Millan R, Mouginot J, Rabatel A and Morlighem M. (2022) Ice velocity and thickness of the world’s glaciers. Nature Geoscience, 15(2), 124-129. https://doi.org/10.1038/s41561-021-00885-z.
- Millan R, Mouginot J, Rabatel A and Morlighem M. (2022) Author Correction: Ice velocity and thickness of the world’s glaciers. Nature Geoscience https://doi.org/10.1038/s41561-022-01106-x.
- Ohmura A (2004) Cryosphere during the twentieth century. In Sparling JY and Hawkesworth CJ, eds. The state of the planet: frontiers and challenges in geophysics. Washington DC, American Geophysical Union, 239–257. https://doi.org/10.1029/150GM19.
- Radić V and Hock R (2010) Regional and global volumes of glaciers derived from statistical upscaling of glacier inventory data. Journal of Geophysical Research: Earth Surface, 115(F1), https://doi.org/10.1029/2009JF001373.
- Radić V, Bliss A, Beedlow AC, Hock R, Miles E. and Cogley JG (2014) Regional and global projections of twenty-first century glacier mass changes in response to climate scenarios from global climate models. Climate Dynamics 42(1), 37-58. https://doi.org/10.1007/s00382-013-1719-7 
- Raper SC and Braithwaite RJ (2005) The potential for sea level rise: New estimates from glacier and ice cap area and volume distributions. Geophysical Research Letters 32(5). https://doi.org/10.1029/2004GL021981
- RGI Consortium, 2017. Randolph Glacier Inventory - A Dataset of Global Glacier Outlines, Version 6. NSIDC: National Snow and Ice Data Center, Boulder, Colorado USA. doi: https://doi.org/10.7265/4m1f-gd79.
