
<!-- README.md is generated from README.Rmd. Please edit that file -->

# deepredeff <img src="man/figures/logo.png" align="right" width="120" />

<!-- badges: start -->
<!-- [![CRAN_Status_Badge](https://www.r-pkg.org/badges/version/deepredeff)](https://cran.r-project.org/package=deepredeff) -->
[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![CRAN\_Status\_Badge](https://github.com/ruthkr/deepredeff/workflows/pkgdown/badge.svg)](https://ruthkr.github.io/deepredeff/)
[![Codecov test
coverage](https://codecov.io/gh/ruthkr/deepredeff/branch/master/graph/badge.svg)](https://codecov.io/gh/ruthkr/deepredeff?branch=master)
[![R build
status](https://github.com/ruthkr/deepredeff/workflows/R-CMD-check/badge.svg)](https://github.com/ruthkr/deepredeff/actions)
<!-- badges: end -->

**deepredeff** is a package to predict effector protein given amino acid
sequences. This tool can be used to predict effectors from three
different pathogens, which are oomycete, fungi, and bacteria.

## Installation

### Requirements

First, install the deepredeff package from GitHub as follows:

``` r
# install.packages("devtools")
devtools::install_github("ruthkr/deepredeff")
```

The deepredeff package uses the Keras and tensorflow. To install both
the core Keras library as well as the TensorFlow backend use the
install\_keras() function:

``` r
library(deepredeff)
install_keras()
```

This will provide you with default CPU-based installations of Keras and
TensorFlow. If you want a more customized installation, you can see the
documentation for install\_keras().

## Documentation

To use deepredeff, you can read the documentations on the following
topics:

## Quick start

This is a basic example which shows you how to predict effector
sequences if you have fasta file:

``` r
# Load the package
library(deepredeff)

# Define the fasta path from the sample data
bacteria_fasta_path <- system.file(
  "extdata/example", "bacteria_sample.fasta", 
  package = "deepredeff"
)

# Predict the effector candidate using bacteria model
pred_result <- deepredeff::predict_effector(
  input = bacteria_fasta_path,
  model = "bacteria"
)
#> Loaded models: cnn_gru, cnn_lstm, gru_emb, lstm_emb for bacteria.
#> Loaded models successfully!

# View results
pred_result %>%
  dplyr::mutate(
    name = stringr::str_replace_all(name, "\\|", "⎮"),
    sequence = stringr::str_sub(sequence, 1, 25)
  ) %>%
  knitr::kable()
```

| name                                                                                                                                                                               | sequence                  |      prob |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------ | --------: |
| tr⎮A0A0N8SZV2⎮A0A0N8SZV2\_PSESY Type III secretion system effector HopAI1 OS=Pseudomonas syringae pv. syringae OX=321 GN=ALO45\_04155 PE=4 SV=1                                    | MPINRPAFNLKLNTAIAQPTLKKDA | 0.9483424 |
| tr⎮A5CLR7⎮A5CLR7\_CLAM3 Pat-1 protein OS=Clavibacter michiganensis subsp. michiganensis (strain NCPPB 382) OX=443906 GN=pat-1 PE=4 SV=1                                            | MQFMSRINRILFVAVVSLLSVLGCC | 0.0798177 |
| sp⎮B2SU53⎮PTHX1\_XANOP TAL effector protein PthXo1 OS=Xanthomonas oryzae pv. oryzae (strain PXO99A) OX=360094 GN=pthXo1 PE=1 SV=2                                                  | MDPIRSRTPSPARELLPGPQPDRVQ | 0.9943361 |
| tr⎮C0SPN9⎮C0SPN9\_RALSL Uncharacterized protein RSc2139 OS=Ralstonia solanacearum OX=305 GN=RSc2139 PE=4 SV=1                                                                      | MSIGRSKSVAGASASHALASGENGS | 0.8418443 |
| tr⎮D2Z000⎮D2Z000\_RALSL Type III effector protein OS=Ralstonia solanacearum OX=305 GN=rip61 PE=4 SV=1                                                                              | MPPPIRNARTTPPSFDPSAAGDDLR | 0.9953785 |
| tr⎮Q8XX20⎮Q8XX20\_RALSO Putative multicopper oxidase, type 3 signal peptide protein OS=Ralstonia solanacearum (strain GMI1000) OX=267608 GN=RSc2298 PE=4 SV=1                      | MSHMTFNTWKAGLWRLAAAAVLSLL | 0.0645516 |
| tr⎮Q87UH8⎮Q87UH8\_PSESM Taurine ABC transporter, periplasmic taurine-binding protein OS=Pseudomonas syringae pv. tomato (strain ATCC BAA-871 / DC3000) OX=223283 GN=tauA PE=4 SV=1 | MKLHFSLRLLTALSLTGATFLAQAA | 0.0492858 |
| tr⎮Q4ZTI0⎮Q4ZTI0\_PSEU2 Amino acid ABC transporter substrate-binding protein, PAAT family OS=Pseudomonas syringae pv. syringae (strain B728a) OX=205918 GN=Psyr\_2503 PE=4 SV=1    | MHRGPSFVKACAFVLSASFMLANTV | 0.3061618 |
| tr⎮Q4ZR15⎮Q4ZR15\_PSEU2 Sensor protein OS=Pseudomonas syringae pv. syringae (strain B728a) OX=205918 GN=Psyr\_3375 PE=4 SV=1                                                       | MRRQPSLTLRSTLAFALVAMLTVSG | 0.0722144 |
| tr⎮D4I1R4⎮D4I1R4\_ERWAC Outer-membrane lipoprotein LolB OS=Erwinia amylovora (strain CFBP1430) OX=665029 GN=lolB PE=3 SV=1                                                         | MLSSNRRLLRLLPLASLLLTACGLH | 0.0489914 |

After getting the result, you can plot the probability distribution of
the result as follows:

``` r
ggplot2::autoplot(pred_result)
```

<img src="man/figures/README-pred_result_plot-1.png" style="display: block; margin: auto;" />

More examples with different input formats are available on functions
documentations and vignettes, please refer to the documentation.
