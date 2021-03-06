Custom template element
============================

| The parser supports all AIML tags defined in the system, including template_nodes.conf.
| template_nodes.conf also allows you to change the implementation of an element or add your own.
| ``Changing the implementation of an element may cause the parser to stop working or have a significant effect on performance or the behavior of other elements, so use it only after understanding the overall behavior.``


.. code:: ini

   word = programy.parser.template.nodes.word.TemplateWordNode
   authorise = programy.parser.template.nodes.authorise.TemplateAuthoriseNode
   random = programy.parser.template.nodes.rand.TemplateRandomNode
   condition = programy.parser.template.nodes.condition.TemplateConditionNode
   srai = programy.parser.template.nodes.srai.TemplateSRAINode
   sraix = programy.parser.template.nodes.sraix.TemplateSRAIXNode
   get = programy.parser.template.nodes.get.TemplateGetNode
   set = programy.parser.template.nodes.set.TemplateSetNode
   map = programy.parser.template.nodes.map.TemplateMapNode
   bot = programy.parser.template.nodes.bot.TemplateBotNode
   think = programy.parser.template.nodes.think.TemplateThinkNode
   normalize = programy.parser.template.nodes.normalise.TemplateNormalizeNode
   denormalize = programy.parser.template.nodes.denormalise.TemplateDenormalizeNode
   person = programy.parser.template.nodes.person.TemplatePersonNode
   person2 = programy.parser.template.nodes.person2.TemplatePerson2Node
   gender = programy.parser.template.nodes.gender.TemplateGenderNode
   sr = programy.parser.template.nodes.sr.TemplateSrNode
   id = programy.parser.template.nodes.id.TemplateIdNode
   size = programy.parser.template.nodes.size.TemplateSizeNode
   vocabulary = programy.parser.template.nodes.vocabulary.TemplateVocabularyNode
   eval = programy.parser.template.nodes.eval.TemplateEvalNode
   explode = programy.parser.template.nodes.explode.TemplateExplodeNode
   implode = programy.parser.template.nodes.implode.TemplateImplodeNode
   program = programy.parser.template.nodes.program.TemplateProgramNode
   lowercase = programy.parser.template.nodes.lowercase.TemplateLowercaseNode
   uppercase = programy.parser.template.nodes.uppercase.TemplateUppercaseNode
   sentence = programy.parser.template.nodes.sentence.TemplateSentenceNode
   formal = programy.parser.template.nodes.formal.TemplateFormalNode
   that = programy.parser.template.nodes.that.TemplateThatNode
   thatstar = programy.parser.template.nodes.thatstar.TemplateThatStarNode
   topicstar = programy.parser.template.nodes.topicstar.TemplateTopicStarNode
   star = programy.parser.template.nodes.star.TemplateStarNode
   input = programy.parser.template.nodes.input.TemplateInputNode
   request = programy.parser.template.nodes.request.TemplateRequestNode
   response = programy.parser.template.nodes.response.TemplateResponseNode
   date = programy.parser.template.nodes.date.TemplateDateNode
   interval = programy.parser.template.nodes.interval.TemplateIntervalNode
   system = programy.parser.template.nodes.system.TemplateSystemNode
   extension = programy.parser.template.nodes.extension.TemplateExtensionNode
   learn = programy.parser.template.nodes.learn.TemplateLearnNode
   learnf = programy.parser.template.nodes.learnf.TemplateLearnfNode
   first = programy.parser.template.nodes.first.TemplateFirstNode
   rest = programy.parser.template.nodes.rest.TemplateRestNode
   log = programy.parser.template.nodes.log.TemplateLogNode
   oob = programy.parser.template.nodes.oob.TemplateOOBNode
   xml = programy.parser.template.nodes.xml.TemplateXMLNode
   addtriple = programy.parser.template.nodes.addtriple.TemplateAddTripleNode
   deletetriple = programy.parser.template.nodes.deletetriple.TemplateDeleteTripleNode
   select = programy.parser.template.nodes.select.TemplateSelectNode
   uniq = programy.parser.template.nodes.uniq.TemplateUniqNode
   search = programy.parser.template.nodes.search.TemplateSearchNode

| Each template element inherits the ``programy.parser.template.nodes.base.TemplateNode``  as a base class.
| programy.parser.template.nodes.base.TemplateNode is the base class of a template element, which encapsulates an XML element into the node attributes of the template class. It is also used to evaluate textification, including child elements, during template evaluation.

The main methods to override are the following.

-  ``def parse_expression(self, graph, expression)`` - Parsing to the elements of XML. If it can't be expanded, throw an appropriate exception if necessary.
-  ``def resolve_to_string(self, bot, clientid)`` - Expand an element into a string (response), which is called by resolve().
-  ``def resolve(self, bot, clientid)`` - It is called by brain to expand individual template elements into a string. Iterates through child elements to create a string and concatenates them.
-  ``def to_string(self)`` - A method that converts element information to a string representation for debugging and logging purposes.
-  ``def to_xml(self, bot, clientid)`` - Converts the content of an element to XML format. This is a method that can be used to create an aiml file as part of the :ref:`learnf<template_learnf>` or to output to `Braintree` in XML format.
