.. highlight:: rst

Sphinx 첫걸음
=======================

이 문서는 Sphinx 를 사용하면서 다루는 일반적인 작업에 대한 튜토리얼 수준의 개요를 제공하기 위해 작성되었다.

초록색 화살표는 작업과 관련된 심화 정보로의 링크를 가리킨다.


Sphinx 설치
--------------

배포된 패키지 또는 아래 코드(`PyPI <https://pypi.python.org/pypi/Sphinx>`_)를 이용해 Sphinx 를 설치한다. ::

   $ pip install Sphinx


문서화 소스 설정
------------------------------------

reStructuredText 문서 소스의 Sphinx 모음으로의 루트 디렉토리를 :term:`source directory` (소스 디렉토리) 라 한다.
이 디렉토리는 Sphinx 배열 파일(:file:`conf.py`) 을 포함한다.
Sphinx 배열 파일에서 Sphinx 가 소스를 읽고 문서를 빌드하는 방식에 대한 모든 사항을 설정할 수 있다.

Sphinx 는 :program:`sphinx-quickstart` 스크립트를 지원한다.
이 스크립트는 몇가지 질문을 통해 유용한 배열값이 담긴 디폴트 :file:`conf.py` 과 소스 디렉토리를 설정한다.
아래와 같이 실행한다. ::

   $ sphinx-quickstart

실행되는 질문들에 답하면 된다. ("autodoc" 확장에 yes 를 꼭 입력하자.)

자동 API 문서 생성기인 :program:`sphinx-apidoc` 도 있다. ;
:doc:`/man/sphinx-apidoc` 에서 이에 대한 세부사항을 볼 수 있다.


문서 구조 정의
---------------------------

:program:`sphinx-quickstart` 를 실행하면 :file:`conf.py` ,
마스터 문서, :file:`index.rst` 를 포함하는 소스 디렉토리가 생성된다.
:term:`master document` 의 주된 기능은 시작 페이지를 제공하고
컨텐츠 계층 테이블의 출처(또는 *toctree* ) 를 포함하는 것이다.
이것이 Sphinx 가 reStructuredText 에 추가하는 가장 주된 요소이며
이를 통해 단일 계층 문서들로부터 여러 파일로의 연결이 가능해진다.

.. sidebar:: reStructuredText 명령어

   ``toctree`` 는 여러 조각의 마크업인 reStructuredText :dfn:`directive` (명령어) 이다.
   명령어는 인수, 옵션, 내용을 가진다.

   *인수* 는 명령어명를 잇는 더블 콜론 뒤에 주어진다.
   명령어의 인수 포함 여부와 그 갯수는 명령어에 따라 다르다.

   *옵션* 은 필드 리스트의 형태로 인수 뒤에 주어진다.
   ``toctree`` 명령어의 옵션은 ``maxdepth`` 이다.

   *내용* 은 옵션 또는 인수 뒤에 오는 공백행 이후에 온다.
   명령어가 내용을 갖는지는 명령어에 따라 내용의 쓰임새에 따라 다르다.

   일반적으로 명령어에 대해 알아야 할 것은
   **내용의 첫줄은 옵션과 동일하게 들여써야 한다** 는 것이다.


명령어는 일단 비어있고 하고 아래와 같은 형태이다. ::

   .. toctree::
      :maxdepth: 2

명령어 *내용* 에 문서 목록을 추가한다. ::

   .. toctree::
      :maxdepth: 2

      intro
      tutorial
      ...

위 예시는 이 문서의 toctree 와 동일한 형태이다.
포함할 문서는 :term:`document name` 로 주어진다.
사용자는 파일 확장자명을 생략하고 디렉토리 분리를 위해 슬래시를 사용한다.

|more| :ref:`the toctree directive <toctree-directive>` (toctree 명령어)에 대해 더 알아보기.

이제 toctree 에 목록화한 파일들을 생성하고 내용을 작성할 수 있고
toctree 명령어가 위치한 곳에 파일의 섹션명이 삽입된다. ("maxdepth" 계층에 맞추어)
추가로 Sphinx는 이제 문서의 계층과 순서를 알고 있다.
(파일들은 스스로 ``toctree`` 명령어를 포함할 수 있다. 필요하다면 이를 이용해 더 깊은 계층이 내포된 문서를 생성할 수 있다.)


목차 추가
--------------

Sphinx 소스 파일에서 표준 reStructuredText의 기능을 대부분 사용할 수 있고
Sphinx에 의해 추가된 몇몇 기능들이 있다.
예를 들어, 모든 파일 레퍼런스를 :rst:role:`ref` 기능을 사용해 간편하게 추가할 수 있다.

HTML 버전을 보고 있다면 문서의 소스를 사이드바의 "Show Source" 링크를 사용해 볼 수 있다.

|more| :ref:`rst-primer` 에서 reStructuredTex 에 대한 상세한 개요를,
:ref:`sphinxmarkup` 에서 Sphinx 가 추가한 마크업의 전체 목록을 볼 수 있다.


빌드 실행
-----------------

위에서 몇몇 파일과 목차를 추가했다. 문서의 첫번째 빌드를 만들어 보자.
:program:`sphinx-build` 로 빌드를 시작한다. 아래와 같이 실행한다. ::

   $ sphinx-build -b html sourcedir builddir

*sourcedir* 은 :term:`source directory`(소스 디렉토리) 이고 *builddir* 은 빌드될 문서가 위치할 곳이다.
:option:`-b <sphinx-build -b>` 옵션은 빌더를 지정한다. 위 예시에서는 HTML 파일을 빌드한다.

|more| :program:`sphinx-build` 가 지원하는 모든 옵션은
:program:`sphinx-build man page <sphinx-build>` 를 참고한다.

그러나 :program:`sphinx-quickstart` 스크립트는 :file:`Makefile` 과 :file:`make.bat` 를 생성해
작업이 더 쉬워진다. 아래와 같이 실행하면 된다. ::

   $ make html

지정한 빌드 디렉토리에서 HTML 문서를 빌드하려면
가능한 대상을 보여주는 인수 없이 ``make`` 를 실행하면 된다.

.. admonition:: PDF 문서를 생성하려면?

   ``make latexpdf`` 는 :mod:`LaTeX builder <sphinx.builders.latex.LaTeXBuilder>` 를
   실행해 손쉽게 pdfTex toolchain 을 불러온다.


문서화 대상
-------------------

Sphinx의 주된 목표 중 하나는 어떤 :dfn:`domain` (도메인)에서도
쉽게 :dfn:`objects` (넓은 의미에서)를 문서화하는 것이다.
도메인이란 마크업과 레퍼런스 서술이 완비되어 있으며 동일하게 속하는 객체 타입의 모임이다.

가장 대표적인 도메인은 파이썬 도메인이다.
예시 문서로서 파이썬의 기본 제공 함수 ``enumerate()`` 를 들자면
아래 파일을 소스 파일 중 하나에 추가했을 것이다. ::

   .. py:function:: enumerate(sequence[, start=0])

      Return an iterator that yields tuples of an index and an item of the
      *sequence*. (And so on.)

위 파일은 아래와 같이 구현된다.:

.. py:function:: enumerate(sequence[, start=0])

   Return an iterator that yields tuples of an index and an item of the
   *sequence*. (And so on.)

이 명령어의 인수는 나타내고자 하는 객체의 :dfn:`signature` 로 내용은 위 파일의 내용과 같다.
여러개의 signature 를 각각의 라인에 줄 수 있다.

파이썬 도메인은 디폴트 도메인이 되기도 하며 도메인명 마크업을 붙일 필요가 없다. ::

   .. function:: enumerate(sequence[, start=0])

      ...

디폴트 도메인의 디폴트 설정을 사용에서 위 예시는 동일하게 작동한다.

:rst:dir:`py:class` 나 :rst:dir:`py:method` 와 같이
다른 타입의 파이썬 객체를 문서화 하기 위한 여려 명령어들이 있다.
각각의 객체 타입을 위한 상호 레퍼런스(:dfn:`role`) 도 있다.
아래 마크업은 ``enumerate()`` 의 문서로 링크를 생성한다. ::

   The :py:func:`enumerate` function can be used for ...

위 예시의 증명: :func:`enumerate` 로의 링크

앞서 언급했듯이 디폴트가 파이썬 도메인이라면 ``py:`` 는 생략해도 된다.
어떤 파일이 ``enumerate()`` 의 실제 문서를 포함하는지는 중요치 않다.
Sphinx 가 파일을 찾아내 링크를 생성할 것이다.

signature 형태, 포맷화된 출력을 보기 좋게 하는 방법,
매개변수 타입으로의 링크와 같은 특정 기능을 추가하는 방법 등은
도메인마다 특별한 규칙을 가질 수 있다. (C/C++ 도메인 등)
도메인에 따라 signature 형태, 포맷화된 출력 대한 특별한 체계를 가질 수 있다.

|more| :ref:`domains` 에서 사용 가능한 모든 도메인과 명령어 규칙을 볼 수 있다.


기본 배열
-------------------

앞서 우리는 Sphinx가 문서를 처리하는 방식을 결정하는 :file:`conf.py` 파일에 대해 다뤘다.
파이썬 소스 파일과 같이 실행되는 :file:`conf.py` 파일에서 사용자는 배열값을 결정한다.
고급 사용자 : 이 파일은 Sphinx에 의해 실행되므로 파일 내부에서 :data:`sys.path` 를 확장하거나
문서화하는 버전을 확인하는 모듈을 가져오는 것과 같이 중요한 작업들을 할 수 있다.

변경하고자 하는 배열값이 이미 :program:`sphinx-quickstart` 에 의해
:file:`conf.py` 에 입력되어 주석처리 됐을 것이다.
(표준 파이선 문법으로: ``#`` 가 나머지 라인을 주석처리 한다.)
디폴트값을 변경하려면 해시 기호를 지우고 값을 변경한다.
:program:`sphinx-quickstart` 가 자동으로 추가하지 않은 배열값을 변경하려면 추가 지정을 하면 된다.

파일이 문자열, 숫자, 리스트 등의 파이썬 문법을 사용한다는 것을 숙지하자.
첫줄의 인코딩 선언이 디폴트라면 파일은 UTF-8 로 저장된다.
어떤 문자열에서든 ASCII 가 아닌 문자를 사용한다면 파이썬 유니코드 문자열을 사용해야 한다.
(예시 ``project = u'Exposé'``).

|more| 가능한 모든 배열값에 대한 문서 :ref:`build-config` .


Autodoc
-------

파이썬 코드를 문서화 할 때 소스 파일에 많은 문서를 사용하는 것은 흔한 일이다.
Sphinx는 :dfn:`extension` 로 모듈로부터 가져온 docstring의 포함을 지원하며 이를 "autodoc" 이라 한다.
(extension 이란 Sphinx 를 위한 추가 기능을 제공하는 파이썬 모듈이다.)

autodoc을 사용하려면 :file:`conf.py` 에서 autodoc 을 활성화 해야 한다.
:confval:`extensions` 배열값이 지정될 리스트에 ``'sphinx.ext.autodoc'`` 문자열을 입력해 활성화 할 수 있다.
추가로 사용자의 재량에 따라 몇가지 명령어가 있다.

예를 들어, ``io.open()`` 함수를 문서화 하려면 소스파일로부터 함수의 signature 와 docstring 을 읽어야 한다.
아래와 같이 입력한다. ::

   .. autofunction:: io.open

모든 클래스 혹은 모듈까지도 자동으로 문서화 할 수 있다.
자동 명령어를 위해 아래와 같이 member 옵션을 사용한다. ::

   .. automodule:: io
      :members:

autodoc 은 docstring 을 추출하기 위해 모듈을 가져와야 한다.
따라서 :file:`conf.py` 내부의 :py:data:`sys.path` 로의 경로를 알맞게 추가해야 한다.

.. warning::

   :mod:`~sphinx.ext.autodoc` 는 문서화 될 모듈을 **가져온다**.
   어떤 모듈이던 가져오는 과정에서 부작용이 있다면
   ``sphinx-build`` 를 실행할 때 ``autodoc`` 에 의해 부작용도 실행된다.

   스크립트를 문서화 한다면 (라이브러리 모듈과 반대로)
   스크립트의 주요 루틴이 ``if __name__ == '__main__'`` 조건에 의해 보호되는지 확인해야 한다.

|more| autodoc 의 기능에 대한 완전한 설명은 :mod:`sphinx.ext.autodoc` 를 보라.

Intersphinx
-----------

`Python documentation`_ 을 포함하는 다수의 Sphinx 문서는 인터넷 상에서 발행된다.
당신의 문서에서 인터넷에 발행된 문서로의 링크를 만들고 싶다면 :mod:`sphinx.ext.intersphinx` 를 사용하면 된다.

.. _Python documentation: https://docs.python.org/3

intersphinx 을 사용하려면 :file:`conf.py` 에서 intersphinx 을 활성화 해야 한다.
:confval:`extensions` 리스트에 ``'sphinx.ext.intersphinx'`` 문자열을 입력하여
:confval:`intersphinx_mapping` 배열값을 설정함으로써 intersphinx 를 활성화 할 수 있다.

예를 들어 파이썬 라이브러리 매뉴얼에 ``io.open()`` 를 링크하고 싶다면
아래와 같이 :confval:`intersphinx_mapping` 설정한다. ::

   intersphinx_mapping = {'python': ('https://docs.python.org/3', None)}

이제 ``:py:func:`io.open``` 과 같이 상호 레페런를 작성할 수 있다.
어떤 상호 레퍼런스건 현재 문서 세트에서 알맞은 대상을 찾을 수 없다면
:confval:`intersphinx_mapping` 에 설정된 문서를 보게 된다.
(이 과정은 유효한 대상의 리스트를 받기 위한 URL 로의 접근을 필요로 한다.)
Intersphinx 는 ``:ref:`` 를 포함한 다른 :ref:`domains' <domains>` 기능에서도 작동하지만
``:doc:`` 과 같은 도메인이 없는 기능에서는 작동하지 않는다.

|more| intersphinx 의 기능에 대한 완전한 설명은 :mod:`sphinx.ext.intersphinx` 를 보라.


더 다룰 내용들
-------------------------

- :doc:`Other extensions <extensions>`:

  * :doc:`ext/math`,
  * :doc:`ext/viewcode`,
  * :doc:`ext/doctest`,
  * ...
- 정적 파일
- :doc:`Selecting a theme <theming>`
- :doc:`setuptools`
- :ref:`Templating <templating>`
- 확장의 사용
- :ref:`Writing extensions <dev-extensions>`


.. rubric:: Footnotes

.. [#] 이것은 일반적인 레이아웃이지만 :term:`configuration directory` 라는
       :file:`conf.py` 는 다른 디렉토리에도 둘 수 있다.
       :program:`sphinx-build man page <sphinx-build>` 에서 더 많은 정보를 얻을 수 있다.

.. |more| image:: more.png
          :align: middle
          :alt: more info
