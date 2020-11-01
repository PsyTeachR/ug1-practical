# (APPENDIX) Appendices {-} 

# Installing R on your computer

## Why should I install R on my computer?

The R Server is cuts down on a lot of installation problems and it means that you have all the packages and functions you need already installed. However, it requires an internet connection to use and additional when it comes time to submit your R assessments, if you don't have R on your computer it means that you won't be able to open the files you download from the server to check they're ok before you submit them.

It is not necessary to install R on your computer, however, now that we're over the initial anxiety spike of using R for the first time, you may find it helpful.

## Windows

If you are using Windows, you should download and install the following:
* [R](https://cran.r-project.org/bin/windows/base/)
* [R Studio](https://rstudio.com/products/rstudio/download/#download)
* [RTools](https://cran.r-project.org/bin/windows/Rtools/)

Once you've installed all three programs, restart your computer. Then, open RStudio (not R) and run the below code:


```r
install.packages("tidyverse")
```

This will install the `tidyverse` package on your computer. You can still use the server to do all your work, but having R on your computer will make it easier to view the files.

If you have any problems installing R, please book into a GTA session as they should be able to help you with any installation problems.

## Mac

If you are using a Mac, you should download and install the following:
* [R](https://www.stats.bris.ac.uk/R/)
* [R Studio](https://rstudio.com/products/rstudio/download/#download)
* [XQuartz](https://www.xquartz.org/)

There have been a number of issues installing R on Macs. We recommend you watch this [walkthrough video](https://www.youtube.com/watch?v=90IdULVGmYY).

If you are using a Mac with the Catalina OS, we also recommend you read this [troubleshooting guide](https://psyteachr.github.io/FAQ/installing-r-and-rstudio.html#i-am-using-macos-10.15-catalina)

## Chromebooks

Please note that you cannot currently install R on a Chromebook, please continue to use the R Server.
