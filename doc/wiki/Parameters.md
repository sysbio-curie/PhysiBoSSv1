<h1> Parameters </h1>

<p> 
In this page, we list all possible parameters that can be precised in the parameter files in the simulations. We will udpate this list as new parameters are added (or some deleted).
The parameter file is composed of different parts, according to the type of parameters to define:
</p>


* [Simulation](#-simulation-)
* [Cell properties](#-cell-properties-)
* [Network](#-network-)
* [Initial configuration](#-initial-configuration-)

---
<h2> Simulation </h2>

Parameters that refer to global properties of the simulation, e.g. the numerical time step, are precised in the `simulation` parameters element. The table below gives the list of possible parameters that can be specified in this section. We precised the default value that is used in PhysiBoSS, and when relevant, a range of plausible values for each parameter.

| Parameter name | Default value (advised range) | Description |
|:--------------:|:--------------:|:-----------:|
| time_step | 0.01 min (0.001-1) | Diffusion time scale (smallest time scale). If no entities are diffusing, can be higher. From [[1]](#-references-)|
| mechanics_time_step | 0.1 min (0.01-5) | Time scale of motion, cell volume changes. From [[1]](#-references-) |
| cell_cycle_time_step | 6 min (0.5-10) | Cell cycle time scale, change of cell phase. From [[1]](#-references-) |
| maximal time     | 4320 min | Duration of simulated time | 
| output_intervals | 60 min | Frequency at which cells position and states are written to file. |
| output_densities | 600 min | Frequency at which microenvironment densities concentration are written to file. |
| write_passive_cells | 0 (0 or 1)   | If write the position of passive cells in output files or not | 
| write_ratio_voxels  | 0.5 (0-1) | Proportion of microenvironment voxel values to write to output files (writing all of them can be pretty heavy) |
| number_of_threads | 10 | Number of threads for parallel computing. Depends on the machine used. | 
| friction_passive_cells | 0.0001 (0.00001-1) | How easy will it be to move passive cells (if high, fixed cells). |	
| mode_cell_cycle | 0 (0 or 1) | Mode of calculation of the cell cycle: 0, cycling is defined as in [[1)](#-references-). 1: cycling is dependant on the boolean network outputs. |
| number of densities | >= 1 | Number of diffusing entities. By default, there is just one (oxygen). |
| density_*0* | oxygen | Name of the diffusing densities. Increment the indices in "density_0" to define more than 1 (e.g. `<density_1> tnf </density_1>`)... |		
| bounding_box_xmin | -200 µm (10-10000) | Definition of the boundray box surrounding all the simulation space |
| bounding_box_xmax | +200 µm (10-10000)  | Definition of the boundray box surrounding all the simulation space |
| bounding_box_ymin | -200 µm (10-10000)  | Definition of the boundray box surrounding all the simulation space |
| bounding_box_ymax | +200 µm (10-10000)   | Definition of the boundray box surrounding all the simulation space |
| bounding_box_zmin | -200 µm (10-10000)   | Definition of the boundray box surrounding all the simulation space |
| bounding_box_zmax | +200 µm (10-10000)   | Definition of the boundray box surrounding all the simulation space |
| minimum_voxel_size | 30 µm (1-50) | Spatial discretisation of the microenvironment. Length of one side of the cube (voxel). |
| svg_coloring_mode | 0 or 1 | How to color the cells in svg output: 0, cytoplasmic radius and nuclear radius are colored differently; 1, cells are colored according to their phase |  

Example of the `simulation` part of the parameter file:
```xml
<simulation>
	<time_step> 0.02 </time_step>
	<mechanics_time_step> 0.1 </mechanics_time_step>
	<cell_cycle_time_step> 2 </cell_cycle_time_step>
	<maximal_time> 1440 </maximal_time>
	<write_passive_cells> 0 </write_passive_cells>
	<mode_cell_cycle> 1  </mode_cell_cycle>
	<number_of_densities> 2 </number_of_densities>
	<density_0> oxygen </density_0>
	<density_1> tnf </density_1>
	<write_ratio_voxels> 0.3 </write_ratio_voxels>
</simulation>
```

---
<h2> Cell properties </h2>
In this section of the parameter file, the properties common to all cells of one cell line are given. This section will be repeated for each cell line (e.g. if you simulate a cell population composed of 2 different cell lines, there will be 2 `cell_properties` sections in the parameter file). The order in which the cell properties are given in the file correspond to the cell lines in the simulation (cell line 0, cell line 1...).  


| Parameter name | Default value (advised range) | Description |
|:--------------:|:--------------:|:-----------:|
| motility_amplitude_min | 5 (0.0001-20) | Mobility of the cell, varying from min value to max according to cell's internal state. Empirical unit: 0.0001 very slow (fixed), 20 fast motion |
| motility_amplitude_max | 5 (0.0001-20) | Mobility of the cell, varying from min value to max according to cell's internal state. Empirical unit: 0.0001 very slow (fixed), 20 fast motion |
| polarity_coefficient | 0 (0-1) | Amount of polarization of the cell: if 0, movement is totally random. If 1, movement is totally defined by the cell's current polarity axis |
| persistence | 0.5 (0.0001-1000) | Persistence of the polarity axis: when value is high, cell will keep its polarity axis constant for a long time. For low value, it will aligned to its previous velocity very fast |
| mode_motility | 0 (0 or 1) | How motility is calculated. If 0, motion is always random. If 1, motion is biaised towards its polarity axis according to its polarization coefficient | 
| homotypic_adhesion_min | 0.17 (0-10) | Cell-cell adhesion coefficient, for cells of the same cell line (0 means cells ignore each other). Discussed in [[2]](#-references-). The value of the coefficient varies from min to max value according to its internal state (recruitment of cadherins) |  
| homotypic_adhesion_max | 0.17 (0-10) | Cell-cell adhesion coefficient, for cells of the same cell line (0 means cells ignore each other). Discussed in [[2]](#-references-). The value of the coefficient varies from min to max value according to its internal state (recruitment of cadherins) |  
| heterotypic_adhesion_min | 0.17 (0-10) | Cell-cell adhesion coefficient, for cells of different cell lines (0 means cells ignore each other). Discussed in [[2]](#-references-). The value of the coefficient varies from min to max value according to its internal state (recruitment of cadherins) |  
| heterotypic_adhesion_max | 0.17 (0-10) | Cell-cell adhesion coefficient, for cells of different cell lines (0 means cells doesn't attract each other). Discussed in [[2]](#-references-). The value of the coefficient varies from min to max value according to its internal state (recruitment of cadherins) |
|cell_cell_repulsion | 10 (0-50) | Hard-core repulsion coefficient of 2 cells that overlap. Discussed in [[2]](#-references-) |
| ecm_adhesion_min | 0 (0-50) | Strength of cell adhesion to the ECM (depends also on the local ECM density). 0 means it does not adhere to the matrix. The value of this coefficient varies from min to max value according to its internal state (recruitment of integrins) |
| ecm_adhesion_max | 0 (0-50) | Strength of cell adhesion to the ECM (depends also on the local ECM density). 0 means it does not adhere to the matrix. The value of this coefficient varies from min to max value according to its internal state (recruitment of integrins) |
| ecm_degradation | 0 (0-50) | Coefficient of cell degradation of the ECM (activity of the MMPs). Emperical unit |
| cell_basement_membrane_repulsion | 10 (0-100) | Hard-core repulsion between membrane (if defined) and a cell. If 0, the cell ignore the outer membrane (can cross it). Discussed in [[2]](#-references-) |
| cell_ecm_repulsion | 10 (0-100) | Hard-core repulsion between ECM and a cell. If 0, the cell ignore the ECM (can cross it). Similar to cell-basement membrane repulsion coefficient |
| max_interaction_factor | 1.4 (1-2) | Factor of distance (compared to cell radius) until which a cell can reach (with filopodia for example). Discussed in [[2]](#-references-) |
| contact_cell_cell_threshold | 1 (0-5) | Threshold of contact from which cell will be considered as having neighbor, for input of the boolean network. If 0, then the input "Neighbor" of the network will always be 1 |
| contact_cell_ECM_threshold | 1 (0-5) | Threshold of contact between the cell and the ECM density from which cell will be considered as adhering to the matrix, for input of the boolean network. If 0, then the input "ECM" of the network will always be 1 |
| *protein*_threshold | 1 (0-5) | Threshold of amount of *protein* bound to the cell (internal accumulator) above which the cell is considered to internalize this protein (input of the corresponding entity will be 1 in the boolean network). The name *protein* can be used to set the default for all densities. The name of a particular density can also be precised (e.g. `tnf_threshold`) |
| cell_radius | 8.5 µm (1-30) | Radius of a cell, just after division (minimal radius). Default value corresponds at a 3T3 fibroblast cell |
| nucleus_radius | 5 µm (0.5-10) | Radius of the nucleus of the cell, just after division |
| fluid_fraction | 0.75 (0-1)  | Fraction of the cell that is liquid vs solid. See in [[1]](#-references-) |
| phenotype_number | 0 (0;1;2;3;4) | Choice of cell phenotype (definition and properties of the cell phases). Available choices are: 0, MCF7 like cell as in [[1]](#-references-); 1, MCF7 with fast deaths; 2, MCF7 with a cycle duration of 24h; 3, 3T3 like cell; 4, G0 cells (cells cannot change phase, stay quiescents) |
| secretion_rate | 0.1 fg/cell/min (0-5)| Speed of a density production by a cell when the cell secretion is active. This value was chosen from a range of plausible value for TNF from [[3]](#-references-) | 
| uptake_rate | 0.0025 /voxel/min (0-0.1) | Quantity of density (e.g. TNF) consumed by a cell when it is available. Emperical value |


Example of a cell properties definition:
```xml
<cell_properties>
    <polarity_coefficient> 0.1 </polarity_coefficient>
    <motility_amplitude_min> 0.01 </motility_amplitude_min>
    <motility_amplitude_max> 0.01 </motility_amplitude_max>
    <mode_motility> 1 </mode_motility>
    <homotypic_adhesion_min> 2 </homotypic_adhesion_min>
    ....
    <phenotype_number> 3 </phenotype_number>
    <secretion_rate> 0.1 </secretion_rate>
</cell_properties>
```
---
<h2> Network </h2>
In this section of the parameter file, we defined the properties relative to the boolean network computation. The majority of the parameters concerning the boolean network structure are defined in the MaBoSS network files, with the MaBoSS conventions. Here, the parameters that can be defined are those specific to PhysiBoSS simulations: update time step of the network and the definition of mutation specific to each cell line. Indeed, in this version of PhysiBoSS, the same MaBoSS configuration files are used for all cell lines, but the parameters as the transition rates can be varied accross cell lines to simulate mutations in specific genes up(or down)-regulations.


| Parameter name | Default value (advised range) | Description |
|:--------------:|:--------------:|:-----------:|
| network_update_step | 10 min (0.1-100) | Frequency of updates of the network state. Compute a given number of MaBoSS iterations (defined in MaBoSS files) every `network_update_step` times. |
| mutation_*0* | | Definition of a mutation: change the transition rate for one gene up(down)-regulation for one cell line |
| mutation_*0*->symbol_name | e.g. $High_IKK | Defines the transition rate value to change. Must be defined in the MaBoSS network file with the exact same name |
| mutation_*0*->cell_line | 0 (0,1,2...) | Defines for which cell line this value of the transition rate is defined |
|mutation_*0*->rate | 0.0 (0-1 in general) | The value of the transition rate (0 will never happen, 1 is default speed) |

```xml
<network>
	<network_update_step> 10 </network_update_step>
	<mutation_0>
		<symbol_name> $High_IKK </symbol_name>
		<cell_line> 0 </cell_line>
		<rate> 0.0 </rate>
	</mutation_0>
	<mutation_1>
		<symbol_name> $High_IKK </symbol_name>
		<cell_line> 1 </cell_line>
		<rate> 1.0 </rate>
	</mutation_1>
	<mutation_2>
		<symbol_name> $High_cFLIP </symbol_name>
		<cell_line> 0 </cell_line>
		<rate> 0.0 </rate>
	</mutation_2>
	<mutation_3>
		<symbol_name> $High_cFLIP </symbol_name>
		<cell_line> 1 </cell_line>
		<rate> 1.0 </rate>
	</mutation_3>
</network>
```
	
---
<h2> Initial configuration </h2>

| Parameter name | Default value (advised range) | Description |
|:--------------:|:--------------:|:-----------:|
| sphere_radius | 100 µm (10-10000) | If no initial file is provided, length of the initial sphere of cells to create. To choose from experimental conditions to mimick. |
| load_cells_from_file | init.txt | Name of the file containing the initial position of all the cells |
| create_ecm_from_file | ecm.txt | Name of the file containing the initial value of ECM concentrations (must contains a list of position (x,y,z) and density value). |
| number_of_passive_celles | 0 (0-100000) | If no initial file is provided, number of passive cells to add (around the active cells sphere). |
| time_passive_cells | 1000000 min (0-maxtime)| Change the repulsive capacity of the passive cells during the simulation, after the time given (before not repulsing=phantom cells, after repulsing cells=obstacle). By default, value above max time of simulation: no change |
| mode_injection | -1 (-1 or 0 or 1) | How injection of a density is simulated. Options are: -1, no injection; 0, injection in all the voxels of the space; 1; injection only in the voxels at the outer boundary of the space | 
| time_add_egf | 1000000 min (0-maxtime)| Time interval between EGF injection (default value is higher than simulation maximal time, so no injection) |
| time_add_tnf | 1000000 min (0-maxtime)| Time interval between TNF injection (default value is higher than simulation maximal time, so no injection) |
| duration_add_tnf | 10 min (0-maxtime)| Duration of the pulse injection when there is injection to do |
| time_remove_tnf  | 1000000 min (0-maxtime)| Time at which to clear the microenvironment of all TNF (corresponding by example to a change of solution) |
| *densityName*_concentration | 7.2 fg/µm^3 (0-100)|Concentration of injected density (name of the density followed by `_concentration`: e.g. `tnf_concentration`) | 
| membrane_shape | none | Addition of an outer "membrane" that constrains all the cells inside this geometry. Can be: `none`, `duct` or `sphere` |
| membrane_length | 0 | If a shape is defined, length of the geometry (e.g. radius for the sphere) |

Example of the initial configuration part of the parameter file:
```xml
<initial_configuration>
	<load_cells_from_file> init.txt </load_cells_from_file>
	<membrane_shape> sphere </membrane_shape>	
	<membrane_length> 470 </membrane_length>
	<oxygen_concentration> 40 </oxygen_concentration>
	<tnf_concentration> 1 </tnf_concentration>	
	<mode_injection> 1 </mode_injection>
</initial_configuration>
```
---
<h2> References </h2>
[1] Ghaffarizadeh A., Heiland R., Friedman S.H, Mumenthaler S.M. and Macklin P. PhysiCell : [an Open Source Physics-Based Cell Simulator for 3-D Multicellular Systems](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005991). Plos Computational Biology 2018
[2] P. Macklin, M.E. Edgerton, A.M. Thompson and V. Cristini. [Patient-calibrated agent-based modelling of ductal carcinoma in situ (DCIS): From microscopic measurements to macroscopic predictions of clinical progression](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322268/). J Theor Biol. 2012.
[3] Liu Y., Kwa T., Revzin A. Simultaneous detection of cell-secreted TNF-alpha and IFN-alpha using micropatterned aptamer-modified electrodes. Biomaterials. 2012.