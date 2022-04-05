Lesson 1 - theory
=================


Natural Language Processing (NLP) is a field which studies techniques to automatically 
extract meaning from language. In formal languages, the semantics of the language is 
typically defined by construction, while in natural languages as spoken between humans, 
the question of how language give rise to meaning is not fully (at all?) understood. 

NLP is to a large extent considered an applied field, focused on solving specific tasks 
related to the extraction of information from text. Some of the common tasks include:
  - Automatic speech recognition
  - CCG
  - Common sense
  - Constituency parsing
  - Coreference resolution
  - Data-to-Text Generation
  - Dependency parsing
  - Dialogue
  - Domain adaptation
  - Entity linking
  - Grammatical error correction
  - Information extraction
  - Intent Detection and Slot Filling
  - Language modeling
  - Lexical normalization
  - Machine translation
  - Missing elements
  - Multi-task learning
  - Multi-modal
  - Named entity recognition
  - Natural language inference
  - Part-of-speech tagging
  - Paraphrase Generation
  - Question answering
  - Relation prediction
  - Relationship extraction
  - Semantic textual similarity
  - Semantic parsing
  - Semantic role labeling
  - Sentiment analysis
  - Shallow syntax
  - Simplification
  - Stance detection
  - Summarization
  - Taxonomy learning
  - Temporal processing
  - Text classification
  - Word sense disambiguation


The field of computational linguistics has long tried to tackle the problem of using 
computational methods to extract meaning from text, and many of the techniques used in
NLP owe their creating to computational linguistics. Traditionally, these tools were 
created by formulating rules for how language should be parsed, and semantics was 
derived from these syntactic structures (e.g. parse trees).

However, natural language can often be ambiguous which makes designing formal methods
to parse it's meaning difficult. This has given rise to statistical methods, which 
instead of explicitly trying define rules for language parsing instead uses data to 
correlate language with some task at hand.

It's a hotly debated topic whether statiscial methods can get to the _understanding_ of 
text, but in this workshop we'll gloss over that and instead focus on the practical 
application of these methods.


Language representation
=======================
One recurring theme of this workshop is "how should we represent language?". Fundamentally, 
in the context of NLP the representation of language is in the form of **text**. While techniques 
for speech recognition can be considered part of NLP, they are typically used to transcribe 
speech into text, after which the standard techniques for dealing with text are used.

In NLP we often have some body of text we use to train our statistical models, 
and this body of text is referred to as a *corpus* (from latin, meaning body). 
These corpora might be just text extracted from some written records or it can
be annotated with task-specific annotations, for example each word might be 
tagged with which part of speech it is or whether it referres to a unique named entity.

Tokenization
============
Due to historical reasons, much of NLP was developed 
for *english*, and some of the assumptions which went in to early methods breaks 
for other languages. 

One long standing question of how to go from text to meaning is what the basic 
building blocks should be. The act of splitting a string of characters into 
basic building blocks is referred to as *tokenization*. Historically the natural 
choice for english was breaking text into words, often by splitting on whitespace 
and punctuation.

While this kind of works for english, it leads to a number of challenges for other 
languages such as german (where combining multiple words into one long word is common) 
and chinese (where "words" are not separated by whitespace).

In this workshop we'll not focus overly much on tokenization, since for contemporary 
NLP is solved in a datadriven fashion. We will however look at how we can use simple 
tokenization of english using whitespace and how this leads to some issues.

To be less tied to the notion of our tokens being words, we often use `term` instead, 
which denotes the basic semantic units we use in our NLP methods.

The power law curse
===================
A phenomena which was observed early on when statistical methods where used for 
NLP is that of Zipf's law. While the phenomena had been observed before, linguist 
George Kinglsey Zipf spent much effort into popularizing it and seeking to understand 
it's origin. Roughly it was observed that the frequency of words in natural text
followed a very skewed distribution, where the most frequent word (e.g. `the`) 
stands for a significant fraction of all words in the corpus. Conversely, most semantically 
meaningful words (e.g. `neural` or `processing`) are *very* uncommon. This leads to a frequency 
distribution of words which is often referred to as power-law. 

Without going into details about power law distribution, when it comes to statistical models 
of language is enough to say that the number of occurances of most interesing words is low
 in a corpus. This is something we need to understand to devise statistical methods for 
 learning interesting things abouts words.


Bag of words
============
Provided we have succeded in breaking our text down into tokens, one question is 
how we could analyze this text.

In this workshop we'll mainly focus on the task of semantic similairty, but we can think of 
ways to easily extend this into document classifcation or question answering. If we can 
somehow say that two documents are similar, and we know that one of the documents is 
about `machine learning`, then we can infer that likely the other document is as well.

One obvious way to do this is to ask whether documents contain the same words. If 
two documents tend to have the same words, then likely they are about the same thing.

The bag-of-words (BoW) model is a simple way of doing this. In BoW We can think of 
each document as being represented by a set of counters, one  
per word in our vobulary (the set of words we are considering). The counters show us 
how many times each word occur in the document. Since the distribution of terms 
typically follow Zipf's law, most entries for a document will be 0 (that is, most 
words are not used in a given document).

If we order these counters in a sequence, we can organize the 
information about all of our documents in a matrix which shows the relationship 
between document and terms, and refer to this matrix as a document-term matrix.

In this kind of representation of text, we're discarding all of the syntactical 
information from the text and only keep the words and their frequencies. While 
this might seem overly destructive we'll see that for many problems it actually 
works quite well.

Since documents vary in length, the counts of a word will also vary, so we typically 
normalize the counts to instead be the fraction of the words frequency in the document. 
This means that the sum of a document vector is 1, and we can think of the elements 
as containing the probability of getting that term when randomly chosen one from the 
document.

Each document can now be thought of as a vector of word counts, where most 
places are 0. We can easily define similarity measures between documents based 
on these vectors. Some popular similarity metrics are:
  - Jaccard index (or Tanimoto index): The ratio of the intersection of two sets over the union.
  - Manhattan distance: The sum of absolute values of the difference between the vectors
  - Euclidean distance: The square root of the sum of squared differences between the vectors


Factorizing the document-term matrix
====================================


Random indexing
===============
The issue of


The issue of frequency
======================
One fundamental issue plagues our BoW-model, and that is the problem that some terms 
dominates in the distance. In english, words like `a`, `the`, `and` are so common 
that they will contain the majority of counts for any document, and some differences 
in their usage might contribute most to any similarity between documents. 
Put it in another way, if you were to randomly pick a word from a document, 
it's highly likely to be a word which tells you nothing of what the document is about.

One natural way of solving this is to _weigh_ each term differently depending on it's 
overall frequency, so a word which is very commom (and thus likely to be relatively 
unimportant) gets assigned less weight in the distance calculation than one which 
is used rarely.

While many schemes for deciding on the frequency exists, we'll use a simple one which 
merely takes the negative logarithm of how many documents a term occurs in over the total number of documents:

$-log \frac{1+n_t}{N}$

Where $n_t$ is the number of documents the term occurs in and $N$ is the total 
number of documents. This means that if the term occurs in all documents 
(which words like `the`, `and` are likely to do) this weight will be close to 
log(1)=0, while if it occurs in very few documents the weight will high. The 1 
in the numerator is to make sure we don't end up taking the logarithm of 0.


Language bias
=============

