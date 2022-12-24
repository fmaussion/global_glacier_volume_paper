# Global glacier volume Code & Data

Code & Data for *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2022: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.*

## Licence 

The code is released under BSD3. It's all coded in python 3. You'll need pandas, geopandas, rioxarray, xarray, matplotlib, and seaborn to run the notebooks.

The data is licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Some of the data given in the the data folder are taken from previous publications. They are included in table format here (`.csv`) since they provided input to our calculations or were used in the figures. If you use these data, please refer to the original studies: Millan et al., 2022; Farinotti et al., 2019; Radić et al., 2014; Grinsted, 2013; Huss and Farinotti, 2012; Marzeion et al., 2012; Radić and Hock, 2010; Raper and Braithwaite 2005; Dyurgerov and Meier, 2005; Ohmura, 2004; Meier and Bahr, 1996.

**If you use the plots in the `figures` folder or the data in the `tables_regional_correction` folder, please refer to** *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2022: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.* 

## Code

See the [code](code) folder for the notebooks producing figures and tables.

## Figures

Used in the paper:
- [Figure 1: maps](figures/plot_maps_bright.png)
- [Figure 2: volume comparison](figures/plot_global_and_reg_log.png) ([pdf](figures/plot_global_and_reg_log.pdf))

## Corrected regional and global volumes

In the [tables_regional_glacier_volume](tables_regional_glacier_volume) folder, you will find three CSV files.

### Glacier per glacier volume

[all_glaciers_m22_f19.csv](tables_regional_glacier_volume/all_glaciers_m22_f19.csv): volume estimates for each individual glacier recalculated from the original ice thickness data given by [Millan et al. (2022)](https://www.nature.com/articles/s41561-021-00885-z) (M22) and provided by [Farinotti et al. (2019)](https://www.nature.com/articles/s41561-019-0300-3) (F19). To derive the M22 estimates, reprojection and subsetting of the original M22 data files was necessary - we did this with the OGGM framework ([Maussion et al., 2019](https://gmd.copernicus.org/articles/12/909/2019/)).

The original M22 thickness data are available from [this website](https://www.sedoo.fr/theia-publication-products/?uuid=55acbdd5-3982-4eac-89b2-46703557938c), the F19 data from [this one](https://www.research-collection.ethz.ch/handle/20.500.11850/315707).

Details of the columns:
- `rgi_id`: unique glacier identifier from the Randolf Glacier Inventory (RGI, version 6.0)
- `rgi_area_km2`: area of the glacier as provided by the RGI 6.0. 
- `millan_vol_km3`: volume of the glacier as computed from the original M22 dataset
- `millan_area_km2`: area of the glacier as computed from the original M22 dataset (the differences to RGI6 originate from the raster representation and incomplete data coverage in M22) 
- `millan_perc_cov`: percentage of the glacier area covered by M22 (recalculated in this study, `millan_area_km2`) relative to area in the RGI 
- `f19_vol_km3`: volume of the glacier as obtained from the original F19 dataset 
- `millan_vol_adj`: volume of the glacier as computed from the original M22 dataset, but adjusted for regional undercatch (pixels in the original M22 dataset non-attributable to a specific glacier because of reprojection issues). Since this undercatch is not attributable to single glaciers, we compute the ratio of the total regional volume from the original M22 files to the regional sum of individual glaciers (`millan_vol_km3`), yielding regional correction factors of 2.5 ± 1.5% depending on the region. 
- `vas_millan_vol`: volume as computed by volume area scaling (Eq. S1 in Hock et al., 2023), with the RGI area as reference. Scaling parameters are computed for each RGI region separately. They are fitted regionally to the glacier per glacier M22 dataset (`millan_vol_adj` column) for all glaciers with >95% area coverage (see Hock et al., 2013 for details and the regional tables below for the parameter values).

### Corrected regional volumes

[volume_table_corrected.csv](tables_regional_glacier_volume/volume_table_corrected.csv): recalcuated and upscaled regional glacier volume based on M22 compared to original M22 and F19 data, as well as additional data used in the re-calculation. See Hock et al. (2023) for a full description and the code in [millan_regional_volume_scaling.ipynb](code/millan_regional_volume_scaling.ipynb) for implementation details.

[volume_table_corrected_rounded.csv](tables_regional_glacier_volume/volume_table_corrected_rounded.csv): same as [volume_table_corrected.csv](tables_regional_glacier_volume/volume_table_corrected.csv) but rounded for readability.

Details of the columns:

0. `index`: Region number of the Randolf Glacier Inventory (RGI, version 6.0). M22 added some regions together - we do the same for consistency.
1. `RGI area (km²)`: total glacier area in the region as provided by the RGI 6.0 dataset.
2. `M22 area uncorrected (km²)`: regional glacier area covered by ice volume estimates in M22; recalculated in this study based on the original gridded dataset provided by M22 (Note that in some cases the area differs somewhat from those reported in M22, Table 1).
3. `M22 coverage (%)`: ratio of of glacier area covered by M22 (recalculated in this study, column 2) to RGI area (column 1).
4. `M22 volume uncorrected (km³)`: regional glacier volume recalculated in this study based on the original gridded dataset provided by M22 (volume refers to areas in column 2).
5. `M22 volume as reported (km³)`: regional volume as reported in the original M22 study. Should be similar to column 4: differences arise from probable reporting errors in M22.
6. `F19 volume (km³)`: regional glacier volume recalculated in this study based on the original gridded dataset provided by F19. These numbers correspond almost perfectly to the numbers reported in F19 (colum 7). We recompute them here for consistency with M22 and to avoid rounding issues in publication table reporting.
7. `F19 volume as reported (km³)`: regional volume as reported in the original F19 study. Should be similar to column 6: differences arise from rounding errors and floating point accuracy.
8. `M22-F19 volume difference uncorrected (%)`: (column 4 - column 6) / (column 6) * 100.
9. `M22 volume on subset (km³)`: "subset" refers to a subset of glaciers in this region with M22 ice thickness data coverage > 95%. The volume is recalculated as in column 4, but only for this subset.
10. `F19 volume on subset (km³)`: glacier volume from F19 on the same subset as column 9. Columns 9 and 10 allow a "true" comparison, comparing the two datasets over a domain where both dataset have a good coverage (> 95%).
11. `M22-F19 volume difference on subset (%)`: (column 9 - column 10) / (column 10) * 100. Fair comparison of the two datasets.
12. `VAS parameter c`: the regional parameter `c` in the volume area scaling `V` = `c A^gamma`. Fitted on M22 data processed by us over all glaciers in the subset.
13. `VAS parameter gamma`: the regional parameter `gamma` in the volume area scaling `V` = `c A^gamma`. Fitted on M22 data processed by us over all glaciers in the subset.
14. `M22 volume corrected method 1 (km³)`: regional glacier volume by M22 (column 9) upscaled to the total RGI area using method 1 (V-A scaling) and the parameters in columns 12 and 13.  See Hock aet al., (2023) for details.
15. `M22 volume corrected method 2 (km³)`: regional glacier volume (column 9) upscaled to the RGI area using method 2 (based on F19 data). See Hock et al., (2023) for details.
16. `M22-F19 volume difference method 1 (%)`: (column 14 - column 6) / (column 6) * 100.
17. `M22-F19 volume difference method 2 (%)`: (column 15 - column 6) / (column 6) * 100. By design, this should be the same as column 11 and is used for verification.
