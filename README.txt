#####################
###               ###
### Documentation ###
###      of       ###
### Comparison of ###
### Publishers in ###
### the DNB Data  ###
###               ###
#####################

## 0. Introduction
~~~~~~~~~~~~~~~~~~

In the focus of the following project is the S. Fischer Verlag, founded 1886 by Samuel Fischer in Berlin.

This scripts 1. scrape data according to a query from the DNB in MARC21-XML format, 2. parses, filters and outputs the data to a tsv-file, which than can be worked with more easily.


## 1. run "dnb_scraper" with dnb-query
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

count     query
‾‾‾‾‾‾   ‾‾‾‾‾‾
2137  : vlg all "S. Fischer"      and jhr within "1894 1936"
1907  : vlg all "Ullstein"        and jhr within "1894 1936"
1801  : vlg all "Diederichs"      and jhr within "1894 1936"
844   : vlg all "Insel-Verlag"    and jhr within "1894 1936"
662   : vlg all "Rowohlt"         and jhr within "1894 1936"
562   : vlg all "Albert Langen"   and jhr within "1894 1936"
259   : vlg all "Kurt Wolff"      and jhr within "1894 1936"
48    : vlg all "Bermann-Fischer" and jhr within "1894 1936"

Total:	 8220


## 2. run "dnb_parser" with retrieved data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

count     field-name  field-number      
‾‾‾‾‾‾   ‾‾‾‾‾‾‾‾‾‾‾  ‾‾‾‾‾‾‾‾‾‾‾‾
7555    : author      100 a
7555    : title       245 a
4247    : subtitle    245 b
3405    : edition     250 a
7555    : location    264 a
7555    : publisher   264 b
7555    : year        264 c

This means, that 665 items (which is the difference between 8220 total scraped items and maximum 7555 fields) do not have appropriate title-information.

In the parsing process, all tabs and spaces more than 1 have been reduced to one space.

If a field does not exist, it is filled with "NaN".

Because some entries do not have a propper value in the first fields of the PUBLISHER ("[s. n.] @" in 87 entries) resp. LOCATION ("[s. l.] @" in 90 entries) they have to be taken from the next field with the same element name.


## 3. see results in "data_parsed.tsv"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The scraped and parsed data is saved to the file "data_parsed.tsv", which is a 'tab-separated values'-file, which means, that the fields of the table are seperated by a tab.

Because some fields of years (from 1461 entries) are not regular numbers, but are in paranthesis or structured irregular (see examples below), they have to be treated especially.
 * c 1934
 * 1934 ([Ausg.] 1933)
 * 1934 [Ausg. 1935]
 * 1936 [Ausg.] 1935
 * 1924 [Ausg.: 1923]
 * 1934 Ausg. 1933
 * [Ausg. 1927]
 * 1933-34
 * 1921/22
 * 1927 - 29
 * 1910-1922
 * 1926-
 * 1920 ff.
 * [um 1936]
 * [1936]
 * [1935?]
 * ([ca. 1919])
 * ([1935])

To work with the dataset, it has to be cleaned up:
 * unify the form of all years (see above)
 * unify the names of all publishers
 * unify the names of all locations

To this end, one should:
 * remove the following characters from the field of years and location:
   * "(", ")", "[" and "]" 
   * "ca. …", "um …", "…-", "…-…", "… - …", "… ff.","…/…", "… Ausg. …", "c …".
