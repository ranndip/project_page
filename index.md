[LAMMPS implementation of rapid artificial neural network derived interatomic potentials](https://www.sciencedirect.com/science/article/pii/S0927025621002068)


### Authors
[Doyl Dickel](https://scholar.google.com/citations?hl=en&user=htw4EoMAAAAJ)

[Mashroor Nitol](https://scholar.google.com/citations?user=cyDU6TkAAAAJ&hl=en)

[Christopher Barrett](https://scholar.google.com/citations?hl=en&user=DMtfKn0AAAAJ)


### Abstract
An implementation of a rapid artificial neural network (RANN) style potential in the LAMMPS molecular dynamics package is presented here which utilizes angular screening to reduce computational complexity without reducing accuracy. For the smallest neural network architectures, this formalism rivals the modified embedded atom method (MEAM) for speed and accuracy, while the networks approximately one third as fast as MEAM were capable of reproducing the training database with chemical accuracy. The numerical accuracy of the LAMMPS implementation is assessed by verifying conservation of energy and agreement between calculated forces and pressures and the observed derivatives of the energy as well as by assessing the stability of the potential in dynamic simulation. The potential style is tested using a force field for magnesium and the computational efficiency for a variety of architectures is compared to a traditional potential models as well as alternative ANN formalisms. The predictive accuracy is found to rival that of slower methods. While machine learning approaches have been successfully used to represent interatomic potentials, their speed has typically lagged behind conventional formalisms. This is often due to the complexity of the structural fingerprints used to describe the local atomic environment and the large cutoff radii and neighbor lists used in the calculation of these fingerprints. Even recent machine learned methods are at least 10 times slower than traditional formalisms. An implementation of a rapid artificial neural network (RANN) style potential in the LAMMPS molecular dynamics package is presented here which utilizes angular screening to reduce computational complexity without reducing accuracy. For the smallest neural network architectures, this formalism rivals the modified embedded atom method (MEAM) for speed and accuracy, while the networks approximately one third as fast as MEAM were capable of reproducing the training database with chemical accuracy. The numerical accuracy of the LAMMPS implementation is assessed by verifying conservation of energy and agreement between calculated forces and pressures and the observed derivatives of the energy as well as by assessing the stability of the potential in dynamic simulation. 

### Artificial Neural Network Architecture
The energy of a particular atom <img src="https://latex.codecogs.com/svg.latex?i" title="i" />, determined by its environment, is the last of N layers of the neural network. The values for any particular layer, <img src="https://latex.codecogs.com/svg.latex?{\bf{A}}^n" title="{\bf{A}}^n" />,after the first is determined by the previous layer and the weight and bias matrices <img src="https://latex.codecogs.com/svg.latex?\mathbf{W}^n" title="\mathbf{W}^n" /> and <img src="https://latex.codecogs.com/gif.latex?\mathbf{B}^n" title="\mathbf{B}^n" />:

<img src="https://latex.codecogs.com/svg.latex?Z^n_{l_n}=\sum_{l_{n-1}}{W^n_{l_nl_{n-1}}A^{n-1}_{l_{n-1}}&plus;B^n_{l_n}}" title="Z^n_{l_n}=\sum_{l_{n-1}}{W^n_{l_nl_{n-1}}A^{n-1}_{l_{n-1}}+B^n_{l_n}}" />

<img src="https://latex.codecogs.com/svg.latex?A^n_{l_n}=g^n(Z^n_{l_n})" title="A^n_{l_n}=g^n(Z^n_{l_n})" />

Where <img src="https://latex.codecogs.com/svg.latex?l_n" title="l_n" /> is the number of neurons in layer <img src="https://latex.codecogs.com/svg.latex?n" title="n" /> and <img src="https://latex.codecogs.com/svg.latex?g^n(x)" title="g^n(x)" /> is a nonlinear activation function. The input layer, <img src="https://latex.codecogs.com/svg.latex?\mathbf{A}^0" title="\mathbf{A}^0" /> is given by the structural fingerprint of the local atomic environment. The ouput layer, <img src="https://latex.codecogs.com/svg.latex?\mathbf{Z}^N" title="\mathbf{Z}^N" />, will always contain a single node, representing the energy, and so <img src="https://latex.codecogs.com/svg.latex?\mathbf{W}^N" title="\mathbf{W}^N" /> will always be a row vector and <img src="https://latex.codecogs.com/svg.latex?\mathbf{B}^N" title="\mathbf{B}^N" /> always a single number.

For the RANN style, we use the fingerprint style is motivated by the Modified Embedded Atom Method (MEAM) formalism with the addition of angular screening. In this style, two different kind of input fingerprints are considered. First, simple pair interactions are considered and summed over all the neighbors of a given atom. For a given atom labeled <img src="https://latex.codecogs.com/svg.latex?i" title="i" />, we define a set of pair potentials interactions with the form:

<img src="https://latex.codecogs.com/svg.latex?F_n=\sum_{j\neq&space;i}{(\frac{r_{ij}}{r_e})^ne^{-\alpha_n\frac{r_{ij}}{r_e}}f_c(\frac{r_c-r_{ij}}{\Delta&space;r})S_{ij}}" title="F_n=\sum_{j\neq i}{(\frac{r_{ij}}{r_e})^ne^{-\alpha_n\frac{r_{ij}}{r_e}}f_c(\frac{r_c-r_{ij}}{\Delta r})S_{ij}}" />

Where <img src="https://latex.codecogs.com/svg.latex?j" title="j" /> labels all the neighbors atoms of <img src="https://latex.codecogs.com/svg.latex?i" title="i" /> within a cutoff radius <img src="https://latex.codecogs.com/svg.latex?r_c" title="r_c" />, <img src="https://latex.codecogs.com/svg.latex?n" title="n" /> is an integer, different for each member of the pairwise contributions to the fingerprint, <img src="https://latex.codecogs.com/svg.latex?r_e" title="r_e" /> is the equilibrium nearest neighbor distance, <img src="https://latex.codecogs.com/svg.latex?S_{ij}" title="S_{ij}" /> is an angular screening term, and <img src="https://latex.codecogs.com/svg.latex?\alpha_n" title="\alpha_n" /> are metaparameters, which can be tuned to better optimize the potential. 
The second kind of fingerprint function considers three body terms, with a form similar to the partial electron densities used in MEAM:

<img src="https://latex.codecogs.com/svg.latex?G_{m,k}=\sum_{j,k}{\cos^m{\theta_{jik}}e^{-\beta_k\frac{r_{ij}&plus;r_{ik}}{r_e}}f_c(\frac{r_c-r_{ij}}{\Delta&space;r})f_c(\frac{r_c-r_{ik}}{\Delta&space;r})S_{ij}S_{ik}}" title="G_{m,k}=\sum_{j,k}{\cos^m{\theta_{jik}}e^{-\beta_k\frac{r_{ij}+r_{ik}}{r_e}}f_c(\frac{r_c-r_{ij}}{\Delta r})f_c(\frac{r_c-r_{ik}}{\Delta r})S_{ij}S_{ik}}" />

where <img src="https://latex.codecogs.com/svg.latex?\theta_{jik}" title="\theta_{jik}" /> is the angle between <img src="https://latex.codecogs.com/svg.latex?r_{ij}" title="r_{ij}" /> and <img src="https://latex.codecogs.com/svg.latex?r_{ik}" title="r_{ik}" />, <img src="https://latex.codecogs.com/svg.latex?m" title="m" /> is a non-negative interger and <img src="https://latex.codecogs.com/svg.latex?\beta_k" title="\beta_k" /> is a set of metaparameters which determine the length scale of the different terms. 

A particular ANN potential will consist of a fixed structural fingerprint, number and length of each hidden layer, weight and bias matrices for each layer and activation functions for each layer. For the potentials considered here we have used the following activation functions:

<img src="https://latex.codecogs.com/svg.latex?g^N(x)=x&space;\label{eq:activation1}" title="g^N(x)=x \label{eq:activation1}" />

<img src="https://latex.codecogs.com/svg.latex?g^n(x)=\frac{x}{10}&plus;\frac{9}{10}\log(e^x&plus;1)\;&space;\textrm{for}&space;\;n<N" title="g^n(x)=\frac{x}{10}+\frac{9}{10}\log(e^x+1)\; \textrm{for} \;n<N" />

The size of the weight and bias matrices will depend on the length of the fingerprint and the length of each hidden layer.

Angular screening, whereby the effective interaction between atoms is reduced or eliminated by the presence of an atom located between them can effectively limit the neighbor list in a similar way without the unphysical results. Such a method has been utilized by MEAM and the same screening method has been employed here. Briefly, the screening between two atoms is determined from the product of all the screening interactions by other atoms in the neighborhood:

<img src="https://latex.codecogs.com/svg.latex?S_{ij}=\prod_{k\neq&space;i,j}S_{ikj}" title="S_{ij}=\prod_{k\neq i,j}S_{ikj}" />

where <img src="https://latex.codecogs.com/svg.latex?S_{ikj}" title="S_{ikj}" /> is calculated from a geometric construction considering the ellipse formed by the 3 atoms with <img src="https://latex.codecogs.com/svg.latex?r_{i,j}" title="r_{i,j}" /> one of the axes. The screening parameter, <img src="https://latex.codecogs.com/svg.latex?C_{ikj}" title="C_{ikj}" /> is then given by:

<img src="https://latex.codecogs.com/svg.latex?C_{ikj}=1&plus;2\frac{r^2_{ij}r^2_{ik}&plus;r^2_{ij}r^2_{jk}-r^4_{ij}}{r^4_{ij}-(r^2_{ik}-r^2_{jk})^2}" title="C_{ikj}=1+2\frac{r^2_{ij}r^2_{ik}+r^2_{ij}r^2_{jk}-r^4_{ij}}{r^4_{ij}-(r^2_{ik}-r^2_{jk})^2}" />

and then screening value is

<img src="https://latex.codecogs.com/svg.latex?S_{ikj}=f_c\left(\frac{C_{ikj}-C_{min}}{C_{max}-C_{min}}\right)" title="S_{ikj}=f_c\left(\frac{C_{ikj}-C_{min}}{C_{max}-C_{min}}\right)" />

where <img src="https://latex.codecogs.com/svg.latex?f_c" title="f_c" /> is the same cutoff function used for the radial cutoff and <img src="https://latex.codecogs.com/svg.latex?C_{max}" title="C_{max}" /> and <img src="https://latex.codecogs.com/svg.latex?C_{min}" title="C_{min}" /> are metaparameters which can be tuned to determine which neighbors can be excluded from calculations.

The effect of including angular screening can be demonstrating by considering the change of an individual fingerprint as the length scale is changed continuously. In the absence of angular screening, the value of the fingerprint will change rapidly at particular values of the lattice constant as new neighbors enter the radial screening distance. If angular screening is included with metaparameters such that, for example, only 3rd nearest neighbors are ever included regardless of lattice parameter, the change in the value is considerably smoother. 


### How to develop a RANN potential
**1.  DFT database**
A fairly large database is needed to ensure the wide range of atomic environments. A detail procedure can be found in our [publcations](https://www.sciencedirect.com/science/article/pii/S0927025620306984) Typically the user needs to have the following DFT simulations:

- _Stain:_ Strain of primitive/unitcell, up to +-10% from ideal, which consisting in volumetric, shear	and in-plane strain. 

- _Thermal:_ Simulation of thermally excited atoms along with changes in cell parameters (+-3% from ideal). Atoms displaced from their relaxed ground-state position by a randomly generated vector within a cutoff distance ranging from 0.1-0.5 angstrom. The temperature of a given system was estimated by the per atom difference of energy between the perturbed system and the ground state divided by Boltzmann constant . 

- _Free surface:_ Free surface of common planes, where atoms are perturbed randomly same as thermal data. 

- _Defects:_ Defects including interstitial, di vacancy in (1st, 2nd and 3rd nearest neighbor list), tri, quad and quin vacancies. 

- _Planer separation distance:_ To predict the decohesion energy and fracture behavior accurately, a dataset of common planer separation is required.

- _Amorphous:_ A amorphous dataset is required to predict melting temperature correctly. 

**2. Quantum espresso to LAMMPS dump format**

Once the user have the DFT database, the outputs are required to convert in LAMMPS dump format. A python code to convert Quantum espresso output to LAMMPS dump format is given in MISC repository.

**3. Training**

The calibration repository has c++ source code to train the data. Please go through the README file in that repository to compile the code, training input file and potential file description

**4.  LAMMPS installation and usage of potential**

pair_style rann requires the USER-RANN package. It is only enabled if LAMMPS was built with that package. Additionally, if any spin fingerprint styles are used LAMMPS must be built with the SPIN package as well.

**Installation with RANN package**

* Copy User-RANN folder to the LAMMPS src/ folder.
* make-serial/make-mpi to install lammps are the same as specified in the following page. https://lammps.sandia.gov/doc/Build_make.html

**How to use the potentials**

In your LAMMPS input script, specify 
```
pair_style rann
pair_coeff file Type1_element Type2_element Type3_element...
```
**Examples:**
```
pair_style rann
pair_coeff ** Mg.rann Mg
pair_coeff ** MgAlalloy.rann Mg Mg Al Mg
```
