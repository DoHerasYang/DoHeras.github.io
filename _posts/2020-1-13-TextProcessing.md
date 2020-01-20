---
layout: post
title: 'Text Processing'
date: 2020-01-13
author: DoHerasYang
color: rgb(255,222,32)
cover: ''
tags: Lecture-Course



---

# Text Processing - Lecture

>The content of this blog is based on the course COM6115(Text Processing (AUTUMN 2019~20)) at the University of Sheffield.
>
>**@All the konowledge rights are reserved by the owner of this course's materials,Dr Mark Hepple and Dr Chenghua Lin at the University of Sheffield.**

---

[toc]

---

### 1. IR(Information Retrieval)

Information Retrieval (IR): concerned with developing algorithms and models for retrieving relevent documents from text collections.

+ Text collection = some set of ‘documents’ / Query: user indication of what s/he want。
+ Purpose:  Finding pages that contain the words in the query by “relevance” to the query and clever indexing.

#### Issues:

+ How can I formulate a query? 

  Normally keywords, could be natural language.

+ How are the documents represented? 

  indexing

+ How does the system find the best-matching document? 

  retrieval model

+ How are the results presented to me?

  unsorted list, ranked list, clusters

+ How do we know whether the system is any good?

  evalution

#### Indexing:

The task of finding terms that describe documents well

+ Manual: indexing by humans (using fixed vocabularies) / labour and training intensive
+ Automatic: Term manipulation (certain words count as the same term) / Term weighting (certain terms are more important than others) / Index terms must only derive from text

> MeSH — Medical Subject Headings (Manuall indexing)
>
> a very large controlled vocabulary for describing/indexing medical documents, e.g. journal papers and books. It provides a hierarchy of descriptors (a.k.a. subject headings).
>
> hierarchy has a number of top-level categories, e.g.:
>  	Anatomy [A]
>  	Organisms [B]
>  	Diseases [C]
> 	 Chemicals and Drugs [D]
>  	Analytical, Diagnostic and Therapeutic Techniques and Equipment [E]
>  	Psychiatry and Psychology [F]
>  	Biological Sciences [G]
>
> Evalution:
>
> + Advantage:  1)High precision searches 2)Works well for closed collections (books in a library).
> + Disadvantage: 1)Searchers need to know terms to achieve high precision. 2)Labellers need to be trained to achieve consistency. 3)Collections are dynamic → schemes change constantly.

> Automatic Indexing:
>
> + Use the <u>natural language</u> as indexing language
>
> + Implementation:  <u>inverted files</u> 
>
>   + record each term, the <u>ids</u> of thedocuments in which it appears
>   + only matters if it <u>does</u> or <u>does not</u> appear - not how many times
>   + (doc_id)
>
>   ![01](/Pictures/Text Processing/01.png)
>
>   + also recoed the count of occurrences within each document.
>   + help find documents more relevant to query.
>   + (doc_id : occurance_number )
>
>   ![02](/Pictures/Text Processing/02.png)

### Automated retrieval models:

#### Bag-of-words Approach

+ Standard approach to represent the documents
  + record what words are present
  + Usually, plus count of term in each document
+ Ignore the relation between words
  + order of the words / permutation of the words

#### Terms

Common to just use the words, but pre-process them for generalisation.

+ Method:

  + Tokenisation: split words from punctuation (get rid of punctuation)

    + e.g. word-based. → word based   three-issues: → three issues

  + Capitalisation: normalise all words to lower (or upper) case

    + e.g. Cat and cat should be seen as the same term, but should we conflate Turkey and turkey

  + Lemmatisation: conflate different inflected forms of a word to their basic form (singular, present tense, 1st person):

    + e.g. cats, cat → cat  have, has, had → have  worried, worries → worry

  + Stemming:

    + conflate morphological variants by chopping their affix

  + Normalisation: heuristics to conflate variants due to spelling, hyphenation, spaces, etc.

    + e.g. USA and U.S.A. and U S A → USA

      e.g. chequebook and cheque book → cheque book

      e.g. word-sense and word sense → word-sense

+ Problem of Single and Multi-words

  + To aid recognition of phrases, might allow multi-word terms
  + Solution
    + identify multi-word phrases during retrieval
    + Positional indexes, storing position terms in documents, can help

- Positional indexs:
  + ![07](/Pictures/Text Processing/07.png)
  + Approach:
    + Binary weights - 0/1: whether or not term is present in document
      + But documents with multiple occurrences of query keyword may be more relevant
    + Frequency of term in document: like the examples we have seen
      + Common terms: not very useful for discriminating relevant documents
    + Frequency in document vs in collection: weight terms highly if
      + They are frequent in relevant documents . . . but
      + They are infrequent in collection as a whole

#### Stop List

+ Use Stop list removal to exclude “non-content” words
+ Usually most frequent (and least useful for retrieval)
+ Effect:
  + greatly reduces the size of the inverted index

#### IR Method

> Boolean Method(Binary)
>
> + Purpose: 
>   + evaluate the relevance and suffience of term for match
>   + provides a simple logical basis for deciding whether any document should be returned.
>     + whether basic terms of query do/do not appear in the document
>     + the meaning of the logical operators
>
> + Algorithm:
>
>   + frequency of document terms
>   + not all search terms necessarily present in document
>   + vector space model
>
> + Approach:
>
>   + Basic search terms(keywords)
>   + using boolean operators
>
> + Boolean Operators:
>
>   + AND, OR, NOT, BUT, XOR (exclusive OR)
>   + E.g.:  Monte-Carlo AND (importance OR stratification) BUT gambling
>
> + Set-theoretic interpretation:
>
>   + $d(E)$ denote the document set for expression E
>   + $E$ either a basic term or boolean expression
>     + $d(E1 \,AND\, E2) = d(E1) ∩ d(E2)$
>     + $d(E1 \,OR\, E2) = d(E1) ∪ d(E2)$
>     + $d(NOT\, E) = d(E)^c$
>     + $d(E1 \,BUT\, E2) = d(E1) − d(E2)$
>   + Some example:
>     + ![03](/Pictures/Text Processing/03.png)
>     + ![04](/Pictures/Text Processing/04.png)
>     + ![05](/Pictures/Text Processing/05.png)
>
> + Conclusion:
>
>   + Advantage:
>     + Higher requirements for user: Expert knowledge needed to create high-precision queries
>     + Often used by bibliographic search engines (library)
>
>   + Disadvantage:
>     + Most users not familiar with writing Boolean queries → not natural
>     + Most users don’t want to wade through 1000s unranked result lists →
>       unless very specific search in small collections
>     + This is particularly true of web search → large set of docs

> Vector Space Model
>
> + Description:
>
>   + Documents are points in high-dimensional vector space
>     + each term in index is a dimension → sparse vectors
>     + values are frequencies of terms in documents, or variants of frequency
>   + Queries are also represented as vectors (for terms that exist in index)
>
> + Priori:
>
>   + Select document(s) with highest document–query similarity
>   + Document–query similarity is a model for relevance (ranking)
>   + With ranking, the number of returned documents is less relevant → users start at the top and stop when satisfied
>
> + Approach:
>
>   + compare vector of query against vector of each document
>
>     + to rank documents according to their similarity to the query
>     + ![06](/Pictures/Text Processing/06.png)
>
>   + Each document and query are represented as a vector of $n$ values:
>
>     $\vec{d^i} = (\vec{d_1^i},\vec{d_2^i},\vec{d_3^i}, \cdots,\vec{d_n^i} )$         $\vec{q} = (q_1,q_2,q_3, \cdots , q_n)$
>
>   + Similarity:
>     $$
>     \sqrt{\sum\limits_{k=1}^N}(q_k - d_k)^2
>     $$
>
>     + E.g.:
>
>       + Doc1 and Q
>
>         $\sqrt{(9-0)^2 + (0-1)^2 + (1-0)^2 + (0-1)^2} = 9.15$
>
>       + Doc2 and Q
>
>         $\sqrt{(0-0)^2 + (1-1)^2 + (0-0)^2 + (10-1)^2} = 9.15$
>
>   + Evalution:
>
>     + distance is large for vectors of different lengths, even if by only one
>       term (e.g. Doc2 and Q)
>
>     + means frequency of terms given too much impact
>
>     + Better similarity metric, used in vector-space model: cosine of the angle between two vectors $\vec{x}$ and $\vec{y}$: ( For the each value of text and query, when the term occurs, the value should be $1$ and if not, the value should be $0$ for each $x_i$ and $y_i$ )
>       $$
>       cos(\vec{x},\vec{y}) = \frac{\vec{x} \cdot \vec{y}}{\left|\vec{x}\right|    \left|\vec{y}\right|} = \frac{\sum\limits_{i=1}^{n}x_iy_i}{\sqrt{\sum\limits_{i=1}^{n}x^2}\sqrt{\sum\limits_{i=1}^{n}y^2}}
>       $$
>
>       + It can be interpreted as the <u>normalised correlation coefficient</u>:
>
>         i.e. it computes how well the $x_i$ and $y_i$ correlate, and then divides by the
>         length of the vectors, to scale for their magnitude.
>
>       + Value:
>
>         + range from:
>           + 1, for vectors pointing in the same direction, to
>           + 0, for orthogonal vectors, 
>           + -1, for vectors pointing in opposite directions

>  **Inverse document frequency**( <u>**IDF**</u> )
>
>  Term Weighting
>
>  + ![08](/Pictures/Text Processing/08.png)
>
>  + informativeness of terms
>
>   + Idea that less common terms are more useful to finding relevant docs
>   + document frequency(df) reflects this difference
>   + collection frequency(cf) fails to distinguish them (i.e. very similar counts)
>
>  + Informativeness is inversely related to (document) frequency
>
>   + less common terms are more useful to finding relevant documents
>   + more common terms are less useful to finding relevant documents
>
>  + Compute
>
>  $$
>   \frac{\left|D\right|}{df_w}
>  $$
>
>   + Value reduces as $df_w$ gets larger, tending to 1 as $df_w$ approaches $|D|$
>
>   + Value very large for small dfw— over-weights such cases
>
>   + To moderate this, take $\textbf{log}$: Inverse document frequency( <u>**IDF**</u> )
>     $$
>     idf_{w,D} = log\frac{\left|D\right|}{df_w}
>     $$
>
>     + ![09](/Pictures/Text Processing/09.png)
>     + ![10](/Pictures/Text Processing/10.png)
>
>   + BUT Not all terms describe a document equally well
>
>     + Putting it all together: **tf.idf**
>
>       + Terms which are frequent in a document are better:
>         $$
>         \mathbf{tf_{w,d} = freq_{w,d}}
>         $$
>
>       + Combine the two to give tf.idf term weighting:
>         $$
>         \mathbf{tf.idf_{w,d,D} = tf_{w,d}\cdot\,idf_{w,D}}
>         $$
>
>       + e.g.
>
>         ![11](/Pictures/Text Processing/11.png)
>
>       + e.g.
>
>         ![12](/Pictures/Text Processing/12.png)
>
>         ![13](/Pictures/Text Processing/13.png)

> PageRank Algorithm
>
> + Key method to exploit link structure of web: PageRank algorithm
>   + Assigns a score to each page on web: its PageRank score
>   + can be seen to represent the page’s authority (or quality)
> + Idea:
>   + Link from page A to page B confers authority on B
>   + how much authority is conferred depends on:
>     + the authority (PageRank score) of A, and its number of out-going links
>     + i.e. A’s authority is shared out amongst its out-going links
>   + note that this measure is recursively defined
>     + i.e. score of any page depends on score of every other page
>   + PageRank scores have an alternative interpretation:
>     + probability that a random surfer will visit that page
>   + its PageRank score: a measure of its authority

#### Summary

+ Component / technique

  + Ranking (cosine, dot-product, . . . )
  + Term selection  (stopword removal, stemming, . . . )
  + Term weighting  (binary, TF, TF.IDF, . . . )

+ Relevance

  + Evaluation of effectiveness in relation to the relevance of the documents retrieved
  + Relevance is judged in a binary way, even if it is in fact a continuous judgement
    + Impossible when the task is to rank thousands or millions of options: too subjective, too difficult
  + In IR research/development scenarios, one cannot afford humans looking at results of every system/variant of system
  + Instead, performance measured/compared using a pre-created benchmarking corpus, a.k.a. gold-standard dataset, which provides:
    + a standard set of documents, and queries
    + a list of documents judged relevant for each query, by human subjects
    + relevance scores, usually treated as binary

+ Evaluation of IR systems – Metrics

  + AIM

    + get as much good stuff as possible
    + get as little junk as possible

  + The two aspects of this aim are addressed by two separate measures — <u>recall</u> and <u>precision</u>.

    ![14](/Pictures/Text Processing/14.png)

    + Recall : $\frac{A}{A+C}$ proportion of relevant documents returned
    + Precision: $\frac{A}{A+B}$ proportion of retrieved documents that are relevant
    + Both measures have range: [0 . . . 1]
    + Precision and Recall address the relation between the retrieved and relevant sets of documents

  + Trade-off Between Recall and Precision

    + ![15](/Pictures/Text Processing/15.png)

  + Recall and Precision - Which is the better system?

    + ![16](/Pictures/Text Processing/16.png)

    + F-measure

      + F measure (also called F1):

        + combines precision and recall into a single figure, and it is a harmonic mean

        + gives equal weight to both:

          P = Precision     R = Retrievaled 
          $$
          \mathbf {F = \frac{2PR}{P + R}}
          $$

        + penalises low performance in one value more than arithmethic mean:

          ![17](/Pictures/Text Processing/17.png)

    + $F_\beta$

      + allows user to determine relative importance of P vs. R, by varying β
      + F1 is a special case of Fβ (where β = 1)

  + Precision at a cutoff

    + Measures how well a method ranks relevant documents before non-relevant documents。

      ![18](/Pictures/Text Processing/18.png)

  + Average Precision

    + Aggregates many precision numbers into one evaluation figure

    + Precision computed for each point a relevant document is found, and figures averaged

      ![19](/Pictures/Text Processing/19.png)

#### Evaluation

+ Strengths
  + Can search huge document collections very rapidly
  + Insensitive to genere and domain of the texts
  + Relatively straightforward to implement
    + challenges scaling to huge, dynamic document collections
+ Weakness:
  + Documents are returned not information/answers
    + user must further read texts to extract information
    + output is unstructured so limited possibilities for direct data mining/further processing

 

---

### 2. Sentiment Analysis

> **Abstract**
>
> + Explain the relevance of the topic
> + Differentiate between objective and subjective texts
> + List the main elements in a sentiment analysis system
> + Provide a critical summary of the main approaches to the problem
> + Explain how sentiment analysis systems are evaluated.
>
> **General Goal**:
>
> + Extract opinions, sentiments and emotions expressed by humans in texts and use this information for business, intelligence, etc. purposes. Can’t be done manually: huge volumes of opinionated text (esp. Big Data on the Web).
>   + Product review mining: Which features of the iPhone 11 customers like and which do they dislike?
>   + Review classification: Is a movie review positive or negative?
>   + Tracking sentiments toward topics over time: Is anger about the government policies growing or cooling down?
>   + Prediction (election outcomes, market trends): Will the Tories win the next election?
>
> **Sentient Analysis**
>
> ![20](/Pictures/Text Processing/20.png)
>
> + The task of opinion mining is: given an opinionated document:
>   + Discover all quintuples
>   + Discover some of these components
> + With that, one can structure the unstructured:
>   + Traditional data and visualisation tools can be used to slice, dice and visualise the results.
>   + Qualitative and quantitative analysis can be done.

#### Binary (lexicon-based)

+ Rule-based subjectivity classifier:  a sentence/document is subjective. 

  if it has at least n (say 2) words from the emotion words lexicon; a sentence/document is objective otherwise.

+ Rule-based sentiment classifier:  for subjective sentences/documents, count positive and negative words/phrases in the sentence/document.

  If more negative than positive words/phrases, then negative; otherwise, positive (if equal, neutral).

+ Rule-based sentiment classifier (feature-level): 

  + Assume features can be identified in previous step by information extraction techniques, e.g., battery, phone, screen.
  + For each feature, count positive and negative emotion words/phrases from the lexicon.
  + If <u>more negative than positive words/phrases, then negative; otherwise, positive (if equal, neutral).</u>

+ Rule-based sentiment classifier (feature-based)

  ![21](/Pictures/Text Processing/21.png)

#### Caveats

+ Other emotion words have context-dependent orientations, e.g.
  + small power consumption = positive
  + small screen = negative
+ Can store more fine-grained sentiment information in lexicon and add additional rules.

#### Gradable 

> + Use of ranges of sentiment instead of a binary system, to deal with intensifiers like:
>   + absolutely, utterly, completely, totally, nearly, virtually, essentially, mainly, almost, e.g.: absolutely awful
> + And grading adverbs like:
>   + Very, little, dreadfully, extremely, fairly, hugely, immensely, intensely, rather, reasonably, slightly, unusually, e.g.: a little bit cold

+ Rule-based gradable sentiment classifier

  + The lexicon: word-lists with pre-assigned emotional weights, e.g:

    Neg. dimension ($\text{C_neg}$ ): {-5,...,-1},  Pos. dimension ($\text{C_pos}$ ): {+1,...,+5}

    ![22](/Pictures/Text Processing/22.png)

  + Additional general rules to change the original weights:

    + Negation rule: <br>E.g.: “I am not good today”.
      Emotion(good)= +3; <br>“not” is detected in neighbourhood (of 5 words around); <br>so emotional valence of “good” is decreased by 1 and sign is inverted → Emotion(good) = −2
    + Capitalization rule:<br> E.g. “I am GOOD today”.<br>Emotion(good)= +3; Add +1 to positive words → Emotion(GOOD) = +4<br>Likewise, in “I am AWFUL today”.<br>

  + <u>**Intensifier rule**</u>:

    + Needs a list of intensifiers: “definitely”, “very”, “extremely”, etc. 
    + Each intensifiers has a weight
    + The weight is added to positive terms
    + The weight is subtracted from negative terms
    + E.g.: “I am feeling very good”.<br>Emotion(good)= +3; emotional valence of “good” increased by 1<br>→ Emotion(good) = +4
    + E.g. “This was an extremely boring game”<br>Emotion(boring)=−3; emotional valence of “boring” decreased by −2<br>→ Emotion(boring) = −5

  + **<u>Diminisher rule</u>**:

    + Need a list: “somewhat”, “barely”, “rarely”, etc.
    + Each intensifiers has a weight
    + The weight is subtracted from positive terms
    + The weight is added to negative terms
    + E.g.: “I am somewhat good”.
      Emotion(good)= +3; emotional valence of “good” decreased by 1 → Emotion(good) = +2
    + E.g. “This was a slightly boring game”
      Emotion(boring)=−3; emotional valence of “boring” increased by 1 → Emotion(boring) = −2

  + <u>**Exclamation rule**</u>: 

    + Functions like intensifiers. 
    + E.g.: “Great show!!!”.
      Emotion(great)= +3; Weight(!!!) = 2
      → Emotion(great) = 5

  + **<u>Emoticon rule</u>**:

    + Each has its own emotional weight, like an emotion word.

    + E.g.: Emotion(🙂) = +2; Emotion(🙁) = −2. 

      E.g.: “I can’t believe this
      product 😣

  + Decision

    + ![23](/Pictures/Text Processing/23.png)

      ![24](/Pictures/Text Processing/24.png)

  + Evalution:

    + Advantages:
      + Works effectively with different texts: forums, blogs, etc.
      + Language independent - as long as an up-to-date lexicon of emotion words is available
      + Doesn’t require data for training
      + Can be extended with additional lexica, e.g. for new emotion words/symbols as they become popular, esp. in social media
    + Disadvantages:
      + Requires a lexicon of emotion words, which should be fairly comprehensive, covering new words, abbreviations (LOL, m8, etc.), misspelled words, etc.

  + Collect relevant words/phrases that can be used to express sentiment. Determine the emotion of these subjective word/phrases.

    + Manually: word lists with pre-assigned emotional weights
    + Semi-automatically
      + Dictionary-based: find synonyms/antonyms of seed emotion words in dictionaries like WordNet
      + Corpus-based: find synonyms/antonyms of seed emotion words in corpora
    + Semi-automatically created from seed words: start with seed positive and
      negative words:
      + Search for synonyms/antonyms in dictionaries like WordNet; OR
      + Build patterns from seed words/phrases to search on large corpora,
        like the Web:
        + “beautiful and” (+)
        + "low cost but"(-)
        + "very nice and"(+)
    + Machine Learning 
      + Subjectivity classifier: first run binary classifier to identify and then eliminate objective segments
      + Sentiment classifier with remaining segments: learn how to combine and weight different attributes to make predictions. E.g. Naive Bayes

#### Corpus-based (Machine Learning)

> **Basic Knowledge**:
>
> ![25](/Pictures/Text Processing/25.png)
>
> ![26](/Pictures/Text Processing/26.png)
>
> ![27](/Pictures/Text Processing/27.png)
>
> ![28](/Pictures/Text Processing/28.png)
>
> ![29](/Pictures/Text Processing/29.png)
>
> ![30](/Pictures/Text Processing/30.png)
>
> ![31](/Pictures/Text Processing/31.png)

+ Example

  + ![32](/Pictures/Text Processing/32.png)

  + Prior:

    + P(positive) = count(positive)/N = 3/7 = 0.43
    + P(negative) = count(negative)/N = 4/7 = 0.57

  + Likelihoods

    + $$
      \mathbf{P(t_j|c_i) = \frac{count(t_j,c_i)}{count(c_i)}}
      $$

    + ![33](/Pictures/Text Processing/33.png)

  + Relative frequencies for prior (P(ci)) and likelihood($P(t_j|c_i$) makethe model in a Naive Bayes classifier.

  + At decision (test) time, given a new segment to classify, this model is applied to find the most likely class for the segment
    $$
    \mathbf{argmax\,P(c_i) = \prod\limits_{j=1}^nP(t_j|c_i) }
    $$

  + e.g.

    + ![34](/Pictures/Text Processing/34.png)

      + $P(positive) ∗ P(fantastic|positive) ∗ P(good|positive) ∗ P(lovely|positive)$
        3/7 ∗ 1/10 ∗ 1/10 ∗ 1/10 = 0.00043
      + $P(negative) ∗ P(fantastic|negative) ∗ P(good|negative) ∗ P(lovely|negative)$
        4/7 ∗ 0/8 ∗ 0/8 ∗ 0/8 = 0
      + sentiment = positive

    + ![35](/Pictures/Text Processing/35.png)

      + $P(positive) ∗ P(great|positive) ∗ P(great|positive) ∗ P(great|positive)$

        3/7 ∗ 1/10 ∗ 1/10 ∗ 1/10 = 0.00043

      + $P(negative) ∗ P(great|negative) ∗ P(great|negative) ∗ P(great|negative)$

        4/7 ∗ 2/8 ∗ 2/8 ∗ 2/8 = 0.00893

      + sentiment = negative

    + what if both the positive and negative value equals to zero



+ Add smoothing to feature counts (add 1 to every count).

  + Likelihoods
    $$
    \mathbf{P(t_j|c_i) = \frac{count(t_j,c_i)+1}{count(c_i)+\left|V\right|}}
    $$
    where the $\left|V\right|$ is the number of distinct attributes in training(all classes)

    number fo classes is the number of the total documents

  + example

    + ![36](/Pictures/Text Processing/36.png)

    + $P(positive) ∗ P(boring|positive) ∗ P(annoying|positive) ∗ P(unimaginative|positive)$

      3/7 ∗ ((0 + 1)/(10 + 12)) ∗ ((0 + 1)/(10 + 12)) ∗ ((0 + 1)/(10 + 12)) = 0.000040

    + $P(negative) ∗ P(boring|negative) ∗ P(annoying|negative) ∗ P(unimaginative|negative)$

      4/7 ∗ ((0 + 1)/(8 + 12)) ∗ ((0 + 1)/(8 + 12)) ∗ ((1 + 1)/(8 + 12)) = 0.000143

    + sentiment = negative

+ Evaluation:

  + It’s simple and will work well if data is not sparse

  + How can we improve?

    + Using all words (in Naive Bayes) works well in some tasks
    + Finding subsets of words may help in other tasks
    + Using only adjectives can be limiting. Verbs like hate, dislike; nouns like love; words for inversion like not; intensifiers like very
    + Pre-built polarity lexicons can be helpful
    + Negation is important

  + Can contrast direct opinions versus more complex comparative opinions:

    + Direct sentiment expressions on target objects
      + “the picture quality of this camera is great.”
    + Comparisons expressing similarities or differences between objects,
      + e.g., “car x is cheaper than car y.”

  + How do we quantify how well our Sentiment Analysis systems work?

    + Create experimental datasets (aka test corpora)

    + Compare (positive vs negative) system to human classifications

    + Compute metrics like

      ![37](/Pictures/Text Processing/37.png)

---

### 3.Natural Language Generation

>Scanerio:
>
>![38](/Pictures/Text Processing/38.png)
>
>![39](/Pictures/Text Processing/39.png)

>**Goals**:
>
>+ Decide on content: to determine what information to communicate
>+ Decide on rhetorical structure: to determine how to structure the information to make a coherent text

#### How to Choose Content

+ Theoretical approach: deep reasoning based on deep knowledge of user, task, context, etc
+ Pragmatic approach: write schemas which try to imitate human-written texts in a corpus
+ Statistical approach: use learning techniques to learn content rules from corpus

#### Theoretical Approach

+ Deduce what the user needs to know, and communicate this
+ in-depth knowledge
  + User(knowledge,task,etc)
  + Context, domain,world
+ AI 
  + applies logical rules to the knowledge base to deduce new information
+ Evaluation
  + Lack knowledge about user
  + Lack knowledge of context
  + Hard to do in practice because we don't have good models of the effects of choices
  + Very hard to maintain knowledge base
    like new usersm new regulations, etc.

#### Statistical Approach

+ Statistical/learning techniques (including deep learning)
  + Parse corpus, align with source data, use machine learning algorithms to learn content selection rules/schemas/cases
+ Worth considering if large corpora available

#### Pragmatic Approach: Schema

+ Analyse corpus texts (after aligning them to data), and manually infer content and structure rules.
+ Typically based on imitating patterns seen in human-written texts
  + Revised based on user feedback
+ Specify structure as well as content
+ Evaluation:
  + Sometimes corpus texts may not be very good from a microplanning perspective

#### Text structure

+ Rhetorical Relations: describe how the parts of a text are linked to each other.
+ Example:
  + ![40](/Pictures/Text Processing/40.png)
  + ![41](/Pictures/Text Processing/41.png)
  + ![42](/Pictures/Text Processing/42.png)

#### Lexical Choice

+ the task of choosing the right words or lemmas to express the contents of the message
+ Issues:
  + Frequency (affects readability) 
  + Formality
  + Focus, expextations
  + Technical terms
  + Convention



#### Statistics-Based Lexical Choice for NLG from Quantitative Information
+ Goal 
  + Systems need to “know” what expressions are most suitable for expressing a given piece of information.
  + To develop a statistical algorithm for lexical choice for quantitative information, which can
    + Detect the relationship between data dimensions (aka. attributes) and words
    + Does not rely on hand-crafted rules;
    + Predict both when and which words should be used
    + One word can refer to multiple dimensions
+ ![43](/Pictures/Text Processing/43.png)
+ Methodology(Vector)
  + We represent each attribute (e.g. wind speed) as a combination of some weighted <u>key-points</u>.
    + Taking the min and max values of the attribute (from training data)
    + Key-points are evenly spaced between the min and max values
  + The number of the key-points for an attribute are fixed
  + An example of deriving the key point weights for the attribute value ws=9 (i.e., wind speed dimension).
    + ![44](//Pictures/Text Processing/44.png)
  + Similarly, a data record (i.e., a set of attribute-value pairs) can be represented by multiple groups of key-points, e.g.:
    + ws = 9 → [0.1, 0.9, 0, 0, 0]
    + dir = 2 → [0.97, 0.03, 0, 0, 0]
  + Thus, to represent a set of attribute-value pairs, we concatenate the individual weight vectors, e.g.:
    + {ws = 9, dir = 2} → [0.1, 0.9, 0, 0, 0, 0.97, 0.03, 0, 0, 0]
  + The entire data-text corpus can then be represented with a vector matrix (K), whose row corresponds to the weight vector of a data record.
    + ![45](/Pictures/Text Processing/45.png)
  + We use a column vector (namely ei)to represent the text of a datarecord Each element of $ei$
    indicates whether a word appears in the data record, e.g.:
    + ![46](/Pictures/Text Processing/46.png)
  + We represent words in the corpus using the same weight vectors whose values are unknown.
  + To estimate $vi$ for word $i$ given a data-to-text corpus as input
  + $v_i$ and $v_d$ should be close to each other in the vector space if word $i$ appears in data record $d$
    + ![47](/Pictures/Text Processing/47.png)
    + appear $(i, d1) = 1$ if word $i$ appears in data record $d$ and $0$ otherwise.
  + Our task is to find the weight vector vi for each word i, such that the similarity of $v_i$ and $v_d$ is close to appear $(i, d)$ as much as possible for each data record $(d)$.
    + ![48](/Pictures/Text Processing/48.png)

#### Microplanning

+ Decide how to best express a message in language
+ Imitating corpus works to some degree, but not perfectly
+ Key is better understanding of how linguistic choices affected readers
  + Our SumTime weather-forecast generator microplans better than human forecasters

#### Relisation 

+ Creating linear text from (typically) structured input; ensuring syntactic correctness
+ Take care of details of language
+ Problem
  + There are lots of finicky details of language which most people developing NLG systems don’t want to worry about
  + Solution:
    + Automate this using a realiser

#### Morphology

+ In linguistics, morphology is the study of words, how they are <u>formed</u>, and their <u>relationship</u> to other words in the same language.
+ Variations of a root form of a word
+ Inflectional morphology
+ Derivational morphology - change meaning



---

### 4.Information Extraction(IE)

> **Definition**
>
> From each text in a set of unstructured natural language texts identify information about predefined classes of <u>entities, relationships or events</u> and record this information in a structured form by either:
>
> + Annotating the source text, e.g. using XML tags;
> + Filling in a data structure separate from the text, e.g a template or database record or “stand-off annotation”
> + The activity of populating a structured information repository (database) from an unstructured, or free text, information source
> + The activity of creating a semantically annotated text collection (cf.“The Semantic Web”)
>
> **Purpose:**
>
> + searching or analysis using conventional database queries
> + data-mining;
> + generating a summary (perhaps in another language);
>
> **Task** 
>
> + Given: a document collection and a predefined set of entities, relations
>   and/or events
> + Return: a structured representation of all mentions of the specified entities, relations and/or events
>
> **Evaluation**:
>
> + Strengths
>   + Extracts facts from texts, not just texts from text collections
>   + Can feed other powerful applications( databases, semantic indexing engines, data mining tools)
> + Weakness:
>   + Systems tend to be genre/domain specific and porting to new genres and domains can be time-consuming/requires expertise
>   + Limited accuracy
>   + Computationally demanding, so performance issues on very large collections

#### Entity Extraction

+ For each textual mention of an entity of one of a fixed set of types identify its extent and its type
+ Coreference
  + Multiple references to the same entity in a text are rarely made using the same string:
    + Pronouns – Tony Blair . . . he
    + Names/definite descriptions - Tony Blair
    + Abbreviated forms
    + Orthographic variants
  + Different textual expressions that refer to the same real world entity are said to corefer.
  + Clearly IE systems are more useful if they can recognise which text mentions are coreferential.
  + **Coreference Task**: link togther all textual references to the same real world entity, regardless of whether the surface form is a name or not

#### Relation Extraction

+ Task:
  + identify all assertions of relations, usually binary, between entities identified in entity extraction
+ May be divided into two subtasks:
  + <u>Relation detection</u>: find pairs of entities between which a relation holds
  + <u>Relation classification</u>: for pairs of entities between which a relation holds, determine what the relation is
+ Challenging 
  + Discovering relations frequently depends upon being able to follow coreference links.
  + The same relation may be expressed in many different ways:

#### Event Detection

+ Task
  + identify all reports of event instances, typically of a small set of classes
+ May be divided into two subtasks
  + Event detection: find mentions of events in text
  + Event classification: assign detected events to one of a set of classes
+ Events may be simply viewed as relations. However they are typically complex relations that
  + Are temporally situated and often of relatively short duration
  + Involve multiple role players
  + Are often expressed across multiple sentences

#### Approach

+ Knowledge Engineering Approaches
+ Supervised Learning Approaches
+ Bootstrapping Approaches
+ Distant Supervision Approaches

> **Knowledge Engineering Approaches**
>
> ![49](/Pictures/Text Processing/49.png)

> **Supervised Learning Approach**
>
> + Systems are given texts with manually annotated entities + relations
> + For each entity/relation create a training instance
>   + k words either side of an entity mention
>   + k words to the left of entity 1 and to the right of entity 2 plus the words in between
> + Training instances represented in terms of features
>   + words, parts of speech, orthographic characteristics, syntactic info
> + Systems may learn
>   + patterns that match extraction targets
>   + Classifiers that classify tokens as beginning / inside / outside a tage type
> + Learning techniques include: covering algorithms, HMMs, SVMs

> **Bootstrapping Approaches**
>
> + A technique for relation extraction that requires only minimal supervision
> + Systems are given
>   + seed tuples
>   + seed patterns
> + System searches in large corpus for 
>   + occurences of seed tuples and then extracts a pattern that matches the context of the seed tuple
>   + matches of seed patterns from which it harvests new tuples
> + New tuples are assumed to stand in the required relation and are added to the tuple store

> **Distant Supervision Approaches**
>
> + Sometimes also called “weakly labelled” approaches
> + Assumes a (semi-)structured data source, such as
>   + which contains tuples of entities standing in the relation of interest and, ideally, a pointer to a source text
> + Tuples from data source are used to label
>   + the text with which they are associated, if available
>   + documents from the web, if not
> + Labelled data is used to train a standard supervised named entity or relation extraction system

#### Evaluation

+ **keys**
  + Correct answers, called <u>keys</u>, are produced manually for each extraction task (filled templates or SGML annotated texts)
+ **responses**
  + Scoring of system results, called responses, against keys is done automatically.
+ **interannotator agreement**
  + At least some portion of the answer keys are multiply produced by different humans so that interannotator agreement figures can be computed.
+ Principal metrics – borrowed from information retrieval
  + Precision (how much of what system returns is correct)
  + Recall(how much of what is correcrt system returns)
  + F-measure(a weighted combination of precision and recall)
+ **Shared Task challenges**
  + Shared Task challenges are community wide exercises in which groups of researchers engage in a friendly competition to build systems to address a common task.



#### Named Entity Recognition

+ **Tasks**
  + for each textual mention of an entity of one of a fixed set of types identify its <u>extent</u> and its <u>type</u>.
+ **Recap**
  + Types of entities which have been addressed by IE systems
    + Named individuals 
      + Organisations, persons, locations, books, films, ships, restaurants . . .
    + Named Kinds
      + Proteins, chemical compounds/drugs, diseases, aircraft components . . .
    + Times
      + temporal expressions – dates, times of day
    + Measures
      + temporal expressions – dates, times of day
  + Multiple references to the same entity in a text are rarely made using the same string:
    + Pronouns – They . . . he
    + Names/defiinite descriptions  - Tony Blair . . . the Prime Minister
    + Abbreviated forms - Theresa May ... May; United Nations ... UN
    + Orthographic variants 
  + Different textual expressions that refer to the same real world entity are said to <u>corefer</u>
  + **Coreference Task**: link togther all textual references to the same real world entity, regardless of whether the surface form is a name or not

> **Knowledge Engineering Approaches to NER**
>
> + Such systems typically use
>   + named entity lexicons and
>   + Manually authored pattern / action rules or regular expression
> + System has three main stages:
>   + Lexical processing
>   + NE parsing
>   + Discourse interpretation
> + Lexical processing
>   + Many rule-based NER systems made extensive use of specialised lexicons of proper names, such as <u>gazetteers</u> – lists of place names
>   + **Why not use even larger gazetteers?**
>     + Many NEs occur in multiple categories - the larger the lexicons the greater ambiguity
>     + the listing of names is never complete, so need some mechanism to type unseen NEs in any case
>   + **Principles**
>     + Tokenisation, sentence splitting, morphological analysis
>     + Part-of-speech tagging 
>       + tags known proper name words andunknown uppercase-initial words as proper names (NNP, NNPS)  NNP = Proper nouns
>     + Name List / Gazetter Lookup and Tagging (organisations, locations, persons, company designators. person titles)
>     + Trigger Word Tagging
>       + certain words in multi-word names function as trigger words, permitting classification of the name
>       + system has trigger words for various orgs,gov't institutions,locations
> + **NE Parsing**
>   + Using the presetrules to identify the unclassified proper name 
>   + ![50](/Pictures/Text Processing/50.png)
> + Discourcse Interpretation - Coreference Resolution
>   + When the name class of an antecedent (anaphor) is known thenestablishing coreference allows the name class of the anaphor (antecedent) to be established.
>   + An unclassified PN may be co-referential with a variant form of a classified PN. (PN = Pronoun)
>     + **** Ford – Ford Motor Co.
>   + An unclassified PN may be co-referential with a definite NP which permits the PN’s class to be inferred
>     + E.g. Kellogg ... the breakfast cereal manufacturer
>   + Semantic type information
>     + noun-noun qualification
>       + When an unclassified PN qualifies an organisation-related object then the PN is classified as an organisation; e.g. Erickson stocks
>     + Possessives
>       + when an unclassified PN stands in a possessive relationto an organisation post, then the PN is classified as an organisation; e.g. vice president of ABC, ABC’s vice president
>     + Apposition
>       + when an unclassified PN is apposed with a known orgnisation post, the former name is classified as a person name; e.g. Miodrag Jones, president of XYZ
> + Evaluation
>   + Strengths
>     + High performance - only several points behind human annotators
>     + Transparent - easy to understand what system is doing/why
>   + Weakness
>     + Porting to another domain requires substantial rule re-engineering
>     + Acquisition of domain-specific lexicons
>     + Rule writing requires high levels of expertise

> **Supervised learning approaches to NER**
>
> + **Task**
>   + Supervised learning approaches aim to address the portability problems inherent in knowledge engineering NER
>     + Instead of manually authoring rules, systems learn from annotated examples
>     + Moving to new domain requires only annotated data in the domain – can be supplied by domain expert without need for expert computational linguist
> + **Sequence Labelling**
>   + Systems may learn
>     + <u>patterns</u> that match extraction targets
>     + <u>classifiers</u> that label tokens as beginning/inside/outside a tage type
>   + In sequence labelling for NER, each token is given one of three label
>     types:
>     + ![51](//Pictures/Text Processing/51.png)
>     + Scheme is called `BIO` or sometimes `IOB`
>   + Each training instance(token) is typically represented as a set of **features**
>   + **Features** can be not only the characteristics of the token itxself but of neighbouring tokens as well
>     + the classifier extracts features from
>       + input string
>       + left predictions
> + **Carreras et al.(2003)**
>   + 