This section first outlines the adopted XML configuration format with an example. Then, it iterates a few issues around configuration names and GLOBAL section (PlatformConfiguration).

Let's first see an example file that follows the proposed file format. It has a few sections (components) within it. Each name-values are represented with a property element. The element has one attribute (name), and actual value is written as element value within the property element.
  
  
<?xml version="1.0" encoding="utf-8"?>  
<!DOCTYPE configuration [  
<!ENTITY myVar "Some common #PCDATA that can be reused... ">  
]>  

\<configuration>  
\<section name="PlatformConfiguration">  
\<property name="activatedEDA">core.MyEDA\</property>  
\<property name="language">EN\</property>  
\</section>  
\<section name="PhoneticDistanceComponent">  
\<property name="KlingonDictionaryPath">$RESOURCE/VD/KDict.cvs\</property>  
\<property name="beta">0.1\</property>  
\<subsection name="instance1">  
\<property name="consonantScore">1.0\</property>  
\<property name="vowelScore">0.6\</property>  
\<property name="alpha">0.17663311\</property>  
\</subsection>  
\<subsection name="instance2">  
\<property name="consonantScore">0.6\</property>  
\<property name="vowelScore">1.0\</property>  
\<property name="alpha">0.17663311\</property>  
\</subsection>  
\</section>  
\<section name="core.MyEDA">  
\<property name="myLongKey">&myVar;\</property>  
\<property name="someOption">PhoneticDistanceComponent,EditDistanceComponent\</property>  
\</section>  
\</configuration>  
  
  

Note that,
* All section names must be unique (globally).
* All subsection names must be unique within the section.
* All property names are unique in each table (section/subsection)

If any violation is observed (including those and possible others), CommonConfig must raise an exception
while loading the file.

A small note on configuration value names and PlatformConfiguration (Global) region:

* PlatformConfiguration section: This global section of the common configuration holds some configuration data that will be shared among all components and EDAs. For the first prototypes, we will only adopt two values. activatedEDA holds the name of the EDA that is selected in this configuration. language holds the language of the current configuration.
* Default configuration: Each EDA implementer must provide a suitable default configuration, so the user can start using the EDA with relative ease.
* Names within a section (component): Each name within each component configuration section is independent. The component implementer can use the name that is suitable for the module. In the future, we might iterate over configuration names, and we will try to normalize common names, if such need arises.
