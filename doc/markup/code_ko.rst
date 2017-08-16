.. highlight:: rest

.. _code-examples:

코드 예제 표시
---------------------

.. index:: pair: code; examples
           single: sourcecode

Python 소스 코드나 인터랙티브 세션의 예제는 표준 reST literal block을 사용하여
표현된다. 그것들은 앞 단락의 끝에 ``::`` 에 의해 시작되고 들여 쓰기로 구분된다.

인터랙티브 세션을 나타내는 것은 프롬프트와 Python 코드와 함께 출력을 포함해야 한다.
인터랙티브 세션에는 특별한 마크업이 필요하지 않다. 입력 또는 출력의 마지막 줄이
표시된 후에 "사용되지 않은" 기본 프롬프트가 없어야 한다. 이것은 *하면 안되는* 예이다::

   >>> 1 + 1
   2
   >>>

Syntax 강조는 `Pygments <http://pygments.org>`_ 와 함께 현명게 처리된다:

* 각 소스 파일에는 "강조 표시 언어"가 있다. 파일의 대부분은 Python 코드를
강조해야 하기 때문에 기본값은 ``'python'`` 이지만 문서 전체 기본값을
:confval:`highlight_language` 설정 값으로 설정할 수 있다.

* Python 강조 모드에서 인터랙티브 세션은 자동으로 인식되고 적절히 강조가 된다.
일반 Python 코드는 parsing이 가능한 경우에만 강조된다. (따라서 Python을
기본값으로 사용할 수 있지만 여기저기 산재된 shell command나 다른 코드 블록의
경우 강조되지 않는다.)

* 강조 언어는 다음과 같이 사용되는``highlight`` directive를 사용하여 변경 할 수 있다:

  .. rst:directive:: .. highlight:: language

     예::

        .. highlight:: c

     이 언어는 다음 ``highlight`` directive가 나올 때까지 사용된다.

* 다른 언어의 코드를 표시해야하는 문서의 경우에는 강조 언어가 직접 지정된
:rst:dir:`code-block' directive가 있다.

  .. rst:directive:: .. code-block:: language

     다음과 같이 사용::

        .. code-block:: ruby

           Some Ruby code.

     이 directive의 alias인 :rst:dir:`sourcecode` 도 사용 가능하다.

* 강조 언어의 유효한 값은 다음과 같다.

  * ``none`` (no highlighting)
  * ``python`` (:confval:`highlight_language` 가 지정되지 않을 경우의 기본값)
  * ``guess`` (Pygments 내용을 기반으로 추측. 잘 알려진 특정 언어들에 대해서만 작동)
  * ``rest``
  * ``c``
  * ... `Pygments가 지원하는 기타 lexer alias <http://pygments.org/docs/lexers/>`_.

* 선택한 언어로 강조 표시가 실패한 경우 (즉, Pygments가 "오류" 토큰을 내보낼 경우)
블록은 강조 표시되지 않는다.

줄 번호
^^^^^^^^^^^^

Pygments can generate line numbers for code blocks.  For
automatically-highlighted blocks (those started by ``::``), line numbers must be
switched on in a :rst:dir:`highlight` directive, with the ``linenothreshold``
option::

   .. highlight:: python
      :linenothreshold: 5

This will produce line numbers for all code blocks longer than five lines.

For :rst:dir:`code-block` blocks, a ``linenos`` flag option can be given to
switch on line numbers for the individual block::

   .. code-block:: ruby
      :linenos:

      Some more Ruby code.

The first line number can be selected with the ``lineno-start`` option.  If
present, ``linenos`` is automatically activated as well::

   .. code-block:: ruby
      :lineno-start: 10

      Some more Ruby code, with line numbering starting at 10.

Additionally, an ``emphasize-lines`` option can be given to have Pygments
emphasize particular lines::

    .. code-block:: python
       :emphasize-lines: 3,5

       def some_function():
           interesting = False
           print 'This line is highlighted.'
           print 'This one is not...'
           print '...but this one is.'

.. versionchanged:: 1.1
   ``emphasize-lines`` has been added.

.. versionchanged:: 1.3
   ``lineno-start`` has been added.


Includes
^^^^^^^^

.. rst:directive:: .. literalinclude:: filename

   Longer displays of verbatim text may be included by storing the example text
   in an external file containing only plain text.  The file may be included
   using the ``literalinclude`` directive. [1]_ For example, to include the
   Python source file :file:`example.py`, use::

      .. literalinclude:: example.py

   The file name is usually relative to the current file's path.  However, if it
   is absolute (starting with ``/``), it is relative to the top source
   directory.

   Tabs in the input are expanded if you give a ``tab-width`` option with the
   desired tab width.

   Like :rst:dir:`code-block`, the directive supports the ``linenos`` flag
   option to switch on line numbers, the ``lineno-start`` option to select the
   first line number, the ``emphasize-lines`` option to emphasize particular
   lines, and a ``language`` option to select a language different from the
   current file's standard language.  Example with options::

      .. literalinclude:: example.rb
         :language: ruby
         :emphasize-lines: 12,15-18
         :linenos:

   Include files are assumed to be encoded in the :confval:`source_encoding`.
   If the file has a different encoding, you can specify it with the
   ``encoding`` option::

      .. literalinclude:: example.py
         :encoding: latin-1

   The directive also supports including only parts of the file.  If it is a
   Python module, you can select a class, function or method to include using
   the ``pyobject`` option::

      .. literalinclude:: example.py
         :pyobject: Timer.start

   This would only include the code lines belonging to the ``start()`` method in
   the ``Timer`` class within the file.

   Alternately, you can specify exactly which lines to include by giving a
   ``lines`` option::

      .. literalinclude:: example.py
         :lines: 1,3,5-10,20-

   This includes the lines 1, 3, 5 to 10 and lines 20 to the last line.

   Another way to control which part of the file is included is to use the
   ``start-after`` and ``end-before`` options (or only one of them).  If
   ``start-after`` is given as a string option, only lines that follow the first
   line containing that string are included.  If ``end-before`` is given as a
   string option, only lines that precede the first lines containing that string
   are included.

   With lines selected using ``start-after`` it is still possible to use
   ``lines``, the first allowed line having by convention the line number ``1``.

   When lines have been selected in any of the ways described above, the
   line numbers in ``emphasize-lines`` also refer to the selection, with the
   first selected line having number ``1``.

   When specifying particular parts of a file to display, it can be useful to
   display the original line numbers. This can be done using the
   ``lineno-match`` option, which is however allowed only when the selection
   consists of contiguous lines.

   You can prepend and/or append a line to the included code, using the
   ``prepend`` and ``append`` option, respectively.  This is useful e.g. for
   highlighting PHP code that doesn't include the ``<?php``/``?>`` markers.


   If you want to show the diff of the code, you can specify the old
   file by giving a ``diff`` option::

      .. literalinclude:: example.py
         :diff: example.py.orig

   This shows the diff between example.py and example.py.orig with unified diff
   format.

   .. versionadded:: 0.4.3
      The ``encoding`` option.
   .. versionadded:: 0.6
      The ``pyobject``, ``lines``, ``start-after`` and ``end-before`` options,
      as well as support for absolute filenames.
   .. versionadded:: 1.0
      The ``prepend`` and ``append`` options, as well as ``tab-width``.
   .. versionadded:: 1.3
      The ``diff`` option.
      The ``lineno-match`` option.
   .. versionchanged:: 1.6
      With both ``start-after`` and ``lines`` in use, the first line as per
      ``start-after`` is considered to be with line number ``1`` for ``lines``.

Caption and name
^^^^^^^^^^^^^^^^

.. versionadded:: 1.3

A ``caption`` option can be given to show that name before the code block.
A ``name`` option can be provided implicit target name that can be referenced
by using :rst:role:`ref`.
For example::

   .. code-block:: python
      :caption: this.py
      :name: this-py

      print 'Explicit is better than implicit.'


:rst:dir:`literalinclude` also supports the ``caption`` and ``name`` option.
``caption`` has an additional feature that if you leave the value empty, the shown
filename will be exactly the one given as an argument.


Dedent
^^^^^^

.. versionadded:: 1.3

A ``dedent`` option can be given to strip indentation characters from the code
block. For example::

   .. literalinclude:: example.rb
      :language: ruby
      :dedent: 4
      :lines: 10-15

:rst:dir:`code-block` also supports the ``dedent`` option.


.. rubric:: Footnotes

.. [1] There is a standard ``.. include`` directive, but it raises errors if the
       file is not found.  This one only emits a warning.
