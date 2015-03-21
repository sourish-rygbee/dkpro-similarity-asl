**DKPro Similarity** is an open source framework for text similarity. Our goal is to provide a comprehensive repository of text similarity measures which are implemented using standardized interfaces. The framework is designed to complement [DKPro Core](http://code.google.com/p/dkpro-core-asl), a collection of software components for natural language processing (NLP) based on the Apache UIMA framework. DKPro Similarity comprises a wide variety of measures ranging from ones based on simple n-grams and common subsequences to high-dimensional vector comparisons and structural, stylistic, and phonetic measures. In order to promote the reproducibility of experimental results and to provide reliable, permanent experimental conditions for future studies, DKPro Similarity additionally comes with a set of full-featured experimental setups which can be run out-of-the-box and be used for future systems to built upon.

This project contains the components that are licensed under the Apache Software License (ASL) version 2. Additional components are available in the [GPL-licensed repository](http://code.google.com/p/dkpro-similarity-gpl).

Versions:
  * The latest stable version is **2.1.0** of October 8, 2013.
    * Major bug fixes compared to somewhat unstable 2.0.0 release.
  * Previous versions:
    * Version **2.0.0** of September 26, 2013.
      * Change in the main version due to backwards incompatible changes in the package structure
      * A lot of bug fixes, additional measures, and experimental setups.
    * Version **1.0.1** of August 12, 2013.
  * Current development version: 2.2.0-SNAPSHOT

## Getting Started ##

Check out our [Getting Started guide](http://code.google.com/p/dkpro-similarity-asl/wiki/GettingStarted) and our [API documentation](http://dkpro-similarity-asl.googlecode.com/svn/apidocs/index.html). You may also want to have a closer look at our [ACL 2013 system demonstration paper](http://www.ukp.tu-darmstadt.de/fileadmin/user_upload/Group_UKP/publikationen/2013/ACL_Demo_2013_Dab_CameraReady.pdf) which summarizes the architecture, the available text similarity measures, and the existing experimental setups.

## Included Experiments ##
#### Word Pair Similarity Experiments ####

The project contains a ready-made experiment with the most common evaluation datasets for word pair similarity. [Learn more ...](WordPairSimilarity.md)

#### Word Choice Experiments / TOEFL Synonym Questions ####

Pipeline with datasets for word choice / TOEFL Synonym Question experiments.
[Wiki Page in ACL Wiki](http://www.aclweb.org/aclwiki/index.php?title=TOEFL_Synonym_Questions_%28State_of_the_art%29) on the topic.

#### Recognizing Textual Entailment (RTE) experiments ####

Pipelines and datasets for RTE 1-5 experiments.
[Wikki Page in ACL Wiki](http://aclweb.org/aclwiki/index.php?title=Recognizing_Textual_Entailment) on the topic.

#### `*`SEM 2013 Shared Task: Semantic Textual Similarity ####

For all users interested in the [Shared Task of the \*SEM 2013 conference](http://ixa2.si.ehu.es/sts/), we describe [here](SemEval2013.md) one of the task's offical baseline systems, which is roughly the system ranked best in the [SemEval-2012 exercises](http://ixa2.si.ehu.es/starsem/proc/pdf/STARSEM-SEMEVAL051.pdf).

## Reference ##
If you plan to refer to DKPro Similarity in your publications, please cite

Daniel Bär, Torsten Zesch, and Iryna Gurevych. **DKPro Similarity: An Open Source Framework for Text Similarity**, in _Proceedings of the 51st Annual Meeting of the Association for Computational Linguistics: System Demonstrations_, pages 121-126, August 2013, Sofia, Bulgaria. [(pdf)](http://aclweb.org/anthology/P/P13/P13-4021.pdf) [(bib)](http://aclweb.org/anthology/P/P13/P13-4021.bib)

DKPro Similarity is currently jointly maintained by [Language Technology Lab](http://www.langtech.inf.uni-due.de/), Universität Duisburg-Essen and [UKP Lab](http://www.ukp.tu-darmstadt.de/), Technische Universität Darmstadt.

## Code Example ##

In this example, we want to compute similarity between two given texts which are already lemmatized. We assume that lemmatization has already been done e.g. with a DKPro pipeline.
As a similarity measure, we choose a popular word n-gram model by Lyon et al. (2004). Moreover, make sure that both the _`*`.algorithms.api-asl_ and the _`*`.algorithms.lexical-asl_ dependency modules have been added to your pom.xml, as described in the [Getting Started Guide](http://code.google.com/p/dkpro-similarity-asl/wiki/GettingStarted).

```
// this similarity measure is defined in the dkpro.similarity.algorithms.lexical-asl package
// you need to add that to your .pom to make that example work
// there are some examples that should work out of the box in dkpro.similarity.example-gpl 
TextSimilarityMeasure measure = new WordNGramJaccardMeasure(3);    // Use word trigrams

String[] tokens1 = "This is a short example text.".split(" ");   
String[] tokens2 = "A short example text could look like that.".split(" ");

// only works from 2.1.0-SHAPSHOT onwards, for previous versions you need to convert to Collection<String> first
double score = measure.getSimilarity(tokens1, tokens2);

System.out.println("Similarity: " + score);
```

## Interfaces & Algorithms ##
The algorithms collected in this framework implement one of the following interfaces:
  * **TermSimilarityMeasure(String, String)** - Similarity between two terms.
  * **TextSimilarityMeasure(`Collection<String>`, `Collection<String>`)** - Similarity between two collections of strings representing whole documents.
  * **TextSimilarityMeasure(`String[]`, `String[]`)** - Similarity between two arrays of strings representing whole documents.
  * **JCasTextSimilarityMeasure(JCas, JCas)** - Similarity between two texts based on a UIMA JCas representation.

### Overview of Algorithms ###
| **Module** | **Algorithms** |
|:-----------|:---------------|
| algorithms.lexical | GreedyStringTiling, Jaro, Levenshtein, LongestCommonSubsequence, MongeElkan, NGramBased, ... |
| algorithms.lsr| Based on lexical-semantic resources such as WordNet or Wikipedia, e.g. GlossOverlap, JiangConrath, LeacockChodorow, Lin, Resnik, WuPalmerComparator |
| algorithms.sound | Metaphone, Soundex |
| algorithms.sspace | LSA |
| algorithms.style | FunctionWordFrequency, MTLD, TypeTokenRatio |
| algorithms.vsm | Vector-space models, e.g. ESA |
| algorithms.wikipedia | Special Wikipedia measures like, WikipediaLinkMeasure or measures based on the CategoryGraph. |
| dkpro.core | UIMA resources for the core algorithms. |
| dkpro.io | UIMA readers for the usual similarity datasets: Meter, RTE, SemEval, WebisCPC11 |

## Related Projects ##
  * [DKPro Core](http://code.google.com/p/dkpro-core-asl/)
  * [DKPro Lexical Semantic Resources](http://code.google.com/p/dkpro-lsr/)
  * [JWPL Wikipedia API](http://code.google.com/p/jwpl/)