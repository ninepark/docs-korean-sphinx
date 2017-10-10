.. highlight:: rest

.. _inline-markup:

인라인 마크업
=================

스핑크스는 해석 텍스트 기능을 사용하여 의미 마크업을 문서에 삽입한다.
그들은 ``:rolename:`content``` 형태로 쓰여진다.

.. note::

   디폴트 기능인 ```content``` 는 기본적으로 특별한 의미가 없다. 원하는대로 자유롭게 사용해도
   된다. (예: 변수 이름) :confval:`default_role` 설정값을 이용하여 이미 정의된 기능으로
   설정하는게 가능하다. (예: :rst:role:`any` role을 사용하여 검색을 하거나
   :rst:role:`py:obj` role을 사용하여 Python 객체를 검색)

도메인에 의해 추가된 기능에 대해서는 :ref:`domains` 참조한다.


.. _xref-syntax:

상호 레퍼런스 문법
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

상호 레퍼런스는 여러 의미 해석 텍스트 기능에 의해 생성된다.
기본적으로, ``:role:`target``` 의 형식으로 쓰면 *role* 의 형식을 가지며 *target* 이라는
이름을 가지는 항목에 링크가 생성된다. 이 때, 링크 텍스트는 *target* 과 동일하다.

그러나 상호 레퍼런스 기능을 보다 다양하게 만드는 몇 가지 추가 기능도 있다.

* reST 다이렉트 하이퍼 링크처럼 표제와 레퍼런스 대상을 제공 할 수 있다.
  예를 들어, ``:role:`title <target>``` 는 *target* 을 참조하지만 링크 텍스트는 *title*
  이 된다.

* 내용 앞에 ``!`` 를 붙이면 레퍼런스와 하이퍼 링크가 생성되지 않는다.

* 내용 앞에 ``~`` 를 붙이면 대상의 마지막 구성 요소가 링크 텍스트가 된다. 예를 들어,
  ``:py:meth:`~Queue.Queue.get``` 는 ``Queue.Queue.get`` 을 참조하지만 링크 텍스트로
  ``get`` 만을 표시한다. 이 기능은 모든 상호 레퍼런스 기능에서 작동하지는 않지만 도메인에
  따라 다르다.

  HTML 출력에서, mouse-hover에서의 tool-tip으로 표시하는 등의 링크의 ``title`` 속성은
  항상 완전 대상 명칭이 된다.


.. _any-role:

모든 상호 레퍼런스
------------------------------

.. rst:role:: any

   .. versionadded:: 1.3

   이 편리한 기능은 참조되는 텍스트에 대해 유효한 대상을 찾기 위해 최선을 다한다.

   * 먼저, :rst:role:`doc`, :rst:role:`ref`, :rst:role:`option` 에 의해
     참조되는 표준 상호 레퍼런스 대상을 찾는것을 시도한다.

     그리고 확장에 의해 표준 도메인에 추가 된 사용자 정의 객체
     (:meth:`.add_object_type` 참조)도 검색된다.

   * 그리고는 불려온 모든 도메인 안의 객체(대상)을 찾는다. 얼마나 구체적으로 일치해야
     하는지는 각 도메인에 달려 있다. 예를 들어, 파이썬 도메인에서 ``:any:`Builder``` 는
     ``sphinx.builders.Builder`` 클래스를 찾아낸다.

   대상이 하나도 없거나 여러 개인 경우 경고가 발생한다. 여러 대상의 경우 "any"를 특정 기능으로 변경할
   수 있다.

   이 기능은 :confval:`default_role` 로 설정하기에 적합하다. 마크업에 오버헤드가
   별로 없이 많은 상호 레퍼런스를 작성할 수 있기 때문이다. 예를 들어, 아래의 파이썬
   함수 문서에서::

      .. function:: install()

         This function installs a `handler` for every signal known by the
         `signal` module.  See the section `about-signals` for more information.

   보통 ``:term:`handler``` 형태의 용어 사전의 용어, 보통 ``:py:mod:``signal```
   또는 ``:mod:`signal``` 형태의 파이썬 모듈, 보통 ``:ref:`about-signals```
   형태의 섹션의 참조가 가능하다.

   :rst:role:`any` 기능은 :mod:`~sphinx.ext.intersphinx` 확장과 함께 같이 사용
   가능하다. 만약 로컬 상호 레퍼런스가 발견되지 않는다면, 모든 객체 유형의 intersphinx
   inventory도 검색된다.


상호 레퍼런스 객체
-----------------------------

아래의 기능들은 해당 도메인과 함께 설명되어있다:

* :ref:`Python <python-roles>`
* :ref:`C <c-roles>`
* :ref:`C++ <cpp-roles>`
* :ref:`JavaScript <js-roles>`
* :ref:`ReST <rst-roles>`


.. _ref-role:

임의의 위치의 상호 레퍼런스
-----------------------------------------

.. rst:role:: ref

   모든 문서에서 임의의 위치에 대한 상호 레퍼런스를 지원하기 위해서 표준 reST 라벨이 사용된다.
   이를 위해 라벨 이름은 전체 문서에서 고유해야 한다. 라벨 참조에는 두 가지 방법이 있다:

   * 섹션 제목 바로 앞에 라벨을 배치하면 ``:ref:`label-name``` 을 사용하여 라벨을
     참조 할 수 있다. 예::

        .. _my-reference-label:

        Section to cross-reference
        --------------------------

        This is the text of the section.

        It refers to the section itself, see :ref:`my-reference-label`.

     그런 다음 ``:ref:`` 기능은 섹션에 대한 링크를 생성하고, 링크 제목은 "Section to
     cross-reference"가 된다. 심지어 섹션과 레퍼런스가 다른 소스 파일에 있어도 된다.

     자동 라벨은 figure(삽화/도표)에도 사용 가능하다. 다음과 같이 주어졌을 경우::

        .. _my-figure:

        .. figure:: whatever

           Figure caption

     ``:ref:`my-figure`` 레퍼런스는 링크 텍스트 "Figure caption"과 함께 그림에 대한
     레퍼런스를 삽입한다.

     이는 :dudir:`table` 명령어를 사용한 테이블에 대해서도 캡션을 명시하였다면,
     마찬가지로 작동한다.

   * 섹션 제목 앞에 놓이지 않은 라벨도 여전히 ​​참조가 가능 하지만,
     ``:ref:`Link title <label-name> ``` 와 같은 식으로 제목을 명시해 주어야 한다.

   .. note::

      레퍼런스 라벨은 밑줄(``_``)로 시작해야 한다. 하지만 라벨을 참조 할 때에는
      밑줄 생략 해야 한다. (위의 예시 참조)

   ```Section title`_`` 과 같은 표준 reStructuredText를 사용한 섹션에 대한
   링크보다 :rst:role:`ref` 의 사용을 권장한다. 이는 파일이 다른경우나 섹션 제목이 변경되는
   경우에도 작동하며 상호 레퍼런스를 지원하는 빌더들과도 작동하기 때문이다.


문서 상호 레퍼런
-------------------------------

.. versionadded:: 0.6

문서에 직접 링크하는 방법도 있다.

.. rst:role:: doc

   지정된 문서에 링크한다. 문서 이름은 절대경로 또는 상대경로로 지정할 수 있다. 예를 들어,
   ``sketches/index`` 문서에서 ``:doc:`parrot``` 과 같은 레퍼런스가 있다면 링크는
   ``sketches/parrot`` 로 연결하게 된다. 만약 reference가 ``:doc:`/people``` 또는
   ``:doc:`../people``` 이라면 링크는 ``people`` 로 연결된다.

   ``:doc:`Monty Python members </people>``` 와 같이 링크 텍스트가 제공되지 않았다면 링크
   캡션은 주어진 문서의 제목이 된다.


다운로드 가능한 파일 레퍼런스
----------------------------------

.. versionadded:: 0.6

.. rst:role:: download

   이 기능은 소스 트리에 있는 reST 문서는 아니지만 다운로드 가능한 파일들에 링크하는 것을 가능하게 한다.

   이 기능을 사용하면 참조 되는 파일이 빌드시(HTML 출력에만 해당) 출력에 포함되도록 자동으로
   표시된다. 다운로드 가능한 모든 파일은 output 디렉토리의 ``_downloads`` 서브디렉토리에 저장된다.
   중복된 파일 이름은 알아서 처리된다.

   예::

      See :download:`this example script <../example.py>`.

   주어지는 파일 이름은 대개는 현재 소스 파일이 들어있는 디렉토리에 상대적이다. 하지만 만약
   ``/`` 로 시작하는 절대경로로 지정된다면, 이는 최상위 소스 디렉토리에 대해 상대적으로 취급된다.

   위에서는 ``example.py`` 파일이 output 디렉토리에 복사되고 적절한 링크가 생성된다.

   사용할 수 없는 다운로드 링크를 표시하지 않으려면, 다음 기능이 있는 전체 단락을 wrap해야 한다::

      .. only:: builder_html

         See :download:`this example script <../example.py>`.

도표 번호에 맞춘 상호 레퍼런스
----------------------------------------------

.. versionadded:: 1.3

.. versionchanged:: 1.5
   `numref` role can also refer sections.
   And `numref` allows `{name}` for the link text.

.. rst:role:: numref

   지정된 도표, 테이블, 코드 블럭,섹션에 링크한다. 여기에는 표준 reST 라벨이
   사용된다. 이 기능을 사용하면 "Fig. 1.1"과 같이 도표 번호와 함께 링크 텍스트가 있는
   도표에 대한 레퍼런스가 삽입된다.

   ``:numref:`Image of Sphinx (Fig. %s) <my-figure>``` 와 같이 링크 텍스트가 주어지면,
   링크 캡션은 레퍼런스의 제목이 된다. 특수 문자로 쓰인 `%s` 와 `{number}` 는 도표
   번호로 대체되고, `{name}` 은 도표 캡션으로 대체 된다. 링크 텍스트가 주어지지 않으면,
   :confval:`numfig_format` 의 값이 링크 텍스트의 디폴트로 사용된다.

   만약 :confval:`numfig` 가 ``False`` 이면, 도표는 번호가 매겨지지 않는다. 따라서 이
   기능은 레퍼런스가 아니라 라벨이나 링크 텍스트를 삽입한다.

관련된 기타 항목의 상호 레퍼런스
------------------------------------------------------

다음 기능들은 상호 레퍼런스를 만들지만 객체를 참조하지는 않는다.

.. rst:role:: envvar

   환경 변수. 인덱스 항목들이 생성된다. 또한 일치하는 :rst:dir:`envvar`
   명령어가 있다면, 이에 대한 링크를 생성한다.

.. rst:role:: token

   문법 토큰의 이름. (:rst:dir:`productionlist` 명령어 사이에 링크를
   생성하는데 사용)

.. rst:role:: keyword

   파이썬에서 키워드의 이름. 만약 존재한다면, 이 이름을 가지는 레퍼런스 라벨에 대한 링크를
   생성한다.

.. rst:role:: option

   실행 가능한 프로그램에 대한 커맨드 라인 옵션. 이것은 :rst:dir:`option` 명령어가 있는
   경우, 이에 대한 링크를 생성한다.


다음 기능은 :ref:`용어 사전 <glossary-directive>` 에 있는 용어에 대한 상호 레퍼런스를
생성한다:

.. rst:role:: term

   용어 사전의 용어에 대한 레퍼런스. 용어 사전은 ``glossary`` 명령어를
   사용하여 만들어지며, 용어과 정의가 있는 정의 목록을 포함한다. ``term`` 마크업과 같은
   파일에 있을 필요는 없다. 예를 들어, 파이썬 문서는 ``glossary.rst`` 파일에 글로벌
   용어 사전 하나만을 가지고 있다.

   만약 용어 사전에서 설명하지 않은 용어를 사용한다면, 빌드 중에 warning이 표시된다.


기타 의미
~~~~~~~~~~~~~~~~~~~~~~~~~

다음 기능들은 텍스트 서식을 지정하는 것 외에는 특별한 작업을 수행하지는 않는다:

.. rst:role:: abbr

   약어. 만약 기능의 내용에 괄호로 둘러싸인 설명이 포함되어 있으면, 이는
   특수하게 다루어진다. HTML에서는 tool-tip으로 표시되고 LaTeX에서는 한 번만 출력된다.

   Example: ``:abbr:`LIFO (last-in, first-out)```.

   .. versionadded:: 0.6

.. rst:role:: command

   ``rm``과 같은 OS레벨에서의 명령어.

.. rst:role:: dfn

   텍스트에서 용어가 정의되는 부분을 표시해 준다. (다만 인덱스 항목은 생성되지 않는다.)

.. rst:role:: file

   파일 또는 디렉토리의 이름. 내용 안에서 중괄호를 사용하여 "변하는" 부분을 나타낼 수 있다. 예::

      ... is installed in :file:`/usr/lib/python2.{x}/site-packages` ...

   이 경우, 빌드 된 문서에서 ``x`` 는 파이썬의 마이너 버전 번호로 대체된다.

.. rst:role:: guilabel

   상호 작용 유저 인터페이스의 일부로 제공된 라벨은 ``guilabel`` 을 사용하여 나타내야
   한다. 여기에는 :mod:`curses` 와 같은 텍스트 기반 라이브러리를 사용하여 만들어진, 텍스트 기반
   인터페이스의 라벨이 포함된다. 인터페이스에 사용되는 모든 라벨에는 이 기능으로 지정해 주어야
   한다. 이는 버튼 라벨, 창 이름, 필드 명칭, 메뉴, 메뉴 선택 이름, 심지어 선택
   목록 내의 값도 포함한다.

   .. versionchanged:: 1.0
      An accelerator key for the GUI label can be included using an ampersand;
      this will be stripped and displayed underlined in the output (example:
      ``:guilabel:`&Cancel```).  To include a literal ampersand, double it.

.. rst:role:: kbd

   키보드 입력 시퀀스를 표시. 무엇이 조합키를 이루는지는 플랫폼 또는 응용 프로그램 별로 규칙이 다를 수
   있다. 관련 규칙이 없으면 신규 사용자와 비 원어민 사용자의 편의성을 높이기 위해 변환키를 이름 그대로
   써줘야 한다. 예를 들어, *xemacs* 의 키입력 시퀀스는 ``:kbd:`C-x C-f``` 와 같이 표시 될 수
   있지만 특정 응용 프로그램이나 플랫폼에 대한 언급이 없으면, 동일한 시퀀스는
   ``:kbd:`Control-x Control-f``` 와 같이 써줘야 한다.

.. rst:role:: mailheader

   RFC 822 스타일의 메일 헤더 이름. 이 마크업은 헤더가 이메일 메시지에 사용되고 있음을
   의미하지는 않지만, 동일한 "스타일"의 헤더를 참조하는 데 사용될 수 있다. 이것은 다양한
   MIME 사양으로 정의 된 헤더에도 사용된다. 헤더 이름은 일반적으로 실제로 사용되는
   것과 동일한 방식으로 입력해야 한다. 일반적으로 camel-casing 방식이 선호된다. 예:
   ``:mailheader:`Content-Type```.

.. rst:role:: makevar

   :command:`make` 변수 의 이름.

.. rst:role:: manpage

   섹션을 포함한 유닉스 메뉴얼 페이지에 대한 참조. 예: ``:manpage:`ls(1)```.

.. rst:role:: menuselection

   메뉴 선택은 ``menuselection`` 기능을 사용하여 표시해줘야 한다. 이 옵션은 하위 메뉴 선택과
   특정 작업 선택을 포함한 모든 메뉴 선택 시퀀스를 표시하는 데 사용된다. 개별 선택 항목의 이름은
   ``-->`` 로 구분해 주어야 한다.

   예를 들어, "Start > Programs" 와 같은 선택을 나타내기 위해서는 다음과 같은 마크업을
   사용한다::

      :menuselection:`Start --> Programs`

   만약 뒤에 대화창이 열리는 것을 알려주는 줄임표와 같은 특수한 표식이 따라오는 선택을 포함시킬 때는
   뒤의 표식은 선택 이름에서 누락시킨다.


   ``menuselection`` 은 또한 :rst:role:`guilabel` 과 같이 ampersand accelerator를
   지원한다.

.. rst:role:: mimetype

   MIME 타입 또는 MIME 타입의 구성요소의 이름.

.. rst:role:: newsgroup

   Usenet 뉴스그룹의 이름.

.. rst:role:: program

   실행 프로그램의 이름. 플랫폼에 따라서 실행 파일의 파일 이름과 다를 수 있다. 특히 윈도우 프로그램의
   경우 ``.exe`` 와 같은 확장자를 생략해야 한다.

.. rst:role:: regexp

   정규표현식. 따옴표를 포함하면 안된다.

.. rst:role:: samp

   코드와 같은 리터럴 텍스트. :rst:role:`file` 에서 언급했듯이, 내용 안에 중괄호를 사용하여
   "변하는" 부분을 나타낼 수 있다. 예를 들면, ``:samp:`print 1+{variable}``` 에서
   ``variable`` 부분은 강조되어 표시된다.

   "가변 부분" 표시가 필요하지 않다면, 표준인 ````code```` 를 사용해도 된다.

또한 인덱스 항목을 생성하는 :rst:role:`index` 기능이 있다.

다음 기능들은 외부 링크를 생성한다.

.. rst:role:: pep

   파이썬 개선 제안 (pep) 에 대한 레퍼런스. 적절한 인덱스 항목이 생성된다.
   "PEP *number*\ "라는 텍스트가 생성되며, HTML 출력에서 이 텍스트는 지정된 PEP의 온라인
   사본에 대한 하이퍼 링크가 된다. ``:pep:`number#anchor``` 를 사용하여 특정 섹션에
   링크 할 수도 있다.

.. rst:role:: rfc

   Internet Request for Comments에 대한 레퍼런스. 적절한 인덱스 항목이 생성된다.
   "RFC *number*\ "라는 텍스트가 생성되며, HTML 출력에서 이 텍스트는 지정된 RFC의 온라인 사본에
   대한 하이퍼 링크가 된다. ``:rfc:`number#anchor``` 를 사용하여 특정 섹션에 링크 할
   수도 있다.


하이퍼 링크를 포함하기 위해서는 표준 reST 마크업만으로 충분하므로, 이를 위한 특수한 기능은
존재하지 않는다.


.. _default-substitutions:

대체
~~~~~~~~~~~~~~~~~

도큐멘테이션 시스템은 디폴트로 세 가지 대체를 제공한다. 이는 빌드 설정 파일에 설정되어 있다.

.. describe:: |release|

   도큐멘테이션이 가리키는 프로젝트 배포로 대체. 이는 알파/베타/배포 참여자
   태그를 포함하는 전체 버전 문자열이다. (예: ``2.5.2b3``) :confval:`release` 로
   설정할 수 있다.

.. describe:: |version|

   도큐멘테이션이 가리키는 프로젝트 버전으로 대체. 이는 major와 minor 부분으로만 구성된
   버전 번호를 얘기한다. (예: 전체 버전 번호가 2.5.1이더라도 2.5로 표시)
   이는 :confval:`version` 에서 설정할 수 있다.

.. describe:: |today|

   문서를 읽는 오늘 날짜 또는 빌드 설정 파일에 설정된 날짜로 대체. 일반적으로 ``April 14, 2007``
   의 형식을 가진다. :confval:`today_fmt` 와 :confval:`today` 로 설정 가능.
