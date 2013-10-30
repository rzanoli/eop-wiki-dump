Configuration files are used to create and use a particular instance of an EDA. With them is for example possible to specify which resources the EDA has to use (e.g. WordNet), the data set to be annotated and so on. This section first outlines the adopted XML configuration format with an example. Then, it iterates a few issues around configuration names and GLOBAL section (PlatformConfiguration).
Let's first see an example file that follows the proposed file format. It has a few sections (components)
within it. Each name-values are represented with a property element. The element has one attribute
(name), and actual value is written as element value within the property element.

        <section name="PlatformConfiguration">
                <property name="activatedEDA">eu.excitementproject.eop.core.MaxEntClassificationEDA</property>
                <property name="language">DE</property>
        </section>
        
        <section name="BagOfWordsScoring">
        </section>
        
        <section name="BagOfLemmasScoring">
        </section>
        
        <section name="BagOfLexesScoring">
                <property name="withPOS">true</property>
                <property name="DerivBaseResource"></property>
        </section>
        
        <section name="DerivBaseResource">
         <property name="scoreInfo">true</property>
                <property name="scoreConfidence">0.01</property>
        </section>
        
        <section name="eu.excitementproject.eop.core.MaxEntClassificationEDA">
                <property name="modelFile">./src/main/resources/model/MaxEntClassificationEDAModel_Base+DBPos_DE</property>
                <property name="trainDir">./target/DE/dev/</property>
                <property name="testDir">./target/DE/test/</property>
                <property name="classifier">10000,1</property>
                <property name="Components">BagOfWordsScoring,BagOfLemmasScoring,BagOfLexesScoring</property>
        </section>


Note that,
* All section names must be unique (globally).
* All subsection names must be unique within the section.
* All property names are unique in each table (section/subsection)

A small note on configuration value names and PlatformConfiguration (Global) region:
* PlatformConfiguration section: This global section of the common configuration holds some
configuration data that will be shared among all components and EDAs. For the first prototypes,
we will only adopt two values. activatedEDA holds the name of the EDA that is selected in this
configuration. language holds the language of the current configuration.
* Default configuration: Each EDA implementer must provide a suitable default configuration, so the
user can start using the EDA with relative ease.
* Names within a section (component): Each name within each component configuration section is
independent. The component implementer can use the name that is suitable for the module. In the
future, we might iterate over configuration names, and we will try to normalize common names, if
such need arises.

The EDAs in the current release of the code use the configuration files in different ways: some EDAs have a few number of configuration files, each of them containing different instances of the EDA, whereas others have a configuration file for each of their instances.
