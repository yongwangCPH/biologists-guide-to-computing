Structuring and storing data
============================

Data structures
---------------

In :doc:`how-to-think-like-a-computer` we introduced some basic data types,
including integers, floating point numbers, characters and strings. However,
most of the time we are not interested in individual instances of integers or
floating point values, we want to analyse lots of data points, often of a
heterogeneous nature.

We need some way of structuring our data. In computing there are several well
defined data structures for accomplishing this. Three data structures of particular
interest are lists, dictionaries and graphs. Lists and dictionaries are generally
useful and graphs are of particular interest in biology.

A list, also known as an array, is basically a collection of elements in a
specific order. The most common use case is to simply go through all the
elements in a list and do something with them. For example if you had a stack of
CVs on your desk you could go through them and depending on whether or not the
font was too small you could through them in the bin or not. This is similar to
what we did with the FASTA file in :doc:`first-steps-towards-automation`, if a
line did not contain the expression "OS=Homo sapiens" it was ignored.

Because lists have an order associated with them they can also be used to
implement different types of queuing behaviour such as "first-in-first-out" and
"first-in-last-out". Furthermore, individual elements can usually be accessed
through their numerical indices.

A dictionary, also known as a map or an associative array, is an unordered
collection of elements that can be accessed through their associated
identifiers. In other words each entry in a dictionary has a name, the
identifier, and a value. For example, suppose that you needed to store
information about the flowering time for different species of flowers. In this
case it would make a lot of sense to have a system where you could look up the
flowering time information based on the species name.

It is worth noting that it is possible to create nested data structures. For
example, think of a spreadsheet. The spreadsheet could be represented as a list
containing lists. In other words the elements of the outer list would represent
the rows in the spreadsheet and the elements in an inner list would represent
values of cells at different columns for a specific row.

A graph, also known as a tree, is a data structure that links nodes together
via edges. This should sound relatively familiar to you as it is the basic
concept behind phylogenetic trees. Each node represents a species and the edges
represent the inferred evolutionary relationships between the species.

Because of their general utility lists and dictionaries are built-in to many
high level programming languages such as Python and JavaScript. However, data
structures for graphs are generally not.


Data persistence
----------------

Suppose that your program has generated a phylogenetic tree and it has used
this tree to determine that scientists and baboons are more closely related
than expected. Result! At this point the program is about to finish. What
should happen to the phylogenetic tree? Should it be thrown away or will you
still need access to it even though you have got your killer result? If the
former is the case you don't need to do anything the data structure will
disappear when the program exits. As outlined in
:doc:`how-to-think-like-a-computer` RAM is volatile. Therefore if the latter is
the case you will want to make your tree persistent by writing it to disk.

At this point you have a choice: you can store the data in a binary format that
only your program understands or you can store the data as a plain text file.
Storing your data in a binary format has advantages in that the resulting files
will be smaller and they will load quicker. However, in the next section you will
find out why you should (almost) always choose to store your data as plain text
files.

The beauty of plain text files
------------------------------

Plain text files are great because you can open and edit them on any computer.
The operating system does not matter as ASCII and Unicode are universal
standards.  With the slight caveat that you may occasionally have to deal with
converting between Windows and Unix line endings (as discussed earlier in
:doc:`how-to-think-like-a-computer`).

Furthermore, they are easy to use. You can simply open them in your text editor
of choice and start typing away.

Some software companies rely on their software being the only software to be
able to work with the binary files that their software produces to ensure that
users have to renew their licence. This is not great from the users point of
view. It also makes it hard to share data with people that do not have a
licence to the software in question. Making use of plain text files and
software that can output data in plain text works around this problem.

Finally, there is a rich ecosystem of tools available for working with plain
text files.  Apart from text editors, there are all of the Unix command line
tools. We looked at some of these in :doc:`first-steps-towards-automation`.
In the next chapter, :doc:`keeping-track-of-your-work`, we will look at a
tool called ``git`` that can be used to track changes to plain text files.


Useful plain text file formats
------------------------------

There are many well established file formats for representing data in plain
text. These have arisen to solve different types of problems.

Plain text files are commonly used to store notes, for example the minutes of a
meeting or a list of ideas. When writing these types of documents one wants to
be able to make use of headers, bullet points etc. A popular file format for
creating such documents is `markdown
<https://daringfireball.net/projects/markdown/>`_. Markdown (MD) provides a
simple way to add formatting such as headers and bullet lists by providing a
set of rules of for how certain plain text constructs should be converted to
HTML and other document formats.

.. code-block:: none

    # Level 1 header

    ## Level 2 header

    Here is some text in a paragraph.
    It is possible to *emphasize words with italic*.
    It is also possible to **strongly emphansize words in bold**.

    - First item in a bullet list
    - Second item in a bullet list

    1. First item in a numbered list
    2. Second item in a numbered list

    [Link to BBC website](www.bbc.com)

    ![example image](path/to/example/image.png)

Hopefully the example above is self explanatory. For more information have a
look at the `official markdown syntax page
<https://daringfireball.net/projects/markdown/syntax>`_.

Another scenario may be wanting to record tabular data, for example the results
of a scientific experiment. In other words the type of data you would want to
store in a  spreadsheet. Comma separated value (CSV) files are ideally suited
for this. This file format is relatively basic, values are simply separated by
commas and the file can optionally start with a header. It is worth noting
that you can include a comma in a value by surrounding it by double quotes. Below
is an example of a CSV file containing two rows and three columns containing

.. code-block:: none

    Last name,First name(s),Age
    Smith,Alice,34
    Smith,"Bob, Carter",56

Another scenario, when coding, is the ability to store richer data structures,
such as lists or dictionaries, possibly nested within each other. There are two
popular file formats for doing this `JavaScript Object Notation
<http://www.json.org/>`_ (JSON) and `YAML Ain't Markup Language
<http://www.json.org/>`_ (YAML).

.. sidebar:: Recursive acronyms

    You may ask yourself why the full name of YAML includes the word YAML.
    This is because programmers are fond of `recursion
    <https://en.wikipedia.org/wiki/Recursion_%28computer_science%29>`_,
    procedures whose implementation call themselves. Other famous
    recursive acronyms include GNU (GNU's Not Unix), curl (C URL Request
    Library) and Fiji (Fiji Is Just ImageJ).

JSON was designed to be easy to for machines to generate and parse and is used
extensively in web applications as it can be directly converted to JavaScript
objects. Below is an example of JSON representing a list of scientific discoveries, where
each discovery contains a set of key value pairs.

.. code-block:: json

    [
      {
        "year": 1965,
        "scientist": "Robert Hooke",
        "experiment": "light microscopy",
        "discovery": "cells"
      },
      {
        "year": 1944,
        "scientist": "Barbara McClintock",
        "experiment": "breeding maize plants for colour",
        "discovery": "jumping genes"
      }
    ]

YAML is similar to JSON in that it is a data serialisation standard. However, it
places more focus on being human readable. Below is the same data structure
represented using YAML.

.. code-block:: yaml

    ---
      - 
        year: 1965
        scientist: "Robert Hooke"
        experiment: "light microscopy"
        discovery: "cells"
      -
        year: 1944
        scientist: "Barbara McClintock"
        experiment: "breeding maize plants for colour"
        discovery: "jumping genes"

A nice feature of YAML is the ability to add comments to the data giving further explanation
to the reader. These comments are ignored by programs parsing the files.

.. code-block:: yaml

    ---
      # TODO: include an entry for Anton van Leeuwenhoek here.
      - 
        year: 1965
        scientist: "Robert Hooke"
        experiment: "light microscopy"
        discovery: "cells"
      -
        year: 1944
        scientist: "Barbara McClintock"
        experiment: "breeding maize plants for colour"
        discovery: "jumping genes"


Find a good text editor and learn how to use it
-----------------------------------------------

A key step to boost your productivity is to find a text editor that suits you, and
learning how to make the most of it.

Popular text editors include `Sublime Text <http://www.sublimetext.com/>`_,
`Geany <http://www.geany.org/Main/HomePage>`_ and `Atom <https://atom.io/>`_.

If you enjoy working on the command line I would highly recommend experimenting
with command line editors. Popular choices include `nano
<http://www.nano-editor.org/>`_, `emacs <https://www.gnu.org/software/emacs/>`_
and `vim <http://www.vim.org/>`_. The former is easy to learn, whereas the latter
two give much more power, but are somewhat more difficult to learn.

.. sidebar:: Vim is great!

    Personally, I use ``vim`` for everything. It is one of the few editors that
    is installed by default on any Unix based system. Furthermore, it is
    extremely powerful and allows you to do everything using the keyboard. I
    like this because using the mouse for extended periods of time makes my
    index finger hurt.

    If you have half an hour spare I highly recommend that you try running
    the ``vimtutor`` command in a terminal.


Key concepts
------------

- Lists, also known as arrays, are ordered collections of elements
- Dictionaries, also known as maps and associative arrays, are unordered
  collections of key-value pairs
- Graphs, also known as trees, links nodes via edges and is of relevance to
  phylogenetic trees
- In computing persistence refers to data outliving the program that generated
  it
- If you want any data structures that you have generated to persist you need
  to write them to disk
- Saving your data as plain text files is almost always preferable to saving it
  as a binary blobs
- There are a number of useful plain text file formats for you to make the most
  of
- Don't invent your own file format
- Learn how to make the most out of your text editor of choice