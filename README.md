# TimeEvolvingMPO Development Guideline

Guideline aimed at developers that are part of the tempo collaboration.

## Contents:

1. Purpose of the TimeEvolvingMPO package
2. General development organisation
3. Installation of the public and private repositories
4. How the branches are organised
5. Maintaining the quality of the package
6. General design: API & backend
7. API design
8. How to contribute
- Appendix A: Contact information and project roles
- Appendix B: Links

## 1. Purpose of the TimeEvolvingMPO package
The purpose of the TimeEvolvingMPO package is to make TEMPO related algorithms publicly accessable and easy to use. It aims at theoretical and experimental physicists that have little or no knowledge on the technical details of TEMPO. It is also designed to be easily extendable with current and future TEMPO related algorithms. It achieves this by distinguishing between physical information and algorithm specific information. The objects that represent the physical information can then be processed by different TEMPO related algorithms, thus also allowing for a playground to compare the performance of the algorithms for specific physical problems. 


## 2. General development organisation
The package is set up as an open source project (Apache license 2.0) at <https://github.com/tempoCollaboration/TimeEvolvingMPO>, and as such hopes to attract contributors outside the tempo collaboration. In addition to this there is a non-public copy of this repository at <https://bitbucket.org/jkeeling/tempoincubato>, which has at least one additional `development` branch. This repository can only be accessed by people that are part of the tempo collaboration (see Appendix A) and serves as an incubator for new features that are not yet public. Once a new feature is ready for publication (possibly with an accompanying preprint on arXiv.org) the feature can be merged into the public `master` branch.

## 3. Installation of the public and private repositories
The installation of the public repository should be as easy as possible. This is currently the installation via PyPI
with:
```bash
$ python3 -m pip install time_evolving_mpo
```

For the installation of the private repository you will first need to get access to it, by contacting a current administrator  of the project (see Appendix A) and sending your email address that is linked to the bitbucket account you wish to use. 
For generating the documentation of the private repository or to set up the general development environment you will need `python==3.6`, `pip`, `git` and `tox` installed. 

There exists a video tutorial on how to get set up to use the public and private repositories. Email a current project administrator to grant you access to this video.

## 4. How the branches are organised
ToDo

## 5. Maintaining the quality of the package
ToDo

## 6. General design: API & backend
ToDo

## 7. API design
ToDo

## 8. How to contribute
ToDo

## Appendix A: Contact information and project roles
**Project administrators:**
- **Gerald E. Fux** (gf52@st-andrews.ac.uk)
- Jonathan Keeling (jmjk@st-andrews.ac.uk)

**Project maintainers:**
- **Gerald E. Fux** (gf52@st-andrews.ac.uk)
- Peter Kirton (peter.kirton@strath.ac.uk)

**Members of the TEMPO collaboration:**
- A subset of the scientific collaborators arround the authors of the original TEMPO paper [Strathearn2018].

[Strathearn2018] Strathearn, A., Kirton, P., Kilda, D., Keeling, J.
and Lovett, B.W., 2018. *Efficient non-Markovian quantum dynamics using
time-evolving matrix product operators.* Nature communications, 9(1), pp.1-9.

## Appendix B: Links
ToDo
