#################
Lesson 2 - Background
#################

In the previous lesson we saw how simple term frequencies could be used to create representations of text. 
While these worked to some extent, they are fundamentally limited in how they can represent language. In particular, 
the meaning a word has in its context is lost.

This has long been an issue for bag-of-words models, and one way of trying to introduce more structure is a so called n-gram model. 
Instead of only treating a document as a bag of its individual terms, we can add subsequences of terms. If we for example also include 
all ordered pairs of terms in a document, we would be using a *bigram* model. If we instead use subsequences of 3 words, we would have 
a trigram model and so on. We generally refer to this representation as an *n-gram* representation, where the n is the length of the subsequences.

Data sparsity
=============

One fundamental issue of n-gram models is that the *support* for each n-gram decreases very rapidly as we increase the length of the subsequences. This means that if we go up to say 5-grams, almost all such 5-sequence terms will be unique.














...








Neural Networks for contextual representations
==============================================

While computational linguistics has tried to define rule based methods of tackling the issues of words in context, often through semantic parsing, this has proven to be brittle. 