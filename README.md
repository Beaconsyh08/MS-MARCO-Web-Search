# Datasets for MS MARCO Web Search 

## Introduction

MS MARCO Web Search is a large-scale information-rich Web dataset, featuring millions of real clicked query-document labels. This dataset closely mimics real-world web document and
query distribution, provides rich information for various kinds of downstream tasks. It incorporates the largest open web document dataset, ClueWeb22, as our document corpus. 
ClueWeb22 includes about 10 billion high-quality web pages, sufficiently large to serve as representative web-scale data. It also contains rich information from the web pages, such as visual
representation rendered by web browsers, raw HTML structure, clean text, semantic annotations, language and topic tags labeled by industry document understanding systems, etc. MS MARCO Web
Search further contains 10 million unique queries from 93 languages with millions of relevant labeled query-document pairs collected from the search log of the Microsoft Bing search engine to serve
as the query set.

It offers a retrieval benchmark on 100 million document set with three web retrieval challenge tasks that demands innovations in both machine learning and information retrieval
system research domains: embedding model, embedding retrieval, and end-to-end retrieval challenges. The main goal of the leaderboard is to study what retrieval methods work best and what retrieval 
methods are cost-efficient when a large amount of data is available.

Moreover, MS MARCO Web Search also offers 5 times larger real click labels for the whole 10 billion document set. Researchers can use this dataset to verify whether methods that work on small data also work on large data.

## Citation

If you use the MS MARCO Web Search dataset, or any dataset derived from it, please cite the [paper](https://arxiv.org/abs/XXX):

@article{XXX,  
  title={MS MARCO Web Search: A Large-scale Information-rich Web Dataset with Millions of Real Click Labels},  
  author={Qi Chen, Xiubo Geng, Corby Rosset, Carolyn Buractaon, Jingwen Lu, Tao Shen, Kun Zhou, Chenyan Xiong, Yeyun Gong1, Paul Bennett, Nick Craswell, Xing Xie, Fan Yang, Bryan Tower, Nikhil Rao, Anlei Dong, Wenqi Jiang, Zheng Liu, 
  Mingqin Li, Chuanjie Liu, Jason Li, Rangan Majumder, Jennifer Neville, Andy Oakley, Knut Magne Risvik, Harsha Vardhan Simhadri, Manik Varma, Yujing Wang, Linjun Yang, Mao Yang, Ce Zhang},  
  journal={arXiv preprint arXiv:XXX},  
  year={2024}  
}  

## Tasks

There are three tasks: embedding model, embedding retrieval, and end-to-end retrieval rankings.

### Embedding Model Ranking Task

The first task focuses on embedding model ranking. The large-scale web data volume requires large embedding models to guarantee sufficient knowledge coverage. It requires balancing the following two goals: good model generalization ability
and efficient train/inference speed. Given a query, you are expected to rank documents from the full collection based on their relevance to the query. You can submit up to **100 documents** for this task. It models the embedding model quality. The metrics we evaluate include:
- Mean Reciprocal Rank (MRR): the average of the multiplicative inverse of the rank of the first correct result, which is widely used for evaluating the model quality.
- Recall: the average percentage of ground truth items (test query-document labels) recalled during the search.
- Throughput (QPS): All queries are provided at once, and we measure the wall clock time between the ingestion of the vectors and when all the results are output using all the threads in a machine. Then the throughput is calculated as the processed queries per second (QPS).
- Latency: we measure the 50, 90 and 99 percentile query latency at certain QPS.

|Baselines | MRR@10 | recall@1 | recall@5 | recall@10 | recall@20 | recall@100  | QPS | P50 latency | P90 latency | P99 latency |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|[DPR](https://github.com/facebookresearch/DPR)       | 0.542  | 45.12%  | 66.04%  | 72.10%   | 76.80%   | 87.54%     | 698 | 9.896 ms | 10.018 ms | 11.430 ms |
|[ANCE](https://github.com/microsoft/ANCE)      |0.633   | 54.18%  | 75.53%  | 80.53%   | 84.17%   | 91.17%     | 698 | 9.896 ms | 10.018 ms | 11.430 ms |
|[SimANS](https://github.com/microsoft/SimXNS/tree/main/SimANS)    |0.649   | 55.86%  | 76.84%  | 81.78%   | 85.23%   | 91.98%     | 698 | 9.896 ms | 10.018 ms | 11.430 ms |

### Embedding Retrieval Ranking Task

Embedding models need to co-work with the embedding retrieval system to serve a web scale dataset. The second task focuses on embedding retrieval algorithm/system performance and accuracy. We take the embedding vectors
generated by one of the baseline models as the ANN vector set. The goal of this challenge is to call for ANN algorithm innovations to minimize the accuracy gap between approximate search and
brute-force search while still preserving good system performance. In this task, we only evaluate ANN Recall (take the brute-force vector search results as the ground truth), Throughout and Latency.

|Baselines | ANN recall@1 | ANN recall@10 | ANN recall@100 | QPS | P50 latency | P90 latency | P99 latency  |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|[SPANN](https://github.com/microsoft/SPTAG) | 87.97% | 80.55% | 69.84% | 625 | 10.411 ms | 10.873 ms | 11.334 ms |
|[DiskANN](https://github.com/microsoft/DiskANN) | 91.46% | 87.07% | 69.73% | 2691 | 21.968 ms | 37.841 | 69.462 ms |

### End-to-end Retrieval Ranking Task

In the web scenario, the result quality and system performance of the end-to-end retrieval system are the most important metrics in comparing different solutions. This challenge task encourages any kind of solutions,
including an embedding model plus ANN system, inverted index solution, hybrid solution, neural indexer, and large language model, etc.

|Baselines | MRR@10 | recall@1 | recall@5 | recall@10 | recall@20 | recall@100  | QPS | P50 latency | P90 latency | P99 latency |
| --- |  :---: |  :---: |  :---: |  :---: |  :---: |  :---: |   :---: |   :---: |   :---: |   :---: | 
|[Elasticsearch BM25](https://github.com/elastic/elasticsearch) | 0.296 | 22.30% | 39.04% | 46.00% | 52.42% | 63.87% | 149 | 312.025 ms | 1065.141 ms | 3745.546 ms |
|[DPR](https://github.com/facebookresearch/DPR) + [SPANN](https://github.com/microsoft/SPTAG) | 0.467 | 39.21% | 56.66% | 61.27% | 64.69% | 70.28% | 625 | 21.924 ms | 23.017 ms | 34.217 ms |
|[ANCE](https://github.com/microsoft/ANCE)+ [SPANN](https://github.com/microsoft/SPTAG) | 0.580 | 49.87% | 68.59% | 72.94% | 75.86% | 80.18% | 625 | 21.924 ms | 23.017 ms | 34.217 ms |
|[SimANS](https://github.com/microsoft/SimXNS/tree/main/SimANS) + [SPANN](https://github.com/microsoft/SPTAG) | 0.585 | 50.63% | 68.79% | 73.14% | 75.85% | 79.82% |625 | 21.924 ms | 23.017 ms | 34.217 ms |

## Datasets

### 100M dataset

| Type   | Filename                                                                                                              | File size |              Num Records | Format                                                         |
|--------|-----------------------------------------------------------------------------------------------------------------------|----------:|-------------------------:|----------------------------------------------------------------|
| ClueWeb22 Collection                                | https://lemurproject.org/clueweb22.php/                            | --- |                         10B  | --- |
| Document ID in ClueWeb22 | [doc_hash_mapping.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/doc_hash_mapping.tsv) |     8.34 GB |               210,894,832  | tsv: docid in ClueWeb22, docid     |
| Train  | [queries_train.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/queries_train.tsv)  |     678.36 MB |                 9,206,475  | tsv: qid, query, languages                              |
| Train  | [qrels_train.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/qrels_train.tsv)      |   194.93 MB |                 9,346,695  | TREC qrels format                                              |
| Dev    | [queries_dev.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/queries_dev.tsv)        |       675.2 KB |                     9,253   | tsv: qid, query, languages      |
| Dev    | [qrels_dev.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/qrels_dev.tsv)          |    173.19 KB |                   9,402  | TREC qrels format                                              |
| Test    | [queries_test.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/queries_test.tsv)          |     734.33 KB |                   9,374  | tsv: qid, query, languages       |
| Test    | [qrels_test.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/qrels_test.tsv)          |   180.32 KB |                  9,374 | TREC qrels format       |
| Document Embedding Vectors | [vectors.bin](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/passage_vectors/vectors.bin), [metaidx.bin](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/passage_vectors/metaidx.bin), [meta.bin](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/passage_vectors/meta.bin) | 289.16GB | 100,924,960 | [Binary Format](https://github.com/microsoft/SPTAG/blob/main/docs/GettingStart.md#input-file-format) |
| Query Embedding Vectors | [vectors.bin](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/query_vectors/vectors.bin), [metaidx.bin](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/query_vectors/metaidx.bin), [meta.bin](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/query_vectors/meta.bin) | 27.47 MB | 9,374 | [Binary Format](https://github.com/microsoft/SPTAG/blob/main/docs/GettingStart.md#input-file-format) |
| Embedding Retrieval Truth | [truth.txt](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/vectors/SimANS/truth.txt) | 7.97 MB | 9,374 | [Truth Format](https://github.com/microsoft/SPTAG/blob/main/docs/GettingStart.md#input-file-format) |

### 10B dataset

| Description                                           | Filename                                                                                                                | File size |                        Num Records | Format                                                         |
|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|----------:|-----------------------------------:|----------------------------------------------------------------|
| ClueWeb22 Collection                                | https://lemurproject.org/clueweb22.php/                            | --- |                         10B  | --- |
| Train  | [queries_train.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/queries_train.tsv)  |     678.36 MB |                 9,206,475  | tsv: qid, query, languages                              |
| Train  | [qrels_train.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/10B_queries/qrels_train.tsv)      |   2.43 GB |          62,302,553         | TREC qrels format                                              |
| Dev    | [queries_dev.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/queries_dev.tsv)        |       675.2 KB |                     9,253   | tsv: qid, query, languages      |
| Dev    | [qrels_dev.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/10B_queries/qrels_dev.tsv)          |    2.35 MB |                   63,314  | TREC qrels format                                              |
| Test    | [queries_test.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/100M_queries/queries_test.tsv)          |     734.33 KB |                   9,374  | tsv: qid, query, languages       |
| Test    | [qrels_test.tsv](https://msmarco.z22.web.core.windows.net/msmarcowebsearch/10B_queries/qrels_test.tsv)          |   2.65 MB |                  40,511 | TREC qrels format       |


### Use of external information

IMPORTANT NOTE: You are allowed to use external information while developing your runs.
However, it is prohibited to use any datasets in your submission except those listed above.
The original MS MARCO Web Search dataset reveals minor details of how the dataset was constructed that would not be available in a real-world search engine; hence, should be avoided.

# Notice

## Terms and Conditions

The MS MARCO Web Search are intended for non-commercial research purposes only to promote advancement in the field of artificial intelligence and related areas, and is made available free of charge without extending any license or other intellectual property rights.
The datasets are provided "as is" without warranty and usage of the data has risks since we may not own the underlying rights in the documents.
We are not be liable for any damages related to use of the dataset.
Feedback is voluntarily given and can be used as we see fit.
By using any of the datasets you are automatically agreeing to abide by these terms and conditions.
Upon violation of any of these terms, your rights to use the dataset will end automatically.

Please contact us at ms-marco-web@microsoft.com if you own any of the documents made available but do not want them in this dataset.
We will remove the data accordingly.
If you have questions about use of the dataset or any research outputs in your products or services, we encourage you to undertake your own independent legal review.
For other questions, please feel free to contact us.

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE-CCA](LICENSE-CCA) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE](LICENSE) file.

Microsoft licenses the MS MARCO Web Search Mark "as-is" and makes no express or implied representations or warranties regarding non-infringement. You must remove all uses of the Mark immediately upon request from Microsoft.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at <http://go.microsoft.com/fwlink/?LinkID=254653>.

Privacy information can be found at <https://privacy.microsoft.com/en-us/>.

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.