# TimeEvolvingMPO Development Guideline

Guideline for developers that are part of the TEMPO collaboration.

**Contents:**

  1. Purpose of the TimeEvolvingMPO package
  2. General development organisation
  3. Installation of the public and private repositories
  4. How the branches are organised
  5. Maintaining the quality of the package
  6. General design: API & backend
  7. How to contribute
  - appendix A: Contact information and project roles
  - appendix B: Links and bibliography


## 1. Purpose of the TimeEvolvingMPO package
The purpose of the TimeEvolvingMPO package is to make TEMPO related algorithms publicly accessible and easy to use. It aims at theoretical and experimental physicists that have little or no knowledge on the technical details of TEMPO. It is also designed to be easily extendable with current and future TEMPO related algorithms. It achieves this by distinguishing between physical information and algorithm specific information. The objects that represent the physical information can then be processed by different TEMPO related algorithms, thus also allowing for a playground to compare the performance of the algorithms for specific physical problems.


## 2. General development organisation
The package is set up as an open source project (Apache license 2.0) at <https://github.com/tempoCollaboration/TimeEvolvingMPO>, and as such hopes to attract contributors outside the tempo collaboration. In addition to this there is a non-public copy of this repository at <https://bitbucket.org/jkeeling/tempoincubator>, which has at least one additional `development` branch. This repository can only be accessed by people that are part of the tempo collaboration (see appendix A) and serves as an incubator for new features that are not yet public. Once a new feature is ready for publication (possibly with an accompanying preprint on arXiv.org) the feature can be merged into the public `master` branch.

## 3. Installation of the public and private repositories
The installation of the public repository should be as easy as possible. This is currently the installation via PyPI
with:
```bash
$ python3 -m pip install time_evolving_mpo
```

For the installation of the private repository you will first need to get access to it by contacting a current administrator  of the project (see appendix A) and attaching your email address of the bitbucket account you wish to use.
For generating the documentation of the private repository or to set up the general development environment you will need `python==3.6`, `pip`, `git` and `tox` installed.

There exists a video tutorial on how to get set up to use the public and private repositories. Email a current project administrator (see appendix A) to grant you access to this video.

## 4. How the branches are organised
The main branch of the public repository is the `master` branch, i. e. all the public pull requests are merged into this branch. The `master` branch on the private repository is periodically updated by the `master` branch from the public repository and should therefore always contain identical commits. Next to the `master` branch the private repository has a `development` branch that contains the features that will soon be made public. In addition to this there can be several branches with names of the form `dev-feature-name` that contain a new features that are not yet on the verge of being published. When a new feature is ready to be published the corresponding `dev-feature-name` is merged into the `development` branch, which then in turn is merged into the master branch. It is therefore important not to park code in the development branch that can't be published, as this would hinder other features to be published when they are ready.

The `development` branch serves as a reference for all the feature branches, to make the interplay between them smooth and to avoid design conflicts as good as possible. This can be achieved by adding module, class, method and function headers with
```python
raise NotImplementedError()
```
statements into the `development` branch before creating a new feature branch and then filling it with actual code. This  allows developers of other features to be aware of the structure of other features already before they are actually coded.

Making decisions on the development structure as well as the code structure is the primary responsibility of the maintainers of the project (see appendix A).

## 5. Maintaining the quality of the package
All contributors of this project should be well aware of the difference between making a python script publicly available and a proper open source python package. For this project we aim to

- Make the package easily accessible:
  - make it available on PyPI
  - offer Tutorials (idealy for all features)
  - offer a documentation that gives a pedagogic overview of the most important functionality
  - offer a detailed documentation of all functionality
- Have a well thought through code structure:
  - a well defined API and backend
  - carefully choose what objects contain what sort of information and methods.
  - carefully choose about how large data (dynamics and process tensors) are stored
  - carefully choose about how computation intense parts are organised (checkpointing etc.)
- Guarantee stabil code performance:
  - using `pytest` and writing as many test cases as possible (aim at 100% coverage)
- Meet coding style standards:
  - using `pylint` to write code that is conform with [PEP08](https://www.python.org/dev/peps/pep-0008/).

The contribution guidelines [`CONTRIBUTING.md`](https://github.com/tempoCollaboration/TimeEvolvingMPO/blob/master/CONTRIBUTING.md) (for the public repository) / [`incubator-CONTRIBUTING.md`](https://github.com/tempoCollaboration/TimeEvolvingMPOdevGuideline/blob/master/incubator-CONTRIBUTING.md) (for the private repository) and [`PULL_REQUEST_TEMPLATE.md`](https://github.com/tempoCollaboration/TimeEvolvingMPO/blob/master/PULL_REQUEST_TEMPLATE.md) try to reflect these aims.


## 6. General design: API & backend
BBecause the TimeEvolvingMPO aims at being easily extendable with current and future TEMPO related algorithms it is necessary to carefully choose the design of the code. In the current approach is almost fully object oriented and there are essentially three layers:

1. The physical layer
2. The TEMPO algorithms layer
3. The backend layer

**The physical layer**: consists of objects that describe physical quantities, like a system (with a specific Hamiltonian) or the spectral density of an environment. To give an example, all spectral density classes need to be a derived from `BaseSD`, and therefore need to have a method to compute two-time correlations. A derived class that represents an ohmic spectral density `OhmicSD` could then, for example, use analytical expressions (that are specific to the ohmic case) to compute two-time correlation functions. Another derived class that represents a Lorentzian spectral density would encode the position and the width of the Lorentzian.

**The TEMPO algorithms layer**: Gathers the information from the physical layer and feeds it (with particular simulation parameters) into the backend. It has the opportunity to make use of specific, extra information from the physical layer. To get back to the example of the spectral density: while the original version of TEMPO only needs to know the correlation function of an environment and therefore treats all derived classes of `BaseSD` in the same way, a future version (or another feature) could treat the the case of a Lorentzian spectral density by performing a polaron transformation, rather then doing a full TEMPO computation.

**The backend layer**: Is the part of the code where the task has been reduced to a mathematically well defined computation, such as a specific tensornetwork or an integral. For each algorithm X there should exist a corresponding BaseXBackend that clearly defines the in and output of the backend for this algorithm. This allows to create more than one implementation of the computation intensive bits, which can then be easily chosen at runtime before starting the computation. This not only allows to compare the performance of different implementations, it also allows to write hardware specialised implementations (single CPU, cluster, GPU) while keeping the other two layers untouched.

The design of the API (application programming interface) is currently discussed in the issues section of the private repository: [API design](https://bitbucket.org/jkeeling/tempoincubator/issues/2)


## 7. How to contribute
There is a detailed description of how to contribute to the public repository in the file [`CONTRIBUTING.md`](https://github.com/tempoCollaboration/TimeEvolvingMPO/blob/master/CONTRIBUTING.md) and a checklist for pull requests
[`PULL_REQUEST_TEMPLATE.md`](https://github.com/tempoCollaboration/TimeEvolvingMPO/blob/master/PULL_REQUEST_TEMPLATE.md).

The contribution guideline for the private repository is exactly the same, except that it takes place on bitbucket rather then on github.


## Appendix A: Contact information and project roles

**Project administrators:**
- [**Gerald E. Fux**](https://github.com/gefux) (<gf52@st-andrews.ac.uk>)
- Jonathan Keeling (<jmjk@st-andrews.ac.uk>)
- Brendon W. Lovett (<bwl4@st-andrews.ac.uk>)

**Project maintainers:**
- [**Gerald E. Fux**](https://github.com/gefux) (<gf52@st-andrews.ac.uk>)
- [Peter Kirton](https://github.com/peterkirton) (<peter.kirton@strath.ac.uk>)

**Members of the TEMPO collaboration** (alphabetical)
- [**Gerald E. Fux**](https://github.com/gefux) (*University of St Andrews*) <gf52@st-andrews.ac.uk>
- **Erik Gauger** (*Heriot-Watt University*) <e.gauger@hw.ac.uk>
- **Jonathan Keeling** (*University of St Andrews*) <jmjk@st-andrews.ac.uk>
- [**Peter Kirton**](https://github.com/peterkirton) (*University of Strathclyde*) <peter.kirton@strath.ac.uk>
- [**Thibaut Lacroix**](https://github.com/tfmlaX) (*University of St Andrews*) <tfml1@st-andrews.ac.uk>
- **Brendon W. Lovett** (*University of St Andrews*) <bwl4@st-andrews.ac.uk>


## Appendix B: Links and bibliography

|                   |  link                                                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------ |
| Github            | <https://github.com/tempoCollaboration/TimeEvolvingMPO>                                          |
| Bitbucket         | <https://bitbucket.org/jkeeling/tempoincubato>                                                   |
| Documentation     | <https://TimeEvolvingMPO.readthedocs.io>                                                         |
| PyPI              | <https://pypi.org/project/time-evolving-mpo/>                                                    |
| Tutorial          | <https://mybinder.org/v2/gh/tempoCollaboration/TimeEvolvingMPO/master?filepath=tutorial.ipynb>   |
| API design        | <https://bitbucket.org/jkeeling/tempoincubator/issues/2>                                         |
| CONTRIBUTING.md   | <https://github.com/tempoCollaboration/TimeEvolvingMPO/blob/master/CONTRIBUTING.md>              |
| incubator-CONTRIBUTING.md | <https://github.com/tempoCollaboration/TimeEvolvingMPOdevGuideline/blob/master/incubator-CONTRIBUTING.md> |
| TEMPO paper       | <https://doi.org/10.1038/s41467-018-05617-3> or <https://arxiv.org/abs/1711.09641>               |

[Strathearn2018] Strathearn, A., Kirton, P., Kilda, D., Keeling, J.
and Lovett, B.W., 2018. *Efficient non-Markovian quantum dynamics using
time-evolving matrix product operators.* Nature communications, 9(1), 3322.
