```bash
# returns lines with search occurance
grep "search string" filename.ext 
# filter whole words -w 
# non case sensitive -i
# line number -n
# lines before -B
# lines after -A
# context -C (-A -B)
grep -win -C 4 "search string" filename.ext
# all in current directory: ./*.ext
# recursive search -r
# see filename -l
# number of matches in file -c
grep -wirc "search string" .

# regular expression -P
```