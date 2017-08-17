.. highlight:: rest

.. _inline-markup:

Inline markup
=============

Sphinx는 interpreted text role(역할)을 사용하여 semantic markup을 문서에 삽입한다.
그들은 ``:rolename:`content``` 형태로 쓰여진다.

.. note::

   디폴트 role인 ```content``` 는 기본적으로 특별한 의미가 없다. 원하는대로 자유롭게 사용해도
   된다. 예: 변수 이름. :confval:`default_role` 설정값을 사용하여 만들어진 역할로 설정하는게
   가능하다. 예: :rst:role:`any` role을 사용하여 검색을 하거나 :rst:role:`py:obj`
   role을 사용하여 Python 객체를 검색.

Domain에 의해 추가된 role에 대해서는 :ref:`domains` 참조.


.. _xref-syntax:

Cross-referencing syntax
~~~~~~~~~~~~~~~~~~~~~~~~

cross-reference(상호참조)는 많은 semantic interpreted text role에 의해 생성된다. 기본적으로
``:role:`target``` 이라고 쓰면 *role* 이 가리키는 타입의 *target* 이라고 이름지어진 항목에 링크가
생성된다. 링크 텍스트는 *target* 과 동일하다.

그러나 cross-reference role을 보다 다양하게 만드는 몇 가지 추가 기능이 있다.

* reST direct hyperlink에서 처럼 title과 reference target을 제공 할 수 있다.
  예: ``:role:`title <target>``` 는 *target* 을 참조하지만 링크 텍스트는 *title* 이 된다.

* 내용 앞에 접두로 ``!`` 를 붙이면 reference와 hyperlink가 생성되지 않는다.

* 내용 앞에 접두로 ``~`` 를 붙이면 링크 텍스트는 target의 마지막 구성 요소가 된다. 예를 들어,
  ``:py:meth:`~Queue.Queue.get``` 는 ``Queue.Queue.get`` 을 참조하지만 링크 텍스트로
  ``get`` 만 표시한다. 이 기능은 모든 cross-reference role에서 작동하지는 않지만 domain에
  따라 다르다.

  HTML 출력에서, 링크의 ``title`` 속성(예: mouse-hover에서의 tool-tip으로 표시)은
  항상 full target name이 된다.


.. _any-role:

Cross-referencing anything
--------------------------

.. rst:role:: any

   .. versionadded:: 1.3

   이 convenience role은 reference text에 대해 유효한 target을 찾기 위해 최선을 다한다.

   * 먼저, :rst:role:`doc`, :rst:role:`ref` 또는 :rst:role:`option` 에 의해
     참조되는 표준 cross-reference target을 찾는것을 시도한다.

     그리고 extension에 의해 standard domain에 추가 된 사용자 정의 객체
     (:meth:`.add_object_type` 참조)도 검색된다.

   * 그리고는 불려온 모든 domain 안의 객체(target)를 찾는다. 얼마나 구체적으로 일치해야
     하는지는 각 domain에 달려 있다. 예를 들어, Python domain에서 ``:any:`Builder``` 는
     ``sphinx.builders.Builder`` class를 찾아낸다.

   대상이 하나도 없거나 여러 개인 경우 경고가 발생한다. 여러 대상의 경우 "any"를 특정 role로 변경할
   수 있다.

   이 role은 :confval:`default_role` 로 설정하기에 적합한 후보이다. markup overhead가
   별로 없이 많은 cross-reference를 작성할 수 있기 때문이다. 예를 들어, 아래의 Python function
   documentation에서::

      .. function:: install()

         이 function은 `signal` module이 알고있는 모든 signal에 대해 `handler` 를 설치한다.
         더 자세한 정보는 `about-signals` 섹션을 참조하면 된다.

   Glossary의 term (보통 ``:term:`handler```), Python module
   (보통 ``:py:mod:``signal``` 또는 ``:mod:`signal```)과 section (보통 ``:ref:`about-signals```) 참조가 가능하다.

   :rst:role:`any` role은 :mod:`~sphinx.ext.intersphinx` extension과 함께 같이 사용
   가능하다. 만약 로컬 cross-reference가 발견되지 않는다면, 모든 객체 유형의 intersphinx
   inventory도 검색된다.


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

   * 섹션 제목 바로 앞에 라벨을 배치하면 ``:ref:`label-name``` 을 사용하여 라벨을 참조 할 수 있다.
   예::

        .. _my-reference-label:

        상호참조할 섹션
        --------------------------

        이것은 섹션의 텍스트이다.

        이것은 섹션 자체를 참조한다. :ref:`my-reference-label` 참조.

     그런 다음 ``:ref:`` role은 섹션에 대한 링크를 생성하고, 링크 제목은 "Section to
     cross-reference"가 된다. 심지어 섹션과 reference가 다른 소스 파일에 있어도 된다.

     자동 라벨은 figure(삽화/도표)와도 작동한다. 다음과 같이 주어졌을 경우::

        .. _my-figure:

        .. figure:: whatever

           Figure caption

     ``:ref:`my-figure`` reference는 링크 텍스트 "Figure caption"과 함께 그림에 대한
     reference를 삽입한다.

     이는 :dudir:`table` directive를 사용한 table에 대해서도 caption을 명시하였다면,
     마찬가지로 작동한다.

   * 섹션 제목 앞에 놓이지 않은 라벨도 여전히 ​​참조가 가능 하지만, ``:ref:`Link title <label-name> ```
     와 같은 식으로 제목을 명시해 주어야 한다.

   .. note::

      참조 label은 underscore(``_``)로 시작해야 한다. 하지만 라벨을 참조 할 때에는
      underscore를 생략 해야 한다. (위의 예시 참조)

   ```Section title`_`` 과 같은 표준인 reStructuredText의 section에 대한 link보다
   :rst:role:`ref` 의 사용을 권장한다. 이는 파일이 다른경우나 섹션 제목이 변경되는 경우에도 작동하며
   cross-reference를 지원하는 builder들과도 작동하기 때문이다.


문서 상호참조
---------------------------

.. versionadded:: 0.6

문서에 직접 링크하는 방법도 있다.

.. rst:role:: doc

   지정된 문서에 링크한다. 문서 이름은 절대적 또는 상대적으로 지정할 수 있다. 예를 들어,
   ``sketches/index`` 문서에서 ``:doc:`parrot``` 과 같은 reference가 있다면 링크는
   ``sketches/parrot`` 로 연결하게 된다. 만약 reference가 ``:doc:`/people``` 또는
   ``:doc:`../ people``` 이라면 링크는 ``people`` 로 연결된다.

   ``:doc:`Monty Python members </people> ``` 와 같이 링크 텍스트가 제공되지 않았다면 링크
   캡션은 주어진 문서의 제목이 된다.


다운로드 가능한 파일 참조
------------------------------

.. versionadded:: 0.6

.. rst:role:: download

   이 role은 소스트리에 있는 reST 문서는 아니지만 다운로드 가능한 파일들에 링크하는 것을 가능하게 한다.

   이 role을 사용하면 참조 된 파일이 빌드시(HTML 출력에만 해당) output에 포함되도록 자동으로 표시된다.
   다운로드 가능한 모든 파일은 output 디렉토리의 ``_downloads`` 서브디렉토리에 저장된다. 중복된
   파일 이름은 알아서 처리된다.

   An example::

      See :download:`this example script <../example.py>`.

   주어지는 파일 이름은 대개는 현재 소스 파일이 들어있는 디렉토리에 상대적이다. 하지만 만약
   절대적(``/`` 로 시작)이라면, 최상위 소스 디렉토리에 대해 상대적으로 취급된다.

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

   링크 텍스트가 주어지면 (예: ``:numref:`Image of Sphinx (Fig. %s) <my-figure>```),
   링크 caption은 reference의 제목이 된다. 특수 문자로는 `%s` 와 `{number}` 는 figure
   number로 대체되고, `{name}` 은 figure caption으로 대체 된다. 링크 텍스트가 주어지지 않으면,
   :confval:`numfig_format` 의 값이 링크 텍스트의 디폴트 값으로 사용된다.

   만약 :confval:`numfig` 가 ``False`` 이면, figure는 번호가 매겨지지 않는다. 따라서 이
   role은 참조가 아니라 label이나 링크 텍스트를 삽입한다.

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

   Python에서 keyword의 이름. 만약 존재한다면, 이 이름을 가지는 reference label에 대한 링크를
   생성한다.

.. rst:role:: option

   실행 가능한 프로그램에 대한 command-line 옵션. 이것은 :rst:dir:`option` directive가 있는
   경우 이에 대한 링크를 생성한다.


다음 role은 :ref:`glossary <glossary-directive>` 에 있는 term에 대한 cross-reference를
생성한다:

.. rst:role:: term

   용어집의 용어에 대한 reference. 용어집은 ``glossary`` directive를 사용하여 만들어지며,
   용어들과 정의가있는 정의 목록을 포함한다. ``term`` markup과 같은 파일에 있을 필요는 없다.
   예를 들어 Python의 docs는 ``glossary.rst`` 파일에 글로벌 용어집 하나를 가지고 있다.

   만약 glossary에서 설명하지 않은 term을 사용한다면, 빌드 중에 warning이 표시된다.


기타 semantic markup
~~~~~~~~~~~~~~~~~~~~~

다음 role들은 텍스트 서식을 지정하는 것 외에는 특별한 작업을 수행하지는 않는다.

.. rst:role:: abbr

   Abbreviation(약어). 만약 role의 내용에 괄호안에 들어간 설명이 포함되어 있으면, 이는 특수하게
   다루어진다. HTML에서는 tool-tip으로 표시되고 LaTeX에서는 한 번만 출력된다.

   Example: ``:abbr:`LIFO (last-in, first-out)```.

   .. versionadded:: 0.6

.. rst:role:: command

   The name of an OS-level command, such as ``rm``.

.. rst:role:: dfn

   텍스트에서 term이 정의되는 부분을 표시해 준다. (색인 항목은 생성되지 않는다.)

.. rst:role:: file

   파일 또는 디렉토리의 이름. 내용 안에서 중괄호를 사용하여 "변하는" 부분을 나타낼 수 있다. 예::

      ... is installed in :file:`/usr/lib/python2.{x}/site-packages` ...

   빌드 된 documentation에서는 ``x`` 는 Python의 마이너 버전 숫자로 대체된다.

.. rst:role:: guilabel

   인터랙티브 사용자 인터페이스의 일부로 제공된 레이블은 ``guilabel`` 을 사용하여 나타내야 한다.
   여기에는 :mod:`curses` 또는 다른 텍스트 기반 라이브러리를 사용하여 만들어진 텍스트 기반
   인터페이스의 label이 포함된다. 인터페이스에 사용되는 모든 label에는 이 role로 표시해 주어야
   한다. 이는 단추 label, 창 제목, 필드 이름, 메뉴, 메뉴 선택 이름, 심지어 선택 목록내의 값도
   포함한다.

   .. versionchanged:: 1.0
      An accelerator key for the GUI label can be included using an ampersand;
      this will be stripped and displayed underlined in the output (example:
      ``:guilabel:`&Cancel```).  To include a literal ampersand, double it.

.. rst:role:: kbd

   키보드 입력을 표시. 무엇이 조합키를 이루는지는 플랫폼 또는 응용 프로그램 별로 규칙이 다를 수
   있다. 관련 규칙이 없으면 신규 사용자와 비 원어민의 사용 편의성을 높이기 위해 변환키를 이름대로
   써줘야 한다. 예를 들어, *xemacs* 의 키입력 시퀀스는 ``:kbd:`C-x C-f``` 와 같이 표시 될 수
   있지만 특정 응용 프로그램이나 플랫폼에 대한 언급이 없으면, 동일한 시퀀스는
   ``:kbd:`Control-x Control-f``` 와 같이 써줘야 한다.

.. rst:role:: mailheader

   RFC 822 스타일의 메일 헤더 이름. 이 markup은 헤더가 이메일 메시지에 사용되고 있음을
   의미하지는 않지만, 동일한 "스타일"의 헤더를 나타내는 데 사용될 수 있다. 이것은 다양한
   MIME specification으로 정의 된 헤더에도 사용된다. 헤더 이름은 일반적으로 실제로 사용되는
   것과 동일한 방식으로 입력해야 한다. 일반적으로 camel-casing 방식이 선호된다. 예:
   ``:mailheader:`Content-Type```.

.. rst:role:: makevar

   :command:`make` variable 의 이름.

.. rst:role:: manpage

   섹션을 포함한 Unix manual page에 대한 참조. 예 :``:manpage:`ls(1)```.

.. rst:role:: menuselection

   메뉴 선택은 ``menuselection`` role을 사용하여 표시해줘야 한다. 이 옵션은 하위 메뉴 선택과
   특정 작업 선택을 포함한 모든 메뉴 선택 시퀀스 표시하는 데 사용된다. 개별 선택 항목의 이름은
   ``-->`` 로 구분해 주어야 한다.

   예를 들어 "Start > Programs" 선택을 나타내기 위해서는 다음과 같은 markup을
   사용한다::

      :menuselection:`Start --> Programs`

   만약 뒤에 특수한 표식(예: 대화창 여는 것을 알려주는 줄임표)이 따라오는 선택을 포함시킬 때는
   뒤의 표식은 선택 이름에서 누락시킨다.


   ``menuselection`` 은 또한 :rst:role:`guilabel` 과 같이 ampersand accelerator를
   지원한다.

.. rst:role:: mimetype

   MIME type의 이름 또는 MIME type의 구성요소의 이름.

.. rst:role:: newsgroup

   Usenet 뉴스그룹의 이름.

.. rst:role:: program

   실행 프로그램의 이름. 플랫폼에 따라서 실행 파일의 파일 이름과 다를 수 있다. 특히 Windows 프로그램의
   경우 ``.exe`` 와 같은 확장자를 생략해야 한다.

.. rst:role:: regexp

   정규표현식. 따옴표를 포함하면 안된다.

.. rst:role:: samp

   코드와 같은 literal text. :rst:role:`file` 에서 언급했듯이, 내용 안에 중괄호를 사용하여
   "변하는" 부분을 나타낼 수 있다. 예를 들면, ``:samp:`print 1+{variable}``` 에서 ``variable``
   부분은 강조되어 표시된다.

   "가변 부분" 표시가 필요하지 않다면, 표준인 ````code```` 를 사용하면 된다.

또한 색인 항목을 생성하는 :rst:role:`index` role이 있다.

다음 role들은 외부 링크를 생성한다.

.. rst:role:: pep

   Python Enhancement Proposal에 대한 참조. 적절한 색인 항목이 생성된다. "PEP *number*\ "라는
   텍스트가 생성된다. HTML output에서 이 텍스트는 지정된 PEP의 온라인 사본에 대한 하이퍼링크가 된다.
   ``:pep:`number#anchor``` 를 사용하여 특정 섹션에 링크 할 수 있다.

.. rst:role:: rfc

   Internet Request for Comments에 대한 참조. 적절한 색인 항목이 생성된다. 
   "RFC *number*\ "라는 텍스트가 생성된다. HTML 출력에서 이 텍스트는 지정된 RFC의 온라인 사본에
   대한 하이퍼링크가 된다. ``:rfc:`number#anchor``` 를 사용하여 특정 섹션에 링크 할 수 있다.


하이퍼링크를 포함하기 위해서는 standard reST markup으로 충분하므로, 이를 위한 특수한 role은
존재하지 않는다.


.. _default-substitutions:

Substitutions
~~~~~~~~~~~~~

Documentation 시스템은 디폴트로 세 가지 substitution을 제공한다. 이는 빌드 설정 파일에 설정되어
있다.

.. describe:: |release|

   문서가 가리키는 프로젝트 release로 대체. 이는 alpha/beta/release canditate tag를 포함하는
   full version string이다. (예: ``2.5.2b3``) :confval:`release` 로 설정할 수 있다.

.. describe:: |version|

   문서가 가리키는 프로젝트 version으로 대체. 이는 major와 minor 부분으로만 구성된 버전을
   얘기한다. (예: 2.5 전체 버전 숫자가 2.5.1이더라도) :confval:`version`로 설정할 수 있다.

.. describe:: |today|

   오늘 날짜(문서를 읽는 날짜) 또는 빌드 설정 파일에 설정된 날짜로 대체. 일반적으로 ``April 14, 2007``
   형식을 가진다. :confval:`today_fmt` 와 :confval:`today` 로 설정 가능.
