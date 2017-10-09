.. highlight:: rest

기타 마크업
========================

.. _metadata:

File-wide 메타 데이터
----------------------

reST는 "field list"라는 개념을 가지고 있다. 이들은 다음과 같이 주어지는 필드들의 시퀀스이다::

   :fieldname: Field content

파일 상단 근처의 필드 리스트는 "docinfo"로 docutils에 의해 파싱된다, 일반적으로 저자, 발행일,
기타 메타 데이터를 기록하는 데 사용된다. *스핑크스* 에서는 모든 마크업 앞에 오는 필드 리스트는
문서 메타 데이터로 docinfo에서 스핑크스 환경으로 이동되며 출력에 표시되지 않는다.
문서 제목 다음에 나타나는 필드 리스트는 정상적으로 docinfo에 포함되어 출력에 표시된다.

현재 인식되는 메타 데이터 필드는 다음과 같다:

``tocdepth``
   이 파일의 목차에서의 최대 깊이.

   .. versionadded:: 0.4

``nocomments``
   만약 설정된다면, 웹 애플리케이션에서 이 소스 파일에서 생성된 페이지에 comment form을 표시하지
   않는다.

``orphan``
   만약 설정된다면, 이 파일이 toctree에 포함되지 않았다는 경고가 발생하지 않는다.

   .. versionadded:: 1.0


메타 정보 마크업
---------------------------

.. rst:directive:: .. sectionauthor:: name <email>

   현재 섹션의 작성자를 나타낸다. 프레젠테이션과 이메일 주소에 사용 할 수 있도록 작성자
   이름이 인수에 포함되어야 한다. 주소의 도메인 이름 부분은 소문자이여야 한다. 예::

      .. sectionauthor:: Guido van Rossum <guido@python.org>

   기본적으로 이 마크업은 출력에 반영되지 않지만 기여도를 기록하는데는 요긴하다. 하지만
   설정값을 :confval:`show_authors` 에서 ``True`` 로 해놓으면 출력에 표시가 가능하다.


.. rst:directive:: .. codeauthor:: name <email>

   :rst:dir:`sectionauthor` 가 문서의 저자(들)을 지정하듯이, :rst:dir:`codeauthor`
   명령어는 주어진 코드의 작성자 이름을 지정하며, 여러번 사용 가능하다. 마찬가지로
   :confval:`show_authors` 설정값이 ``True`` 인 경우에만 출력을 생성한다.


인덱스 생성 마크업
---------------------------

스핑크스는 :ref:`domains` 에서 설명 된 것처럼 모든 객체의 설명(함수, 클래스, 인수)
로부터 인덱스 항목를 자동으로 생성한다.

그러나 인덱스를 보다 포괄적으로 만들고, 정보가 주로 언어 레퍼런스와 같이 정보 단위에
포함되지 않는 문서에서 인덱스 항목를 사용 할 수 있도록 해주는 마크업도 있다.

.. rst:directive:: .. index:: <entries>

   이 명령어는 하나 이상의 인덱스 항목이 들어간다. 각 항목은 타입과 값으로 구성되며,
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

   위의 명령어는 5개의 항목을 포함하며, 생성 된 인덱스의 항목으로 변환되어 인덱스 명령문의
   정확한 위치(오프라인 매체의 경우 해당 페이지의 번호)로 연결된다.

   인덱스 명령어는 소스의 해당 위치에서 상호 레퍼런스 대상을 생성하기 때문에 위의
   예제에서와 같이 참조하는 것 *앞에* 넣는 것이 좋다.

   가능한 항목의 타입은 다음과 같다.

   single
      단일 인덱스 항목을 생성한다. 세미콜론을 사용함으로 하위 항목을 생성하는 것도 가능하다.
      (이 표기법은 아래의 항목들에도 사용된다)
   pair
      ``pair: loop; statement`` 는 ``loop; statement`` 와 ``statement; loop``
      두개의 인덱스 항목을 만들기 위한 축약 표현이다.
   triple
      마찬가지로, ``triple: module; search; path`` 는 ``module; search path``,
      ``search; path module``, ``path; module search`` 세개의 인덱스 항목을
      만든다.
   see
      ``see: entry; other`` 는 ``entry`` 에서 ``other`` 를 가리키는 인덱스 항목을
      만든다.
   seealso
      ``see`` 와 같지만 "see" 대신에 "see also" 를 삽입한다.
   module, keyword, operator, object, exception, statement, builtin
      이것들은 모두 두개의 인덱스 항목을 만든다. 예를들어, ``module: hashlib`` 는
      ``module; hashlib`` 와 ``hashlib; module`` 을 만든다. 다만 이것들은
      파이썬에만 한정되어 있으며 deprecate 되었다.


   앞에 느낌표를 붙여서 "main" 인덱스 항목을 표시할 수 있다. "main" 항목에 대한 레퍼런스는
   생성 된 인덱스에서 강조된다. 예를 들어 두 페이지에 다음과 같은게 있고::

      .. index:: Python

   그리고 다른 한 페이지에는 다음과 같은게 있다면::

      .. index:: ! Python

   세개의 백링크들 중에서 제일 나중의 것만 강조된다.

   만약 인덱스 명령어가 "single" 항목만 포함한다면 다음과 같은 축약형을 쓸 수 있다::

      .. index:: BNF, grammar, syntax, notation

   이렇게 하면 네 개의 인덱스 항목이 생성된다.

   .. versionchanged:: 1.1
      Added ``see`` and ``seealso`` types, as well as marking main entries.

.. rst:role:: index

   :rst:dir:`index` 명령어는 다음 문단의 시작 부분에 연결되는 블럭 레벨의 마크업이다.
   링크 대상이 사용되는 곳에서 직접 링크 대상을 설정하는 대응되는 기능도 있다.

   기능의 내용은 간단한 구절일 수 있으며, 이는 텍스트로 유지되고 인덱스 항목으로 사용 가능하다.
   또한 상호 레퍼런스의 명시된 대상들과 같은 스타일로 텍스트와 인덱스 항목의 조합일 수도
   있다. 이 경우, "target" 부분은 위의 명령어에서 설명한대로 전체 항목이 될 수도
   있다. 예를 들면::

      This is a normal reST :index:`paragraph` that contains several
      :index:`index entries <pair: index; entry>`.

   .. versionadded:: 1.1


.. _tags:

태그에 기반한 컨텐츠 포함
-----------------------------------

.. rst:directive:: .. only:: <expression>

   주어진 *expression* 이 true인 경우에만 명령어의 내용을 포함한다. 다음과 같이
   expression은 태그로 구성되어야 한다.

      .. only:: html and draft

   정의되지 않은 태그는 false, 정의 된 태그(``-t`` 커맨드 라인 옵션을 통해 또는
   :file:`conf.py` 안에서 설정, :see:ref:`here <conf-tags>` 참조)는 true로 간주된다.
   ``html and (latex or draft)`` 와 같이 괄호를 사용하는 부올리언 표현식도 지원된다.

   현재 빌더(``html``, ``latex`` 또는 ``text``)의 *형식* 과 *명칭* 은 항상
   태그 [#]_ 로 설정된다. 여기에선, 형식과 명칭을 구별하기 위해서 접두로 ``format_`` 과
   ``builder_`` 가 추가된다. 예를 들어, epub builder는 ``html``, ``epub``,
   ``format_html``, ``builder_epub`` 태그를 정의한다.

   이 표준 태그들은 설정 파일을 읽은 *후에* 지정되므로 설정 파일에서는 사용 할 수 없다.

   모든 태그는 `Identifiers and keywords
   <https://docs.python.org/2/reference/lexical_analysis.html#identifiers>`_
   에서 설정된 표준 파이썬 식별자 문법을 따라야 한다. 즉, 태그
   표현식은 파이썬 변수의 문법을 따르는 태그로만 구성 될 수 있다.
   ASCII에서는 ``A`` 부터 ``Z`` 까지의 대문자와 소문자, 밑줄 ``_``, ``0`` 부터 ``9``
   까지의 숫자(첫번째 문자 제외)로 구성된다.

   .. versionadded:: 0.6
   .. versionchanged:: 1.2
      Added the name of the builder and the prefixes.

   .. warning::

      이 명령어는 문서의 내용만 제어하도록 설계되었다. 섹션, 라벨 등은 제어 할 수
      없다.


테이블
----------

:ref:`reStructuredText 테이블 <rst-tables>` 을 사용하면 된다. 즉, 다음 중 하나를
사용한다.

- 격자 테이블 문법 (:duref:`ref <grid-tables>`),
- 간편 테이블 문법 (:duref:`ref <simple-tables>`),
- :dudir:`csv-table` 문법,
- :dudir:`list-table` 문법.

:dudir:`table` 명령어는 *격자* 와 *간편* 테이블 문법의 wrapper 역할을 한다.

위의 예시들은 HTML 출력에서는 잘 작동하지만, LaTeX에서 테이블을 사용할 때에는 몇 가지 문제가 생긴다.
예를 들면 컬럼 너비를 자동으로 올바르게 판별하질 못한다. 이러한 이유로 다음 명령어가
존재한다.

.. rst:directive:: .. tabularcolumns:: column spec

   이 명령어는 소스 파일에서 나타나는 다음 테이블의 "컬럼 사양" 설정한다. 여기서 사양은
   스핑크스가 테이블 변환에 사용하는 LaTeX ``tabulary`` 패키지의 환경에서 사용하는 두번째
   인수이다. 다음과 같은 값을 가질 수 있다::

      |l|l|l|

   이것은 좌로 정렬된 세 개의 연속적인 컬럼을 의미한다. 긴 텍스트가 있어 자동으로
   분할되어야 하는 경우, 표준인 ``p{width}`` 를 사용하거나 tabulary의 자동
   지정자를 사용한다:

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

   기본적으로 스핑크스는 테이블 레이아웃에서 모든 컬럼에 ``J`` 를 사용한다.

   .. versionadded:: 0.3

   .. versionchanged:: 1.6
      Merged cells may now contain multiple paragraphs and are much better
      handled, thanks to custom Sphinx LaTeX macros. This novel situation
      motivated the switch to ``J`` specifier and not ``L`` by default.

   .. hint::

      Sphinx는 실제로 ``T`` 지정자 가 들어간 ``\newcolumntype{T}{J}`` 를 사용한다.
      이전 디폴트로 되돌리려면 LaTeX preamble에 ``\newcolumntype{T}{L}`` 를 넣으면
      된다. (:confval:`latex_elements` 참조)

      Tabulary에서 자주 발생하는 문제로는, 내용이 거의 없는 컬럼들이 "압축" 된다는 것이다.
      최소 컬럼 너비는 ``\tymin`` 이라고 불리는 tabulary의 매개 변수이다. 이는
      LaTeX preamble에서 ``\setlength{\tymin}{40pt}`` 식으로 사용하는 걸로 글로벌
      설정이 가능하다.

      그렇지 않으면, 그 컬럼에 :rst:dir:`tabularcolumns` 명령어에 ``p{40pt}``
      식으로 직접 명시하면 된다. ``l`` 지정자를 사용 할 수도 있지만, 만약 병합 된 셀이
      컬럼과 겹치게 된다면, 컬럼 너비를 설정하는 작업이 더 어려워진다.

   .. warning::

      30개 이상의 행을 가진 테이블은 페이지 분할을 위해서 ``tabulary`` 를 사용하지 않고
      ``longtable`` 을 이용해서 렌더링 된다. 이 경우에는 ``L``, ``R``, 등의 지정자는
      작동하지 않는다.

      객체 설명, 블록 인용, 목록 등의 list와 같은 요소를 포함하는 테이블은 ``tabulary`` 로
      바로 사용 할 수 없다. 따라서 이 경우에는 ``tabularcolumns`` 명령어를 제공하지
      않는다면 표준 LaTeX ``tabular`` (또는 ``longtable``) 환경으로 설정 되어 버린다.
      만약 ``tabularcolumns`` 명령어를 사용한다면, 테이블은 ``tabulary`` 로 설정
      되지만, 이 경우, 이러한 리스트와 같은 요소를 포함한 컬럼에는 ``p{width}``
      구조(또는 아래에 기술된 Sphinx의 ``\X`` 와 ``\Y`` 지정자)를 사용해야 한다.

      리터럴 블럭은 ``tabulary`` 로는 사용 할 수 없다. 따라서 리터럴 블럭을 포함하는
      테이블은 항상 ``tabular`` 로 설정되어야 한다. 리터럴 블럭에 사용되는 verbatim
      environment는 ``p{width}``(및 ``\X`` 또는 ``\Y``)가 설정된 컬럼에서만
      작동하므로 스핑크스는 리터럴 블럭이 포함 된 테이블에 대해 이러한 컬럼 사양을
      생성한다.

   스핑크스 1.5부터는 ``\X{a}{b}`` 지정자가 사용된다. (지정자에 *백슬래시* 가
   있다는 점에 주목) 이것은 ``p{width}`` 와 비슷하지만 현재 행의 너비의 비율 ``a/b`` 로
   설정된다. 이는 :rst:dir:`tabularcolumns` 에서도 사용 가능하다. 심지어 일부 LaTeX 매크로가
   ``\X`` 라고 정의되었어도 문제가 되지 않는다.

   ``b`` 는 총 컬럼 수일 *필요는 없으며*, ``\X`` 지정자의 분수 합계가 1이 될
   *필요도 없다*. 예를 들어 ``|\X{2}{5}|\X{1}{5}|\X{1}{5}|`` 는 허용되는 표현이며,
   이 때, 테이블은 라인 너비의 80%를 차지하게 된다.

   이것은 :dudir:`table` 명령어의 ``:widths:`` 옵션에 의해 사용된다.

   스핑크스 1.6 이후부터는 ``\Y{0.15}`` 와 같이 소수 표현을 허용하는 ``\Y{f}`` 지정자도
   존재한다. 이것은 ``\X{3}{20}`` 와 똑같은 효과를 가진다.

   .. versionchanged:: 1.6

      Merged cells from complex grid tables (either multi-row, multi-column, or
      both) now allow blockquotes, lists, literal blocks, ... as do regular cells.

      Sphinx's merged cells interact well with ``p{width}``, ``\X{a}{b}``, ``Y{f}``
      and tabulary's columns.

수학
--------

:ref:`math-support` 참조.

.. rubric:: Footnotes

.. [#] 대부분의 빌더는 명칭과 형식이 동일하다. 현재에는 html 빌더에서 파생된
       빌더만 빌더 형식과 빌더 명칭을 구별한다.

       현재 빌더의 태그는 ``conf.py`` 에서 사용할 수 없으며, 빌더가 초기화 된
       후에만 사용 할 수 있다.
