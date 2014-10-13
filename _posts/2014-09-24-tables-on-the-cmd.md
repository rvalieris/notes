---
layout: post
title: Tables on the command line
---


**union of rows**
{% highlight bash %}
cat a b
{% endhighlight %}

**union of columns ([convolution](https://en.wikipedia.org/wiki/Convolution_(computer_science)))**
{% highlight bash %}
spaces on empty cells : paste a b
repeat on empty cells : parallel -k --xapply 'echo {1}"\t"{2}' :::: a b
enumerate rows        : cat -n file
{% endhighlight %}

**intersection of sets ([join](https://en.wikipedia.org/wiki/Join_(relational_algebra)#Joins_and_join-like_operators))**

*sort by the key first !*
{% highlight bash %}
cat a
1       a
3       b
5       x
cat b
2       a
3       b
4       c
8       d

join -j2 -e"NA" -a1 -a2 -o"0,1.1,2.1" a b
a 1  2
b 3  3
c NA 4
d NA 8
x 5  NA
{% endhighlight %}

**column subset ([projection](https://en.wikipedia.org/wiki/Projection_(relational_algebra)))**
{% highlight bash %}
cant reorder columns : cut -f3,5 file
more flexible        : awk '{print$5"\t"$3}' file
{% endhighlight %}

**row subset ([selection](https://en.wikipedia.org/wiki/Selection_(relational_algebra)))**
{% highlight bash %}
column 3 bigger than 5 : awk '$3>5' file
column 5 equal to 'X'  : awk '$5=="X"' file
get row #1234          : awk 'NR==1234' file
first 20 lines         : head -n20 file
last 20 lines          : tail -n20 file
all but the last 20    : head -n-20 file
skip the first 19      : tail -n+20 file     
{% endhighlight %}

**[cartesian product](https://en.wikipedia.org/wiki/Cartesian_product)**
{% highlight bash %}
easy way  : parallel -k 'echo {1} {2}' ::: 1 2 3 ::: 4 5 6
iterative : for i in {1..3}; do for j in {4..6}; do echo $i $j; done; done
{% endhighlight %}

**[aggregation](https://en.wikipedia.org/wiki/Join_(relational_algebra)#Aggregation)**
{% highlight bash %}
count          : wc -l
sum            : awk '{sum += $1} END{print sum}'
concat         : paste -s -d","
average        : awk '{sum += $1} END{print sum/NR}'
minimum        : awk 'NR==1{min=$1} min>$1{min=$1} END{print min}'
maximum        : awk 'NR==1{max=$1} max<$1{max=$1} END{print max}'
distinct count : uniq -c
{% endhighlight %}

**grouping (aggregation by column)**
{% highlight bash %}
group by with count   : datamash -sg 1 count 2
group by with sum     : datamash -sg 1 sum 2
group by with mean    : datamash -sg 1 mean 2
group by with min/max : datamash -sg 1 min 2 max 2
{% endhighlight %}

**order rows/columns**
{% highlight bash %}
order full rows   : sort file
by column         : sort -k2,2 file
multiple columns  : sort -k2,2 -k5,5 file
numeric order     : sort -k3,3n file
reverse key order : sort -k1,1r file
stable sort       : sort -s file
distinct keys     : sort -u file
field separator   : sort -k2,2 -t"<TAB>" file
reverse row order : tac file
shuffle row order : shuf file / sort -R file
{% endhighlight %}

**distinct rows**

*sort first !*
{% highlight bash %}
distinct rows     : uniq file
count occurencies : uniq -c file
duplicates only   : uniq -d file
uniques only      : uniq -u file
{% endhighlight %}

**intersection of rows**

*sort first !*
{% highlight bash %}
common on both : comm -12 a b
exclusive of a : comm -23 a b
exclusive of b : comm -13 a b
{% endhighlight %}

**flip rows by columns ([transpose](https://en.wikipedia.org/wiki/Transpose))**
{% highlight bash %}
cat file | datamash transpose
{% endhighlight %}

**search**
{% highlight bash %}
simple           : grep pattern file
hightlight match : grep --color pattern file
match context    : grep -C5 pattern file
match count      : grep -c pattern file
perl regex       : grep -P '^a\d+\s\s' file
word search      : grep -w word file
invert search    : grep -v pattern file
fixed string     : grep -F string file
match limit      : grep -m2 pattern file
capture match    : grep -Po '\d+' file
for speed        : LC_ALL=C grep -F -m1 string file
{% endhighlight %}

**pretty print tables**
{% highlight bash %}
standard              : column -t -s "\t"
dont merge delimiters : column -n -t -s "\t" # debian only
{% endhighlight %}

**progress viewer**
{% highlight bash %}
basic        : pv input | ... > output
specify size : cat file | pv -s 10k | ... > output
count lines  : pv -l file | ... > output
{% endhighlight %}

**visualise files**
{% highlight bash %}
show file without wrap : less -S file
visible control chars  : less -U file
show colors in file    : less -R file
{% endhighlight %}

**See also**

* <http://www.gnu.org/software/datamash>
* <http://www.gnu.org/software/parallel>
* <https://github.com/harelba/q>
* <http://stedolan.github.io/jq/tutorial>
* <https://github.com/dinedal/textql>
* <http://hackage.haskell.org/package/txt-sushi>
* <https://news.ycombinator.com/item?id=7175830>
* <http://en.wikipedia.org/wiki/Relational_algebra>

