# Backward Digit Span (BDS)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1415363.svg)](https://doi.org/10.5281/zenodo.1415363)

The BDS is working memeoy test using backward recall of sequences of digits.


## Citation

We also advise mentioning the software versions you used,
in particular the versions of the `BDS` and `psychTestR` packages.
You can find these version numbers from R by running the following commands:

``` r
library(BDS)
if (!require(devtools)) install.packages("devtools")
x <- devtools::session_info()
x$packages[x$packages$package %in% c("BDS", "psychTestR"), ]
```

## Installation instructions (local use)

1. If you don't have R installed, install it from here: https://cloud.r-project.org/

2. Open R.

3. Install the ‘devtools’ package with the following command:

`install.packages('devtools')`

4. Install the BDS:

`devtools::install_github('klausfrieler/BDS')`

## Usage

### Quick demo 

You can demo the BDS at the R console, as follows:

``` r
# Load the BDS package
library(BDS)

# Run a demo test, with feedback as you progress through the test,
# and not saving your data
BDS_demo()

# Run a demo test, skipping the training phase, and only asking 5 questions, as well a changinge the language
BDS_demo(num_items = 5, take_training = FALSE, language = "DE")
```

### Testing a participant

The `BDS_standalone()` function is designed for real data collection.
In particular, the participant doesn't receive feedback during this version.

``` r
# Load the BDS package
library(BDS)

# Run the test as if for a participant, using default settings,
# saving data, and with a custom admin password
BDS_standalone(admin_password = "put-your-password-here")
```

You will need to enter a participant ID for each participant.
This will be stored along with their results.

Each time you test a new participant,
rerun the `BDS_standalone()` function,
and a new participation session will begin.

You can retrieve your data by starting up a participation session,
entering the admin panel using your admin password,
and downloading your data.
For more details on the psychTestR interface, 
see http://psychtestr.com/.

The BDS currently supports English (EN) and  German (DE).
You can select one of these languages by passing a language code as 
an argument to `BDS_standalone()`, e.g. `BDS_standalone(languages = "DE")`,
or alternatively by passing it as a URL parameter to the test browser,
eg. http://127.0.0.1:4412/?language=DE (note that the `p_id` argument must be empty).

## Installation instructions (Shiny Server)

1. Complete the installation instructions described under 'Local use'.
2. If not already installed, install Shiny Server Open Source:
https://www.rstudio.com/products/shiny/download-server/
3. Navigate to the Shiny Server app directory.

`cd /srv/shiny-server`

4. Make a folder to contain your new Shiny app.
The name of this folder will correspond to the URL.

`sudo mkdir BDS`

5. Make a text file in this folder called `app.R`
specifying the R code to run the app.

- To open the text editor: `sudo nano BDS/app.R`
- Write the following in the text file:

``` r
library(BDS)
BDS_standalone(admin_password = "put-your-password-here")
```

- Save the file (CTRL-O).

6. Change the permissions of your app directory so that `psychTestR`
can write its temporary files there.

`sudo chown -R shiny BDS`

where `shiny` is the username for the Shiny process user
(this is the usual default).

7. Navigate to your new shiny app, with a URL that looks like this:
`http://my-web-page.org:3838/BDS

## Implementation notes

By default, the BDS  implementation always estimates participant abilities
using weighted-likelihood estimation.
We adopt weighted-likelihood estimation for this release 
because this technique makes fewer assumptions about the participant group being tested.
This makes the test better suited to testing with diverse participant groups
(e.g. children, clinical populations).

## Usage notes

- The BDS runs in your web browser.
- By default, image files are hosted online on our servers.
The test therefore requires internet connectivity.
