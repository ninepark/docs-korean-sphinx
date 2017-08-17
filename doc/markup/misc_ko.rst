.. highlight:: rest

기타 마크업
====================

.. _metadata:

파일 전체 메타데이터
------------------------------

reST는 "field list"라는 개념을 가지고 있다. 이들은 다음과 같이 주어진 일련의
필드이다.

   :fieldname: Field content

파일 상단 근처의 field list는 저자, 발행일 및 기타 metadata를 기록하는 데 일반적으로
사용되는 "docinfo"로 docutils에서 파싱된다. *Sphinx* 에서 모든 다른 마크업 앞에
오는 field list는 문서 metadata로 docinfo에서 Sphinx 환경으로 이동되며 출력에
표시되지 않는다. 문서 제목 다음에 나타나는 field list는 정상적으로 docinfo에 포함되어
output에 표시된다.

다음과 같은 metadata field들이 인식된다:

``tocdepth``
   이 파일의 목차에서의 최대 깊이.

   .. versionadded:: 0.4

``nocomments``
   만약 설정된다면, 이 소스 파일에서 생성된 페이지에에 대한 comment form을
웹 애플리케이션에서 표시하지 않는다.

``orphan``
   만약 설정된다면, 이 파일이 toctree에 포함되지 않았다는 경고가 발생하지 않는다.

   .. versionadded:: 1.0


Meta-information markup
-----------------------

.. rst:directive:: .. sectionauthor:: name <email>

   현재 섹션의 작성자를 식별합니다. argument에는 프레젠테이션과 이메일 주소에
사용할 수 있도록 작성자 이름이 포함되어야 한다. 주소의 도메인 이름 부분은
소문자이여야 한다. 예::

      .. sectionauthor:: Guido van Rossum <guido@python.org>

   기본적으로 이 마크업은 출력 결과에 반영되지 않지만, (기여도를 기록하는데 요긴) 설정값을
:confval:`show_authors` 에서 ``True`` 로 설정하면 출력 가능하다.


.. rst:directive:: .. codeauthor:: name <email>

   :rst:dir:`sectionauthor` 가 문서의 저자(들)을 지정하듯이, :rst:dir:`codeauthor`
directive는 설명 되는 코드의 작성자 이름을 지정하며, 여러번 사용 가능하다.
마찬가지로 :confval:`show_authors` 설정값이 ``True`` 인 경우에만 출력을 생성한다.


Index-generating markup
-----------------------

Sphinx는 :ref:`domains` 에서 설명 된 것처럼 모든 객체(함수, 클래스 또는 속성)
설명으로부터 인덱스 항목을 자동으로 생성한다.

그러나 색인을 보다 포괄적으로 만들고, 정보가 주로 language refrence와 같이
정보 단위에 포함되지 않은 문서에서 색인 항목을 사용할 수 있도록 마크업을
명시적으로 사용할 수도 있다.

.. rst:directive:: .. index:: <entries>

   이 directive는 하나 이상의 index 항목이 들어간다. 각 항목은 콜론으로 구분 된
유형과 값으로 구성된다.

   For example::

      .. index::
         single: execution; context
         module: __main__
         module: sys
         triple: module; search; path

      The execution context
      ---------------------

      ...

   이 directive는 5 개의 항목을 포함하며, 생성 된 색인의 항목으로 변환되어 색인 명령문의
정확한 위치(또는 오프라인 매체의 경우 해당 페이지 번호)로 연결된다.

   인덱스 directive는 소스의 해당 위치에서 cross-reference 대상을 생성하기 때문에
위의 예제에서와 같이 참조하는 것 *앞에* 넣는 것이 좋다.

   가능한 항목 유형은 다음과 같다.

   single
      단일 색인 항목을 작성한다. 세미콜론을 사용함으로 하위항목으로 만드는 것도
가능하다. (이 표기법은 아래의 항목들에도 유효하다)
   pair
      ``pair: loop; statement`` 은 ``loop; statement`` 와 ``statement; loop``
두개의 색인 항목을 만들기 위한 축약이다.
   triple
      마찬가지로, ``triple: module; search; path`` 는 ``module; search path``,
``search; path, module``, ``path; module search`` 세개의 색인 항목을 만들기
위한 축약이다.
   see
      ``see: entry; other`` 는 ``entry`` 에서 ``other`` 를 가리키는 색인 항목을 만든다.
   seealso
      ``see`` 와 같지만 "see" 대신에 "see also" 를 삽입한다.
   module, keyword, operator, object, exception, statement, builtin
      이것들은 모두 두개의 색인 항목을 만든다. 예를들어, ``module: hashlib`` 는
``module; hashlib`` 와 ``hashlib; module`` 를 만든다. (다만 이것들은 Python에만
한정되어 있으며 deprecate 되었다)


   앞에 느낌표를 접두하여 "main" 색인 항목을 표시할 수 있다. "main" 항목에 대한 참조는
생성 된 인덱스에서 강조된다. 예를 들어 두 페이지에 다음과 같은게 있고::

      .. index:: Python

   그리고 한 페이지에는 다음과 같이 있다면::

      .. index:: ! Python

   세개의 백링크들 중에서 나중의 것이 강조된다.

   "단일" 항목만 포함하는 색인 directive의 경우에는 다음과 같은 축약이 있다::

      .. index:: BNF, grammar, syntax, notation

   이렇게하면 네 개의 색인 항목이 작성된다.

   .. versionchanged:: 1.1
      Added ``see`` and ``seealso`` types, as well as marking main entries.

.. rst:role:: index

   While the :rst:dir:`index` directive is a block-level markup and links to the
   beginning of the next paragraph, there is also a corresponding role that sets
   the link target directly where it is used.

   The content of the role can be a simple phrase, which is then kept in the
   text and used as an index entry.  It can also be a combination of text and
   index entry, styled like with explicit targets of cross-references.  In that
   case, the "target" part can be a full entry as described for the directive
   above.  For example::

      This is a normal reST :index:`paragraph` that contains several
      :index:`index entries <pair: index; entry>`.

   .. versionadded:: 1.1


.. _tags:

Including content based on tags
-------------------------------

.. rst:directive:: .. only:: <expression>

   Include the content of the directive only if the *expression* is true.  The
   expression should consist of tags, like this::

      .. only:: html and draft

   Undefined tags are false, defined tags (via the ``-t`` command-line option or
   within :file:`conf.py`, see :ref:`here <conf-tags>`) are true.  Boolean
   expressions, also using parentheses (like ``html and (latex or draft)``) are
   supported.

   The *format* and the *name* of the current builder (``html``, ``latex`` or
   ``text``) are always set as a tag [#]_.  To make the distinction between
   format and name explicit, they are also added with the prefix ``format_`` and
   ``builder_``, e.g. the epub builder defines the tags  ``html``, ``epub``,
   ``format_html`` and ``builder_epub``.

   These standard tags are set *after* the configuration file is read, so they
   are not available there.

   All tags must follow the standard Python identifier syntax as set out in
   the `Identifiers and keywords
   <https://docs.python.org/2/reference/lexical_analysis.html#identifiers>`_
   documentation.  That is, a tag expression may only consist of tags that
   conform to the syntax of Python variables.  In ASCII, this consists of the
   uppercase and lowercase letters ``A`` through ``Z``, the underscore ``_``
   and, except for the first character, the digits ``0`` through ``9``.

   .. versionadded:: 0.6
   .. versionchanged:: 1.2
      Added the name of the builder and the prefixes.

   .. warning::

      This directive is designed to control only content of document.  It could
      not control sections, labels and so on.


Tables
------

Use :ref:`reStructuredText tables <rst-tables>`, i.e. either

- grid table syntax (:duref:`ref <grid-tables>`),
- simple table syntax (:duref:`ref <simple-tables>`),
- :dudir:`csv-table` syntax,
- or :dudir:`list-table` syntax.

The :dudir:`table` directive serves as optional wrapper of the *grid* and
*simple* syntaxes.

They work fine in
HTML output, however there are some gotchas when using tables in LaTeX: the
column width is hard to determine correctly automatically.  For this reason, the
following directive exists:

.. rst:directive:: .. tabularcolumns:: column spec

   This directive gives a "column spec" for the next table occurring in the
   source file.  The spec is the second argument to the LaTeX ``tabulary``
   package's environment (which Sphinx uses to translate tables).  It can have
   values like ::

      |l|l|l|

   which means three left-adjusted, nonbreaking columns.  For columns with
   longer text that should automatically be broken, use either the standard
   ``p{width}`` construct, or tabulary's automatic specifiers:

   +-----+------------------------------------------+
   |``L``| flush left column with automatic width   |
   +-----+------------------------------------------+
   |``R``| flush right column with automatic width  |
   +-----+------------------------------------------+
   |``C``| centered column with automatic width     |
   +-----+------------------------------------------+
   |``J``| justified column with automatic width    |
   +-----+------------------------------------------+

   The automatic widths of the ``LRCJ`` columns are attributed by ``tabulary``
   in proportion to the observed shares in a first pass where the table cells
   are rendered at their natural "horizontal" widths.

   By default, Sphinx uses a table layout with ``J`` for every column.

   .. versionadded:: 0.3

   .. versionchanged:: 1.6
      Merged cells may now contain multiple paragraphs and are much better
      handled, thanks to custom Sphinx LaTeX macros. This novel situation
      motivated the switch to ``J`` specifier and not ``L`` by default.

   .. hint::

      Sphinx actually uses ``T`` specifier having done ``\newcolumntype{T}{J}``.
      To revert to previous default, insert ``\newcolumntype{T}{L}`` in the
      LaTeX preamble (see :confval:`latex_elements`).

      A frequent issue with tabulary is that columns with little contents are
      "squeezed". The minimal column width is a tabulary parameter called
      ``\tymin``. You may set it globally in the LaTeX preamble via
      ``\setlength{\tymin}{40pt}`` for example.

      Else, use the :rst:dir:`tabularcolumns` directive with an explicit
      ``p{40pt}`` (for example) for that column. You may use also ``l``
      specifier but this makes the task of setting column widths more difficult
      if some merged cell intersects that column.

   .. warning::

      Tables with more than 30 rows are rendered using ``longtable``, not
      ``tabulary``, in order to allow pagebreaks. The ``L``, ``R``, ... specifiers
      do not work for these tables.

      Tables that contain list-like elements such as object descriptions,
      blockquotes or any kind of lists cannot be set out of the box with
      ``tabulary``. They are therefore set with the standard LaTeX ``tabular`` (or
      ``longtable``) environment if you don't give a ``tabularcolumns`` directive.
      If you do, the table will be set with ``tabulary`` but you must use the
      ``p{width}`` construct (or Sphinx's ``\X`` and ``\Y`` specifiers described
      below) for the columns containing these elements.

      Literal blocks do not work with ``tabulary`` at all, so tables containing
      a literal block are always set with ``tabular``. The verbatim environment
      used for literal blocks only works in ``p{width}`` (and ``\X`` or ``\Y``)
      columns, hence Sphinx generates such column specs for tables containing
      literal blocks.

   Since Sphinx 1.5, the ``\X{a}{b}`` specifier is used (there *is* a backslash
   in the specifier letter). It is like ``p{width}`` with the width set to a
   fraction ``a/b`` of the current line width. You can use it in the
   :rst:dir:`tabularcolumns` (it is not a problem if some LaTeX macro is also
   called ``\X``.)

   It is *not* needed for ``b`` to be the total number of columns, nor for the
   sum of the fractions of the ``\X`` specifiers to add  up to one. For example
   ``|\X{2}{5}|\X{1}{5}|\X{1}{5}|`` is legitimate and the table will occupy
   80% of the line width, the first of its three columns having the same width
   as the sum  of the next two.

   This is used by the ``:widths:`` option of the :dudir:`table` directive.

   Since Sphinx 1.6, there is also the ``\Y{f}`` specifier which admits a
   decimal argument, such has ``\Y{0.15}``: this would have the same effect as
   ``\X{3}{20}``.

   .. versionchanged:: 1.6

      Merged cells from complex grid tables (either multi-row, multi-column, or
      both) now allow blockquotes, lists, literal blocks, ... as do regular cells.

      Sphinx's merged cells interact well with ``p{width}``, ``\X{a}{b}``, ``Y{f}``
      and tabulary's columns.

Math
----

See :ref:`math-support`.

.. rubric:: Footnotes

.. [#] For most builders name and format are the same. At the moment only
       builders derived from the html builder distinguish between the builder
       format and the builder name.

       Note that the current builder tag is not available in ``conf.py``, it is
       only available after the builder is initialized.

