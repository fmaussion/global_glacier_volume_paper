# Global glacier volume Code & Data

Code & Data for *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2022: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.*

## Licence 

It's all coded in python 3. You'll need pandas, geopandas, matplotlib, and seaborn for this to work.

The code is released under BSD3. Some of the data given in the the data folder have been previously published. They are included here since they provided input to our calculations or for direct comparison with our results. If you use these data, please refer to the original studies: Millan and others, 2022; Farinotti and others, 2019; Radić and others, 2014; Grinsted, 2013; Huss and Farinotti, 2012; Marzeion and others, 2012; Radić and Hock, 2010; Raper and Braithwaite 2005; Dyurgerov and Meier, 2005; Ohmura, 2004; Meier and Bahr, 1996.

**If you use the plots in the `figures` folder or the data in the `tables_regional_correction` folder, please refer to** *Hock R., Maussion, F., Marzeion, B. and Nowicki, S., 2022: What is the global glacier ice volume outside the ice sheets?, Journal of Glaciology, accepted.*

## Code

See the [code](code) folder for the notebooks.

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
- `index`: unique glacier identifier from the Randolf Glacier Inventory (RGI, version 6.0)
- `millan_vol_km3`: volume of the glacier as computed from the original M22 dataset
- `millan_area_km2`: area of the glacier as computed from the original M22 dataset (the differences to RGI6 originate from the raster representation and incomplete data coverage in M22) 
- `millan_perc_cov`: percentage of the glacier area covered by M22 (recalculated in this study, `millan_area_km2`) relative to area in the RGI 
- `f19_vol_km3`: volume of the glacier as obtained from the original F19 dataset 
- `millan_vol_adj`: volume of the glacier as computed from the original M22 dataset, but adjusted for regional undercatch (pixels in the original M22 dataset non-attributable to a specific glacier because of reprojection issues). This is not a correction for the missing area with respect to RGI6, but a regional correction necessary for the sum of individual glaciers to match the regional volume estimates from the original M22 dataset. The difference to  `millan_vol_km3` is small (< 5% in most cases).
- `vas_millan_vol`: volume as computed by volume area scaling (Eq. S1 in Hock and others), with the RGI area as reference. Scaling parameters are computed for each RGI region separately. They are fitted to the M22 dataset for all glaciers with >95% area coverage (see Hock and others for details and the regional tables below for the parameter values).

### Corrected regional volumes

TODO : include Regine's comments

**The tables in [tables_regional_correction](tables_regional_correction) are equivalent to the additional data table of the J. Glaciology paper - a few columns have been renamed and reorganized for the paper.**

In `millan_table_corrected.csv`, we revise Table 1 in the paper on three aspects:
1. The values indicated in both the F19 and M22 publications are rounded for readability, which makes certain numbers in small regions too coarse for comparison and exact percentages. Here the numbers are directly extracted from the original data files of each respective study.
2. Region 19 (Antarctic and Subantarctic) is now considered as a whole, like in F19
3. The regional volumes are corrected for missing data coverage in the M22 product. For this correction, we use two different methods (see paper).

See the code in [millan_regional_volume_scaling.ipynb](code/millan_regional_volume_scaling.ipynb) for more details.

Details of the columns:
- `RGI area (km²)`: total glacier area in the region as provided by the RGI dataset
- `M22 area uncorrected (km²)`: total glacier area in the region covered by the M22 dataset
- `M22 coverage (%)`: the area coverage in the M22 dataset, as computed by dividing `M22 area uncorrected (km²)` by `RGI area (km²)`. These numbers should be close to (but not identical to) Table 1 in M22, because they report velocity coverage, not thickness.
- `M22 volume uncorrected (km³)`: total volume of the region provided by the M22 dataset (without correction). These numbers should be identical to M22 Table 1 (they are not exact in some cases, but close enough - we assume rounding or small reporting errors in the paper).
- `F19 volume (km³)`: regional volume in the F19 dataset.
- `M22-F19 volume difference uncorrected (%)`: the difference in volume as reported in Table 1 of M22, but computed by us by dividing `M22 volume uncorrected (km³)` by `F19 volume (km³)`. The numbers should be close enough.
- `M22 volume on subset (km³)`: the total M22 volume on the subset of glaciers with M22 data coverage > 95% (i.e. good quality of both datasets)
- `F19 volume on subset (km³)`: the total F19 volume on the same subset
- `M22-F19 volume difference on subset (%)`: the difference in volume computed over the same subset 
- `VAS parameter c`: the regional parameter `c` in the volume area scaling `V` = `c A^gamma`. Fitted over all glaciers in the subset.
- `VAS parameter gamma`: the regional parameter `gamma` in the volume area scaling `V` = `c A^gamma`. Fitted over all glaciers in the subset
- `M22 volume corrected method 1 (km³)`:  the corrected regional volumes, computed with Method 1 (using volume-area scaling to estimate the fraction of the volume not covered by M22)
- `M22 volume corrected method 2 (km³)`: the corrected regional volumes, computed with Method 2 (using F19 to estimate the fraction of the volume not covered by M22)
- `M22-F19 volume difference method 1 (%)`: the new regional differences (in %) computed with Method 1
- `M22-F19 volume difference method 2 (%)`: the new regional differences (in %) computed with Method 2
