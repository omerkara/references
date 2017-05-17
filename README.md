# My BibTeX database


```
## Last Updated: 2017-05-17 16:30:52
```

License: Public Domain (CC-0)

This is the bibtex (.bib) file containing all of my bibliographic references. Figured I'd share it publicly for no reason.



Here are some basic statistics on its contents:


```r
library("RefManageR")
library("ggplot2")
bib <- suppressWarnings(RefManageR::ReadBib("references.bib", check = FALSE))
dat <- as.data.frame(bib)
dat$year <- as.numeric(dat$year)
```

```
## Warning: NAs introduced by coercion
```

```r
dat$journal[is.na(dat$journal)] <- dat$journaltitle[is.na(dat$journal)]
```

## Citation Types


```r
dat$bibtype <- factor(dat$bibtype, levels = names(sort(table(dat$bibtype))))
ggplot(dat, aes(x = bibtype)) + geom_bar() + 
  xlab("Count") + ylab("Citation Type") + coord_flip()
```

![plot of chunk bibtype](http://i.imgur.com/YxUNWi6.png)

## Journals


```r
datj <- aggregate(bibtype ~ journal, data = dat, FUN = length)
datj <- head(datj[order(datj$bibtype, decreasing = TRUE), ], 30)
datj$journal <- factor(datj$journal, levels = rev(datj$journal))
ggplot(datj, aes(x = journal, y = bibtype)) + geom_bar(stat = "identity") + 
  ylab("Count") + xlab("Journal") + coord_flip()
```

![plot of chunk journal](http://i.imgur.com/m8OlPdq.png)

## Authors


```r
aut <- unlist(lapply(unlist(lapply(bib, function(x) unclass(x$author)), recursive = FALSE), `[[`, "family"))
aut <- as.data.frame(head(sort(table(aut), decreasing = TRUE), 50))
aut$aut <- factor(aut$aut, levels = rev(aut$aut))
ggplot(aut, aes(x = aut, y = Freq)) + geom_bar(stat = "identity") + 
  ylab("Count") + xlab("Author Surname") + coord_flip()
```

![plot of chunk authors](http://i.imgur.com/vROL4Ww.png)

## Publication Years


```r
ggplot(dat[dat$year > 1900, ], aes(x = year)) + geom_bar() +
  xlab("Publication Year") + ylab("Count")
```

```
## Warning: Removed 62 rows containing non-finite values (stat_count).
```

![plot of chunk year](http://i.imgur.com/BjltrEc.png)


