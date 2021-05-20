
### Authors
Doyl Dickel

Mashroor Nitol

Christopher Barrett

### Abstract
An implementation of a rapid artificial neural network (RANN) style potential in the LAMMPS molecular dynamics package is presented here which utilizes angular screening to reduce computational complexity without reducing accuracy. For the smallest neural network architectures, this formalism rivals the modified embedded atom method (MEAM) for speed and accuracy, while the networks approximately one third as fast as MEAM were capable of reproducing the training database with chemical accuracy. The numerical accuracy of the LAMMPS implementation is assessed by verifying conservation of energy and agreement between calculated forces and pressures and the observed derivatives of the energy as well as by assessing the stability of the potential in dynamic simulation. The potential style is tested using a force field for magnesium and the computational efficiency for a variety of architectures is compared to a traditional potential models as well as alternative ANN formalisms. The predictive accuracy is found to rival that of slower methods. While machine learning approaches have been successfully used to represent interatomic potentials, their speed has typically lagged behind conventional formalisms. This is often due to the complexity of the structural fingerprints used to describe the local atomic environment and the large cutoff radii and neighbor lists used in the calculation of these fingerprints. Even recent machine learned methods are at least 10 times slower than traditional formalisms. An implementation of a rapid artificial neural network (RANN) style potential in the LAMMPS molecular dynamics package is presented here which utilizes angular screening to reduce computational complexity without reducing accuracy. For the smallest neural network architectures, this formalism rivals the modified embedded atom method (MEAM) for speed and accuracy, while the networks approximately one third as fast as MEAM were capable of reproducing the training database with chemical accuracy. The numerical accuracy of the LAMMPS implementation is assessed by verifying conservation of energy and agreement between calculated forces and pressures and the observed derivatives of the energy as well as by assessing the stability of the potential in dynamic simulation. 

### Artificial Neural Network Architecture
The energy of a particular atom _i_, determined by its environment, is the last of N layers of the neural network. The values for any particular layer, <img src="https://latex.codecogs.com/gif.latex?{\bf{A}}^n" title="{\bf{A}}^n" />,after the first is determined by the previous layer and the weight and bias matrices <img src="https://latex.codecogs.com/gif.latex?\mathbf{W}^n" title="\mathbf{W}^n" /> and <img src="https://latex.codecogs.com/gif.latex?\mathbf{B}^n" title="\mathbf{B}^n" />:
``
<img src="https://latex.codecogs.com/svg.latex?\Large&space;x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" title="\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />
``
