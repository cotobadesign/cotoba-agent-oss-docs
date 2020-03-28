Bot Configuration
===========================

A bot configuration consists of items that control the overall behavior of the dialog engine.

-  `initial_question <#initial-question>`__
-  `default_response <#default-response>`__
-  `empty_string <#empty-string>`__
-  `max_search_depth <#max-search-depth>`__
-  `override_properties <#override-properties>`__
-  `exit_response <#exit-response>`__

In addition, it consists of the following subsections such as bot string processing and additional function settings.

-  :doc:`conversations <Config_Bot_Conversation>`
-  :doc:`splitter/joiner <Config_Bot_Sentence>`
-  :doc:`spelling <Config_Bot_Spelling>`
-  :doc:`transration <Config_Bot_Translation>`
-  :doc:`sentiment <Config_Bot_Sentiment>`

Configuration Example

.. code:: yaml

    bot:
        version: v1.0

        brain: brain

        initial_question: Good morning.
        initial_question_srai: Good evening.
        default_response: unknown
        default_response_srai: YEMPTY
        empty_string: YEMPTY
        exit_response: Exit.
        exit_response_srai: YEXITRESPONSE

        override_properties: true

        max_question_recursion: 1000
        max_question_timeout: 60
        max_search_depth: 100
        max_search_timeout: 60
        max_search_condition: 20
        max_search_srai: 50
        max_categories: 20000

        spelling:
            load: false
            classname: programy.spelling.norvig.NorvigSpellingChecker
            corpus: file
            check_before: false
            check_and_retry: false

        conversations:
        save: true
        load: false

        joiner:
            classname: programy.dialog.joiner.joiner_jp.SentenceJoiner
            join_chars: .?!。？！
            terminator: 。

        splitter:
            classname: programy.dialog.splitter.splitter_jp.SentenceSplitter
            split_chars: 。



initial_question
---------------------------

.. csv-table:: Feature List
  :header: "Set value","Content"
  :widths: 40, 60

        "initial_question_srai","A start-up utterance. The utterance to be made at startup of client. The scenario described in AIML works and generates a response."
        "initial_question","Startup response. Specifies the response to return if the scenario corresponding to initial_question_srai is not listed."

default_response
---------------------------

.. csv-table:: Feature List
  :header: "Set value","Content"
  :widths: 40, 60

        "default_response_srai","An utterance executed if there is no matching pattern. The scenario described in AIML works and generates a response."
        "default_response","Response to return if there is no matching pattern.Specify the response to return if the scenario corresponding to default_response_srai is not described.The :ref:`default-response<storage_file_properties>` in properties has a higher priority than the setting, and the response in properties takes precedence over."


empty_string
---------------------------

.. csv-table:: Feature List
  :header: "Set value","Content"
  :widths: 40, 60

        "empty_string","An utterance to be processed when there is no processing result of pre_processor. If there is no processing result of pre_processor, the scenario operates with this description as an utterance and generates a response."

max_search_depth
---------------------------

.. csv-table:: Feature List
  :header: "Set value","Content"
  :widths: 40, 60

        "max_question_recursion","Maximum number of sentence searches. Specify the maximum number of searches when a long text is input, divided by split characters, and the dialog scenario is executed multiple times. Returns  :ref:`default-response<storage_file_properties>` if the maximum number of times is reached."
        "max_question_timeout","Maximum sentence search time. Specify the maximum processing time in units of seconds when a long text is input, divided by split characters, and the dialog scenario is executed multiple times internally. If the maximum processing time is exceeded,  :ref:`default-response<storage_file_properties>`  is returned."
        "max_search_timeout","Specify the maximum time, in seconds, to search for a word. Specify the maximum search time in seconds for a word search during a sentence search, such as when a long pattern is encountered during a sentence search. Returns  :ref:`default-response<storage_file_properties>` if maximum processing time is exceeded."
        "max_search_depth","Maximum number of word search branches. Specify the maximum number of times that a word can be searched for if it becomes too numerous using  :ref:`wildcards<aiml_pattern_matching>` , :ref:`set<pattern_set>` and so on. Returns  :ref:`default-response<storage_file_properties>`  if the maximum number of word searches is reached."
        "max_search_condition","Maximum number of loops of :ref:`condition loop <condition_looping>` .  Specify the maximum number of loops that can occur when a condition loop is spesified. This prevents an infiniteloop when the condition does not match. Returns :ref:`default-response<storage_file_properties>`  if the maximum number of loops is reached."
        "max_search_srai","Maximum number of :ref:`srai <srai>` recursive calls. Specify the maximum number of recursive calls if the description of srai is a recursive call. Returns :ref:`default-response<storage_file_properties>` if the maximum number of times is reached."
        "max_categories","Maximum number of categories to load. Specify the maximum number of AIML categories to load. If the category specified in AIML exceeds the upper limit, loading is not performed. Categories that were not imported will be described in :ref:`errors<storage_entity>` description as  ``Max categories [n] exceeded``."

override_properties
---------------------------

.. csv-table:: Feature List
  :header: "Set value","Content"
  :widths: 40, 60

        "override_properties","Override permission flag for the name variable. If false, the name variable of the same name is not overwritten."

exit_response
---------------------------

.. csv-table:: Feature List
  :header: "Set value","Content"
  :widths: 40, 60

        "exit_response_srai","End utterance. An utterance to be executed at the end of the client. The scenario described in AIML operates and generates a response."
        "exit_response","End response. exit_response_srai specifies a response to return if the corresponding scenario is not described."





