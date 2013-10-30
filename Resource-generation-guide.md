##Distributional similarity models


The _distsim_ project implements a distributional similarity tool. The input of the process is a corpus, defined in files (with an implementation of the [SentenceReader](https://github.com/hltfbk/Excitement-Open-Platform/wiki/_preview#sentence-reader) interface). The output of the process is composed of two Redis databases, per each generated distributional model. The generation process is characterized by a set of configuration files which defines the type of the distributional models, as well as various engineering aspects. 

A demonstration of the tool, with an executable jar, can be downloaded from the [[artifactory repository|http://hlt-services4.fbk.eu:8080/artifactory/webapp/browserepo.html?pathId=private-internal:BIU/exci-dist-sim/2/exci-dist-sim-2.rar]

For details on distributional similarity methods and a guide to the tool and its options see the [[user guide|https://github.com/hltfbk/Excitement-Open-Platform/wiki/Distsim-user-guide]]. The rest of this paper, describes the steps for generating the basic and common models for the EOP - Lin (proximity and dependency), balanced exclusion, and DIRT -, based on a given set of pre-defined configurations, with special focus on configuration parameters that might be tuned according to the given input corpus.

Note: 

The process makes use of a sort option which in not available in Windows. In addition, the current official distribution of Redis does not support Windows (Microsoft develops and maintains a Win32-64 experimental version of&nbsp;Redis), so it is assumed the the process will be applied on a Unix\Linux platform. In case, you problem with this, please let me know.

##Building distributional similarity model files



A set of running configurations with applicative scripts is provided with the demo.

As a first step, can use these configurations to be build four distributional models for the given corpus, by applying the build-model script, for each of the four model types:

```
>configurations/lin/build-model configurations/lin/proximity/
>configurations/lin/build-model configurations/lin/dependency/
>configurations/bap/build-model configurations/bap/
>configurations/dirt/build-model configurations/dirt/
```

The generated model files will be stored at the 'models' directory.

In order to build models, based on your corpus, you may need to modify some of the configurations, as follows.

###Main configuration parameters

####Input corpus

The demo contains a tiny corpus for English and German. 
You can/should define your own corpus, by setting the path of the corpus directory/file at the _corpus property_ of the _cooccurence-extractor_ module.

```
Models: lin, bap, dirt
File: coocuurence-extraction-memory.xml
Module: cooccurence-extractor
Property: corpus
Value: the path to your corpus dir/file
```

####Sentence reader

The eu.excitementproject.eop.distsim.builders.reader.SentenceReader interface defines the method for reading the next sentences from the corpus, and the representation of the read sentence, e.g., String (raw text), BasicNode (root of the parsed sentence). 

```
Models: lin, bap, dirt
File: coocuurence-extraction-memory.xml
Module: cooccurence-extractor
Property: sentence-reader-class
Value: the name of the SentenceReader class
```


There are various kinds of implementations for this interface, among them the following may be relevant for your corpus:

* **eu.excitementproject.eop.distsim.builders.reader.LineBasedStringSentenceReader**

Returns the next line of the given corpus, as a String.

* **eu.excitementproject.eop.distsim.builders.reader.SerializedNodeSentenceReader**

Returns a BasicNode representation of the next parsed sentence, by deserializating the next line of the stream.

* **eu.excitementproject.eop.distsim.reader.cooccurrence.CollNodeSentenceReader**

Returns a BasicNode representation of the next parsed sentence, by the converting the next lines from Conll representation to a BasicNode.

   Required configuration property: 

   **part-of-speech-class** - the name of a class which extends the eu.excitementproject.eop.common.representation.PartOfSpeech class, mapping a specific set of part-of-speeches into the canonical representation, defined by eu.excitementproject.eop.common.representation.CanonicalPosTag.


* **eu.excitementproject.eop.distsim.builders.reader.UIMANodeSentenceReader**

Returns a BasicNode representation of the next parsed sentence, given a UIMA Cas representation of parsed corpus.

Required configuration  property: 

**ae-template-file** a path for the analysis engine template file of the given UIMA Cas (otherwise, a default one will be selected).

* **eu.excitementproject.eop.distsim.builders.reader.UKwacNodeSentenceReader**

Returns a BasicNode representation of the next parsed sentence, given a UkWAC corpus.

Required configuration  property: 

** is-corpus-index** _true_ for a case of an index UkWac representation.

* **eu.excitementproject.eop.distsim.builders.reader.XMLNodeSentenceReader**

Returns a BasicNode representation of the next parsed sentence, given the EOP's serialization of parsed corpus (as defined in the eu.excitementproject.eop.common.representation.parse.tree.dependency.basic.xmldom.XmlTreePartOfSpeechFactory class).

Required configuration  property: 

**ignore-saved-canonical-pos-tag** _true_ if the part-of-speech representation ignores saved canonical tags (default, true).

The given configuration files in the demo, for instance, defines XMLNodeSentenceReader for the English corpus, and CollNodeSentenceReader for the Germnan one.

In case none of the above fits your corpus representation, you should implement your own SentenceReader class.

#### Method for co-occurrence extraction

The eu.excitementproject.eop.distsim.builders.cooccurrence.CooccurrenceExtraction interface defines the method for extracting co-occurrences from sentences (see [user guide](https://github.com/hltfbk/Excitement-Open-Platform/wiki/Distsim-user-guide#co-occurrence-extraction)). 

```
Models: lin, bap, dirt
File: coocuurence-extraction-memory.xml
Module: cooccurence-extractor
Property: extraction-class
Value: the name of the CooccurrenceExtraction class
```

There are various kinds of implementations for this interface:

* **eu.excitementproject.eop.distsim.builders.cooccurrence.NodeBasedWordCooccurrenceExtraction**
Extraction of co-occurrences, composed of lemma-pos pairs and their dependency relation, from a given parsed sentence, represented by a BasicNode (this class is currently defined in the demo configurations for the lexical models: bap and lin).

Required configuration  properties: **relevant-pos-list** - a (comma separated) list of part-of-speeches, which defines the relevant words for the model (where only words assigned to one of these part-of-speeches are considered). The part-of-speeches are defined by their canonical form, as defined in eu.excitementproject.eop.common.representation.partofspeech.CanonicalPosTag class.

* **eu.excitementproject.eop.distsim.builders.cooccurrence.RawTextBasedWordCooccurrenceExtraction**
Extraction of co-occurrences, composed of word pairs in of a given raw text sentence.

Required configuration properties: 

**window-size** the size of window in which word pairs are taken, e.g., for window-size 2, two words ahead and two words back are candidate pairs for the given word (default, 3).
    
**stop-words-file** the path to a text file, composed of stop words to be filtered from the model, word per line (default, no stop-word list).

* **eu.excitementproject.eop.distsim.builders.cooccurrence.NodeBasedPredArgCooccurrenceExtraction**
Extraction of co-occurrences, composed of predicates and arguments, from a given parsed sentence, represented by a BasicNode (this class is currently defined in the demo configurations for the syntactic model: dirt).

* **eu.excitementproject.eop.distsim.builders.cooccurrence.TupleBasedPredArgCooccurrenceExtraction**
Extraction of co-occurrences, composed of predicates and arguments, based on a given string, composed of a binary predicate and its arguments, in the format: arg1 \t predicate \t arg2

#### Method for element-feature extraction

The eu.excitementproject.eop.distsim.builders.elementfeature.ElementFeatureExtraction interface defines the method for extracting elements and features from co-occurrences (see [user guide](https://github.com/hltfbk/Excitement-Open-Platform/wiki/Distsim-user-guide#element-feature-counting)). 

The properties of the configured ElementFeatureExtraction  class are defined in a separated module, indicated by the element-feature-extraction-module property.
```
Models: lin, bap, dirt
File: element-feature-counting-memory.xml
Module: element-feature-extractor
Property: element-feature-extraction-module
Value: the name of the configuration module which defines the ElementFeatureExtraction class
```

The configuration module which defines the ElementFeatureExtraction should include at least one property - 'class' - which define the name of the ElementFeatureExtraction class. There are various kinds of implementations for this interface, some of them may requitre additional configuration properties (beside the 'class' property), as follows:

* **eu.excitementproject.eop.distsim.builders.elementfeature.LemmaPosBasedElementFeatureExtraction**
Given a co-occurrence of two items, each composed of lemma and pos, and their dependency relation, 
extracts two element-feature pairs where the element is the one lemma-pos and the feature is the other lemma-pos, with or without the dependency relation. (this option is currently configured in the demo for the lexical models: lin and bap).

Required configuration properties:

**stop-word-file** the path to a text file, composed of stop words to be filtered from the model features, word per line (default, no stop-word list).

**include-dependency-relation** should the dependency relation be included as part of the feature (as done in lin-dependency model) or not (as done in lin-proximity model).

**min-count** the minimal number of occurrences for each element (i.e., a word that occurs less then this minimal number would not form an element).

* **eu.excitementproject.eop.distsim.builders.elementfeature.WordPairBasedElementFeatureExtraction**
Given a co-occurrence of two items, each composed of a string word, extracts two element-feature pairs where the element is the element is one of the words and the feature is the other word, with no dependency relation.

Required configuration properties:

**stop-word-file** the path to a text file, composed of stop words to be filtered from the model features, word per line (default, no stop-word list).

**min-count** the minimal number of occurrences for each element (i.e., a word that occurs less then this minimal number would not form an element).

* **eu.excitementproject.eop.distsim.builders.elementfeature.BidirectionalPredArgElementFeatureExtraction**
Given a co-occurrence of predicate and argument and their dependency relation, extracts the predicate as an element, and the dependency relation with the argument as a feature (this option is currently configured in the demo for the syntactic model: dirt).

Required configuration properties:

**stop-word-file** the path to a text file, composed of stop words to be filtered from the model features, word per line (default, no stop-word list).

**slot** is it the first argument of the given binary predicate (X) or the second one (Y).

**min-count** the minimal number of occurrences for each element (i.e., a word that occurs less then this minimal number would not form an element).

The current configuration of the demo defines min-count 10 for lexical models (lin, bap) and 100 for the syntactic one (dirt).

###Storing the model files in Redis DB

In order to use the generated models, they should first be imported into a Redis db. 

Each of the lexical models is stored in two Redis dbs, one for left-to-right similarities, and the other right-to-left similarities. 

For each lexical model, run two redis processes (for the required l2r and r2l databases), with the corresponding configuration files, and run the File-to-redis script for this model

```
>redis/redis-server redis/configurations/lin/proximity/similarity-l2r.conf &
>redis/redis-server redis/configurations/lin/proximity/similarity-r2l.conf &
>configurations/lexical-model-to-redis configurations/lin/proximity/
```

```
>redis/redis-server redis/configurations/lin/dependency/similarity-l2r.conf &
>redis/redis-server redis/configurations/lin/dependency/similarity-r2l.conf &
>configurations/lexical-model-to-redis configurations/lin/dependency/
```

```
>redis/redis-server redis/configurations/bap/similarity-l2r.conf &
>redis/redis-server redis/configurations/bap/similarity-r2l.conf &
>configurations/lexical-model-to-redis configurations/bap/
```

Syntactic models are represented by one Redis DB, for left-2-right similarities. 

For each syntactic model, run the redis processes (for the required l2r database), with the corresponding configuration file, and run the File-to-redis script for this model

```
>redis/redis-server redis/configurations/dirt/similarity-l2r.conf &
>configurations/syntactic-model-to-redis configurations/dirt/
```


For each model, two Redis databases are now generated, one for left-to-right similarities and one for right-to-left similarities. The two databases for each model, are the rdb files, located at the model directory under redis/db  (redis/db/bap, redis/db/dirt, redis/db/lin/proximity, redis/db/dependency). 

Note: 

The process makes use of a sort option which in not available in Windows. In addition, the current official distribution of Redis does not support Windows (Microsoft develops and maintains a Win32-64 experimental version of&nbsp;Redis), so it is assumed the the process will be applied on a Unix\Linux platform. In case, you problem with this, please let me know.



###Usage of the models



####Running the Redis server

In order to access the generated models, you should first run a Redis server for each database of each model.

The redis-server program gets a configuration file, which defines various properties of the server. The database-specific properties are the path to the database (db-filename), the listening port of the server (port), the pid file (pid-file), the working directory (dir), and the swap file (vm-swap-file). 

The demo is provided with a set of Redis configurations for each database (rdb file) of each model. We intend to provide later, a tool which compose automatically generate unique configuration files for each model that is plugged-in to an EDA.

####Running a resource access program

The lexical models (e.g., lin and bap) can be accessed by the eu.excitementproject.eop.distsim.resource.SimilarityStorageBasedLexicalResource class, which implements the LexicalResource interface.

You can test your lexical models by applying the eu.excitementproject.eop.distsim.resource.TestLemmaPosSimilarity program, with the appropriate configuration file (the relevant Redis dbs should be first run, as described above).

The configuration file of the TestLemmaPosSimilarity  program, defines the host and the port of the model's Redis databases (l2r-redis-host, l2r-redis-port, r2l-redis-host, r2l-redis-port), where the host is the host where the redis-server program was run, and the port is the port that was configured for this run (as described above), as well as the number of retrieved rules for each query (top-n-rules), the type of the elements in the database (element-class), and the name of the resource (resource-name).

```
>redis/redis-server redis/configurations/lin/proximity/similarity-l2r.conf &
>redis/redis-server redis/configurations/lin/proximity/similarity-r2l.conf &
>java -cp distsim.jar eu.excitementproject.eop.distsim.resource.TestLemmaPosSimilarity configurations/lin/proximity/knowledge-resource.xml
```

```
>redis/redis-server redis/configurations/lin/dependency/similarity-l2r.conf &
>redis/redis-server redis/configurations/lin/dependency/similarity-r2l.conf &
>java -cp distsim.jar eu.excitementproject.eop.distsim.resource.TestLemmaPosSimilarity configurations/dependency/knowledge-resource.xml
```

```
>redis/redis-server redis/configurations/bap/similarity-l2r.conf &
>redis/redis-server redis/configurations/bap/similarity-r2l.conf &
>java -cp distsim.jar eu.excitementproject.eop.distsim.resource.TestLemmaPosSimilarity configurations/bap/knowledge-resource.xml
```

In case your model is not based on lemm-pos element, but on words (e.g., by configuring LineBasedStringSentenceReader and WordPairBasedElementFeatureExtraction, on a given raw text corpus), you can test it by applying the eu.excitementproject.eop.distsim.application.TestWordSimilarity program, with the appropriate configuration file (the relevant Redis dbs should be first run, as described above).

```
>redis/redis-server redis/configurations/lin/proximity/similarity-l2r.conf &
>redis/redis-server redis/configurations/lin/proximity/similarity-r2l.conf &
>java -cp distsim.jar eu.excitementproject.eop.distsim.application.TestWordSimilarity configurations/lin/proximity/knowledge-resource.xml
```

The dirt model can be accessed by the eu.excitementproject.eop.core.component.syntacticknowledge.SimilarityStorageBasedDIRTSyntacticResource class, which implements the SyntacticResource interface.

You can test your dirt model by applying the eu.excitementproject.eop.core.component.syntacticknowledge.TestDIRTSimilarity program, with the appropriate  configuration file (the relevant Redis dbs should be first run, as described above).

The configuration file of the TestDIRTSimilarity program, defines the host and the port of the model's Redis database (l2r-redis-host, l2r-redis-port), where the host is the host where the redis-server program was run, and the port is the port that was configured for this run (as described above), as well as the number of retrieved rules for each query (top-n-rules), the type of the elements in the database (element-class), and the name of the resource (resource-name).

```
>redis/redis-server redis/configurations/dirt/similarity-l2r.conf &
>java -cp distsim.jar eu.excitementproject.eop.core.component.syntacticknowledge.TestDIRTSimilarity configurations/dirt/knowledge-resource.xml
```


###Scaling to larger corpus


The current version is based on memory. For the case of English, lexical models based on the huge UkWAC corpus, and DIRT model based on the two CDs of Reuters, were generated with 64G RAM. For the case of German and Italian, 32G RAM seems to be sufficient.

When moving to larger corpus, the memory for the Java programs (in the build-model scripts) should be increased accordingly. 

In addition, the number of threads should be set according to the system hardware abilities.

In case, you have a memory problem, there is an option to apply a memory-free map reduce program. Contact us  (meni.adler@gmail.com) for more details.