The EOP source code is hosted on [[Github | https://github.com/hltfbk/Excitement-Open-Platform]] and managed using Git. The EOP GitHub repository consists of three main branches: the release branch is the branch containing the code that is supposed to be in a production-ready state whereas the master branch is the main branch containing the code to be incorporated into the next release. The gh-pages branch contains the EOP web site.

![EOP_Repository](https://raw.github.com/hltfbk/Excitement-Open-Platform/gh-pages/images/repository.jpg)

When the source code in the master branch reaches a stable point and is ready to be released, all of the changes are merged back into release. 



## EOP Code Structure
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

