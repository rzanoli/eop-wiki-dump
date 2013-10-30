Differently to other git workflows, like the centralized workflow, in the adopted forking workflow when new developers want to start working on the project, they do not clone the official repository but create a personal copy of that repository by forking it. In this way they have a server-side copy, whereas to have a copy of the repository on their local machine they have to clone the repository (i.e. this is simply done by the git clone command). The developers work on their local copy of the software and then push their local commit to their own repository on GitHub. Finally, they do a pull request to let the maintainer of the platform know that a contribution is ready to be integrated. In turn, the maintainer checks the update to be sure that the changes do not break the project and merge it into the master branch.
In the rest of this section we will see how to get the code and how to commit the contributions (Section [Source Code and Contributions](#Getting_the_source_code_and_committing_the_contributions)), how to get the lexical resources (e.g. WordNet), the configuration files and the trained models needed to run the platform (Section [Configuration Files, Models and Resources](#Getting_the_resources_and_the_configuration_files)) whereas using Eclipse IDE to develop the EOP code it is another topic that has been discussed (Section [Using Eclipse IDE with EOP](#Using_Eclipse_IDE_with_EOP)). Finally we report some information about the Code Administration Policy (Section [Code Administration Policy](#Code_Administration_Policy)).



## <a name="Getting_the_source_code_and_committing_the_contributions"></a> Source Code and Contributions

The following are the steps that developers have to follow to gives their contribution. A prerequisite for this is to have your GitHub account and local git set up previously.


1. Forking the Excitement Open Platform repository  
From the EOP web page: https://github.com/hltfbk/Excitement-Open-Platform, click the fork button. It will create a copy (your own copy) of the EOP repository in GitHub


2. Cloning your fork  
By the following instructions you will make a copy of your fork and download the code on your local computer.

   <ul>
     <li> mkdir excitement_open_platform-git </li>
     <li> cd excitement_open_platform-git </li>
     <li> git clone git@github.com:user_name/Excitement-Open-Platform.git </li>
   </ul>

   where user_name is developer's user name on GitHub.


3. Configuring remotes  
When a repository is cloned, it has a default remote called origin that points to your fork on GitHub, not the original repository it was forked from. To keep track of the original repository, you need to add another remote named upstream:

   <ul>
     <li> git remote add upstream git@github.com:hltfbk/Excitement-Open-Platform.git </li>
   </ul>


4. Getting changes and merging  
If the original repository you forked your project from gets updated, you can add updates to your fork by the following code:

   <ul>
     <li> git fetch upstream (Getting changes from the original repo) </li>
     <li> git merge upstream/master (Merges changes fetched into your working files) </li>
   </ul>


5. Pushing commits to your remote repository stored on GitHub:
   <ul>
     <li> git push origin master </li>
   </ul>


6. Contributing to the project  
When you want to contribute to the original fork (i.e. EOP repository), you have to send to the administrators a pull request (click on the Pull request button available from the GitHub web page).



## <a name="Getting_the_resources_and_the_configuration_files"></a>Configuration Files, Models and Resources

Resources like WordNet and Wikipedia as well as the configuration files of the platform, the pre-trained models created training the platform on the data sets and the data sets themselves are distributed in a separated archive file that can be downloaded from:

* [[eop-resources-1.0.2.tar.gz | http://hlt-services4.fbk.eu:8080/artifactory/repo/eu/excitementproject/eop-resources/1.0.2.tar/eop-resources-1.0.2.tar.gz]]  

This archive contains internal/external libraries and resources whose licence is compatible with [[General Public License (GPL) version 3 | http://www.gnu.org/licenses/gpl.html]]. In contrast other libraries and resources with licences that are not compatible with this one have to be downloaded and installed separately from their own web site.  



## <a name="Using_Eclipse_IDE_with_EOP"></a> Using Eclipse IDE with EOP

The EOP project has been created as a Maven project. Eclipse with m2e plugin (http://eclipse.org/m2e/) can be used to manage it. The steps to load the project in Eclipse assumes that the EOP project has been already downloaded from its GitHub repository and that both Eclipse and the m2e plugin are already installed. The platform has been tested with Eclipse Juno (http://www.eclipse.org/juno/) and maven v1.3 (http://eclipse.org/m2e/) whereas these are the steps to follow to import the EOP project into Eclipse:

   <ul>
      <li> From the File menu in Eclipse select import </li>
      <li> Select Maven and then Exiting Maven Project </li>
      <li> Click the next button </li>
      <li> Select the root directory containing the pom.xml file; in this case: Excitement-Open-Platform </li>
      <li> Click the finish button </li>
   </ul>

The m2e plugin allows you to run a Maven build within Eclipse. A right click on the pom.xml file available from the eop folder and you can run one of the more common lifecycle phases like clean, install, or package. You can also load up the Run configuration dialog window and configure a Maven build with parameters and more options.



## <a name="Code_Administration_Policy"></a> Code Administration Policy

This section holds project code administration policy of EXCITEMENT open platform: things like what are accepted as contributions, and where the contributions should be included, and with whom you should contact for adding/improving codes. 

### Administrators 
Administrators are the people who have the right to merge code contributions back to EOP upstream. Currently, there are five EOP administrators. Here's the list and their main role for the first release. 

- Asher Stern (Bar Ilan University, BIUTEE EDA and related codes) 
- Roberto Zanoli (FBK, EDITS EDA and EOP development environment (Mvn, GitHub, etc)) 
- Rui Wang (DFKI, TIE EDA and German TE) 
- Tae-Gil Noh (Heidelberg University, German resources, LAP and UIMA type system)
- Amir H. Moin (DFKI, distribution release management)

If you are a developer who belongs to one of the institutions, the person of your institute is your administrator. If you are an external developer (who does not belong to one of the above four institutes), please contact the administrator based on the topics of the administrators.  

Note that administrators are responsible for the following two issues. 
+ Appropriateness of the contribution: For example, the new code is well located in CLC, etc  
+ Quality of the contribution: The code follows the coding standard and component policy of EXCITEMENT, as defined in the specification.  

It is recommended that a developer should contact the administrator before starting the actual development of a new code module. By doing so, the appropriateness of the contribution can be checked with the intention and goal of the contribution, even before the actual development. 

### Project Layout 
One project (maven module) for each EDA: As a default, we keep each EDA as independent module that depends on CLC. When the number of classes for a specific EDA is too small (less than 5), we can put it in the core.

### What kind of code can be part of EXCITEMENT open platform? 
Excitement Open Platform has a clear goal policy on inclusion of a code in the EOP project space. EOP project will include codes that implement the EXCITEMENT open specification: codes that are directly and indirectly used by components that implements EXCITEMENT specification. This means that generic NLP tools, as long as they are not related/used by a component, should not be included in the platform. 
Practically, consult one of the Administrators and ask him/her about adding a module/component in EOP. 


### Improving existing codes: To whom should I discuss?  
- If you are improving something in CLC: coordinate this with one of the administrator. If the issue is not simple, the administrator will discuss the issue with other administrators, to make a concrete decision. 
- If you are improving something not in CLC, but in a module provided by EOP code base: you have to discuss this with the module owner. Each of the module has one or more owners, basically noted in the JavaDoc. Module owners can decline your propose. If module owner is unclear, simply contact one of the administrators. 

In both cases, contacting an administrator as soon as possible and declare your intention and goal is a sound idea. This will make things much clearer for your contribution to be included in EOP. 

### Issue of License 
- Note that the Excitement Open Platform has "dual license" policy. Its main license is GPL v3, but all EOP contributions (codes that are stored in EXCITEMENT GitHub) can be released as LGPL. This dual license policy is to support industrial use cases where EOP has to be used with proprietary licensed code modules. 
- Each and every developer must accept this dual license policy. See [AcceptingLicensePolicy], for the information how you can express your acceptance. This must be done before merging any new contribution back to EOP. 
- If you code uses 3rd-party codes, please make sure you code is not breaking the license compatibility. (For example, linking a proprietary library in main release (GPL v3), can be done on your computer, but such code cannot be re-distributed to EOP main upstream). See [Platform license] for more detail. 
