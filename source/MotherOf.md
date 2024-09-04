<!-- https://stackoverflow.com/questions/40717410/opts-knitsetroot-dir-path-does-not-work-after-rstudio-upgrade-1-0-44 -->

# Conventional Navigation

With the help of `rprojroot`, we can find the root folder of the
package.

    require('rprojroot')

    ## Loading required package: rprojroot

    find_root(is_rstudio_project)

    ## [1] "/data/R/wd_testing"

Let’s use a function to print the current directory and the content of a
`location.txt` file.

    CheckDirectory <- function () {
      print(getwd()) # display the working directory
      
      fi = 'location.txt'
      if (file.exists(fi)) {
        print(readLines(fi))
      }
    }

    CheckDirectory()

    ## [1] "/data/R/wd_testing"
    ## [1] "we are in the root folder..."

Attempt to walk to the “source” subfolder:

    tryCatch(
    {setwd('source')
    CheckDirectory()},
            error = function(cond) {
                message(paste('error:', conditionMessage(cond)))
                return(NA)
            }
    )

    ## [1] "/data/R/wd_testing/source"
    ## [1] "we are in the source folder..."

This gives a warning: &gt; Warning: The working directory was changed to
/data/R/wd\_testing/source inside a notebook chunk. &gt; The working
directory will be reset when the chunk is finished running. &gt; Use the
knitr root.dir option in the setup chunk to change the working directory
for notebook chunks.

… and then to the “data” subfolder:

    tryCatch(
    {setwd('../data')
    CheckDirectory()},
            error = function(cond) {
                message(paste('error:', conditionMessage(cond)))
                return(NA)
            }
    )

    ## error: cannot change working directory

    ## [1] NA

Finally, back to the roots:

    setwd('..')
    CheckDirectory()

    ## [1] "/data/R"

(… this is unexpected; I would not want to walk out of the project
folder.)

Go back to the root:

    setwd(find_root(is_rstudio_project))

# Using the `here` Package

I found [https://here.r-lib.org](this) on reddit:

    require('here')

    ## Loading required package: here

    ## here() starts at /data/R/wd_testing

    here()

    ## [1] "/data/R/wd_testing"

    here('source', 'location.txt')

    ## [1] "/data/R/wd_testing/source/location.txt"
