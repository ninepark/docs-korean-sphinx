.. highlight:: rest

Miscellaneous markup
========================

.. _metadata:

File-wide metadata
----------------------

reST는 "field list"라는 개념을 가지고 있다. 이들은 다음과 같이 주어지는 field들의 시퀀스이다::

   :fieldname: Field content

파일 상단 근처의 field list는 "docinfo"로 docutils에 의해 파싱된다, 일반적으로 저자, 발행일,
기타 metadata를 기록하는 데 사용된다. *Sphinx* 에서는 모든 markup 앞에 오는 field list는
문서 metadata로 docinfo에서 Sphinx environment로 이동되며 output에 표시되지 않는다.
문서 제목 다음에 나타나는 field list는 정상적으로 docinfo에 포함되어 output에 표시된다.

현재 인식되는 metadata field들은 다음과 같다:

``tocdepth``
   이 파일의 목차에서의 최대 깊이.

   .. versionadded:: 0.4

``nocomments``
   만약 설정된다면, 웹 애플리케이션에서 이 소스 파일에서 생성된 페이지에 comment form을 표시하지
   않는다.

``orphan``
   만약 설정된다면, 이 파일이 toctree에 포함되지 않았다는 경고가 발생하지 않는다.

   .. versionadded:: 1.0


Meta-information markup
---------------------------

.. rst:directive:: .. sectionauthor:: name <email>

   현재 section의 작성자를 나타낸다. 프레젠테이션과 이메일 주소에 사용 할 수 있도록 작성자
   이름이 argument에 포함되어야 한다. 주소의 도메인 이름 부분은 소문자이여야 한다. 예::

      .. sectionauthor:: Guido van Rossum <guido@python.org>

   기본적으로 이 markup은 output에 반영되지 않지만 기여도를 기록하는데는 요긴하다. 하지만
   설정값을 :confval:`show_authors` 에서 ``True`` 로 해놓으면 output에 표시가 가능하다.


.. rst:directive:: .. codeauthor:: name <email>

   :rst:dir:`sectionauthor` 가 문서의 저자(들)을 지정하듯이, :rst:dir:`codeauthor`
   directive는 주어진 코드의 작성자 이름을 지정하며, 여러번 사용 가능하다. 마찬가지로
   :confval:`show_authors` 설정값이 ``True`` 인 경우에만 output을 생성한다.


Index-generating markup
---------------------------

Sphinx는 :ref:`domains` 에서 설명 된 것처럼 모든 객체의 설명(function, class,
attribute)으로부터 index entry를 자동으로 생성한다.

그러나 index를 보다 포괄적으로 만들고, 정보가 주로 language refrence와 같이 정보 단위에
포함되지 않는 문서에서 index entry를 사용 할 수 있도록 해주는 markup도 있다.

.. rst:directive:: .. index:: <entries>

   이 directive는 하나 이상의 index entry가 들어간다. 각 entry는 type과 value로 구성되며,
   이 둘은 콜론으로 구분짓는다.

   예::

      .. index::
         single: execution; context
         module: __main__
         module: sys
         triple: module; search; path

      The execution context
      ---------------------

      ...

   위의 directive는 5개의 entry를 포함하며, 생성 된 index의 entry로 변환되어 index 명령문의
   정확한 위치(오프라인 매체의 경우 해당 페이지의 번호)로 연결된다.

   Index directive는 소스의 해당 위치에서 cross-reference 대상을 생성하기 때문에 위의
   예제에서와 같이 참조하는 것 *앞에* 넣는 것이 좋다.

   가능한 entry의 type은 다음과 같다.

   single
      단일 index entry를 생성한다. 세미콜론을 사용함으로 subentry를 생성하는 것도 가능하다.
      (이 표기법은 아래의 항목들에도 사용된다)
   pair
      ``pair: loop; statement`` 는 ``loop; statement`` 와 ``statement; loop``
      두개의 index entry를 만들기 위한 축약 표현이다.
   triple
      마찬가지로, ``triple: module; search; path`` 는 ``module; search path``,
      ``search; path module``, ``path; module search`` 세개의 index entry를
      만든다.
   see
      ``see: entry; other`` 는 ``entry`` 에서 ``other`` 를 가리키는 index entry를
      만든다.
   seealso
      ``see`` 와 같지만 "see" 대신에 "see also" 를 삽입한다.
   module, keyword, operator, object, exception, statement, builtin
      이것들은 모두 두개의 index entry를 만든다. 예를들어, ``module: hashlib`` 는
      ``module; hashlib`` 와 ``hashlib; module`` 을 만든다. 다만 이것들은
      Python에만 한정되어 있으며 deprecate 되었다.


   앞에 느낌표를 붙여서 "main" index entry를 표시할 수 있다. "main" 항목에 대한 reference는
   생성 된 index에서 강조된다. 예를 들어 두 페이지에 다음과 같은게 있고::

      .. index:: Python

   그리고 다른 한 페이지에는 다음과 같은게 있다면::

      .. index:: ! Python

   세개의 backlink들 중에서 제일 나중의 것만 강조된다.

   만약 index directive가 "single" entry만 포함한다면 다음과 같은 축약형을 쓸 수 있다::

      .. index:: BNF, grammar, syntax, notation

   이렇게 하면 네 개의 index entry가 생성된다.

   .. versionchanged:: 1.1
      Added ``see`` and ``seealso`` types, as well as marking main entries.

.. rst:role:: index

   :rst:dir:`index` directive는 다음 문단의 시작 부분에 연결되는 block 레벨의 markup이다.
   링크 대상이 사용되는 곳에서 직접 링크 대상을 설정하는 대응되는 role도 있다.

   Role의 내용은 간단한 구절일 수 있으며, 이는 텍스트로 유지되고 index entry로 사용 가능하다.
   또한 cross-reference의 명시된 대상들과 같은 스타일로 텍스트와 index entry의 조합일 수도
   있다. 이 경우, "target" 부분은 위의 directive에서 설명한대로 full entry가 될 수도
   있다. 예를 들면::

      This is a normal reST :index:`paragraph` that contains several
      :index:`index entries <pair: index; entry>`.

   .. versionadded:: 1.1


.. _tags:

Including content based on tags
-----------------------------------

.. rst:directive:: .. only:: <expression>

   주어진 *expression* 이 true인 경우에만 directive의 내용을 포함한다. 다음과 같이
   expression은 태그로 구성되어야 한다.

      .. only:: html and draft

   정의되지 않은 태그는 false, 정의 된 태그(``-t`` command-line 옵션을 통해 또는
   :file:`conf.py` 안에서 설정, :see:ref:`here <conf-tags>` 참조)는 true로 간주된다.
   ``html and (latex or draft)`` 와 같이 괄호를 사용하는 boolean expression도 지원된다.

   현재 builder(``html``, ``latex`` 또는 ``text``)의 *format* 과 *name* 은 항상
   태그 [#]_ 로 설정된다. 여기에선, format과 name을 구별하기 위해서 접두로 ``format_`` 과
   ``builder_`` 가 추가된다. 예를 들어, epub builder는 ``html``, ``epub``,
   ``format_html``, ``builder_epub`` 태그를 정의한다.

   이 표준 태그들은 설정 파일을 읽은 *후에* 지정되므로 설정 파일에서는 사용 할 수 없다.

   `Identifiers and keywords
   <https://docs.python.org/2/reference/lexical_analysis.html#identifiers>`_
   에서 설정된 표준 Python identifier syntax를 모든 태그는 따라야 한다. 즉, 태그 
   expression은 Python variable의 syntax를 따르는 태그로만 구성 될 수 있다.
   ASCII에서는 ``A`` 부터 ``Z`` 까지의 대문자와 소문자, 밑줄 ``_``, ``0`` 부터 ``9``
   까지의 숫자(첫번째 문자 제외)로 구성된다.

   .. versionadded:: 0.6
   .. versionchanged:: 1.2
      Added the name of the builder and the prefixes.

   .. warning::

      이 directive는 문서의 내용만 제어하도록 설계되었다. Section, label 등은 제어 할 수
      없다.


Tables
----------

:ref:`reStructuredText tables <rst-tables>` 를 사용하면 된다. 즉, 다음 중 하나를
사용한다.

- grid table syntax (:duref:`ref <grid-tables>`),
- simple table syntax (:duref:`ref <simple-tables>`),
- :dudir:`csv-table` syntax,
- :dudir:`list-table` syntax.

:dudir:`table` directive는 *grid* 와 *simple* syntaxe의 wrapper 역할을 한다.

위의 예시들은 HTML 출력에서는 잘 작동하지만, LaTeX에서 테이블을 사용할 때에는 몇 가지 문제가 생긴다.
예를 들면 column width를 자동으로 올바르게 판별하질 못한다. 이러한 이유로 다음 directive가
존재한다.

.. rst:directive:: .. tabularcolumns:: column spec

   이 directive는 소스 파일에서 나타나는 다음 테이블의 "column spec" 설정한다. 여기서 spec은
   Sphinx가 테이블 변환에 사용하는 LaTeX ``tabulary`` 패키지의 환경에서 사용하는 두번째
   argument이다. 다음과 같은 값을 가질 수 있다::

      |l|l|l|

   이것은 left-adjuest된 세 개의 연속적인 column을 의미한다. 긴 텍스트가 있어 자동으로
   분할되어야 하는 경우, 표준인 ``p{width}`` 를 사용하거나 tabulary의 자동
   specifier를 사용한다:

   +-----+------------------------------------------+
   |``L``| flush left column with automatic width   |
   +-----+------------------------------------------+
   |``R``| flush right column with automatic width  |
   +-----+------------------------------------------+
   |``C``| centered column with automatic width     |
   +-----+------------------------------------------+
   |``J``| justified column with automatic width    |
   +-----+------------------------------------------+

   ``LRCJ`` column의 자동 너비는 ``tabulary`` 에 의해 셀의 자연스러운 "가로" 너비로 렌더링
   되도록 설정된다.

   기본적으로 Sphinx는 테이블 레이아웃에서 모든 column에 ``J`` 를 사용한다.

   .. versionadded:: 0.3

   .. versionchanged:: 1.6
      Merged cells may now contain multiple paragraphs and are much better
      handled, thanks to custom Sphinx LaTeX macros. This novel situation
      motivated the switch to ``J`` specifier and not ``L`` by default.

   .. hint::

      Sphinx는 실제로 ``T`` specifier 가 들어간 ``\newcolumntype{T}{J}`` 를 사용한다.
      이전 default로 되돌리려면 LaTeX preamble에 ``\newcolumntype{T}{L}`` 를 넣으면
      된다. (:confval:`latex_elements` 참조)

      Tabulary에서 자주 발생하는 문제로는, 내용이 거의 없는 column들이 "압축" 된다는 것이다.
      최소 column width는 ``\tymin`` 이라고 불리는 tabulary의 parameter이다. 이는
      LaTeX preamble에서 ``\setlength{\tymin}{40pt}`` 식으로 사용하는 걸로 글로벌
      설정이 가능하다.

      그렇지 않으면, 그 column에 :rst:dir:`tabularcolumns` directive에 ``p{40pt}``
      식으로 직접 명시하면 된다. ``l`` specifier를 사용 할 수도 있지만, 만약 병합 된 cell이
      column과 겹치게 된다면, column width를 설정하는 작업이 더 어려워진다.

   .. warning::

      30개 이상의 행을 가진 테이블은 페이지 분할을 위해서 ``tabulary`` 를 사용하지 않고
      ``longtable`` 을 이용해서 렌더링 된다. 이 경우에는 ``L``, ``R``, 등의 specifier는
      작동하지 않는다.

      객체 설명, 블록 인용, 목록 등의 list와 같은 요소를 포함하는 테이블은 ``tabulary`` 로
      바로 사용 할 수 없다. 따라서 이 경우에는 ``tabularcolumns`` directive를 제공하지
      않는다면 표준 LaTeX ``tabular`` (또는 ``longtable``) 환경으로 설정 되어 버린다.
      만약 ``tabularcolumns`` directive를 사용한다면, 테이블은 ``tabulary`` 로 설정
      되지만, 이 경우, 이러한 list와 같은 요소를 포함한 column에는 ``p{width}``
      construct(또는 아래에 기술된 Sphinx의 ``\X`` 와 ``\Y`` specifier)를 사용해야 한다.

      Literal block은 ``tabulary`` 로는 사용 할 수 없다. 따라서 literal block을 포함하는
      테이블은 항상 ``tabular`` 로 설정되어야 한다. Literal block에 사용되는 verbatim
      environment는 ``p{width}``(및 ``\X`` 또는 ``\Y``)가 설정된 column에서만
      작동하므로 Sphinx는 literal block이 포함 된 테이블에 대해 이러한 column spec을
      생성한다.

   Sphinx 1.5부터는 ``\X{a}{b}`` specifier가 사용된다. (specifier에 *백슬래시* 가
   있다는 점에 주목) 이것은 ``p{width}`` 와 비슷하지만 현재 행의 너비의 비율 ``a/b`` 로
   설정된다. 이는 :rst:dir:`tabularcolumns` 에서도 사용 가능하다. 심지어 일부 LaTeX 매크로가
   ``\X`` 라고 정의되었어도 문제가 되지 않는다.

   ``b`` 는 총 column 수일 *필요는 없으며*, ``\X`` specifier의 분수 합계가 1이 될
   *필요도 없다*. 예를 들어 ``|\X{2}{5}|\X{1}{5}|\X{1}{5}|`` 는 허용되는 표현이며,
   이 때, 테이블은 line width의 80%를 차지하게 된다.

   이것은 :dudir:`table` directive의 ``:widths:`` 옵션에 의해 사용된다.

   Sphinx 1.6 이후부터는 ``\Y{0.15}`` 와 같이 소수 표현을 허용하는 ``\Y{f}`` specifier도
   존재한다. 이것은 ``\X{3}{20}`` 와 똑같은 효과를 가진다.

   .. versionchanged:: 1.6

      Merged cells from complex grid tables (either multi-row, multi-column, or
      both) now allow blockquotes, lists, literal blocks, ... as do regular cells.

      Sphinx's merged cells interact well with ``p{width}``, ``\X{a}{b}``, ``Y{f}``
      and tabulary's columns.

Math
--------

:ref:`math-support` 참조.

.. rubric:: Footnotes

.. [#] 대부분의 builder는 name과 format이 동일하다. 현재에는 html builder에서 파생된
       builder만 builder format과 builder name을 구별한다.

       현재 builder의 태그는 ``conf.py`` 에서 사용할 수 없으며, builder가 초기화 된
       후에만 사용 할 수 있다.
