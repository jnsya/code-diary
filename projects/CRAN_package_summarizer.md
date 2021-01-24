# CRAN Server Summarizer

Fields required (O = contained in overview file, D = contained in package description)
● Package name (O)
● Versions (O,D)
● R Version needed (O,D)
● Dependencies (O,D)
● Date/Publication (D)
● Title (D)
● Authors (D)
● Maintainers (D)
● License (O,D)

=> Package(id: integer, name: string, version: string, r_version_needed: string, dependencies: string, license: string, url: string)

Fields from description
- title: text
- publication_date: datetime
- authors: text
- 

## README
### To improve:

This is a working MVP, but there are some areas that could be improved in further iterations:

- Database fields 
  - So far, almost every field is text. This is suboptimal for searching purposes - was added as an MVP. Instead we could do:
  - authors could be an array of each author
  - version could be an array (or something else?) 

- *Performance*
  - The user has to wait for the task to finish.
  - Do the update of each package as an asynchronous background job, for example with sidekiq.
  
- Efficiency
  - Only update records when they've changed
  - Create a local copy of the 
  
- Formatting
 - eg "Rasmus B\xC3\xA5\xC3\xA5th" is the name of one maintainer in the database
 
- Memory
  - We load the entire large list file into memory - it might be better to save it locally, then iterate over smaller batches of it to create the records.

## MVP:
- Create a rails app, with a Package model.
- A service connects to the FTP server and collects only the info contained in the overview file.
- All fields are saved as text in the database.

## Process:
- The packages list file contains brief description of every package on the server.
  (http://cran.r-project.org/src/contrib/PACKAGES.gz​)
- Get the packages list file, and parse it to retrieve some information.
- Use this information to create the URL of each package.
- Access and decompress the URL of each package.
- Retrieve the `DESCRIPTION` file, which contains the rest of the info for that package.


## Challenges
- The `R version` is part of the `depends` field. So it will need to be extracted.
- URL needs to be generated dynamically
  `{package_name}_{version_number}`
  - Working example (with HTTPS!): https://cran.r-project.org/src/contrib/ForestFit_0.6.1.tar.gz

Zlib::GzipReader.open(ftp.get("/pub/R/src/contrib/ForestFit_0.6.1.tar.gz"))
- 

LinkingTo: Rcpp\nURL: http://contoureR.com\nCollate: 'contoureR-package.R' 'RcppExports.R' 'RcppExports-Doc.R' 'convexHull.R' 'delaunayMesh.R' 'contourLines.R' 'orderPoints.R'\n"

## Does Not Work:
YAML.load "Package: contoureR\nType: Package\nTitle: Contouring of Non-Regular Three-Dimensional Data\nVersion: 1.0.5\nDate: 2015-08-10\nAuthor: Nicholas Hamilton\nMaintainer: Nicholas Hamilton <n.hamilton@unsw.edu.au>\nDescription: Create contour lines for a non regular series of points, potentially from a non-regular canvas.\nLicense: GPL (>= 2)\nSystemRequirements: C++11\nDepends: geometry\nImports: Rcpp (>= 0.11.5), reshape, plyr\nSuggests: ggplot2\nLinkingTo: Rcpp\nURL: http://contoureR.com\nCollate: 'contoureR-package.R' 'RcppExports.R' 'RcppExports-Doc.R'\n        'convexHull.R' 'delaunayMesh.R' 'contourLines.R' 'orderPoints.R'\nNeedsCompilation: yes\nPackaged: 2015-08-25 06:13:41 UTC; nick\nRepository: CRAN\nDate/Publication: 2015-08-25 09:09:41\n"


YAML.load "Package: contoureR\nType: Package\nTitle: Contouring of Non-Regular Three-Dimensional Data\nVersion: 1.0.5\nDate: 2015-08-10\nAuthor: Nicholas Hamilton\nMaintainer: Nicholas Hamilton <n.hamilton@unsw.edu.au>\nDescription: Create contour lines for a non regular series of points, potentially from a non-regular canvas.\nLicense: GPL (>= 2)\nSystemRequirements: C++11\nDepends: geometry\nImports: Rcpp (>= 0.11.5), reshape, plyr\nSuggests: ggplot2\nLinkingTo: Rcpp\nURL: http://contoureR.com"

"Package: ForestFit\nType: Package\nTitle: Statistical Modelling for Plant Size Distributions\nAuthor: Mahdi Teimouri\nMaintainer: Mahdi Teimouri <teimouri@aut.ac.ir>\nDescription: Developed for the following tasks. 1 ) Computing the probability density function,\n             cumulative distribution function, random generation, and estimating the parameters\n \t\t\t of the eleven mixture models. 2 ) Point estimation of the parameters of two - \n\t\t\t parameter Weibull distribution using twelve methods and three - parameter Weibull \n\t\t\t distribution using nine methods. 3 ) The Bayesian inference for the three - \n\t\t\t parameter Weibull distribution. 4 ) Estimating parameters of the three - parameter\n\t\t\t Birnbaum - Saunders, generalized exponential, and Weibull distributions fitted to\n\t\t\t grouped data using three methods including approximated maximum likelihood, \n\t\t\t expectation maximization, and maximum likelihood. 5 ) Estimating the parameters\n\t\t\t of the gamma, log-normal, and Weibull mixture models fitted to the grouped data\n\t\t\t through the EM algorithm, 6 ) Estimating parameters of the nonlinear height curve\n\t\t\t fitted to the height - diameter observation, 7 ) Estimating parameters, computing\n\t\t\t probability density function, cumulative distribution function, and generating\n\t\t\t realizations from gamma shape mixture model introduced by Venturini et al. (2008)\n\t\t\t <doi:10.1214/07-AOAS156> , 8 ) The Bayesian inference, computing probability density function,\n\t\t\t cumulative distribution function, and generating realizations from four - parameter \n\t\t\t Johnson SB distribution, and 9 ) Robust multiple linear regression analysis when error\n\t\t\t term follows skewed t distribution.\nEncoding: UTF-8\nLicense: GPL (>= 2)\nDepends: R(>= 3.3.0)\nImports: ars, pracma\nRepository: CRAN\nVersion: 0.6.1\nDate: 2020-07-08\nNeedsCompilation: no\nPackaged: 2020-07-08 10:55:01 UTC; NikPardaz\nDate/Publication: 2020-07-08 11:10:02 UTC\n".gsub(/\t/, '')
