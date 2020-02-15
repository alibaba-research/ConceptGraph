## Alibaba  Conceptual Graph (AliCG)

[![License](https://img.shields.io/github/license/alibaba-research/ConceptGraph?style=flat-square)](https://github.com/alibaba-research/ConceptGraph/blob/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/alibaba-research/ConceptGraph?style=flat-square)](https://github.com/alibaba-research/ConceptGraph/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/alibaba-research/ConceptGraph?style=flat-square&color=blueviolet)](https://github.com/alibaba-research/ConceptGraph/network/members)

## Abstract 

Knowledge is important for text-related applications such as semantic search. Conceptual graphs, which is a particular type of Knowledge Graphs, plays an essential role in semantic search. Prior conceptual graph construction approaches typically extract high-frequent, coarse-grained, and time-invariant concepts from formal texts such as Wikipedia. In real applications, however, it is necessary to extract less-frequent, fine-grained, and time-varying conceptual knowledge and build taxonomy in an evolving manner.  In this paper, we introduce an approach to implementing and deploying the conceptual graph at Alibaba. Specifically, We propose a framework called **AliCG** which is capable of a) extracting fine-grained concepts by a novel bootstrapping with alignment consensus approach, b) mining long-tail concepts with a novel low-resource phrase mining approach, c) updating the graph dynamically via a concept distribution estimation method based on implicit and explicit user behaviors.   


## What is Alibaba Cognitive Concept Graph

In this paper, we introduce the Alibaba Conceptual Graph (AliCG), which is a large-scale conceptual graph of more than 5,000,000 fine-grained concepts, still in fast growth, automatically extracted from noisy search logs. As shown in Figure bellow, AliCG comprises four levels: **level-1** consists of concepts expressing the domain that those instances belong to;  **level-2** consists of concepts referred to the type or subclass of instances;  **level-3** consists of concepts that are the fine-grained conceptualization of instances expressing the implicit user intentions;  **instance layer** includes all instances such as entities and none-entity phrases.  

 ![cg](figs/alicg.jpg)

## Related Works 

There are several related works, and we display them in the table below:

| Knowledge Graph    | Fine-grained | Long-tail | Taxonomy |
|  ----  | ----  |----| ----  |
| YAGO | No |No|Static|
| Dbpedia | No |No|Static|
| Microsoft Concept Graph | No |No|Static|
| Tencent ConcepT | Yes |No|Static|
| Alibaba  Conceptual Graph | Yes |Yes|Dynamic|

## How do we construct? 

**First**, we propose a novel  **bootstrapping with the alignment consensus** approach to tackling the first challenge of extracting fine-grained concepts from noisy search logs. Specifically, we utilize a small number of predefined string patterns to extract concepts, which are then used to expand the pool of such patterns. Further, the new mined concepts are verified with query-title alignment; that is, an essential concept in a query should repeat several times in the document title frequently clicked by the user.  **Second**, we introduce a novel **conceptualized phrase mining and self-training with an ensemble consensus** approach to extract long-tail concepts. On the one hand, we extend the off-the-shelf phrase mining algorithm with conceptualized features to mine concepts unsupervisedly.  On the other hand, we propose a novel low-resource sequence tagging framework, namely, self-training with an ensemble consensus, to extract those scattered concepts.  **Finally**,  we propose a novel **concept distribution estimation method based on implicit and explicit user behaviors** to tackle the taxonomy evolution challenges. We employ concept alignment and take advantage of user's searching and clicking behaviors to estimate the implicit and explicit concept distributions to construct a four-layered concept--instance taxonomy in an evolving manner.  

 ![cg](figs/arc1.jpg)


## How to use?

The sample data of  conceptual graph is show bellow (separeted by \t):

```
盲探    {"kg_info": [{"hot": "367", "guid": "bd77432e-4654-11e5-8ba6-f80f41fb03aa"}], "concepts": [{"concept": "影视", "score": 1.0, "level": 1}, {"isA": "影视", "concept": "电影", "score": 1.0, "level": 2}, {"concept": "影视节目", "score": 0.91, "level": 3}, {"concept": "好看的搞笑电影", "score": 0.03, "level": 3}, {"concept": "犯罪悬疑的影视作品", "score": 0.01, "level": 3}, {"concept": "杜琪峰高人气作品", "score": 0.01, "level": 3}, {"concept": "经典的搞笑电影", "score": 0.01, "level": 3}, {"concept": "杜琪峰影视作品", "score": 0.01, "level": 3}, {"concept": "刘德华经典电影", "score": 0.01, "level": 3}, {"concept": "第33届香港金像奖入围影片", "score": 0.0, "level": 3}, {"concept": "同为杜琪峰导演高人气电影", "score": 0.0, "level": 3}, {"concept": "心理犯罪的影视作品", "score": 0.0, "level": 3}, {"concept": "郑秀文刘德华电影", "score": 0.0, "level": 3}, {"concept": "刘德华与郑秀文电影", "score": 0.0, "level": 3}]}
```


### Text Rewriting

 For each text instance $s$, we extract the concept  $c$ conveyed in the text, and rewrite the text by concatenating each instance with $s$. The rewritten text format is $s$ $c$. We use text rewriting for information retrieval. Note that text rewriting is easy to apply to other classification or sequence labeling tasks. 

### Concept Embedding

 As shown in Figure below, we utilize a two-tower neural network with concept attention and self-attention for learning concept embedding. Then we concatenate the concept embedded with text embedding for different applications such as intent classification and named entity recognition. We utilize key-value memory storage to store instance-concepts pairs for fast online serving. 

 ![cg](figs/concept_embedding.jpg)


### Conceptualized Pretraining

 As pretraining is quite powerful, the conceptual graph can be utilized during the pretraining stage to inject knowledge explicitly.  Specifically, we utilize instances and concept masking strategies with an auxiliary concept prediction loss to integrate conceptual knowledge. 


## Acknowledgment 

We also pay high respect to the "predecessors" of the concept graph include [Yago](https://www.mpi-inf.mpg.de/departments/databases-and-information-systems/research/yago-naga/yago/) from the Max Planck Institute in Germany,  [WordNet](https://wordnet.princeton.edu/) from Princeton University, [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) From google, [Microsoft Concept Graph](https://concept.research.microsoft.com/)  from Microsoft Research Asia,  [Big Cilin](http://www.bigcilin.com/browser/)  from  Harbin Institute of Technology,  [CN-dbpedia](http://kw.fudan.edu.cn/cndbpedia/intro/)   from Fudan University. 


## Disclaimer

This project is not the official product of Alibaba. The experimental results presented in the technical report only indicate the performance under the specific data set and super-parameter combination and do not represent the essence of each model. The experimental results may change due to random number seeds and computing devices. The content of this project is for technical research purposes only and is not intended to be a conclusive basis. The user may use the model arbitrarily within the scope of the license, but we are not responsible for any direct or indirect damages resulting from the use of the content of the project.

## References

[1] ERNIE 2.0: A Continual Pretraining Framework for Language Understanding

[2] ERNIE: Enhanced Language Representation with Informative Entities