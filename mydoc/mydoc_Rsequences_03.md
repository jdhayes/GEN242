---
title: Strings in R Base
keywords: 
last_updated: Sat Apr 30 19:54:48 2016
---

## Basic String Matching and Parsing

### String matching

Generate sample sequence data set


{% highlight r %}
myseq <- c("ATGCAGACATAGTG", "ATGAACATAGATCC", "GTACAGATCAC")
{% endhighlight %}

String searching with regular expression support

{% highlight r %}
myseq[grep("ATG", myseq)] 
{% endhighlight %}

{% highlight txt %}
## [1] "ATGCAGACATAGTG" "ATGAACATAGATCC"
{% endhighlight %}

Searches `myseq` for first match of pattern "AT"

{% highlight r %}
pos1 <- regexpr("AT", myseq) 
as.numeric(pos1); attributes(pos1)$match.length # Returns position information of matches
{% endhighlight %}

{% highlight txt %}
## [1] 1 1 7
{% endhighlight %}

{% highlight txt %}
## [1] 2 2 2
{% endhighlight %}

Searches `myseq` for all matches of pattern "AT"

{% highlight r %}
pos2 <- gregexpr("AT", myseq) 
as.numeric(pos2[[1]]); attributes(pos2[[1]])$match.length # Returns positions of matches in first sequence
{% endhighlight %}

{% highlight txt %}
## [1] 1 9
{% endhighlight %}

{% highlight txt %}
## [1] 2 2
{% endhighlight %}

String substitution with regular expression support

{% highlight r %}
gsub("^ATG", "atg", myseq) 
{% endhighlight %}

{% highlight txt %}
## [1] "atgCAGACATAGTG" "atgAACATAGATCC" "GTACAGATCAC"
{% endhighlight %}

### Positional parsing

{% highlight r %}
nchar(myseq) # Computes length of strings
{% endhighlight %}

{% highlight txt %}
## [1] 14 14 11
{% endhighlight %}

{% highlight r %}
substring(myseq[1], c(1,3), c(2,5)) # Positional parsing of several fragments from one string
{% endhighlight %}

{% highlight txt %}
## [1] "AT"  "GCA"
{% endhighlight %}

{% highlight r %}
substring(myseq, c(1,4,7), c(2,6,10)) # Positional parsing of many strings
{% endhighlight %}

{% highlight txt %}
## [1] "AT"   "AAC"  "ATCA"
{% endhighlight %}

## Random Sequence Generation

### Random DNA sequences of any length


{% highlight r %}
rand <- sapply(1:100, function(x) paste(sample(c("A","T","G","C"), sample(10:20), replace=T), collapse=""))
rand[1:3]
{% endhighlight %}

{% highlight txt %}
## [1] "TTGGTGCCCGCACACGT" "TACTCACGAC"        "AGCTCAACCTAGTTTCG"
{% endhighlight %}

### Count identical sequences


{% highlight r %}
table(c(rand[1:4], rand[1]))
{% endhighlight %}

{% highlight txt %}
## 
##    AGCTCAACCTAGTTTCG CGCTGCTTGGCCCAGCTGAC           TACTCACGAC    TTGGTGCCCGCACACGT 
##                    1                    1                    1                    2
{% endhighlight %}

### Extract reads from reference

Note: this requires `Biostrings` package.


{% highlight r %}
library(Biostrings)
ref <- DNAString(paste(sample(c("A","T","G","C"), 100000, replace=T), collapse=""))
randstart <- sample(1:(length(ref)-15), 1000)
randreads <- Views(ref, randstart, width=15)
rand_set <- DNAStringSet(randreads)
unlist(rand_set)
{% endhighlight %}

{% highlight txt %}
##   15000-letter "DNAString" instance
## seq: TTTTTTAAAGATCGTTCTACACCGGATGCCTTGCTGTGCTGATCTT...CCGTATTCCAACTATACTCCTCCGACCGTTAGATCGGTAGGTGCGG
{% endhighlight %}

