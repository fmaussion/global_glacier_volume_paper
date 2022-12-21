# Global glacier volume Code & Data

Code & Data for *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2022: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.*

## Licence 

The code is released under BSD3. It's all coded in python 3. You'll need pandas, geopandas, rioxarray, xarray, matplotlib, and seaborn to run the notebooks.

The data is licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Some of the data given in the the data folder have been previously published. They are included as table format here (`.csv`) since they provided input to our calculations or for direct comparison with our results. If you use these data, please refer to the original studies: Millan and others, 2022; Farinotti and others, 2019; Radić and others, 2014; Grinsted, 2013; Huss and Farinotti, 2012; Marzeion and others, 2012; Radić and Hock, 2010; Raper and Braithwaite 2005; Dyurgerov and Meier, 2005; Ohmura, 2004; Meier and Bahr, 1996.

**If you use the plots in the `figures` folder or the data in the `tables_regional_correction` folder, please refer to** *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2022: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.* 

## Code

See the [code](code) folder for the notebooks producing figures and tables.

## Figures

Used in the paper:
- [Figure 1: maps](figures/plot_maps_bright.png)
- [Figure 2: volume comparison](figures/plot_global_and_reg_log.png)

## Corrected regional and global volumes

In the [tables_regional_correction](tables_regional_correction) folder, you will find three CSV files.

### Glacier per glacier volume

[all_glaciers_m22_f19.csv](tables_regional_correction/all_glaciers_m22_f19.csv): volume estimates for each individual glacier recalculated from the original ice thickness data given by [Millan et al. (2022)](https://www.nature.com/articles/s41561-021-00885-z) (M22) and provided by [Farinotti et al. (2019)](https://www.nature.com/articles/s41561-019-0300-3) (F19). To derive the M22 estimates, reprojection and subsetting of the original M22 data files was necessary - we did this with the OGGM framework ([Maussion et al., 2019](https://gmd.copernicus.org/articles/12/909/2019/)).

The original M22 thickness data are available from [this website](https://www.sedoo.fr/theia-publication-products/?uuid=55acbdd5-3982-4eac-89b2-46703557938c), the F19 data from [this one](https://www.research-collection.ethz.ch/handle/20.500.11850/315707).

Details of the columns:
- `rgi_id`: unique glacier identifier from the Randolf Glacier Inventory (RGI, version 6.0)
- `rgi_area_km2`: area of the glacier as provided by the RGI 6.0. 
- `millan_vol_km3`: volume of the glacier as computed from the original M22 dataset
- `millan_area_km2`: area of the glacier as computed from the original M22 dataset (the differences to RGI6 originate from the raster representation and incomplete data coverage in M22) 
- `millan_perc_cov`: percentage of the glacier area covered by M22 (recalculated in this study, `millan_area_km2`) relative to area in the RGI 
- `f19_vol_km3`: volume of the glacier as obtained from the original F19 dataset 
- `millan_vol_adj`: volume of the glacier as computed from the original M22 dataset, but adjusted for regional undercatch (pixels in the original M22 dataset non-attributable to a specific glacier because of reprojection issues). This is not a correction for the missing area with respect to RGI6, but a regional correction necessary for the sum of individual glaciers to match the regional volume estimates from the original M22 dataset. The difference to  `millan_vol_km3` is small (< 5% in most cases).
- `vas_millan_vol`: volume as computed by volume area scaling (Eq. S1 in Hock and others), with the RGI area as reference. Scaling parameters are computed for each RGI region separately. They are fitted to the M22 dataset for all glaciers with >95% area coverage (see Hock and others for details and the regional tables below for the parameter values).

### Corrected regional volumes

In `millan_table_corrected.csv`, we revise the regional tables in the original M22 and F19 studies for three aspects:

1. The values indicated in both the F19 and M22 publications are rounded for readability, which makes certain numbers in small regions too coarse for comparison and exact percentages. Here the numbers are directly extracted from the original data files of each respective study.
2. Region 19 (Antarctic and Subantarctic) is now computed for all glaciers in RGI 6.0 for M22 to be comparable to F19.
3. The regional volumes are corrected for missing data coverage in the M22 thickness product. For this correction, we use two different approaches (see Hock and others for methods).

See the code in [millan_regional_volume_scaling.ipynb](code/millan_regional_volume_scaling.ipynb) for implementation details.

Details of the columns:

0. `index`: Region number of the Randolf Glacier Inventory (RGI, version 6.0). M22 added some regions together - we do the same for consistency.
1. `RGI area (km²)`: total glacier area in the region as provided by the RGI 6.0 dataset
2. `M22 area uncorrected (km²)`: regional glacier area covered by ice volume estimates in M22; recalculated in this study based on the original gridded dataset provided by M22 (Note that in some cases the area differs somewhat from those reported in M22, Table 1)
3. `M22 coverage (%)`: ratio of of glacier area covered by M22 (recalculated in this study, column 2) to RGI area (column 1)
4. `M22 volume uncorrected (km³)`: regional glacier volume recalculated in this study based on the original gridded dataset provided by M22 (volume refers to areas in column 2)
5. `F19 volume (km³)`: regional glacier volume recalculated in this study based on the original gridded dataset provided by F12. These numbers correspond almost perfectly to the numbers reported in F19. 
6. `M22-F19 volume difference uncorrected (%)`: (column 4 - column 5) / (column 5) * 100
7. `M22 volume on subset (km³)`: "subset" refers to a subset of glaciers in this region with M22 data coverage > 95%. The volume is recalculated as in column 4, but for this subset.
8. `F19 volume on subset (km³)`: glacier volume from F19 on the same subset as column 7. Columns 7 and 8 allow a "true" comparison, comparing the two datasets over a domain where both dataset have a good coverage.
9. `M22-F19 volume difference on subset (%)`: : (column 7 - column 8) / (column 8) * 100. Fair comparison of the two datasets.
10. `VAS parameter c`: the regional parameter `c` in the volume area scaling `V` = `c A^gamma`. Fitted on M22 data processed by us over all glaciers in the subset.
11. `VAS parameter gamma`: the regional parameter `gamma` in the volume area scaling `V` = `c A^gamma`. Fitted on M22 data processed by us over all glaciers in the subset.
12. `M22 volume corrected method 1 (km³)`: regional glacier volume by M22 (column 7) upscaled to the total RGI area using method 1 (V-A scaling) and the parameters in columns 10 and 11.  See Hock and others for details.
13. `M22 volume corrected method 2 (km³)`: regional glacier volume (column 7) upscaled to the RGI area using method 2 (based on F19 data). See Hock and others for details.
14. `M22-F19 volume difference method 1 (%)`: (column 12 - column 5) / (column 5) * 100. 
15. `M22-F19 volume difference method 2 (%)`: (column 13 - column 5) / (column 5) * 100. By design, this should be the same as column 9 and is used for verification.
