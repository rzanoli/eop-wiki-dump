# About using the TreeTagger 
There are several Linguistic annotation pipelines (LAPs) that runs TreeTagger to get annotations. However, due to the license incompatibility, TreeTagger cannot be shipped with EXCITEMENT platform. TreeTagger has its [[own licence|http://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/Tagger-Licence]], and it does not permit commercial usage: it only permits licensee to use the TreeTagger only for evaluation, research and teaching. It requires its users to agree the TreeTagger license, and download the needed files from [[TreeTagger homepage|http://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/]]. Also, it has to be installed in a specific manner, to be used by the EXCITEMENT platform. 

It sounds like a daunting task, but it is rather easy to install TreeTagger for EXCITEMENT; that is, once you agree and follow the TreeTagger license. 

# Installation steps 

EOP provides a script for the users who wants to install Tree Tagger. The script will help you to visit TreeTagger license, download TreeTagger files and install them as Maven modules. 

### Prerequisite: You need "Ant" make tool. 
If it is not installed in your system (try > "ant" in your command line), you can get it from Ant homepage. (http://ant.apache.org/)   

### Move to the script directory 
Within the source trunk, move to `/lap/src/scripts/treetagger/`. It has a README (this document) and an Ant build script (`build.xml`). The script is provided by DKPro. (Thanks! DKPro project members!) 

### Run ant script in the directory
Run Ant like followings:  
`> ant local-maven`
The script will first opens and let user to visit and read the TreeTagger license. The script will not proceed, unless you read and agree to TreeTagger license. 

Once the user confirms the agreement, the script will download and wrap the binary files and models as Maven modules, and install it on your local Maven repository. It will take some time. If you face some error, like "MD5SUM mismatch", please see the last section of this document. 

Once the script finishes successfuly, maven modules are now installed in the users local computer. The script will show you one more warning, after the successful installation. As the warning suggests, you cannot redistribute any of the generated Maven modules --- since it contains TreeTagger binaries and modules. They are strictly for your personal uses.  

### Edit `/lap/pom.xml` 
Finally, we need to tell Maven where to find TreeTagger binaries and models. This can be done by adding Maven dependencies. The pom file already have the dependency written, but they are commented out on the upstream code. Find 
`<!-- Tree tagger related dependencies -->` 
and un-comment the affected artifacts. They are four maven artifacts. Tree tagger binary itself, German, English and Italian models. It would look like the followings: 

    	<dependency>
     		<groupId>de.tudarmstadt.ukp.dkpro.core</groupId>
     		<artifactId>de.tudarmstadt.ukp.dkpro.core.treetagger-bin</artifactId>
     		<version>LATEST</version>
    	</dependency>
    	<dependency>
     		<groupId>de.tudarmstadt.ukp.dkpro.core</groupId>
     		<artifactId>de.tudarmstadt.ukp.dkpro.core.treetagger-model-de</artifactId>
     		<version>LATEST</version>
    	</dependency>
    	<dependency>
     		<groupId>de.tudarmstadt.ukp.dkpro.core</groupId>
     		<artifactId>de.tudarmstadt.ukp.dkpro.core.treetagger-model-en</artifactId>
     		<version>LATEST</version>
    	</dependency>
    	<dependency>
     		<groupId>de.tudarmstadt.ukp.dkpro.core</groupId>
     		<artifactId>de.tudarmstadt.ukp.dkpro.core.treetagger-model-it</artifactId>
     		<version>LATEST</version>
    	</dependency>

### Check that TreeTagger is actually working
Once you installed the artifacts and added the dependency in POM, it is not time to test. Open up Eclipse (or any tool), and run the unit test class of `TreeTaggerEnTest`. (In LAP module, src/test/java,
package eu.excitementproject.eop.lap.).  

Make sure that the class is not "skipped", but finishing Okay. The test code is designed to skip the test, if tree tagger binaries/models are not found, and generated an exception. If it shows actual
processing: now TreeTagger is installed and working correctly.  

# If an MD5 checksum error stops building of Ant Script 

TreeTagger binaries and models are often updated to a newer version, and stored in the same URL. Thus, MD5 checkshum fails on such case. Current ANT script is updated in June, 2013. 

In such a case, do one of the followings: 

1. update MD5 sum in the ant script: check the url is correct, and if you trust the updated binary, just update MD5 sum as the actual file in the website shows. 

2. notify the responsible person and get an updated version: contact noh@cl.uni-heidelberg.de (Tae-Gil Noh) for an updated ant script. 