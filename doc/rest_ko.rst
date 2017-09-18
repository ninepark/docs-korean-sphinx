.. highlightlang:: rest

.. _rst-primer:

reStructuredText 입분
=======================

이 섹션은 reStructuredText (reST) 의 개념, 문법에 대한 간단한 소개이며
사용자가 생산적인 문서 작업을 하기에 부족함 없는 정보를 제공하는 것이 목적이다.
reST는 간편한 마크업 언어로 고안되었으므로 이 문서는 길지 않을 것이다.

.. seealso::

   `reStructuredText User Documentation
   <http://docutils.sourceforge.net/rst.html>`_ 는 신뢰할 수 있다.
   이 문서의 "ref" 링크들은 reST 레퍼런스 내부의 개별 구조 서술로 연결된다..


문단
----------

reST 문서에서 가장 기본적인 블럭은 문단(:duref:`ref <paragraphs>`) 이다.
문단은 간단히 말해 하나 이상의 공백행으로 구별되는 텍스트 덩어리이다.
파이썬에서와 마찬가지로 reST 에서 들여쓰기는 중요한 요소이다.
따라서 한 문단에 속하는 모든 라인은 동일한 들여쓰기 계층에서 좌로 정렬되어 있어야 한다.


.. _inlinemarkup:

인라인 마크업
-------------

표준 reST 인라인 마크업은 꽤 간단하다. 아래 마크업을 사용하면 된다. :

* 별표 1개: ``*text*`` 강조 (이탤릭체),
* 별표 2개: ``**text**`` 강한 강조 (볼드체)
* 뒷따옴표: ````text```` 코드 샘플.

별표 또는 뒷따옴표가 실행중인 텍스트에 나타나 인라인 마크업 식별자와 혼동 된다면
백슬래시 이스케이프를 사용해야 한다.

위 마크업이 갖는 제한들을 숙지하자. :

* 중첩 되어선 안된다.,
* 마크업 안에 들어갈 내용은 공백으로 시작되거나 끝나선 안된다. ``* text*`` 가 예시이다.
* 공백은 반드시 글자나 숫자가 아닌 문자로 주변 텍스트와 구별되어야 한다.
  ``thisis\ *one*\ word`` 와 같이 백슬래시 이스케이프를 사용하면 된다.

위 제한들은 docutils 의 나중 버전에서 해제될 수 있다.

reST는 사용지 지정 해석 텍스트 기능을 허용한다.
이 기능으로 둘러쌓인 텍스트틑 특정한 방법으로 해석된다.
Sphinx는 해석 텍스트 기능을 통해 의미 마크업과 식별자의 상호 레퍼런스를 해당 섹션에서 기술된 대로 제공한다.
일반적으로 쓰이는 문법은 ``:rolename:`content``` 이다.

표준 reST는 아래 기능들을 제공한다. :

* :durole:`emphasis` -- ``*emphasis*`` 의 대체 철자.
* :durole:`strong` -- ``**strong**`` 의 대체 철자.
* :durole:`literal` -- ````literal```` 의 대체 철자.
* :durole:`subscript` -- 아래첨자 텍스트
* :durole:`superscript` -- 위첨자 텍스트
* :durole:`title-reference` -- 책, 정기 간행물 등의 표제

:ref:`inline-markup` 에서 Sphinx 에 의해 추가된 기능들을 보자.


리스트와 유사 인용 블럭
---------------------------

리스트 마크업(:duref:`ref <bullet-lists>`)도 지원한다. :
적절히 들여쓰고 문단 앞에 별표를 두면 된다.
번호 리스트도 동일한 방법으로 할 수 있다. ``#`` 기호를 사용하면 자동으로 번호가 부여된다. ::

   * 글머리 기호 리스트이다.
   * 두개의 항목이 있으며, 두번째 항목은
     두개의 라인을 사용한다.

   1. 번호 리스트이다.
   2. 여기서도 두개의 항목을 사용했다.

   #. 번호 리스트이다.
   #. 여기서도 두개의 항목을 사용했다.


리스트의 중첩도 가능하다. 공백행으로 부모리스트로부터 분리될 수 있음을 숙지한다. ::

   * 이것은
   * 리스트이다.

     * 중첩된 리스트
     * 하위 항목

   * 이 부분에서 부모 리스트가 다시 시작된다.

정의 리스트(:duref:`ref <definition-lists>`)는 아래와 같이 생성된다. ::

   용어 (텍스트 라인 위에 온다.)
      용어의 정의이며 들여써야 한다.

      정의에 여러개의 문단을 사용할 수 있다.

   다음 용어
      정의 서술.

용어 부분의 텍스트는 한 줄을 넘어선 안된다.

인용 문단(:duref:`ref <block-quotes>`)은 주변 문단보다 많이 들여쓰면 생성된다.

라인 블럭(:duref:`ref <line-blocks>`)으로 줄바꿈을 보존할 수 있다. ::

   | 이 라인들은
   | 소스 파일과 동일하게
   | 줄바꿈 된다.

몇가지 특별한 블럭들이 지원된다. :

* 필드 리스트 (:duref:`ref <field-lists>`)
* 옵션 리스트 (:duref:`ref <option-lists>`)
* 인용 리터럴 블럭 (:duref:`ref <quoted-literal-blocks>`)
* doctest 블 (:duref:`ref <doctest-blocks>`)


소스 코드
-----------

리터럴 코드 블럭 (:duref:`ref <literal-blocks>`)은
문단 끝에 특정 마커 ``::`` 를 두면서 시작된다.
리터럴 블럭은 반드시 들여써야 한다. (다른 문단들과 마찬가지로 공백행으로 주변 문단과 구별된다.)::

   이 것은 일반적인 텍스트 문단이다. 다음 문단은 코드 샘플이다.::

      들여쓰기를 제거하지 않는 한
      이 부분은 실행되진 않는다.

      여러 라인이 올 수 있다.

   이 부분부터 다시 평범한 텍스트 문단이다.

``::`` 마커는 영리하게 처리된다. :

* 그 자체로 문단이 되면 그 문단은 문서에서 사라진다.
* 앞에 공백이 오면 마커는 사라진다.
* 앞에 공백이 오지 않으면 마커는 싱글 콜론으로 대체된다.

이로 인해 위 예시 첫문단의 두번째 문장은 "다음 문단은 코드샘플이다:" 로 구현된다.


.. _rst-tables:

테이블
------

*격자 테이블* (:duref:`ref <grid-tables>`)을 사용하려면 직접 격자를 그려야 한다.
아래와 같이 그린다. ::

   +------------------------+------------+----------+----------+
   | Header row, column 1   | Header 2   | Header 3 | Header 4 |
   | (header rows optional) |            |          |          |
   +========================+============+==========+==========+
   | body row 1, column 1   | column 2   | column 3 | column 4 |
   +------------------------+------------+----------+----------+
   | body row 2             | ...        | ...      |          |
   +------------------------+------------+----------+----------+

*심플 테이블* (:duref:`ref <simple-tables>`) 을 작성하는건 더 쉽지만 제약이 있다.
하나 이상의 행을 포함해야하며 첫번째 컬럼셀을 여러 라인을 포함할 수 없다. 아래와 같이 그린다. ::

   =====  =====  =======
   A      B      A and B
   =====  =====  =======
   False  False  False
   True   False  False
   False  True   False
   True   True   True
   =====  =====  =======

두개 문법을 더 지원한다. : *CSV 테이블* and *리스트 테이블*.
*명시 마크업 문법* 을 사용한다. `Directives`_ 섹션을 보자.

하이퍼 링크
----------

외부 링크
^^^^^^^^^^^^^^

인라인 웹 링크를 가져오기 위해 ```Link text <http://example.com/>`_`` 를 사용한다.
링크 텍스트가 웹 주소라면 특별한 마크업을 사용하지 않아도 된다.
파서가 텍스트로부터 링크와 메일주소를 찾아낸다.

.. important:: 링크 텍스트와 URL이 시작되는 \< 사이에 반드시 공백이 있어야 한다.

링크와 대상 정의(:duref:`ref <hyperlink-targets>`) 를 분리할 수 있다. 예시 ::

   `a link`_ 를 포함하는 문단이다.

   .. _a link: http://example.com/


내부 링크
^^^^^^^^^^^^^^

내부 링크는 Sphinx가 제공하는 특정 reST 기능으로 사용할 수 있다.
특정 마크업(:ref:`ref-role`) 섹션을 참고하자.


섹션
--------

섹션 헤더(:duref:`ref <sections>`)는 섹션 제목 아래에 줄을 그으면 생성된다. (경우에 따라 위에도 긋는다.)
최소한 섹션 제목이 되는 텍스트 만큼 길게 그어야 한다. ::

   =================
   이것은 제목이다.
   =================

섹션 구조는 연속되는 제목에 의해 결정되므로 특정 문자에 제목 계층이 할당되진 않지만
`Python's Style Guide for documenting
<https://docs.python.org/devguide/documenting.html#style-guide>`_ 에서는
아래 규칙들이 사용된다. :

* ``#`` 윗줄과 함께, 부(parts)에 사용됨.
* ``*`` 윗줄과 함께, 장(chapters)에 사용됨.
* ``=``, 섹션에 사용됨.
* ``-``, 하위 섹션에 사용됨.
* ``^``, 하위 섹션 아래의 하위 섹션에 사용됨.
* ``"``, 문단에 사용됨

물론 사용자가 원하는 마커 문자를 사용할수도 있다. (reST documentation 참)
더 깊은 계층을 중첩할 수도 있지만 대부분의 대상 포맷(HTML, LaTex)가 지원하는 중첩 깊이에 제한이 있다는걸 알아두자.


명시 마크업
---------------

reST에서 "명시 마크업" (:duref:`ref <explicit-markup-blocks>`)은
각주, 특별히 강조할 문단, 코멘트, 포괄 명령어와 같이 특별한 처리를 필요로 하는 구조에 사용된다.

명시 마크업 블럭은 ``..`` 로 시작되는 행에서 공백 다음에 오고
동일한 들여쓰기를 갖는 다음 문단에서 종료된다.
(명시 마크업과 일반 문단 사이에 공백행이 있어야 한다.
좀 복잡하게 들릴 수 있지만 작성해보면 직관적으로 알 수 있을 것이다.)


.. _directives:

명령어
----------

명령어(:duref:`ref <directives>`)는 명시 마크업의 포괄 블럭이다.
기능이기 전에 명령어는 reST의 확장 메카니즘이며 Sphinx는 이를 아주 많이 사용한다.

Docutils는 아래 명령어들을 지원한다:

* 경고(Admonitions): :dudir:`attention`, :dudir:`caution`, :dudir:`danger`,
  :dudir:`error`, :dudir:`hint`, :dudir:`important`, :dudir:`note`,
  :dudir:`tip`, :dudir:`warning` and the generic
  :dudir:`admonition <admonitions>`.  (대부분의 테마 스타일은 "note" 와
  "warning")

* 이미지:

  - :dudir:`image` (아래의 `이미지`_ 참고)
  - :dudir:`figure` (캡션과 선택적으로 범례를 포함하는 이미지)

* 추가 구성요소:

  - :dudir:`contents <table-of-contents>` (로컬, 현재 파일에서만, 목차 테이블)
  - :dudir:`container` (컨테이너와 사용자 지정 클래스, HTML에서 외부 ``<div>`` 생성시 유용)
  - :dudir:`rubric` (문서의 섹션과 관련 없는 제목)
  - :dudir:`topic`, :dudir:`sidebar` (특별히 강조되는 구성요소)
  - :dudir:`parsed-literal` (인라인 마크업을 지원하는 리터럴 블럭)
  - :dudir:`epigraph` (블럭 인용과 선택 객체 라인)
  - :dudir:`highlights`, :dudir:`pull-quote` (블럭 인용과 인용만의 클래스 객체)
  - :dudir:`compound <compound-paragraph>` (복합 문단)

* 특정 테이블:

  - :dudir:`table` (테이블과 제목)
  - :dudir:`csv-table` (CSV로부터 생성된 테이블)
  - :dudir:`list-table` (리스트를 포함하는 리스트로부터 생성된 테이블)

* 특정 명령어:

  - :dudir:`raw <raw-data-pass-through>` (미처리 대상 포맷 마크업 포함)
  - :dudir:`include` (다른 파일로부터 온 reStructuredText 포함)
    -- Sphinx에서 파일 경로를 포함하는 절대 경로가 주어질 때,
    이 명령어는 소스 디렉토리에 따라 경로를 받는다.
  - :dudir:`class` (다음에 올 요소에 클래스 객체를 지정한다.) [1]_

* HTML specifics:

  - :dudir:`meta` (HTML ``<meta>`` 태그의 생성)
  - :dudir:`title <metadata-document-title>` (문서 제목을 무시한다.)

* 결정 마크업:

  - :dudir:`default-role` (새로운 디폴트 기능 설정)
  - :dudir:`role` (새로운 기능 설정)

  위 마크업은 마크업이 사용된 파일에서만 유효하므로 Sphinx에서 :confval:`default_role`를 설정하는게 낫다.

:dudir:`sectnum`, :dudir:`header`, :dudir:`footer` 명령어는 사용하지 않는다.

Sphinx에 의해 추가된 명령어는 :ref:`sphinxmarkup` 에 설명되어 있다..

기본적으로 명령어는 명령어명, 인수, 옵션, 내용으로 구성된다.
(이 용어들은 사용자 지정 명령어를 다루는 다음 장에서 다시 사용되므로 기억해 두자.)
아래 예시를 보자. ::

   .. function:: foo(x)
                 foo(y, z)
      :module: some.module.name

      Return a line of text input from the user.

``function`` 은 명령어명이다. 이 명령어는 첫번째 라인과 두번째 라인에 나온 두개의 인수를 받는다.
옵션은 ``module`` 한개이다. (위 예시에서처럼 옵션은 인수 바로 뒤 라인에 오며 콜론으로 지정된다.)
옵션은 명령어 내용과 동일하게 들여써야 한다.

명령어 내용은 공백행 다음에 오며 명령어가 시작되는 부분과 동일하게 들여쓴다.


이미지
------

reST는 이미지 명령어(:dudir:`ref <image>`)를 지원하며 아래오 같이 쓴다. ::

   .. image:: gnu.png
      (options)

Sphinx에서 사용될 때 주어진 파일명(여기선 ``gnu.png``)은 소스 파일과 동일하거나 절대적이어야 한다.
이는 최상위 소스 디렉토리와 동일함을 의미한다.
예를 들어 ``sketch/spam.rst`` 파일은 ``images/spam.png`` 이미지를 가리키고
이는 ``../images/spam.png`` 또는 ``/images/spam.png`` 이다.

Sphinx는 빌딩 시 자동으로 출력 디렉토리의 하위 디렉토리로 이미지 파일을 복사한다.

이미지 사이즈 옵션(``width`` 와 ``height``)의 해석은 아래와 같다.:
사이즈의 단위가 없거나 단위가 픽셀이면 주어진 사이즈는 픽셀을 지원하는 아웃풋 채널에서만 지원된다.
다른 단위들 (``pt`` 는 포인트)은 HTML과 Latex 출력에서 사용된다.
(Tex 단위 ``bp``에서 ``72bp=1in`` 이므로 Latex는 ``pt`` 를 ``bp`` 로 대체한다.)

Sphinx는 확장에 별표를 허용함으로써 표준 docutils 양식을 확장한다. ::

   .. image:: gnu.*

이제 Sphinx는 주어진 패턴에 맞는 모든 이미지를 찾고 이미지 타입을 결정한다.
이후에 각각의 빌더는 후보중 가장 알맞은 이미지를 결정한다.
예를 들어 파일명 ``gnu.*`` 가 주어지고 소스 계층에 :file:`gnu.pdf`, :file:`gnu.png` 두 파일이 존재한다면
LaTex 빌더는 pdf 형식을 HTML 빌더는 png 형식을 선호할 것이다.
지원되는 이미지 타입과 선호도는 :ref:`builders` 명시되어 있다.

이미지 파일명은 공백을 포함해선 안된다.

.. versionchanged:: 0.4
   별표로 끝나는 파일명을 지원.

.. versionchanged:: 0.6
   이미지 경로는 절대 경로도 가능.

.. versionchanged:: 1.5
   latex 대상이 픽셀을 지원. (디폴트 ``96px=1in``).


각주
---------

``[#name]_`` 를 사용해 각주의 위치를 표시하고 문서 하단 "Footnotes" rubric 다음에 각주 본문을 추가한다.
예시::

   Lorem ipsum [#f1]_ dolor sit amet ... [#f2]_

   .. rubric:: Footnotes

   .. [#f1] 첫번째 각주의 텍스트.
   .. [#f2] 두번째 각주의 텍스트.

(``[1]_``) 와 같이 각주의 번호를 명시하거나 (``[#]_``) 를 이용해 자동으로 번호를 부여할 수 있다.
후자의 경우 각주명은 붙지 않는다.


인용
---------

표준 reST 인용(:duref:`ref <citations>`)이 지원되며 추가 기능으로 인용은 "global" 인용이 된다.
이는 모든 인용이 모든 파일에서 레퍼런스가 될 수 있음을 의미한다.
아래와 같이 사용한다.::

   Lorem ipsum [Ref]_ dolor sit amet.

   .. [Ref] 책 또는 기사 레퍼런스, URL이나 어떤 레퍼런스든 가능.

인용 사용법은 각주와 유사하지만 번호 라벨이나 ``#`` 을 사용하지 않는다.


대체
-------------

reST는 대체(:duref:`ref <substitution-definitions>`)를 지원한다.
대체란 텍스트 내부에서 ``|name|`` 으로  지정된 텍스트 또는 마크업의 조각들이다.
명시 마크업 블럭을 포함하는 각주와 같이 명시된다. 아래와 같이. ::

   .. |name| replace:: replacement *text*

또는 ::

   .. |caution| image:: warning.png
                :alt: Warning!

세부사항은 :duref:`reST reference for substitutions <substitution-definitions>` 를 참고하자.

모든 문서에서 적용되는 대체를 사용하고 싶다면 :confval:`rst_prolog` 에 입력하거나
분리된 파일에 넣고 :rst:dir:`include` 를 사용해 대체를 적용하고자 하는 모든 문서에 포함되게 하면 된다.
(포함될 파일에 다른 소스 파일과 다른 파일명 확장을 사용해 Sphinx가 이를 단일 문서로 인식하는 것을 방지해야 한다.)

Sphinx는 몇가지 디폴트 대체를 지정한다. :ref:`default-substitutions` 를 보자.


코멘트
--------

유효하지 않은 마크업 구조(위 각주와 같이)를 갖는 모든 명시 마크업 블럭은 코맨트(:duref:`ref <comments>`) 로 간주된다.
예시 ::

   .. This is a comment.

코멘트 시작점 이후에 들여쓴 텍스트로 여러줄의 코멘트를 구성할 수 있다. ::

   ..
      This whole indented block
      is a comment.

      Still in the comment.


소스 인코딩
---------------

reST에서 엠 대시(em dash)나 저작권 기호와 같은 특수문자는 유니코드 문자로 작성하는게 가장 쉽기 때문에
인코딩을 명시해야 한다. Sphinx는 UTF-8 인코딩을 디폴트로 한다.
:confval:`source_encoding` 배열값을 설정해 인코딩을 변경할 수 있다.


참고
-------

reST 문서를 작성하면서 일반적으로 겪는 몇가지 문제들이 있다.:

* **인라인 마크업의 분리:** 위에서 다룬대로 인라인 마크업은 글자가 아닌 문자로 주변 텍스트와 구별되어야 한다.
  백슬래시 이스케이프 된 공백을 인라인 마크업 양쪽에 두어야 한다.
  :duref:`the reference <substitution-definitions>` 에서 관련된 세부사항을 볼 수 있다.

* **인라인 마크업은 중첩될 수 없다.:** ``*see :func:`foo`*`` 와 같은 마크업은 불가능하다.


.. rubric:: Footnotes

.. [1] 디폴트 도메인이 :rst:dir:`class` 명령어를 포함할 때 명령어에 그림자가 생긴다.
       따라서 Sphinx는 다시 :rst:dir:`rst-class` 로 다시 내보낸다.
