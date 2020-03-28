Custom pattern element
=============================

| The AIML parser supports the AIML tags defined in pattern_nodes.conf.
| You can modify this file to change the implementation of the element or add your own elements.
| ``Changing the implementation of an element may cause the parser to stop working or have a significant effect on performance or the behavior of other elements, so use it only after understanding the overall behavior.``


.. code:: ini

   #AIML 1.0
   root = programy.parser.pattern.nodes.root.PatternRootNode
   word = programy.parser.pattern.nodes.word.PatternWordNode
   priority = programy.parser.pattern.nodes.priority.PatternPriorityWordNode
   oneormore = programy.parser.pattern.nodes.oneormore.PatternOneOrMoreWildCardNode
   topic = programy.parser.pattern.nodes.topic.PatternTopicNode
   that = programy.parser.pattern.nodes.that.PatternThatNode
   template = programy.parser.pattern.nodes.template.PatternTemplateNode

   #AIML 2.0
   zeroormore = programy.parser.pattern.nodes.zeroormore.PatternZeroOrMoreWildCardNode
   set = programy.parser.pattern.nodes.set.PatternSetNode
   bot = programy.parser.pattern.nodes.bot.PatternBotNode

   #Custom
   iset = programy.parser.pattern.nodes.iset.PatternISetNode
   regex = programy.parser.pattern.nodes.regex.PatternRegexNode

| Each pattern element inherits　``programy.parser.pattern.nodes.base.PatternNode`` as a base class.
| The base class for pattern elements, which encapsulates XML elements in node attributes of the pattern class. It also includes matching words to element types during pattern evaluation.

The main methods to override are the following:

-  ``def equivalent(self, other)`` - This method tests whether a processing element is equivalent to other evaluation elements.
-  ``def equals(self, bot, client, words, word_no)`` - This method determines whether a word matches the rules of an element.
-  ``def to_string(self, verbose=True)`` - This method converts element information to a string representation for debugging or logging.
-  ``def to_xml(self, bot, clientid)`` - Converts element content to XML format. This method is used when outputting to `Braintree` in XML format.
