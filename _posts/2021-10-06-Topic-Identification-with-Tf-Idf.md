---
title: "Topic Identification with Tf-Idf"
date: 2021-10-06
mathjax: true
toc: true
categories:
  - study
tags:
  - nlp
---



In this post, I will go over my first attempt at topic identification--the elementary stepping-stone to more advanced natural language processing tasks. 


# Introduction 

Natural language processing, also known as NLP, is the art of making computers understand the human language, or "natural language." NLP can be applied to a vast number of tasks with a variety of purposes, including quickly extracting information from large amounts of text data. 

There are largely two subfields of NLP with rather self-explanatory names: natural language generation (NLG) and natural language understanding (NLU). 

Even though the two are distinct fields, to perform NLG or NLU, there needs to be some common, systematic way to analyze the human language. This analysis consists of several phases, such as morphological, syntax, semantic, and pragmatic analysis. (Chopra et al, 2013)

+ morphological analysis
+ syntax analysis
+ semantic analysis
+ pragmatic analysis

The specifics of those phases are too extensive to include in one blog post, and there are a plethora of research materials in the NLP field. For such reasons, this post will focus specifically on a rather simple task: topic identification. 

The text data that is used in this post comes from the `inaugural` dataset provided by the `nltk` library. The dataset contains information about the inaugural speeches of past U.S. presidents, from Washington to Trump. This post, however, will focus on two of those speeches: the 2005 speech by president Bush and the 2009 speech by president Obama. 

In looking at those two speeches, the most frequent words in each speech will be analyzed, as well as the most frequent common words in both speeches. Next, using the `TfidfModel` module from the `gensim` library, distinct characteristics of the two speeches will be distilled from the text through the extraction of words with the highest tf-idf weight. Using such words, the speeches will be interpreted within their respective contexts. 


# Preprocessing

To use the `inaugural` dataset, first import the required module. Presidents Bush and Obama's speeches are loaded in `string` objects `speech1` and `speech2`. 


```python
import nltk

from nltk.corpus import inaugural
nltk.download('inaugural')


listofIDs = inaugural.fileids()
nameID = [fileid[5:] for fileid in inaugural.fileids()]

print(listofIDs) # // ['1789-Washington.txt', '1793-Washington.txt', '1797-Adams.txt', ...
# print(nameID) // ['Washington.txt', 'Washington.txt', 'Adams.txt', 'Jefferson.txt', ...


def nameToIDIndex(year, name): # Adams -> returns 2
  id = str(year) + "-" + name + ".txt"
  print(id)
  for i in listofIDs:
    if id in i:
      return listofIDs.index(i)

speech1 = inaugural.raw(listofIDs[nameToIDIndex(2009, "Obama")])
# print(speech1) # // "str"

speech2 = inaugural.raw(listofIDs[nameToIDIndex(2005, "Bush")])
# print(speech2) # // "str"


```

    [nltk_data] Downloading package inaugural to /root/nltk_data...
    [nltk_data]   Unzipping corpora/inaugural.zip.
    ['1789-Washington.txt', '1793-Washington.txt', '1797-Adams.txt', '1801-Jefferson.txt', '1805-Jefferson.txt', '1809-Madison.txt', '1813-Madison.txt', '1817-Monroe.txt', '1821-Monroe.txt', '1825-Adams.txt', '1829-Jackson.txt', '1833-Jackson.txt', '1837-VanBuren.txt', '1841-Harrison.txt', '1845-Polk.txt', '1849-Taylor.txt', '1853-Pierce.txt', '1857-Buchanan.txt', '1861-Lincoln.txt', '1865-Lincoln.txt', '1869-Grant.txt', '1873-Grant.txt', '1877-Hayes.txt', '1881-Garfield.txt', '1885-Cleveland.txt', '1889-Harrison.txt', '1893-Cleveland.txt', '1897-McKinley.txt', '1901-McKinley.txt', '1905-Roosevelt.txt', '1909-Taft.txt', '1913-Wilson.txt', '1917-Wilson.txt', '1921-Harding.txt', '1925-Coolidge.txt', '1929-Hoover.txt', '1933-Roosevelt.txt', '1937-Roosevelt.txt', '1941-Roosevelt.txt', '1945-Roosevelt.txt', '1949-Truman.txt', '1953-Eisenhower.txt', '1957-Eisenhower.txt', '1961-Kennedy.txt', '1965-Johnson.txt', '1969-Nixon.txt', '1973-Nixon.txt', '1977-Carter.txt', '1981-Reagan.txt', '1985-Reagan.txt', '1989-Bush.txt', '1993-Clinton.txt', '1997-Clinton.txt', '2001-Bush.txt', '2005-Bush.txt', '2009-Obama.txt', '2013-Obama.txt', '2017-Trump.txt']
    2009-Obama.txt
    2005-Bush.txt


Then go through the three steps below using various modules from the `nltk` library. 

+ Tokenize at the sentence and word levels
+ Remove stop-words
+ Remove non-alphabetic characters


```python
import nltk   

nltk.download('stopwords')
nltk.download('wordnet')
nltk.download('punkt')

from nltk.tokenize import sent_tokenize
from nltk.tokenize import word_tokenize

from nltk.stem import PorterStemmer
from nltk.stem import WordNetLemmatizer

from collections import Counter
from nltk.corpus import stopwords
 

stopList = set(stopwords.words('english'))

 
sentList1 = sent_tokenize(speech1) # split into separate sentences
sentList2 = sent_tokenize(speech2)

wordList1 = word_tokenize(speech1) # split into separate words
wordList2 = word_tokenize(speech2)


# --------------------Remove punctuation marks and stop words ----------------------
puncRemovedList1 = [word for word in wordList1 if word.isalpha()]
puncRemovedList2 = [word for word in wordList2 if word.isalpha()]

print("puncRemovedList1: ", puncRemovedList1)
print("puncRemovedList2: ", puncRemovedList2)

stopRemovedList1 = [word for word in puncRemovedList1 if not word.lower() in stopList]
stopRemovedList2 = [word for word in puncRemovedList2 if not word.lower() in stopList]

print("stopRemovedList1: ", stopRemovedList1)
print("stopRemovedList2: ", stopRemovedList2)

 
 
```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Unzipping corpora/stopwords.zip.
    [nltk_data] Downloading package wordnet to /root/nltk_data...
    [nltk_data]   Unzipping corpora/wordnet.zip.
    [nltk_data] Downloading package punkt to /root/nltk_data...
    [nltk_data]   Unzipping tokenizers/punkt.zip.
    puncRemovedList1:  ['My', 'fellow', 'citizens', 'I', 'stand', 'here', 'today' ...]


Next, lemmatize each word in both speeches using `WwordNetLemmatizer.lemmatize()`.


```python
# Lemmatize words
lemmatizer = WordNetLemmatizer()

# Lemmatize all tokens into a new list: lemmatized
lemmatized1 = [lemmatizer.lemmatize(t) for t in stopRemovedList1]

lemmatized2 = [lemmatizer.lemmatize(t) for t in stopRemovedList2]
```


```python
for i in range(len(lemmatized1)):
  if 'le' in lemmatized1:
    lemmatized1.remove('le')
  if "let" in lemmatized1:
    lemmatized1.remove("let")
  if "u" in lemmatized1:
    lemmatized1.remove("u")

for i in range(len(lemmatized2)):
  if 'seen' in lemmatized2:
    lemmatized2.remove('seen')

print("Lemmatized1: ", lemmatized1[:7]) # print first five words of Lemmatized
print("Lemmatized2: ", lemmatized2[:7])

speech = []
for i in [lemmatized1, lemmatized2]:
  speech.append(i)
```

The output is as follows:

    Lemmatized1:  ['fellow', 'citizen', 'stand', 'today', 'humbled', 'task', 'grateful']
    Lemmatized2:  ['Vice', 'President', 'Cheney', 'Chief', 'Justice', 'President', 'Carter']


Then create two bag-of-word objects to view the most frequent words in each speeches. 


```python
# --------------------Create Bag of Words ----------------------

bow1 = {}
bow2 = {}

for word in lemmatized1:
  if word not in bow1:
    bow1[word] = 1
  else:
    bow1[word] += 1

for word in lemmatized2:
  if word not in bow2:
    bow2[word] = 1
  else:
    bow2[word] += 1

# print(bow)  // "{'South': 5, 'Korea': 12, 'officially': 1, 'Republic': 2" ...  dictionary with {(index1, key1), (index2, key2) ... } 

print("Most mentioned:", max(bow1, key=bow1.get)) # prints index (word) with maximum key (frequency) = "Korea" 
print("Most mentioned:", max(bow2, key=bow2.get))

# leastMentioned = dict(sorted(bow.items(), key=lambda item: item[1])) # dictionary ordering from least -> greatest
# print("\nOrdering: Least to most mentioned:",leastMentioned) # print the least occurring words // {'officially': 1, 'ROK': 1, 'East': 1, 'constituting': 1, 'part': 1

mostMentioned1 = dict(sorted(bow1.items(), key=lambda item: item[1], reverse=True))
mostMentioned2 = dict(sorted(bow2.items(), key=lambda item: item[1], reverse=True))

print("\nMost to least mentioned:", mostMentioned1)
print("\nMost to least mentioned:", mostMentioned2)


# mostMentioned = dict(sorted(bow.items(), key=lambda item: item[0]))
# print("\nOrdering: Alphabetical order:", mostMentioned) # print in alphabetical order

```

    Most mentioned: nation
    Most mentioned: freedom
    
    Most to least mentioned: {'nation': 15, 'new': 11, 'America': 10, 'every': 8, 'must': 8, 'generation': 8, 'work': 7, 'people': 7, 'world': 7, 'common': 6 ... }


We see that the words most mentioned in president Obama's speech are: 'nation' (15 times), "new" (11 times), "America" (10 times), and etc. Meanwhile, for president Bush, the most frequent words were: "freedom" (23 times), "America" (12 times), "liberty" (10 times), and etc. The words that appeared the most in common included: "nation," "America," "every," and "world."

All that we have done so far was simply identifying the words that appeared the most in both speeches. However, to find the keywords that reveal **distinct characteristics** of each speech, we need a different approach that is explained in the following section. 



# TF-IDF Weighting

This section references this [Youtube video](https://www.youtube.com/watch?v=pxYkuWEOKjc&ab_channel=ArtificialIntelligence-AllinOne) by Dr. Christopher Manning, in which he discusses a book that he coauthored with Prabhakar Raghavan and Hinrich Schütze, called *An Introduction to Information Retrieval*.

Suppose we have a hundred academic papers on various fields of astronomy. 


Although those one hundred papers might focus on one hundred different fields, it is reasonable that the words "space" and "stars" will appear in almost every paper. For that reason, if we wish to find keywords that reflect particular characteristics of a certain paper, the words "space" and "stars" will most definitely prove useless. To find these keywords though, we use a method called the *tf-idf weighting scheme*. 

To understand this method, we need to first define several terms. 

 

First, we define the  term *inverse document frequency*, or $\text{idf}$, in the following manner. 

+ $N$: total number of documents
+ $\text{df}_{t}$: document frequency of word $t$

The document frequency of a word is simply the number of documents that contain the word. In essence, the value of $\text{idf}$ is expected to be high, if there is a high concentration of a specific word $t$ in a few documents. 

$$ \text{idf}= log \; (\frac{N}{\text{df}_{t}})\tag{1}$$

 

Similarly, we define the term *text frequency-inverse document frequency*, or the $\text{tf-idf}$, of word $t$ in document $d$ in the this way.

+ $\text{tf}_{t,d}$ : text frequency of word $t$ in document $d$
+ $\text{idf}_{\;t}$: inverse document frequency of word $t$

$$  \text{tf-idf}_{\;t,d} = \text{tf}_{t,d} \cdot \text{idf}_{\;t}\tag{2}  $$

Substitute the expression for $\text{idf}$ in equation (1) into equation (2), and we get the final form of tf-idf. 

$$  \text{tf-idf}_{\;t,d} = \text{tf}_{t,d} \cdot log \; (\frac{N}{\text{df}_{t}}) \tag{3} $$ 

To illustrate tf-idf in action, let's go back to the example of the astronomy papers. If we wish to find keywords that best describe the first paper, it would be intuitive to find words that appear most **frequently** and **exclusively** in that paper. For example, if the first paper focuses specifically on the planet Jupiter, while the other 99 papers focus on the Milky Way galaxy, following the chart below, we can assume that word $t$ is likely to be "Jupiter." 


| Document 1 ($d_1$)        | Document 2 ($d_2$)   |   Document 3 ($d_3$) | ... | Document 100 ($d_{100}$) |
| :---        |    :----:   |:----:  |:----:  | ---: |
| $t \times 100 $ | $t \times 1 $ | $t \times 0 $   | ... | $t \times 0 $|

Using the formulas above, we can get the values of $\text{tf-idf}_{\;t,d_1}$ and $\text{tf-idf}_{\;t,d_1}$. If papers three to one hundred do not contain the word "Jupiter," then the first paper would have a high value for $\text{tf-idf}_{\;t,d_1}$, whereas the other papers would have a low value or a zero. 


$$  \text{tf-idf}_{\;t,d_1} = 100 \cdot log \; (\frac{100}{2}) $$ 
$$  \text{tf-idf}_{\;t,d_2} = 1 \cdot log \; (\frac{100}{2}) $$ 

Therefore, if we search for "Jupiter" among the collection of one hundred papers, the first paper, which has the highest $\text{tf-idf}$ score for the word "Jupiter," would come up. 




If searching for many words, we could use the tf-idf weighting scheme to score documents with respect to a collection of words (denoted as $q$). In this case, we could simply find the highest-scoring document by adding up the invidividual tf-idf weights.  

$$ \text{Score} \;(q,d) = \sum_{t \;\in\; q}  \text{tf-idf}_{\;t,d}   $$ 

In other words, simply sum up the individual tf-idf weights for each word in the collection. 

# Gensim & TF-IDF

Let's try to apply the above formula to actually find out which words in each of the speeches have the highest tf-idf scores. We first create a `dictionary` object called `dict1` that contains all the words in both speeches. 


```python
from gensim.corpora.dictionary import Dictionary

print("lemmatized1: ", lemmatized1[:7]) # print first five words of Lemmatized
print("lemmatized2: ", lemmatized2[:7])

speech = []
for i in [lemmatized1, lemmatized2]:
  speech.append(i)

print("speech: ", speech)

dict1 = Dictionary(speech)
print(type(dict1)) # <class 'gensim.corpora.dictionary.Dictionary'>
print(dict1) # Dictionary(1143 unique tokens: ['Afghanistan', 'America', 'American', 'Americans', 'Arlington']...)
 
```

    lemmatized1:  ['fellow', 'citizen', 'stand', 'today', 'humbled', 'task', 'grateful']
    lemmatized2:  ['Vice', 'President', 'Cheney', 'Chief', 'Justice', 'President', 'Carter']
    speech:  [['fellow', 'citizen', 'stand', 'today', 'humbled', 'task', 'grateful', 'trust', ... 'United', 'States', 'America']]


We then create a corpus that contains two bag-of-words models, with each one corresponding to one speech.  


```python
# afg = dict1.token2id.get("Afghanistan")
# print(afg) # 0
# print(dict1.get(0)) # Afghanistan 

corpus = []
for item in speech:
  corpus.append(dict1.doc2bow(item)) # (0,1) means "afghanistan" shows up once in both speeches. 
 
print("corpus: ", corpus) 
print("corpus[0][:5]: ", corpus[0][:5])
```

    corpus:  [[(0, 1), (1, 10), (2, 2), (3, 3), (4, 1), (5, 1), (6, 1), ... (1134, 2), (1135, 1), (1136, 1), (1137, 1), (1138, 1), (1139, 2), (1140, 1), (1141, 1), (1142, 1)]]
    corpus[0][:5]:  [(0, 1), (1, 10), (2, 2), (3, 3), (4, 1)]


The following code is a simplified, example demonstration of the steps that took to create the above corpus. 


```python
# demonstration with example. 
from gensim.corpora.dictionary import Dictionary

corpus2 = []

speech3 = [['America', 'America', 'U.K', 'U.S', 'U.S'], ['zebra', 'aardvark', 'zebra', 'mouse']]
dict2 = Dictionary(speech3)
print("dict2: ", dict2, "\n")

for item in speech3:
  print("item: ", item)
  print("to be appended:", dict2.doc2bow(item)) # .doc2bow() organizes words in alphabetical order
  corpus2.append(dict2.doc2bow(item)) # (3,1) : 3rd word is "aardvark", freq is 1. 
                                      # (4,1) : 4th word is "mouse", freq is 1. 
                                      # (5,2) : 5th word is "zebra", freq is 2. 

print("\ncorpus2: ", corpus2)
```

    dict2:  Dictionary(6 unique tokens: ['America', 'U.K', 'U.S', 'aardvark', 'mouse']...) 
    
    item:  ['America', 'America', 'U.K', 'U.S', 'U.S']
    to be appended: [(0, 2), (1, 1), (2, 2)]
    item:  ['zebra', 'aardvark', 'zebra', 'mouse']
    to be appended: [(3, 1), (4, 1), (5, 2)]
    
    corpus2:  [[(0, 2), (1, 1), (2, 2)], [(3, 1), (4, 1), (5, 2)]]



```python
print("\nMost to least mentioned:", mostMentioned1)
print("\nMost to least mentioned:", mostMentioned2)
```


    Most to least mentioned: {'nation': 15, 'new': 11, 'America': 10, 'every': 8, 'must': 8, 'generation': 8, 'work': 7, 'people': 7, 'world': 7, 'common': 6, '... 'bless': 1}


Finally, we pass in the corpus object as a parameter to a `TfidfModel` object, in order to view what the weights are like for the words in each speech. 


```python
from gensim.models.tfidfmodel import TfidfModel

article1 = corpus[0]
article2 = corpus[1]

tfidf1 = TfidfModel(corpus) # corpus:  [[(0, 1), (1, 10), (2, 2), (3, 3), (4, 1), (5, 1), (6, 1), (7, 1),
tfidf2 = TfidfModel(corpus)

print(type(tfidf2)) # <class 'gensim.models.tfidfmodel.TfidfModel'>
print(tfidf2) # TfidfModel(num_docs=2, num_nnz=261)

weights1 = tfidf1[article1] # looks at both the first speech and the words in both speeches/ 
weights2 = tfidf2[article2] 

print(type(weights2)) # <class 'list'>
print(weights2) # [(207, 0.1270001270001905), (208, 0.1270001270001905), ... 

weights1 = sorted(weights1, key=lambda w: w[1], reverse=True)
weights2 = sorted(weights2, key=lambda w: w[1], reverse=True)
print("\n")

# Print the top 5 weighted words
for word, weight in weights1[:5]:
    print(dict1.get(word), weight)

print("\n")

for word, weight in weights2[:5]:
    print(dict1.get(word), weight)
```

    <class 'gensim.models.tfidfmodel.TfidfModel'>
    TfidfModel(num_docs=2, num_nnz=1355)
    <class 'list'>
    [(757, 0.03653920476209348), (758, 0.03653920476209348), (759, 0.07307840952418695), (760, 0.03653920476209348), ... (1142, 0.03653920476209348)]


​    
    child 0.16045763595141999
    spirit 0.16045763595141999
    crisis 0.128366108761136
    job 0.128366108761136
    meet 0.128366108761136


​    
    human 0.2192352285725609
    justice 0.1826960238104674
    Liberty 0.1461568190483739
    fire 0.1461568190483739
    tyranny 0.1461568190483739



Now that we have the tf-idf weights, let's try to interpret the speeches in the context of the words with the highest weights. 

If we take a look at president Obama's speech, we see that the words "common," "spirit," "crisis," "across," and "carried" have the highest tf-idf weights. In comparison, these words rarely show up in president Bush's speech.

Considering the context of president Obama's speech, the words selected through tf-idf highlight president Obama's concerns about America and his plea to Americans.  

During president Obama's 2008 election campaign, the state of the U.S. economy was deteriorating rapidly due to the financial crisis of 2007-2008. In fact, the U.S. economy was experiencing double-digit unemployment figures by the end of 2009, up from 5% in 2007. Not only that, but as president Obama pointed out in his speech, the U.S. had been at war in Afghanistan and Iraq for eight years from 2001. In essence, his inauguration took place in times of much uncertainty and socio-economic instability. Thus, it comes without surprise that "crisis" was one of the most used words.  

Regarding the words "spirit" and "job," one can interpret them as showcasing president Obama's resolve to bring forth a refreshing spirit to a stagnant America, just as he himself wrote anew the white-male-dominated history of the U.S. presidency.    

On the other hand, if we look at the words in president Bush's speech, we also see that his campaign themes have been infused into the speech. In the light of the September 11 attacks, Bush attempted to portray himself as a strong leader who was tough on terrorism in his campaign. This gave the words "fire" and "tyranny" high tf-idf weights, because he used the words often to describe terrorism. Similarly, "justice," "liberty," and "human" showed up as keywords because he tried to depict himself as a defender of freedom and human rights.



 # Conclusion

This post dealt with the concept of the tf-idf weighting scheme, as well as an example demonstration of the scheme using the `inaugural` dataset. Although I am, by no means, an expert on natural language processing, I hope I can discover and learn more exciting things in my journey down the NLP road. Thanks for reading. 


# References:

Chopra, Abhimanyu et al. “Natural Language Processing.” International Journal of Technology Enhancements and Emerging Engineering Research 1 (2013): 131-134.
