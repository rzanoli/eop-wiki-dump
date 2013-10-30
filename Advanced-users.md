Please note that if you are not already familiar with the [[EOP]], then you are definitely at the wrong place! If you need a quick user manual, please read the [[Quick (15-minutes) manual | Quick manual for novice users]]. If you need a detailed user manual please read the [[detailed manual | Detailed-manual-for-novice-users]]. 

**NOTE: As soon as you face any issue, which might be a bug, i.e. a software defect, a feature which is not yet supported or any other problem that is caused due to the lack of clarity in the documentation, etc. please do not hesitate to report the issue by creating a new issue in the [[issue tracking system | https://github.com/hltfbk/Excitement-Open-Platform/issues]].**

***

## Supported Entailment Decision Algorithms (EDAs):
* 1. [[The BIUTEE EDA | BIUTEE]]
* 2. [[EditDistance EDA | Edit Distance EDA]]
* 3. [[The TIE (MaxEntClassification) EDA | MaxEntClassificationEDA]]

## Lexical Resources:
* 1. [[Doc for the English knowledge resources | BIUTEE#db-based-knowledge-resources]]
* 2. [[Doc for the Italian knowledge resources | Italian Lexical Resources]]
* 3. [[Doc for the German knowledge resources | GermanLexicalResources]]

## Lexical Acquisition tools

### Distributional similarity tool
* 1. [[Resource generation guide | resource generation guide]]
* 2. [[User guide | distsim user guide]]

### Wikipedia lexical miner tool

***

## The Revision Control System (Source Code Repository)

The [[EOP]] source code is hosted on [[Github | https://github.com/hltfbk/Excitement-Open-Platform]] and managed using the [[Git]] [[revision control system]]. To download the source code, please use a Git client and clone the repository using the command below:

git clone https://github.com/hltfbk/Excitement-Open-Platform.git

The release branch contains the source code of the latest release (the latest development source code is in the master branch). Then, build the [[EOP]] using the Maven command below:

mvn package assembly:assembly 

In order to get more information about the various EOP distribution types and their intended audience, please read [[this page | https://github.com/hltfbk/Excitement-Open-Platform/wiki/Various-eop-distributions]]. 

## How to Run the System from another program ##
@TODO: What if one wants to call/use an entailment engine from one's own Java program?

## The EOP-Resources Package
The [[EOP Resources package]] includes the [[knowledge resources]], the [[configuration files]], the [[models]], the [[datasets]], the [[databases]], etc. As an advanced user, you should have some basic knowledge about them.

### Configuration Files

#### [Overview of the common configuration (first draft)](https://github.com/hltfbk/Excitement-Open-Platform/wiki/Overview-of-the-common-configuration)
@TODO

#### [Configuration file format and syntax (first draft)](https://github.com/hltfbk/Excitement-Open-Platform/wiki/Configuration_files_format)
@TODO

#### Sample configurations? How to customize or develop users' own config?
@TODO

## Entailment Decision Algorithms (EDAs)

### What is an EDA?
An Entailment Decision Algorithm is a Component which takes a Text-Hypothesis pair in input and returns one of a small set of answers. A complete entailment recognition system is trivially an EDA. However, in the interest of re-usability, generic parts of the system should be made into individual Components. Entailment Decision Algorithms communicate with Components through generic specified interfaces.

### Which EDAs are already implemented in the EOP?
BIUTEE is a transformation-based EDA. BIUTEE applies a sequence of knowledge-based transformations, by which the text is transformed into the hypothesis. The system decides whether the text entails the hypothesis by observing the quality of this sequence. 

EditDistanceEDA implements edit distance algorithms. In the training phase it calculates a distance threshold that is applied during the test phase so that pairs resulting in a distance below the threshold are classiﬁed as ENTAILMENT, while pairs above the threshold are classiﬁed as NONENTAILMENT.

MaxEntClassificationEDA is an EDA based on a prototype system called TIE (Textual Inference Engine). It uses the OpenNLP MaxEnt package to train a GIS Model in order to classify Entailment T-H pairs from Non-Entailment ones. Currently, it works for both English and German. The compatible components are: 1) distance components 2) scoring components 3) lexical knowledge components.

### How to modify or extend EDAs?
@TODO