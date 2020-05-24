---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: awk
menu:
  linux_resources:
    name: awk
    weight: 
title: awk
toc: true
type: docs
weight: 
---

<!--
1. replace linux_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace awk with page name e.g. dplyr
4. replace  with weight e.g. 20
-->

## view CSV
- view csv in human readable columns when field has quoted commas (e.g. "1,2,3")
```bash
$ awk '!(NR%2){gsub(",",";")} 1' RS=\" ORS=\" <file.csv> | column -t -s , | less -S
```

## -v flag
- using variable inside AWK, define variables with -v flag
- [https://www.unix.com/shell-programming-and-scripting/118704-awk-comparison-variable.html](https://www.unix.com/shell-programming-and-scripting/118704-awk-comparison-variable.html)
```bash
$ awk -F" " -v d1="$d1" -v d2="$d2" '$1==d1"-"d2"-2009" {print $1,$2,$3,$4,$5}'
```

## col min max
- put min or max of column into a var
```bash
min=`awk 'BEGIN{a=1000}{if ($1<0+a) a=$1} END{print a}' <file>`
echo $min
max=`awk 'BEGIN{a=   0}{if ($1>0+a) a=$1} END{print a}' <file>`
echo $max
```
- 1000 and 0 are just values to begin testing
- 0+a so cast a into numeric, or else will compare lexicographically
- can also do
```bash
$ cut -d " " -f1 <file> | sort -n | sed -n '1s/^/min=/p; $s/^/max=/p'
```
- but it's slower to sort

## define output sep
- change or define output separator
- [https://stackoverflow.com/questions/20844666/setting-the-output-field-separator-in-awk](https://stackoverflow.com/questions/20844666/setting-the-output-field-separator-in-awk)
```bash
$ awk 'BEGIN {FS="\t"; OFS=","; print} {$1=$1}1' <file>
```

## rm dup in 2 col, both directions
- remove dups in 2 rows in either direction
- [https://stackoverflow.com/questions/49343171/remove-duplicated-rows-based-on-two-columns-in-both-directions-and-sorted-by-a](https://stackoverflow.com/questions/49343171/remove-duplicated-rows-based-on-two-columns-in-both-directions-and-sorted-by-a)
```bash
$ awk '!a[$1$2]++ && !a[$2$1]++' <file>
```

## col order
- swap columns
- [https://stackoverflow.com/questions/11967776/swap-two-columns-awk-sed-python-perl](https://stackoverflow.com/questions/11967776/swap-two-columns-awk-sed-python-perl)
```bash
$ awk ' { t = $1; $1 = $2; $2 = t; print; } ' <file>
```

## filter rows with col regex
- filter rows where column value matches regex
- [https://stackoverflow.com/questions/18962153/filter-column-with-awk-and-regexp](https://stackoverflow.com/questions/18962153/filter-column-with-awk-and-regexp)
```bash
$ awk '$1~/^chr([1-9]|[1-9][0-9]|[XY])$/' <file.bed>
```
- matches if column 1 starts with "chr", ends with 1-9 or 10-99 or X or Y
- ~ specifies for regex match (== specifies comparison match)

## another regex comparison
- [https://unix.stackexchange.com/questions/512567/regex-in-if-condition-in-awk](https://unix.stackexchange.com/questions/512567/regex-in-if-condition-in-awk)
- [https://www.linuxquestions.org/questions/linux-general-1/awk-regex-with-variable-796019/](https://www.linuxquestions.org/questions/linux-general-1/awk-regex-with-variable-796019/)
```bash
$ SNP_seed="rs7523690"
$ awk '/'$SNP_seed'/ {print}' <input_file> >> <output_file>
or
$ awk '{ if($11 ~ /'$SNP_seed'/ {print}}' <input_file> >> <output_file>
```

## -F flag
- define field separators with -F flag
- [https://askubuntu.com/questions/342842/what-does-this-command-mean-awk-f-print-4](https://askubuntu.com/questions/342842/what-does-this-command-mean-awk-f-print-4)
```bash
$ awk -F: '{print $4}' <file>
```

## filter rows with multiple cols
- filter rows with multiple comparisons in column values
- [https://www.tim-dennis.com/data/tech/2016/08/09/using-awk-filter-rows.html](https://www.tim-dennis.com/data/tech/2016/08/09/using-awk-filter-rows.html)
```bash
$ awk -F "\t" '{ if(($7 == 6) && ($8 >= 11000000 && $8 <= 25000000)) { print } }' <file>
```
- or
```bash
$ awk '$1==22 && $2<23966388 && $3>23966388' <file>
```

## sub multiple cols
- substitute within multiple columns
```bash
$ awk '{sub(/find1/,replace1,$col1);sub(/find2/,replace2,$col2);print $col1, $col2}' input.txt > output.txt
```

## num cols
- get number of cols in a file
```bash
$ awk '{print NF}' <file> | sort -nu | tail -n1
```

## EOF