To understand the components and their interaction in EOP we just need to start from the EOP architecture in which the most important top-level distinction is between the Linguistic Analysis Pipeline (LAP) and the Entailment Core. We separate these two parts in order to (a), on a conceptual level, ensure that the algorithms in the Entailment Core only rely on linguistic analyses in well-defined ways; and (b), on a practical level, make sure that the LAP and the Entailment Core can be run independently of one another (e.g.,to preprocess all data beforehand).

Since it has been shown that deciding entailment on the basis of unprocessed text is a very difficult
endeavor, the Linguistic Analysis Pipeline LAP is essentially a series of annotation modules that provide linguistic annotation on various layers for the input. The Entailment Core then performs the actual entailment computation on the basis of the processed text.

The Entailment Core itself can be further decomposed into exactly one Entailment Decision Algorithm
*EDA* and zero or more *Components*. An Entailment Decision Algorithm EDA is a special Component which computes an entailment decision for a given Text/Hypothesis pair. Trivially, each complete Entailment Core is an EDA. However, the goal of EXCITEMENT is exactly to identify functionality within Entailment Cores that can be re-used and, for example, combined with other EDAs. Examples of functionality that are strong candidates for Components are WordNet (on the knowledge side) and distance computations between text and hypothesis (on the algorithmic side). Both of these can be combined with EDAs of different natures, and should therefore be encapsulated in Components.


### Supported Entailment Decision Algorithms (EDAs):
* 1. [[The BIUTEE EDA | BIUTEE]]
* 2. [[Edit Distance EDA | Edit Distance EDA]]
* 3. [[The TIE (MaxEntClassification) EDA | MaxEntClassificationEDA]]

### Lexical Resources:
* 1. [[Doc for the English knowledge resources | BIUTEE#db-based-knowledge-resources]]
* 2. [[Doc for the Italian knowledge resources | Italian Lexical Resources]]
* 3. [[Doc for the German knowledge resources | GermanLexicalResources]]
