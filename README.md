# Ibex on Mechanical Turk

## Overview

This is a fork of Alex Drummond's ibex. User documentation for standard ibex can
be found [here](https://github.com/addrummond/ibex/blob/master/docs/manual.md).

The main purpose of the modifications found on this fork are to allow
researchers to easily run ibex directly on Amazon Mechanical Turk, instead of
sending workers to a separate website. This requires relatively minor
alterations to the javascript constituting ibex – mainly, in how results are
handled at the end of an experiment and in how ibex CSS is mangled.

The main features that this fork lacks is advanced counter handling and early
sending of results, which are implemented in ibex using special kinds of
controllers. This means that you probably don't want to rely on this version of
ibex to handle list construction – though randomization within a particular list
is still supported. Instead, you will probably want to construct the relevant
lists yourself – otherwise, you will have to rely on randomly selected counters.

## Running an experiment

As with most Mechanical Turk experiments, there are two files you will need for
running an ibex experiment directly on Mechanical Turk: a HIT template
([www/main.html](https://github.com/aaronstevenwhite/ibex/blob/master/www/main.html))
and a CSV containing the item lists (and other metadata) you will be using (e.g.
[data_includes/example_data.csv](https://github.com/aaronstevenwhite/ibex/blob/master/data_includes/example_data.csv)).

The CSV file must contain five columns: `gitid`, `shuffleSequence`,
`practiceItemTypes`, `defaults`, and `items`. The last four correspond to
javascript variables of the same names that need to be set for ibex to run –
compare
[data_includes/example_data.js](https://github.com/aaronstevenwhite/ibex/blob/master/data_includes/example_data.js)
with
[data_includes/example_data.csv](https://github.com/aaronstevenwhite/ibex/blob/master/data_includes/example_data.csv)).
The values of these columns are slotted into the
[www/main.html](https://github.com/aaronstevenwhite/ibex/blob/master/www/main.html)
template at batch creation time. A gotcha to keep an eye on is that, because we
are embedded javascript within a CSV, you will need to make sure you correctly
escape special characters – especially quotes.

The first column, `gitid`, points to the latest commit identifier for this
repository. This column is necessary because the javascript found in this
repository is served from a [rawgit](https://rawgit.com/) CDN, and thus you will
need to ensure that you are using the latest version of this fork. The current
latest commit for this repository, should be included in every row of your CSV.

If you are accustomed to separating your instructions and demographics forms
into separate HTML files, which you then load into ibex farm, know that these
files will either need to be compiled into the CSV or loaded from an external
web resource using javascript (also included in the CSV).

There are basically two steps for running an experiment:

1. Create a Mechanical Turk HIT template, dropping
[www/main.html](https://github.com/aaronstevenwhite/ibex/blob/master/www/main.html)
directly into the source. Mechanical Turk (or really the ibex validators) may
complain that you haven't defined the `items` variable. This is normal because
the `items` variable (along with the `shuffleSequence`, `practiceItemTypes`, and
`defaults` variables) will only be defined once you upload the CSV containg your
items and metadata, and Mechanical Turk's templating system inserts the
javascript found in that CSV.
2. Create a batch from your HIT template by uploading the CSV contaiing your items.
