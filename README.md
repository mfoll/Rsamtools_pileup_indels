
```r
library(Rsamtools)
bam_file <- "test.bam"
sbp <- ScanBamParam(which = GRanges("chr17", IRanges(7578425, 7578425)))
p_param <- PileupParam(max_depth = 30000, min_base_quality = 0, include_insertions = TRUE)
res <- pileup(bam_file, scanBamParam = sbp, pileupParam = p_param)
res
```

```
##    seqnames     pos strand nucleotide count           which_label
## 1     chr17 7578425      +          +     7 chr17:7578425-7578425
## 2     chr17 7578425      -          +    14 chr17:7578425-7578425
## 3     chr17 7578425      +          -    61 chr17:7578425-7578425
## 4     chr17 7578425      -          -     2 chr17:7578425-7578425
## 5     chr17 7578425      +          A    11 chr17:7578425-7578425
## 6     chr17 7578425      -          A     5 chr17:7578425-7578425
## 7     chr17 7578425      +          C    21 chr17:7578425-7578425
## 8     chr17 7578425      -          C    23 chr17:7578425-7578425
## 9     chr17 7578425      +          G     7 chr17:7578425-7578425
## 10    chr17 7578425      -          G     5 chr17:7578425-7578425
## 11    chr17 7578425      +          T  4866 chr17:7578425-7578425
## 12    chr17 7578425      -          T  6273 chr17:7578425-7578425
```

Note that first four lines don't give details about the actual insertions and deletions. Samtools pilepup actually gives this information and it would be good to have them (optionally) here too. For example here: 

- The 7 insertions on the + strand consist of 6 insertions of a G and 1 insertion of TCA. 
- The 14 insertions on the - strand consist of 11 deletions of a G, 2 of a A and one of a C.
- The 61 deletions on the + strand consist of 41 insertions of TGT, 18 of a T and 2 of TG.
- The 2 deletions on the - strand consist of 2 deletions of a T.

So one possible way would be to output a data frame like this containing the full data:


```
##    seqnames     pos strand nucleotide count           which_label
## 1     chr17 7578425      +         +G     6 chr17:7578425-7578425
## 2     chr17 7578425      +       +TCA     1 chr17:7578425-7578425
## 3     chr17 7578425      -         +G    11 chr17:7578425-7578425
## 4     chr17 7578425      -         +A     2 chr17:7578425-7578425
## 5     chr17 7578425      -         +C     1 chr17:7578425-7578425
## 6     chr17 7578425      +       -TGT    41 chr17:7578425-7578425
## 7     chr17 7578425      +         -T    18 chr17:7578425-7578425
## 8     chr17 7578425      +        -TG     2 chr17:7578425-7578425
## 9     chr17 7578425      -         -T     2 chr17:7578425-7578425
## 51    chr17 7578425      +          A    11 chr17:7578425-7578425
## 61    chr17 7578425      -          A     5 chr17:7578425-7578425
## 71    chr17 7578425      +          C    21 chr17:7578425-7578425
## 81    chr17 7578425      -          C    23 chr17:7578425-7578425
## 91    chr17 7578425      +          G     7 chr17:7578425-7578425
## 10    chr17 7578425      -          G     5 chr17:7578425-7578425
## 11    chr17 7578425      +          T  4866 chr17:7578425-7578425
## 12    chr17 7578425      -          T  6273 chr17:7578425-7578425
```

Note that the last 8 lines are just the same as above, only the indels lines now show more details.
