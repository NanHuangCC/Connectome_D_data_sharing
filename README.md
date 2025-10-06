Here is the filelist for directional connectome provided by Nan Huang.

----------------------------------------------------------------------
# Introduction to the methodology
Briefly we used the tractography-based method "blueprint" to calculate the similarity between regions of the human brain and the macaque brain. And fit the connectome of Macaque brain based on neural tracer experiments summarized by CocoMac to the human brain and get the possible directions of neural connections in the human brain. For details see in manuscripts.

----------------------------------------------------------------------
# Possible main issues
Q1: What is the human brain atlas atlas used?
A1: For cortical regions we applied the original HCP MMP 1.0 atlas for subcortical regions we applied the HCPex atlas, which provides 33 subcortical structures per hemisphere including the VTA, SNc, and key thalamic subdivisions. Since the left and right brain definitions of this HCP MMP 1.0 are opposite to the left and right brain definitions of most brain atlases, we switched the labeling of the left and right brain in the cortical structures to match the orientation of the subcortical structures.
HCP MMP 1.0: Glasser, M. F. et al. A multi-modal parcellation of human cerebral cortex. Nature 536, 171–178. URL https://www.nature.com/articles/nature18933.
HCPex: Huang, C.-C., Rolls, E. T., Feng, J. & Lin, C.-P. An extended human connectome project multimodal parcellation atlas of the human cortex and subcortical areas. Brain Structure and Function 227, 763–778. URL https://doi.org/10.1007/s00429-021-02421-6.

Q2: Is it possible to replace the human brain brain mapping?
A2: Yes! Because we used the fsaverage_LR32k brain surface provided by HCP to compute the blueprint of the cortex. therefore replacing the cortical atlas requires only a simple reaveraging process. However, for subcortical structures the original file is based on voxel-based blueprints. individuals do not correspond to each other one-to-one, and thus need to be fully flushed into the computed blueprint back to be more time-consuming. At the same time, too fine a subcortical structure may lead to missing values in the results.

Q3: Is this connectome reliable? Are there any defects?
A3: This method can be simply understood as a mapping relationship between the neural tracing and main white fiber tract in macaque to human brain given the similarities between human and macaque brains. Compared to the application of the method in other studies such as the construct the relationship between white matter bundles with receptors densities, its association with neural conneection is apparently more direct and relatively more reliable. The neural connectivity of each brain regions often depends on the weighted average of multiple brain regions (see manuscript for details), so the changes in connectivity between brain regions also appear smoother than in the original macaque data. 
At the moment, based on my thinking, this connection group has the following drawbacks to be aware of
1. Inevitable false negatives: Even though Modha & Singh collated more than 400 studies of neural connections based on the Cocomac database, it is still not possible to confirm that this study covers the full range of connections that may exist in the macaque brain. In addition the low number of studies in some brain regions also creates some uncertainty. Although some of the problems are masked by the weighted average results, there are still bound to be false negatives. There are also a small number of pairs of brain regions that are hypothesized to have connectivity but this connectivity is not detected in tractography. Together, these two points constitute a false negative.
2. Cross-hemispheric projection: All neural connectivity direction speculations were based on left hemisphere studies, so for the cross-hemisphere portion we retained the TOP 15% of connections based on connection weights, making the connection density close to that within the single brain hemisphere, Similarly according to the intrahemispheric projections limiting the overly extensive connection of the striatum to the contralateral hemisphere.
3. Evolutionary differences: Evolutionarily humans have larger prefrontal and temporal lobes compared to macaque, but this also region is also connected to the major white matter tracts. The extended brain region was therefore considered as a weighted average of multiple brain regions in macaques. In other words, the method is able to cover some of the differences detected by tractography, but it is difficult to estimate the more complex and detailed new connectivity patterns that may exist in the human brain.
4. Parameter selection: Because the calculated raw data responds to the probability of the existence of a connection (already provided in the file), we can only pick a specific value for retaining the possible connections, which is difficult to define, and we choose here to retain 20% of the connection density.

Q4: Can this method be applied to the prediction of connection direction for individual subject?
A4: Yes, this method can provide subject-special connectome, specifically by constructing transformation matrices based on individual blueprints with the average blueprint of macaque can make predictions about the connectivity patterns of a single individual. The first directional connectome we constructed was tested on a single individual (HCP-102311). Its connectivity pattern is very slightly different from the average connectivity of the left posterior.

cites:
The main connection with the database used in the process:
Connection in macaqu (CocoMac):  Modha, D. S. & Singh, R. Network architecture of the longdistance pathways in the macaque brain. Proceedings of the National Academy of Sciences 107,1348513490
Blue print: Mars, R. B. et al. Whole brain comparative anatomy using connectivity blueprints 7, e35237. 
Xtract: XTRACT - standardised protocols for automated tractography in the human and macaque brain 217, 116923.

----------------------------------------------------------------------
# File  description
......................................................................
1. Directional_Connectome - Connection data that can be used directly for TVB
1.1 DirectionalConnectome_D99_HCPex_LR_log10weighted.csv: Connection group with direction (row:source, column:target). Contains 426 areas in the left and right hemispheres, of which 1:180, 214:393 are cortical (left, right), 181:213, 394:426 are subcortical (left, right). HCP MMP1.0 mapping was applied to cortical regions, supplemented by HCPex mapping of subcortical structures. Corresponding labels are provided in "HCPmmp_HCPex_node_coordinate_ranked.csv". The specific connection strength is calculated based on the average total number of streamlines in the tractography (log10(streamline-counts + 1)). 
1.2 DirectionalConnectome_D99_HCPex_LR_length.csv: Connection lengths based on tractography, calculated from the average length of the streamline between brain regions. (Unit: mm)
1.3 Mean center location of each brain region (in MNI152 space), The first column is the name of the brain region and the second column is the functional division. (Unit: mm)
1.4 Distance matrix based on coordinates (Unit: mm)
.......................................................................
.......................................................................
2. Intermediate_files - Intermediate files in the calculation process
2.1 Blue_prints: Obtained blueprint, rows indicate brain regions columns indicate major white matter bundles homologous to humans and macaques. Values represent the proportion of connections between this brain region and the major white matter tracts. The last contains the transformation matrix from macaque - D99 to HCP
2.2 Connectome data obtained using MRtrix3
2.3 Presumed directional data, DirectionalConnectome_D99_HCPex_left.csv is original data from mapping, The value represents the likelihood that the connection exists. DirectionalConnectome_D99_HCPex_left_dens20.csv obtained after preserving 20% of the connection density. The final assignment is done by tractography results DirectionalConnectome_D99_HCPex_left_dens20_Log10weighted.csv. Right hemisphere equivalent.

----------------------------------------------------------------------
# file list
.
├── Directional_Connectome
│   ├── DirectionalConnectome_D99_HCPex_LR_length.csv
│   ├── DirectionalConnectome_D99_HCPex_LR_log10weighted.csv
│   ├── HCPmmp_HCPex_node_coordinate_ranked.csv
│   └── HCPmmp_HCPex_node_distance_ranked.csv
└── Intermediate_files
    ├── Blue_prints
    │   ├── blueprint_human_HCPex_cortex_HCP-YA100.csv
    │   ├── blueprint_human_HCPex_cortex_HCP-YA100_std.csv
    │   ├── blueprint_human_HCPex_subcortex_HCP-YA100.csv
    │   ├── blueprint_human_HCPex_subcortex_HCP-YA100_sample_std.csv
    │   ├── blueprint_macaque_D99_cortex_BNA.csv
    │   ├── blueprint_macaque_D99_cortex_BNA_sample_std.csv
    │   ├── blueprint_macaque_D99_subcortex_BNA.csv
    │   ├── blueprint_macaque_D99_subcortex_BNA_sample_std.csv
    │   ├── mapping_cortex_D99_HCPex.csv
    │   ├── mapping_subcortex_D99_HCPex.csv
    ├── Connectome
    │   ├── connectivity_HCP-YA100_streamline.csv
    │   ├── connectivity_HCP-YA100_streamline_LR_log10.csv
    │   ├── connectivity_HCP-YA100_streamline_length.csv
    │   ├── connectivity_HCP-YA100_streamline_length_std.csv
    │   └── connectivity_HCP-YA100_streamline_std.csv
    └── Inferred_direction
        ├── DirectionalConnectome_D99_HCPex_left.csv
        ├── DirectionalConnectome_D99_HCPex_left_dens20.csv
        ├── DirectionalConnectome_D99_HCPex_left_dens20_Log10weighted.csv
        ├── DirectionalConnectome_D99_HCPex_right.csv
        ├── DirectionalConnectome_D99_HCPex_right_dens20.csv
        └── DirectionalConnectome_D99_HCPex_right_dens20_Log10weighted.csv