RDF support
===========================================

| RDF (Resource Descriptor Framework) is a W3C recommendation for describing meta-content based on XML.
| The basic data structure of RDF is a set of 3 pieces of data called "triple", each of which consists of a subject (subject), a predicate (predicate), and an object (object).
| For more information, see `RDF Concepts And Abstract <https://www.w3.org/TR/2004/REC-rdf-concepts-20040210/>`__ .

| The RDF entity is composed of 3 elements, and this program supports RDF support in the form of "triple".
| The related AIML tags are addtriple, deletetriple, select, tuple, uniq.

Each "triple" represents a relationship between a subject (subject), a predicate (predicate), and an object (object). 
This triplet is called an element.

AIML triple file
----------------------------------------

| Users can store RDF elements in the form "triple". "triple" is 1 line of text consisting of Subject, Predicate, and Object, separated by a colon ':'.
| The following example is the RDF definition of an airplane.
| In this definition, "subject" is listed as "AIRPLANE" and a number of "predicate" describe each value ("object").

::

   AIRPLANE:hasPurpose:to transport us through the air
   AIRPLANE:hasSize:9
   AIRPLANE:hasSpeed:12
   AIRPLANE:hasSyllables:1
   AIRPLANE:isa:Aircraft
   AIRPLANE:isa:Transportation
   AIRPLANE:lifeArea:Physical

AIML RDF Tag
----------------------------------------

| AIML defines creation, deletion, and search tags such as "addtriple", "deletetriple", "select", and "uniq".
| You can also use tags like "set", "get", "first", and "rest" to process search results.

Load Element
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| This program has a function to load pre-prepared files.
| Reads the RDF definition file under the conditions set in rdf_storage of config.

.. code:: yaml

    client_type:
        storage:
            entities:
                rdf: file

            stores:
                file:
                    type: file
                    config:
                        rdf_storage:
                            dirs: ../storage/rdfs
                            subdirs: true
                            extension: txt

This section uses a definition file that describes the following.

::

   ant:legs:6
   ant:sound:scratch
   bat:legs:2
   bat:sound:eee
   bear:legs:4
   bear:sound:grrrrr
   buffalo:legs:4
   buffalo:sound:moo
   cat:legs:4
   cat:sound:meow
   chicken:legs:2
   chicken:sound:cluck cluck
   dolphin:legs:0
   dolphin:sound:meep meep
   fish:legs:0
   fish:sound:bubble bubble

Element Generation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A new "triple" can be dynamically added using not only loading data but also the addtriple element.

.. code:: xml

   <addtriple>
     <subj>Subject</subj><pred>Predicate</pred><obj>Object</obj>
   </addtriple>

An example of adding animal characteristics.

.. code:: xml

   <addtriple>
     <subj>cow</subj><pred>sound</pred><obj>moo</obj>
   </addtriple>
   <addtriple>
     <subj>dog</subj><pred>sound</pred><obj>woof</obj>
   </addtriple>

However, data added with addtriple is not persisted.

Delete Element
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Any data including data added by the addtriple element or read from the file can be deleted using the deletetriple element. 
This includes not only elements added by addtriple, but also elements read from the file.

.. code:: xml

   <deletetriple>
     <subj>cow</subj><pred>sound</pred><obj>moo</obj>
   </deletetriple>
   <deletetriple>
     <subj>ant</subj><pred>sound</pred><obj>scratch</obj>
   </deletetriple>

| If you specify three elements, only the elements that match all are deleted.
| If only subject and predicate are specified, matching elements are removed, regardless of the value of object.
| If only subject is specified, all elements matching that subject are removed.

Search
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The select element is used to search RDF.

Simple Search
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the case of a simple search, if you specify subject, predicate, and object as the contents of the <q> element, the contents registered as matching results will be returned as a list.

.. code:: xml

   <select>
       <q><subj>dog</subj><pred>sound</pred><obj>woof</obj></q>
   </select>

| If that information exists, the following results are returned.
| [[[["subj", "DOG"], ["pred", "SOUND"], ["obj", "woof"]]]]

If you want to retrieve only one specific element, the following can be described.

.. code:: xml

   <select>
       <q><subj>dog</subj><pred>sound</pred><obj>?</obj></q>
   </select>

| In this case, the following result indicating the contents of the element specified with "?" Is returned.
| [[["?", "woof"]]]

Searching by Variable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| If you want to return multiple elements or receive a list of matching elements, you must use variables.
| Variables are defined in the contents of the vars tag and are prefixed with the variable name "?".
| The target to be obtained by the variable is specified by setting the variable in the tag of the triple element in the query <q>.
| In the following case, variable:?x is subject, variable:?y is predicate, variable:?z is the object to store.

.. code:: xml

   <select>
       <vars>?x ?y ?z</vars>
       <q><subj>?x</subj><pred>?y</pred><obj>?z</obj></q>
   </select>

| By using a variable, you can get all the triples that match the specified data and get the data combination that corresponds to that variable.
| The following example searches for the number of feet (legs) in an animal.

.. code:: xml

   <select>
       <vars>?x ?y</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>?y</obj></q>
   </select>

| If the search results match, you will get the following results.
| [[["?x", "ANT"], ["?y", "6"]], [["?x", "BAT"], ["?y", "2"]], [["?x", "BEAR"], ["?y", "4"]], [["?x", "BUFFALO"], ["?y", "4"]], [["?x", "CAT"], ["?y", "4"]], [["?x", "CHICKEN"], ["?y", "2"]], [["?x", "DOLPHIN"], ["?y", "0"]], [["?x", "FISH"], ["?y", "0"]]]

Complex Condition Search
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| If you need to perform more complex searches, you can chain multiple queries, each joined as a 'and' query.
| There are 2 types of queries. <q> tag returns results matching its condition and <notq> tag returns results not matching its condition.

.. code:: xml

   <select>
       <vars>?x ?y ?z</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>?y</obj></q>
       <notq><subj>?z</subj><pred>legs</pred><obj>0</obj></notq>
   </select>



Data Retrieval
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| The select element is used to create a data set, as in the SQL SELECT statement.
| The following example stores the result of a select element in tuples using the 'set' tag.

.. code:: xml

   <set var="tuples">
       <select>
           <vars>?x ?y</vars>
           <q><subj>?x</subj><pred>sound</pred><obj>?y</obj></q>
       </select>
   </set>

| In this case, the following contents are set in tuples.
| [[["?x", "BAT"], ["?y", "eee"]], [["?x", "BEAR"], ["?y", "grrrrr"]], [["?x", "BUFFALO"], ["?y", "moo"]], [["?x", "CAT"], ["?y", "meow"]], [["?x", "CHICKEN"], ["?y", "cluck cluck"]], [["?x", "DOLPHIN"], ["?y", "meep meep"]], [["?x", "FISH"], ["?y", "bubble bubble"]], [["?x", "DOG"], ["?y", "woof"]]]

If you want to get the data set in a variable, you can use the 'tuple' element as a child element of the 'get' tag and get the data generated from the 'select' element described above.

.. code:: xml

   <get var="?x">
       <tuple>
           <get var="tuples" />
       </tuple>
   </get>
   <get var="?y">
       <tuple>
           <get var="tuples" />
       </tuple>
   </get>

| In this example, the 'var' attribute of get specifies the variable "?x" specified in the select element.
| By specifying "tuples" (list object) that stores the result of the select element in the tuple tag, data corresponding to the variable "?x" can be obtained from "tuples".
| As a result, the following contents can be obtained from the variable "?x".
| BAT BEAR BUFFALO CAT CHICKEN DOLPHIN FISH DOG
| Similarly, the following contents can be obtained from the variable "?y".
| eee grrrrr moo meow cluck cluck meep meep bubble bubble woof

Also, by using the first tag and rest tag for "tuples", partial results can be obtained as follows.

.. code:: xml

   <get var="?x">
       <tuple>
           <first><get var="tuples" /></first>
       </tuple>
   </get>
   <get var="?y">
       <tuple>
           <rest><get var="tuples" /></rest>
       </tuple>
   </get>

| As a result, in the first tag (obtaining first data), the value obtained from the variable "?x" is as follows.
| BAT

| Similarly, the value obtained from the variable "?y" with the rest tag (obtaining data other than the head) is as follows.
| grrrrr moo meow cluck cluck meep meep bubble bubble woof
