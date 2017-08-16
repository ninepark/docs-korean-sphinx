.. highlight:: rst
.. _toctree-directive:

TOC tree
============

.. index:: pair: table of; contents

reST에는 여러 문서를 서로 연결하거나 문서를 여러 출력 파일로 분할 할 수 있는 기능이 없기 때문에 Sphinx는 custom
directive를 사용하여 문서들 관의 관계와 목차를 추가한다. ``toctree`` directive는 이를 위한 핵심 요소이다.

.. note::

   하나의 파일에 다른 파일을 간단하게 "include(포함)"하는 것은 :dudir:`include`
   directive로 가능하다.

.. rst:directive:: toctree

   이 directive는 directive 본문에 지정된 문서의 개별 TOC("sub-TOC tree" 포함)를
   사용하여 현재 위치에 "TOC tree"를 삽입한다. 문서의 상대적 이름(슬래시로 시작하지
   않음)은 directive가 있는 문서에 대해 상대적이며, 절대적 이름은 소스 디렉토리에 대해
   상대적이다. 숫자 ``maxdepth`` 옵션은 트리의 깊이를 나타내기 위해 지정할 수 있다.
   기본적으로는 모든 레벨이 포함된다. [#]_

   다음 예제를 고려해보자(Python 문서의 library reference index에서 가져옴)::

      .. toctree::
         :maxdepth: 2

         intro
         strings
         datatypes
         numeric
         (many more documents listed here)

   이것은 다음 두 가지를 수행한다:

   * 연관된 문서들에 있는 ``toctree`` directive 또한 고려하여, 모든 문서의 깊이 2까지의
     목차를 삽입한다.
   * Sphinx는 문서의 ``intro``, ``string`` 등의 상대적인 순서를 알고 있으며, 현재
     문서(library index)의 하위 항목임을 알고 있다. 이 정보를 이용하여 "다음 장", "이전 장"
     및 "상위 장" 링크를 생성한다.

   **항목**

   :rst:dir:`toctree` 안의 문서 제목들은 참조되는 문서의 제목에서 자동으로 읽혀진다.
   만약 이것이 원하는 것이 아니라면, reST 하이퍼링크와 유사한 syntax(및 Sphinx의
   :ref:`cross-referencing syntax <xref-syntax>`)를 사용하여 제목과 대상을 지정할
   수 있다. 이것은 다음과 같이 사용한다::

       .. toctree::

          intro
          All about strings <strings>
          datatypes

   위의 두번째 줄은 ``strings`` 문서에 링크 할 것이지만, ``strings`` 문서의 제목 대신에
   "All about strings"라는 제목을 사용한다.

   문서 이름 대신 HTTP 주소를 제공하여 외부 링크를 추가 할 수도 있다.

   **섹션 번호**

   HTML 출력에서도 섹션 번호를 원하면 **toplevel** toctree에 ``numbered`` 옵션을
   지정한다. 예를 들면::

      .. toctree::
         :numbered:

         foo
         bar

   번호 매김은 ``foo`` 의 표제에서 시작된다. Sub-toctrees는 자동적으로 번호가 매겨지므로
   ``numbered`` 플래그를 사용하면 안된다.

   ``numbered`` 에 숫자 argument를 지정함으로써 특정한 깊이까지 번호를 매기는
   것도 가능하다.

   **추가 옵션**

   ``caption`` 옵션을 사용하면 toctree 설명을 제공 할 수 있다. ``name`` 옵션을 사용하면
   :rst:role:`ref` 에 의헤 참조 될 수 있는 암시적 대상 이름을 제공 할 수 있다::

      .. toctree::
         :caption: Table of Contents
         :name: mastertoc

         foo

   같은 레벨의 다른 표제가 아닌, 트리의 문서 제목만 나타나게 하려면 ``titlesonly`` 옵션을
   사용하면 된다::

      .. toctree::
         :titlesonly:

         foo
         bar

   ``glob`` 플래그 옵션을 줌으로써 toctree directive에 "globbing"을 사용할 수 있다.
   그러면 모든 항목이 사용 가능한 문서 목록과 대조되고, 일치하는 항목들이 사전 순으로
   목록에 삽입된다. 예::

      .. toctree::
         :glob:

         intro*
         recipe/*
         *

   여기에는 이름이 ``intro`` 로 시작하는 모든 문서들과 ``recipe`` 폴더에 있는 모든 문서들,
   그리고 나머지 모든 문서들(directive를 포함하고있는 문서를 제외하고) 순으로 포함된다.

   특수 항목 이름 ``self`` 는 toctree directive를 포함하는 문서를 가르킨다. 이것은
   toctree에서 "sitemap"을 생성하고자 할 때 유용하다.

   ``reversed`` 플래그 옵션을 사용하여 목록의 항목 순서를 뒤집을 수 있다. 이것은
   ``glob`` 플래그 옵션을 사용하여 파일의 순서를 뒤집을 때 유용 할 수 있다. 예::

      .. toctree::
         :glob:
         :reversed:

         recipe/*

   다음과 같이 지시문에 "hidden" 옵션을 지정할 수도 있다.

      .. toctree::
         :hidden:

         doc_1
         doc_2

   이것은 Sphinx에 문서 계층을 알리지만 directive의 위치에 문서의 링크를 삽입하지
   않는다. 이러한 링크를 직접, 다른 스타일 또는 HTML 사이드바에 삽입하려는 경우에
   의미가 있다.

   단 하나의 최상위 레벨의 toctree를 가지며 다른 모든 하위 레벨의 toctree를 숨기려면
   최상위 레벨의 toctree 항목에 "includehidden" 옵션을 추가하면 된다.

      .. toctree::
         :includehidden:

         doc_1
         doc_2

   그리고 나서 다른 모든 toctree 항목은 "hidden" 옵션으로 제거 할 수 있다.

   결국, :term:`source directory` (또는 하위 디렉토리들)에 있는 모든 문서들은 적어도
   하나의 ``toctree`` directive 안에 나타나야만 한다. Sphinx는 포함되지 않은 파일을
   발견하면 경고 메시지를 내보낸다. 이는 표준 파일 탐색을 통해 파일에 접근 할 수 없다는
   뜻이기 때문이다.

   문서나 디렉토리를 빌드에서 완전히 제외시키기 위해서는 :confval:`exclude_patterns`
   를 사용한다. 빌드는 하지만 Sphinx에 toctree를 통해 도달 할 수 없다는 것을 알리기
   위해서는 :ref:`the "orphan" metadata <metadata>` 를 사용한다.

   TOC tree의 구조에서 "root"에 해당하는 것은 (:confval:`master_doc` 에 의해 선택 된)
   "master document"이다. 이는 문서의 메인 페이지로 사용하거나, ``maxdepth`` 옵션을
   주지 않고 "전체 목차"로 사용할 수 있다.

   .. versionchanged:: 0.3
      Added "globbing" option.

   .. versionchanged:: 0.6
      Added "numbered" and "hidden" options as well as external links and
      support for "self" references.

   .. versionchanged:: 1.0
      Added "titlesonly" option.

   .. versionchanged:: 1.1
      Added numeric argument to "numbered".

   .. versionchanged:: 1.2
      Added "includehidden" option.

   .. versionchanged:: 1.3
      Added "caption" and "name" option.

특수 이름
-------------

Sphinx는 자체 사용을 위해 특정 문서 이름들을 지정해 놓았으며, 이러한 이름을
사용해서는 안된다.

특수 문서 이름(및 그에 의해 생성 된 페이지)은 다음과 같다:

* ``genindex``, ``modindex``, ``search``

  이들은 각각 general index, Python module index, 검색 페이지에 사용된다.

  General index는 module들의 항목들, index를 생성하는 모든 :ref:`object descriptions
  <basic-domain-markup>`, 그리고 :rst:dir:`index` directive를 포함한다.

  Python module index는 각 :rst:dir:`py:module` directive 당 하나의 항목을 포함한다.

  검색 페이지에는  생성 된 JSON 검색 index와 JavaScript를 사용, 생성 된 문서에 대해
  full-text 검색을 하는 양식이 포함되어 있다. 이는 최신 자바 스크립트를 지원하는 모든
  주요 브라우저에서 지원한다.

* ``_`` 로 시작하는 모든 이름

  현재 Sphinx에서는 이러한 이름을 거의 사용하지 않지만, 이런 이름으로 문서나 문서가
  포함된 폴더를 만들면 안된다. (사용자 정의 템플릿 디렉토리의 접두어로 ``_`` 를 사용하는
  것 제외)

.. warning::

   파일 이름에 특수문자가 있는 경우 조심해라. 일부 형식은 이러한 문자를 잘 못 해석 할
   수도 있다.

   * HTML 기반 형식에 ``:`` 을 사용하면 안된다. 링크가 작동하지 않을 수도 있다.

   * ePub 형식에 ``+`` 를 사용하면 안된다. 일부 자원을 찾지 못할 수도 있다.

.. rubric:: Footnotes

.. [#] LaTeX 작성기는 문서에서 첫번째 toctree directive의 ``maxdepth`` 옵션만을
       참조한다.

.. [#] 사용 가능한 globbing syntax: ``*``, ``?``, ``[...]``, ``[!...]`` 과 같은
       standard shell의 일반적인 구문. 단 슬래시와는 매치되지 않는다. *슬래시* 를 포함한 모든
       문자열을 매치시키는 데는 ``**`` 를 사용한다.
