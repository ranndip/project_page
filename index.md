### Authors
...

### Abstract
While machine learning approaches have been successfully used to represent interatomic potentials, their speed has typically lagged behind conventional formalisms. This is often due to the complexity of the structural fingerprints used to describe the local atomic environment and the large cutoff radii and neighbor lists used in the calculation of these fingerprints. Even recent machine learned methods are at least 10 times slower than traditional formalisms. An implementation of a rapid artificial neural network (RANN) style potential in the LAMMPS molecular dynamics package is presented here which utilizes angular screening to reduce computational complexity without reducing accuracy. For the smallest neural network architectures, this formalism rivals the modified embedded atom method (MEAM) for speed and accuracy, while the networks approximately one third as fast as MEAM were capable of reproducing the training database with chemical accuracy. The numerical accuracy of the LAMMPS implementation is assessed by verifying conservation of energy and agreement between calculated forces and pressures and the observed derivatives of the energy as well as by assessing the stability of the potential in dynamic simulation. The potential style is tested using a force field for magnesium and the computational efficiency for a variety of architectures is compared to a traditional potential models as well as alternative ANN formalisms. The predictive accuracy is found to rival that of slower methods.

### Method

$y=x^2$

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ranndip/project_page/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
