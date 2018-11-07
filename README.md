# Dice Thrower from a File Lab

## Learning Goals

- Read Data from a file
- Parse Data from a file
- Create Object-Oriented program

## Introduction

Object Orientation (OO) is a powerful technique that keeps code easy to reason
about. It wraps data and behaviors into little "cells." This makes code that's
easier to understand and maintain. As any human knows, many specialized cells
collaborating can produce an amazing outcome (you, for example).

Most OO instruction focuses on building classes that represent or mimic things
in the real world: `Die`, a `Dog`, or a `DiceRoller`. In this lesson we'll also
see that we can make classes that represent other less-tangible things like
processes, policies, strategies for solving problems or, as we'll see in this
lesson, things we wish we wish a "dumb" file could tell us about the data it
holds.

Before we can power up files with the power of OO, we need to review some facts
about how programs work with data files.

## Read Data from a file

There are many types of files that programming languages can read. Some are
written in binary: 1's and 0's that humans can't read. Files like Word docs,
JPGs, GIFs, and music files are all binary files.

Other types of files, like Ruby source or a text file can be read by humans _as
well as computers_. We call those files "plain text" files.

So any file that _both_ humans and computers can read will have to be in plain
text. But for a computer to read a data file the data needs to be regular and
predictable. The content must follow a _format_. It must be _structured_.
Consider the difference between the body of an email and an HTML `<table>`. The
email body has no regular structure, but the HTML `<table>` does!

HTML is a good example of a _format_ it explains what bits of text can follow
other bits of text. That's why it's possible for a computer to validate HTML.
The file's contents should follow the promise of looking like the HTML format.
JSON files should follow the promise of looking like the JSON format.

The [CSV] format is used to store data that both humans and computers can use.
It acts a bit like a plain-text database. JSON files are a very common way to
share information. While executives might speak Excel, Excel can be exported to
CSV, and languages like Ruby, JavaScript, Python, and Java **all**  know how to
speak CSV.

Here's a sample of some CSV data:

```csv
1,Arya,6,"4,3"
2,Belinda,6,"4,2"
3,Max,6,"5,3"
```

Each row (a sequence of data split on `\n`) is made up of fields which are
bounded by `,`. The first field seems to be an `id`, the second a name, the
third a constant integer, and the fourth a grouping surrounded by `"` and
internally divided into sub-fields based on a `,` again. We have a human- and
computer-readable document that shows regularity in structure. Its structure
follows the CSV format.

## Parse Data from a file

We can use the [CSV] ("comma separated values") library to interpret the
plain-text file read from disk according to the "comma-separated values"
format.

In this lab you'll create a class that manages a CSV file. It will turn "data
from the disk" into "rows of CSV-foratted data." Furthermore, the class will
provide analysis of the data it possesses.

## Create Object-Oriented Program

### Scenario

Our Department of Luck Studies is looking for a new President. To do this, they
had candidates throw many dice rolls in hopes of finding the luckiest
individual. _Unluckily_ the database they were storing this information in
crashed and lost most of its records.  Our dedicated IT team, however, managed
to save 100 of the trials to the file `trials.csv`.

The rows in the CSV represent:

```csv
1,Arya,6,"4,3"
```

1. An `id` for the trial
2. A participant name (a candidate)
3. The maximum number of pips on the die thrown
4. The results of the candidate's throw. Multiple die are separated by a `,`

### First Release: Learn About the Data

We want to analyze this file for the Department of Luck studies.

The tests will guide you in creating a class called `LuckAnalyzer.rb` in
`lib/luck_analyzer.rb`.

* `LuckAnalyzer` class will:
  * Be initialized by being passed the name of the file to read
  * `initialize`
    * Will build a file path from the file name passed in
    * Will "parse" the data in the file at the file path according to the CSV
      format using the Ruby [CSV] library method that reads in a file and
      converts the rows to an `Array` of `Arrays`. Consult the
      [documentation][CSV] for this method
    * The parsed CSV data should be available through a reader called
      `csv_data`
  * Instance methods
    * `common_number_of_trials`: Because the database crashed, we're not sure
      how many trials are common to the participants. We need this class to
      tell us the minimum common number of trials. For example: [A, A, A, A, B,
      B, B, B, B, C, C, C] has a common trial of `3` since C has the fewest.

### Second Release: Who is the Luckiest?

The Department of Luck Studies would like for you to determine the following:

1. "Who is the luckiest candidate?" Where:
  1. "Lucky" is defined as having had a dice roll where the total pips is equal
     to `7`
  2. "Luckiest" is defined as having the most "lucky" rolls within a set of
     size `common_number_of_trials`. We should take as many "lucky" rolls as
     possible
    * Example
     * Gven a common size of 3 where "L" means "Lucky" and "U" means "Unlucky":
      * A has L, L, L, L => [L, L, L] => 3/3 => 100%
      * B has U, U, U, L => [L, U, U] => 1/3 => 33%
      * C has U, U, U, U, U, U, U, U, U, L, U, L => [L, L, U] => 66%

#### Build It!

The Department of Luck Studies (DoLS) famously asks for new features often, so
we want to build this using an OO. DoLS have certified that the current
implementations of `Die` and `DieRoller` are properly lucky. They have
requested, and paid you handsomely, to update the existing classes (provided in
`lib/`). The trick is that _new_ features can't break _existing_ functionality.

* Update `Die` so that:
  * It can be initialized with a permanent value. When a set value is present, all
    calls to the `#roll` instance method return the set value. The permanent
    value should be accessible via a call to `set_value`
 * It provides a `#random_roll?` method which returns `true` if the `Die`'s
   roll value is set randomly and `false` if not.

* Update `DieRoller` so that:
  * It can be initialized with an `Array` of `Die`
  * It has a `lucky?` method to return `true` if the pips total 7, `false`
    otherwise

* Update `LuckAnalyzer`
  * Add a method called `luckiest` to return the luckiest participant as
    defined above. To do so you will need to follow this pseudocoded algorithm

```text
For each row of CSV data
  Find the pips count field, split it on `,`
    For each sub field create a Die with a hard-set values and save them to an
      Array
  Take the Array of Die, and load them up into a DiceRoller because they know
    how to determine whether a roll was `lucky?`
  If the roll was lucky adjust the participant's "lucky percentage"
    Increase their lucky count (numerator) by 1 as its divided by
    `common_number_of_trials` (denominator)
```

## Conclusion

Whew! This is a worthy OO challenge and represents what software developers do
a lot of the time: maintain an existing OO code base, adding features but
keeping the old tests happy while adding new ones! Congratulations!

We also guided you through a process of thinking about how to make small
"cells" of objects collaborate together to make getting answers simpler.

## Resources

[CSV]: https://ruby-doc.org/stdlib-2.0.0/libdoc/csv/rdoc/CSV.html