# Elasticsearch
- Currently, how our search engine works?
- How can we improve it?

# What is Elasticsearch?
- Built on Apache Lucene
- Instead of searching the text directly, it searches an index
- Logical Concepts
    - Index
    - Schema
    - Document
    - Inverted Index![How Inverted Index Works by knowi.com](https://www.knowi.com/wp-content/uploads/2020/03/inverse-index.webp)

# Why Elasticsearch?
- https://db-engines.com/en/ranking/search+engine
- https://typesense.org/typesense-vs-algolia-vs-elasticsearch-vs-meilisearch/
- Query Suggestion
- High availability
- Self-Hosted Option
- Query Field Weights & Boosting
- Negative Keyword Search (`-query`)

||Typesense | Algolia | ElasticSearch | Meilisearch|
|---|---|---|---|---|
|Source Code | Fully open source | Proprietary closed source | Source-available, licensed under SSPL | Fully open source|
|First Commit | 2015 | 2012 | 2010 | 2018|
|Built Using | C++ | C++ | Java | Rust|
|Core Search Algorithm | Built from the ground-up | Built from the ground-up | Built on top of Lucene | Built from the ground-up|
|Best Suited For | Instant Search-as-you-type Experiences for data sets that can fit in RAM, up to 24 TB (or current commercially available RAM size). | Instant Search-as-you-type Experiences for datasets up to 128 GB in size. | General-purpose search & aggregations over petabyte-scale datasets (eg: log data) | Instant Search-as-you-type Experiences for up to a few hundred thousand records, that don't require a production-grade highly-available setup.|
|Primary Index Location | RAM | RAM | Disk, with RAM cache | Disk with Memory Mapped files||
|Number of Documents | No limitation, only constrained by available RAM | Unknown | No limitation, only constrained by available disk space | Slow indexing performance reported for over 10K records.|
|Maximum Indices | No limitation | No limitation | No limitation | No limitation|
|Maximum Index Size | No limitations, only constrained by available RAM | 128 GB | No limitation | 100GB default, can be modified|
|Maximum Words per field | No limitation | No limitation | No limitation | 1000|
|Maximum Record Size | No limitation | 10KB | No limitation | 2GB|
|Number of API Keys | No limitation | 5000 | No limitation | 2|

# Logstash
- Logstash is a light-weight, open-source, server-side data processing pipeline that allows you to collect data from a variety of sources, transform it on the fly, and send it to your desired destination

# How Elasticsearch Works
## Analyzer
- Text analysis enables Elasticsearch to perform full-text search, where the search returns all _relevant_ results rather than just exact matches.
## Tokenizer
- Analysis makes full-text search possible through _tokenization_: breaking a text down into smaller chunks, called _tokens_. In most cases, these tokens are individual words.
- If you index the phrase `the quick brown fox jumps` as a single string and the user searches for `quick fox`, it isn’t considered a match. However, if you tokenize the phrase and index each word separately, the terms in the query string can be looked up individually. This means they can be matched by searches for `quick fox`, `fox brown`, or other variations.
## Normalizer
- Tokenization enables matching on individual terms, but each token is still matched literally. This means:
    - A search for `Quick` would not match `quick`, even though you likely want either term to match the other
    - Although `fox` and `foxes` share the same root word, a search for `foxes` would not match `fox` or vice versa.
    - A search for `jumps` would not match `leaps`. While they don’t share a root word, they are synonyms and have a similar meaning.
- To ensure search terms match these words as intended, you can apply the same tokenization and normalization rules to the query string
- The `normalizer` property of `keyword` fields is similar to `analyzer` except that it guarantees that the analysis chain produces a single token.

# [Field Types](https://www.elastic.co/guide/en/elasticsearch/reference/8.2/mapping-types.html)
- `keyword`: structured
    - emails, phone numbers, ids, tags
- `text`: human-readable contents
  - description of product, email body
- `range`
  - gt, lt
- ...

# Score
## [Context](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html)
- **Query context**
    - “_How well does this document match this query clause?_"
- **Filter context**
    - “_Does this document match this query clause?_”
    - The answer is a simple Yes or No
## [Queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html)
- `term`
    - **exact** query
    - not analyzed
- `match`:
    - matches each token with searched query
        - if a token is `resistor` and we search for `resist`, `resistor` will not be shown
    - it has a `fuzziness` option that finds more results:
        - Changing a character (box → fox)
        - Removing a character (black → lack)
        - Inserting a character (sic → sick)
        - Transposing two adjacent characters (act → cat)
        - this makes `match` a lot heavier
- `match_phrase`
    - **all the terms** must appear in the field
    - they must have the **same order** as the input value
    - there must not be any **intervening terms**
- `multi_match`
    - match on multiple fields
- `wildcard`
    - **contains** query
    - if a token is `resistor` and we search for `*resist*`, `resistor` will be shown
    - it's a lot heavier than `match`

# [Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html)
- Metric aggregations that calculate metrics, such as a sum or average, from field values.
- Bucket aggregations that group documents into buckets, also called bins, based on field values, ranges, or other criteria.
- Pipeline aggregations that take input from other aggregations instead of documents or fields.
