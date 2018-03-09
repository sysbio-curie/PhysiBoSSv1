# PhysiBoSS   
Multiscale simulation of multi-cellular system
 
Overview:

 * [Presentation](#presentation)
 * [Documentation](#documentation)
 * [Usage](#usage)
 * [Remarks](#remarks)

## Presentation 
PhysiBoSS (PhysiCell-MaBoSS) is C++ software for multiscale simulation of heterogeneous multi-cellular system. It integrates together cell's internal signalling pathway model (boolean formalism), physical representation of cell (agent-based) and extra-cellular matrix diffusing or fixed entities. 
It is adapted from [PhysiCell](http://physicell.mathcancer.org) sources, with the boolean network computation inside each cell from [MaBoSS](http://maboss.curie.fr) software. 
  
![Hello world image](./doc/imgs/hello.png?raw=true "PhysiBoSS simulation example") 
 
 
## Documentation 
Code-oriented documentation can be generated with Doxygen:
~~~bash
make doc
~~~  
in the main directory. 
It can be configured in the Doxyfile file present in this directory.
It will generate the html documentation in the doc/html directory. 
You can visualize it in a browser, e.g.:
~~~bash
firefox doc/html/index.html &
~~~

You can also refer to (future) publications with PhysiBoSS for scientific applications of this software and description of the models.

Step-by-step examples with the necessary files to run them are also proposed in the 'examples' directory and on the [Wiki](https://github.com/gletort/PhysiBoSS/wiki) of this repository.

## Usage
### Compiling PhysiBoSS
PhysiBoSS should run and be easily installed on Linux and MacOS system. We also provide a [Docker image](https://github.com/gletort/PhysiBoSS/tree/master/Docker) of PhysiBoSS that can be used if it cannot be installed in your machine.
It requires moderatly recent version of C++ (at least c++11) and OpenMP support. Compilation of MaBoSS library requires `flex` and `bison` library, usually already present (and can be easily installed on e.g. Linux ubuntu with `sudo apt-get install bison flex`).

To install it on Linux system, from a Terminal:
Clone the repository on your local machine, and go inside the main directory. Type `make install`, which will install and compile MaBoSS then PhysiBoSS. The executables will be created in the 'bin' directory if all goes well. 
It can be compiled in 'Debug', 'Release' or 'Proliling' modes, to set in the 'Makefile' file. Default is 'Release' mode (fastest).
You might also have to change your c++ compiler in the Makefile according to your operating system.

Commands list:
~~~bash
git clone https://github.com/gletort/PhysiBoSS.git
cd PhysiBoSS
make install
~~~

If errors happened during the compilation, please refer to the [installation](https://github.com/gletort/PhysiBoSS/wiki/Installation) page.

### Running one simulation
To run a simulation, you need (at least) a XML parameter file indicating the conditions of the simulation, and the networks file (you can find some on [MaBoSS website](http://maboss.curie.fr) and on our <a href="https://github.com/ArnauMontagud/Logical_modelling_pipeline">logical modelling pipeline repository</a>). 
Other options are possible, cf the code-documentation or this repository wiki for more informations.  
 
Example of a parameter file (with only few parameters shown):
~~~xml
  <?xml version="1.0" encoding="UTF-8" ?>
 
  <simulation>
 		<time_step> 0.2 </time_step>
 		<mechanics_time_step> 0.1 </mechanics_time_step>
 		....
  </simulation>
 
  <cell_properties>
 		<mode_motility> 1 </mode_motility>
 		<polarity_coefficient> 0.5 </polarity_coefficient>
 		...
  </cell_properties>
 
  <network>
 		<network_update_step> 10 </network_update_step>
 		...
  </network>
 
  <initial_configuration>
 		<load_cells_from_file> init.txt </load_cells_from_file>
 		...
  </initial_configuration>
~~~ 

### Image and analyse a simulation
To visualize graphically the result of a simulation, with use the software Paraview (or you can also generate a `.svg` snapshot of the simulation). Analysis of the result files were done with python scripts proposed in this directory. For documentation on how to use Paraview to set-up the rendering of PhysiBoSS outputs, see [here](https://github.com/gletort/PhysiBoSS/wiki/Paraviewing), with the explication on how to draw spheres from a set of points (x, y, z, radius).

## Remarks

Please, refer to the [documentation section](https://github.com/sysbio-curie/PhysiBoSS/tree/master/doc/wiki) of this repository for a much more extended description, with step by step examples instructions. 
 
PhysiCell is developed in [Paul Macklin's lab](http://mathcancer.org) at Indiana University (Bloomington, USA).
MaBoSS and PhysiBoSS are developed in the [Computational Systems Biology of Cancer group](http://sysbio.curie.fr) at Institut Curie (Paris, France).

We invite you to use PhysiBoSS for you research and give feedbacks to us. Any help in developing it further is more than welcome.
Do not hesitate to contact us for any comments or difficulties in using PhysiBoSS: physiboss@gmail.com.

This repository can be referred to with its own DOI: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1194827.svg)](https://doi.org/10.5281/zenodo.1194827)

Wishing you to enjoy using PhysiBoSS, 

PhysiBoSS's team. 
