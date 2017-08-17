.. highlight:: rest

.. _inline-markup:

Inline markup
=============

Sphinx는 interpreted text role(역할)을 사용하여 semantic markup을 문서에 삽입한다.
그들은 ``: rolename :`content`` 형태로 쓰여진다.

.. note::

   디폴트 role인 (```content```)는 기본적으로 특별한 의미가 없다. 원하는대로 자유롭게 사용해도 된다.
예: 변수 이름. :confval:`default_role` 설정값을 사용하여 만들어진 역할로 설정하는게 가능하다.
예: :rst:role:`any` role을 사용하여 검색을 하거나 :rst:role:`py:obj` role을 사용하여
Python 객체를 검색.

domain에 의해 추가된 role에 대해서는 :ref:`domains` 참조.


.. _xref-syntax:

Cross-referencing syntax
~~~~~~~~~~~~~~~~~~~~~~~~

cross-reference(상호참조)는 많은 semantic interpreted text role에 의해 생성된다.
기본적으로 ``:role:`target``` 라고 쓰면 *role* 이 가리키는 타입의 *target* 이라고
이름지어진 항목에 링크가 생성된다. 링크 텍스트는 *target* 과 동일하다.

그러나 cross-reference role을 보다 다양하게 만드는 몇 가지 추가 기능이 있다.

* reST direct hyperlink 에서 처럼 title과 reference target을 제공 할 수 있다.
예: ``:role:`title <target>``` 는 *target* 을 참조하지만 링크 텍스트는 *title* 이 된다.

* 내용 앞에 접두로 ``!`` 를 붙이면 reference와 hyperlink가 생성되지 않는다.

* 내용 앞에 접두로 ``~`` 를 붙이면 링크 텍스트는 target의 마지막 구성 요소가 된다.
예를 들어, ``:py:meth:`~Queue.Queue.get``` 는 ``Queue.Queue.get`` 을 참조하지만
링크 텍스트로 ``get`` 만 표시한다. 이 기능은 모든 cross-reference role에서 작동하지는
않지만 도메인에 따라 다르다.

  HTML 출력에서, 링크의 ``title`` 속성(예: mouse-hover에서의 tool-tip으로 표시)은
항상 완전한 target name이 된다.


.. _any-role:

Cross-referencing anything
--------------------------

.. rst:role:: any

   .. versionadded:: 1.3

   이 convenience role은 reference text에 대해 유효한 target을 찾기 위해
최선을 다한다.

   * 먼저, :rst:role:`doc`, :rst:role:`ref` 또는 :rst:role:`option` 에 의해 참조되는
표준 cross-reference target을 찾는것을 시도한다.

     그리고 extension에 의해 standard domain에 추가 된 사용자 정의 객체
(:meth:`.add_object_type` 참조)도 검색된다.

   * 그리고는 불려온 모든 domain 안의 객체(target)를 찾는다.
얼마나 구체적으로 일치해야 하는지는 각 도메인에 달려 있다. 예를 들어, Python domain에서
``:any:`Builder``` 는 ``sphinx.builders.Builder`` class를 찾아낸다.

   대상이 하나도 없거나 여러 개인 경우 경고가 발생한다. 여러 대상의 경우
"any"를 특정 role로 변경할 수 있다.

   이 role은 :confval:`default_role` 로 설정하기에 적합한 후보이다. markup overhead가
별로 없이 많은 cross-reference를 작성할 수 있기 때문이다. 예를 들어, 아래의 Python function documentation에서::

      .. function:: install()

         이 function은 `signal` module이 알고있는 모든 signal에 대해 `handler` 를 설치한다.
더 자세한 정보는 `about-signals` 섹션을 참조하면 된다.

   용어집의 용어(보통 ``:term:`handler```), Python module (보통 ``:py:mod:``signal```
또는 ``:mod:`signal```)과 섹션 (보통 ``:ref:`about-signals```)를 참조하는게 가능하다.

   :rst:role:`any` role은 :mod:`~sphinx.ext.intersphinx` extension과 함께 같이 사용 가능하다.
만약 로컬 cross-reference가 발견되지 않는다면, 모든 객체 유형의 intersphinx inventory도
검색된다.


객체 상호참조
-------------------------

아래의 role들은 해당 domain과 함께 설명되어있다:

* :ref:`Python <python-roles>`
* :ref:`C <c-roles>`
* :ref:`C++ <cpp-roles>`
* :ref:`JavaScript <js-roles>`
* :ref:`ReST <rst-roles>`


.. _ref-role:

임의의 위치 상호참조
-------------------------------------

.. rst:role:: ref

   모든 문서에서 임의의 위치에 대한 cross-reference를 지원하기 위해서 표준 reST 라벨이 사용된다.
이를 위해 라벨 이름은 전체 문서에서 고유해야 한다. 라벨 참조에는 두 가지 방법이 있다.

   * 섹션 제목 바로 앞에 라벨을 배치하면 ``:ref:`label-name``` 을 사용하여 라벨을 참조 할 수 있다. 예::

        .. _my-reference-label:

        상호참조할 섹션
        --------------------------

        이것은 섹션의 텍스트이다.

        이것은 섹션 자체를 참조한다. :ref:`my-reference-label` 참조.

     그런 다음 ``:ref:`` role은 섹션에 대한 링크를 생성하고, 링크 제목은 "Section to cross-reference"가
된다. 심지어 섹션과 reference가 다른 소스 파일에 있어도 된다.

     자동 라벨은 figure(삽화/도표)와도 작동한다. 다음과 같이 주어졌을 경우::

        .. _my-figure:

        .. figure:: whatever

           Figure caption

     ``:ref:`my-figure`` reference는 링크 텍스트 "Figure caption"과 함께 그림에
대한 reference를 삽입한다.

     이는 :dudir:`table` directive를 사용한 table에 대해서도 caption을 명시하였다면,
마찬가지로 작동한다.

   * 섹션 제목 앞에 놓이지 않은 라벨도 여전히 ​​참조가 가능 하지만, ``:ref:`Link title <label-name> ```
와 같은 식으로 제목을 명시해 주어야 한다.

   .. note::

      참조 label은 underscore(``_``)로 시작해야 한다. 하지만 라벨을 참조 할 때에는
underscore를 생략 해야 한다. (위의 예시 참조)

   ```Section title`_`` 와 같은 표준인 reStructuredText의 section에 대한 link보다
:rst:role:`ref` 의 사용을 권장한다. 이는 파일이 다른경우나 섹션 제목이 변경되는
경우에도 작동하며 cross-reference를 지원하는 builder들과도 작동하기 때문이다.


문서 상호참조
---------------------------

.. versionadded:: 0.6

문서에 직접 링크하는 방법도 있다.

.. rst:role:: doc

   지정된 문서에 링크한다. 문서 이름은 절대적 또는 상대적으로 지정할 수 있다.
예를 들어, ``sketches/index`` 문서에서 ``:doc:`parrot``` 과 같은 reference가
있다면 링크는 ``sketches/parrot`` 로 연결하게 된다. 만약 reference가
``:doc:`/people``` 또는``:doc:`../ people``` 이라면 링크는 ``people`` 로
연결된다.

   ``:doc:`Monty Python members </people> ``` 와 같이 링크 텍스트가 제공되지
않았다면 링크 캡션은 주어진 문서의 제목이 된다.


다운로드 가능한 파일 참조
------------------------------

.. versionadded:: 0.6

.. rst:role:: download

   이 role은 소스트리에 있는 reST 문서는 아니지만 다운로드 가능한
파일들에 링크하는 것을 가능하게 한다.

   이 role을 사용하면 참조 된 파일이 빌드시(HTML 출력에만 해당) output에 포함되도록 자동으로
마크된다. 다운로드 가능한 모든 파일은 output 디렉토리의 ``_downloads`` 서브디렉토리에
저장된다. 중복된 파일 이름은 처리된다.

   An example::

      See :download:`this example script <../example.py>`.

   주어지는 파일이름은 대개는 현재 소스 파일이 들어있는 디렉토리에 상대적이다. 하지만
만약 절대적(``/`` 로 시작)이라면, 최상위 소스 디렉토리에 상대적으로 취급된다.

   ``example.py`` 파일이 output 디렉토리에 복사되고 적절한 링크가 생성된다.

   사용할 수없는 다운로드 링크를 표시하지 않으려면 다음 role이 있는 전체 단락을 wrap해야 한다.

      .. only:: builder_html

         See :download:`this example script <../example.py>`.

도표/삽화를 숫자로 상호참조
------------------------------------------

.. versionadded:: 1.3

.. versionchanged:: 1.5
   `numref` role can also refer sections.
   And `numref` allows `{name}` for the link text.

.. rst:role:: numref

   지정된 figure, table, code block, section에 링크한다. Standard reST label이 사용된다.
이 role을 사용하면 "Fig. 1.1"과 같이 figure number와 함께 링크 텍스트가 있는 그림에 대한
reference가 삽입된다.

   링크 텍스트가 주어지면 (예: ``:numref:`Image of Sphinx (Fig. %s) <my-figure>```), 링크
caption은 reference의 제목이 된다. 특수 문자로는 `%s` 와 `{number}` 는 figure number로
대체되고, `{name}`은 figure caption으로 대체 된다. 링크 텍스트가 주어지지 않으면,
:confval:`numfig_format` 의 값이 링크 텍스트의 디폴트 값으로 사용된다.

   만약 :confval:`numfig` 가 ``False`` 이면, figure는 번호가 매겨지지 않는다. 따라서 이 role은
참조가 아니라 label이나 링크 텍스트를 삽입한다.

주목할 만한 항목들에 대한 상호참조
------------------------------------------------------

다음 role들은 cross-reference를 만들지만 객체를 참조하지는 않는다.

.. rst:role:: envvar

   Environment variable. 색인 항목이 생성된다. 또한 일치하는 :rst:dir:`envvar` directive가
있다면, 이에 대한 링크를 생성한다.

.. rst:role:: token

   Grammar token의 이름. (:rst:dir:`productionlist` directive들 사이에 링크를
생성하는데 사용)

.. rst:role:: keyword

   Python에서 keyword의 이름. 만약 존재한다면, 이 이름을 가지는 reference label에 대한
링크를 생성한다.

.. rst:role:: option

   A command-line option to an executable program.  This generates a link to
   a :rst:dir:`option` directive, if it exists.


The following role creates a cross-reference to a term in a
:ref:`glossary <glossary-directive>`:

.. rst:role:: term

   Reference to a term in a glossary.  A glossary is created using the
   ``glossary`` directive containing a definition list with terms and
   definitions.  It does not have to be in the same file as the ``term`` markup,
   for example the Python docs have one global glossary in the ``glossary.rst``
   file.

   If you use a term that's not explained in a glossary, you'll get a warning
   during build.


Other semantic markup
~~~~~~~~~~~~~~~~~~~~~

The following roles don't do anything special except formatting the text
in a different style:

.. rst:role:: abbr

   An abbreviation.  If the role content contains a parenthesized explanation,
   it will be treated specially: it will be shown in a tool-tip in HTML, and
   output only once in LaTeX.

   Example: ``:abbr:`LIFO (last-in, first-out)```.

   .. versionadded:: 0.6

.. rst:role:: command

   The name of an OS-level command, such as ``rm``.

.. rst:role:: dfn

   Mark the defining instance of a term in the text.  (No index entries are
   generated.)

.. rst:role:: file

   The name of a file or directory.  Within the contents, you can use curly
   braces to indicate a "variable" part, for example::

      ... is installed in :file:`/usr/lib/python2.{x}/site-packages` ...

   In the built documentation, the ``x`` will be displayed differently to
   indicate that it is to be replaced by the Python minor version.

.. rst:role:: guilabel

   Labels presented as part of an interactive user interface should be marked
   using ``guilabel``.  This includes labels from text-based interfaces such as
   those created using :mod:`curses` or other text-based libraries.  Any label
   used in the interface should be marked with this role, including button
   labels, window titles, field names, menu and menu selection names, and even
   values in selection lists.

   .. versionchanged:: 1.0
      An accelerator key for the GUI label can be included using an ampersand;
      this will be stripped and displayed underlined in the output (example:
      ``:guilabel:`&Cancel```).  To include a literal ampersand, double it.

.. rst:role:: kbd

   Mark a sequence of keystrokes.  What form the key sequence takes may depend
   on platform- or application-specific conventions.  When there are no relevant
   conventions, the names of modifier keys should be spelled out, to improve
   accessibility for new users and non-native speakers.  For example, an
   *xemacs* key sequence may be marked like ``:kbd:`C-x C-f```, but without
   reference to a specific application or platform, the same sequence should be
   marked as ``:kbd:`Control-x Control-f```.

.. rst:role:: mailheader

   The name of an RFC 822-style mail header.  This markup does not imply that
   the header is being used in an email message, but can be used to refer to any
   header of the same "style."  This is also used for headers defined by the
   various MIME specifications.  The header name should be entered in the same
   way it would normally be found in practice, with the camel-casing conventions
   being preferred where there is more than one common usage. For example:
   ``:mailheader:`Content-Type```.

.. rst:role:: makevar

   The name of a :command:`make` variable.

.. rst:role:: manpage

   A reference to a Unix manual page including the section,
   e.g. ``:manpage:`ls(1)```.

.. rst:role:: menuselection

   Menu selections should be marked using the ``menuselection`` role.  This is
   used to mark a complete sequence of menu selections, including selecting
   submenus and choosing a specific operation, or any subsequence of such a
   sequence.  The names of individual selections should be separated by
   ``-->``.

   For example, to mark the selection "Start > Programs", use this markup::

      :menuselection:`Start --> Programs`

   When including a selection that includes some trailing indicator, such as the
   ellipsis some operating systems use to indicate that the command opens a
   dialog, the indicator should be omitted from the selection name.

   ``menuselection`` also supports ampersand accelerators just like
   :rst:role:`guilabel`.

.. rst:role:: mimetype

   The name of a MIME type, or a component of a MIME type (the major or minor
   portion, taken alone).

.. rst:role:: newsgroup

   The name of a Usenet newsgroup.

.. rst:role:: program

   The name of an executable program.  This may differ from the file name for
   the executable for some platforms.  In particular, the ``.exe`` (or other)
   extension should be omitted for Windows programs.

.. rst:role:: regexp

   A regular expression. Quotes should not be included.

.. rst:role:: samp

   A piece of literal text, such as code.  Within the contents, you can use
   curly braces to indicate a "variable" part, as in :rst:role:`file`.  For
   example, in ``:samp:`print 1+{variable}```, the part ``variable`` would be
   emphasized.

   If you don't need the "variable part" indication, use the standard
   ````code```` instead.

There is also an :rst:role:`index` role to generate index entries.

The following roles generate external links:

.. rst:role:: pep

   A reference to a Python Enhancement Proposal.  This generates appropriate
   index entries. The text "PEP *number*\ " is generated; in the HTML output,
   this text is a hyperlink to an online copy of the specified PEP.  You can
   link to a specific section by saying ``:pep:`number#anchor```.

.. rst:role:: rfc

   A reference to an Internet Request for Comments.  This generates appropriate
   index entries. The text "RFC *number*\ " is generated; in the HTML output,
   this text is a hyperlink to an online copy of the specified RFC.  You can
   link to a specific section by saying ``:rfc:`number#anchor```.


Note that there are no special roles for including hyperlinks as you can use
the standard reST markup for that purpose.


.. _default-substitutions:

Substitutions
~~~~~~~~~~~~~

The documentation system provides three substitutions that are defined by
default. They are set in the build configuration file.

.. describe:: |release|

   Replaced by the project release the documentation refers to.  This is meant
   to be the full version string including alpha/beta/release candidate tags,
   e.g. ``2.5.2b3``.  Set by :confval:`release`.

.. describe:: |version|

   Replaced by the project version the documentation refers to. This is meant to
   consist only of the major and minor version parts, e.g. ``2.5``, even for
   version 2.5.1.  Set by :confval:`version`.

.. describe:: |today|

   Replaced by either today's date (the date on which the document is read), or
   the date set in the build configuration file.  Normally has the format
   ``April 14, 2007``.  Set by :confval:`today_fmt` and :confval:`today`.
