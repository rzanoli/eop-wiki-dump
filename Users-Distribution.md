We provide different distributions for users depending on the fact that they want to get the code as a [Zip File](#Zip_Files), clone the code from the repository [Cloning the code from the GitHub repository](#Cloning_the_code_from_the_GitHub_repository) or they want to use the EOP [Maven Artifacts](#Getting_the_maven_artifacts). Basically the code in the Zip File and that obtained by cloning the project are two ways to obtain the same code whereas the maven artifacts are stored in the EOP maven repository and can be downloaded by Maven as dependencies. Common to all of the distributions is the necessity to download an additional archive file containing the resources like WordNet and Wikipedia and the configuration files needed to use the platform; this file can be downloaded from the EOP repository as described below in [Configuration Files, Models and Resources](#Getting_the_resources_and_the_configuration_files). Differently to users who want to use the platform as it is, people who want to contribute to developing the platform must use the distributions dedicated to the developers (i.e. Developers Distribution),



### <a name="Zip Files"></a> Zip File  
EOP v1.0.2 is now available for download.  
 
* [[Excitement-Open-Platform-1.0.2.zip | https://github.com/hltfbk/Excitement-Open-Platform/releases/tag/v1.0.2.zip]]  
* [[Excitement-Open-Platform-1.0.2.tar.gz | https://github.com/hltfbk/Excitement-Open-Platform/releases/tag/v1.0.2.tar.gz]]  

#### Previous releases
##### v1.0.0
* [[Code | ]]
* [[Resources | ]]
* [[Doc| ]]  

### <a name="Cloning_the_code_from_the_GitHub_repository"></a> Cloning the code from the GitHub repository

The following procedure lists the steps you would need to do to clone the last release of EOP; in the example we assume that the git tool (http://git-scm.com/) has been already installed on your machine.

```
1. mkdir excitement_open_platform-git
2. cd excitement_open_platform-git
3. git clone https://github.com/hltfbk/Excitement-Open-Platform/tree/v1.0.2
```


### <a name="Getting_the_maven_artifacts"></a> Maven Artifacts

EOP is also distributed via the EOP maven artifactory repository and the maven artifacts are located here. They include the binary code, the sources code and the javadoc jars. To use EOP in your project specify the following dependencies in your project:

```
<dependency>
  <groupId>..............</groupId>
  <artifactId>............</artifactId>
  <version>..............</version>
</dependency>
```

@TODO


### <a name="Getting_the_resources_and_the_configuration_files"></a> Configuration Files, Models and Resources
Resources like WordNet and Wikipedia as well as the configuration files of the platform, the pre-trained models created training the platform on the data sets and the data sets themselves are distributed in a separated archive file that can be downloaded from:

* [[eop-resources-1.0.2.tar.gz | http://hlt-services4.fbk.eu:8080/artifactory/repo/eu/excitementproject/eop-resources/1.0.2.tar/eop-resources-1.0.2.tar.gz]]  

This archive contains internal/external libraries and resources whose licence is compatible with [[General Public License (GPL) version 3 | http://www.gnu.org/licenses/gpl.html]]. In contrast other libraries and resources with licences that are not compatible with this one have to be downloaded and installed separately from their own web sites.  

