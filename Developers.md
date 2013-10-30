## EOP repository structure

The EOP source code is hosted on [[Github | https://github.com/hltfbk/Excitement-Open-Platform]] and managed using the Git. The EOP GitHub repository consists of two main branches: the release branch is the branch containing the code that is supposed to be in a production-ready state whereas the master branch is the main branch containing the code to be incorporated into the next release.

![EOP_Repository](https://raw.github.com/hltfbk/Excitement-Open-Platform/gh-pages/images/repository.jpg)

When the source code in the master branch reaches a stable point and is ready to be released, all of the changes are merged back into release. Developers, to commit to the repository, have to request write access permission by this mailing list: eop-dev@googlegroups.com
Then, they can start working with the EOP code following the steps reported below and following the policy always mentioned in the rest of this document.


## EOP code structure
Excitement open platform has various sub projects (sub-modules of Maven). For example, let's see EOP 1.0 sub-modules for example. 

* eop
    - common : Common module contains type systems and data structures that are needed by all submodules. The sub-module also includes all interface (EOP component and EDA APIs) definitions, too. If a data type (or type definition, like UIMA types), has to be shared by all of EOP submodules, that type must be defined in Common. 
    - core : Core modules contains EXCITEMENT components (like distance component, scoring component, lexical resources and syntactic resources). This is the natural place to put a reusable component implementation, that supports one of the defined EXCITEMENT component interface.  
    - lap : LAP (linguistic analysis pipeline) contains various linguistic annotation pipelines and related codes. This is the natural place to put a LAP pipeline code, that supports _LAPAccess_ interface. 
    - gui : this module holds various demo codes and GUI related codes. 
    - transformation : the module contains codes that are related to transformation of parse trees by using knowledge resources. For the moment, only used by BIUTEE, but it contains generic enough transformation tools that can be reused by other codes in the future. 
    - biutee : this module holds codes related to the EDA "BIUTEE". (Each EDA will have its own sub module if it needs more than 5 non-component classes.) 
    - pom.xml : it is the XML file that contains information about the project and configuration details used by Maven to build the EOP project.

You can expect additional Maven sub modules as the project grows. In any case, you will always see the "core sub-modules", that are _core_, _LAP_, and _common_ (CLC). 


### CLC (Core, LAP and Common): the core sub-modules  
CLC is the heart of EOP. Classes in CLC can be utilized by any code within Excitement open platform. 

- Common: interfaces and common data types
- Core: Excitement components 
- LAP: LAP (linguistic annotation pipeline) codes 

For a code to be included in CLC, it has to be _general_. This can be further broken into _naturally general_ and _practically general_. By "naturally general", it means that we need the code, and the platform will not work without the code (e.g. operating system utilities, common data structures). By "practically general", it means that the code is actually used by some components (components + EDAs), and can be used by other components, too. For example, the policy does not permit codes that "might be" useful in the future to be included in CLC (it is not practically general).  

This might sound complicated :-) But practically, this means that you should consult one of the Administrators about appropriateness of your "new component" before you try to develop something for CLC modules. Essentially, the administrators have final say on including something on CLC or not. 


## How to contribute to the EOP code

Differently to other git workflows, like the centralized workflow, in the adopted forking workflow when new developers want to start working on the project, they do not clone the official repository but create a personal copy of that repository by forking it. In this way they have a server-side copy, whereas to have a copy of the repository on their local machine they have to clone the repository (i.e. this is simply done by the git clone command). The developers work on their local copy of the software and then push their local commit to their own repository on GitHub. Finally, they do a pull request to let the maintainer of the platform know that a contribution is ready to be integrated. In turn, the maintainer checks the update to be sure that the changes do not break the project and merge it into the master branch.
These are the steps that a developer has to follow to gives their contribution:

Step 1: Fork the "Excitement Open Platform" repository; that will create a copy (your own copy) of the EOP repository. From the EOP web page: https://github.com/hltfbk/Excitement-Open-Platform, click the fork button.

Step 2: Clone your fork (i.e. making a copy of your fork and downloading the code)
   <ul>
     <li> mkdir excitement_open_platform-git </li>
     <li> cd excitement_open_platform-git </li>
     <li> git clone git@github.com:user_name/Excitement-Open-Platform.git </li>
   </ul>

where user_name is developer's user name on GitHub.

Step 3: Configure remotes (When a repository is cloned, it has a default remote called origin that points to your fork on GitHub, not the original repository it was forked from. To keep track of the original repository, you need to add another remote named upstream):
   <ul>
     <li> git remote add upstream git@github.com:hltfbk/Excitement-Open-Platform.git </li>
   </ul>

Step 4: If the original repository you forked your project from gets updated, you can add updates to your fork by the following code:
   <ul>
     <li> git fetch upstream (Getting changes from the original repo) </li>
     <li> git merge upstream/master (Merges changes fetched into your working files) </li>
   </ul>

Step 5: Pushes commits to your remote repository stored on GitHub:
   <ul>
     <li> git push origin master </li>
   </ul>

Step 6: When you want to contribute to the original fork (i.e. EOP repository), you have to send to the administrators a pull request (click on the Pull request button available from the GitHub web page).


## Jenkins, the continuous integration tool

In the previous section we have seen that, after a pull request made by a developer, the maintainer has to check  the request before merging it. Jenkins, the continuous integration tool we have adopted, monitors both the master branch in the official repository as well as the server-side copy of the developers. As a result the maintainer can check the report prepared by Jenkins for that contribute, both before merging it and after its integration in the master branch. In case of failure Jenkins notifies the maintainer and the developers’ team. Jenkins is available at this address: http://hlt-services4.fbk.eu:8080/jenkins/.

![EOP_Jenkins](https://raw.github.com/hltfbk/Excitement-Open-Platform/gh-pages/images/jenkins.jpg)


## Using Eclipse IDE with the EOP code

The EOP project has been created as a Maven project. Eclipse with m2e plugin (http://eclipse.org/m2e/) can be used to manage it. The steps to load the project in Eclipse assumes that the EOP project has been already downloaded from its GitHub repository and that both Eclipse and the m2e plugin are already installed. The platform has been tested with Eclipse Juno (http://www.eclipse.org/juno/) and maven v1.3 (http://eclipse.org/m2e/) whereas these are the steps to follow to import the EOP project into Eclipse:

   <ul>
      <li> From the File menu in Eclipse select import </li>
      <li> Select Maven and then Exiting Maven Project </li>
      <li> Click the next button </li>
      <li> Select the root directory containing the pom.xml file; in this case: Excitement-Open-Platform </li>
      <li> Click the finish button </li>
   </ul>

The m2e plugin allows you to run a Maven build within Eclipse. A right click on the pom.xml file available from the eop folder and you can run one of the more common lifecycle phases like clean, install, or package. You can also load up the Run configuration dialog window and configure a Maven build with parameters and more options.


## Code Administration Policy
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

## Supported Entailment Decision Algorithms (EDAs):
* 1. [[The BIUTEE EDA | BIUTEE]]
* 2. [[Edit Distance EDA | EditDistanceEDA]]
* 3. [[The TIE (MaxEntClassification) EDA | MaxEntClassificationEDA]]

## Lexical Resources:
* 1. [[Doc for the English knowledge resources | BIUTEE#db-based-knowledge-resources]]
* 2. [[Doc for the Italian knowledge resources | Italian Lexical Resources]]
* 3. [[Doc for the German knowledge resources | GermanLexicalResources]]

## How to add a new Linguistic Analysis Pipeline (LAP)? 
[[HowToAddANewLAP]] outlines the basic knowledge needed for this. It first introduces UIMA CAS, the adopted data representation for preprocessing (linguistic analyiss) result. And then the document describes how to add a new LAP module to the platform, by introducing _LAPAccess_ interface and _LAP_ImplBase_ class. 
@TODO: By Tae-Gil Noh, (ETA) 

##  How to add a new Entailment Decision Algorithm (EDA)?
@TODO: How to add new algorithms to the existing ones

##  How to use EOP as a library and make my code to use the EDAs
@TODO

## Coding Standards 
See [[Coding Guidelines]] for the project wide coding standards. 
