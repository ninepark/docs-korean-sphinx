소개
============

이 문서는 Sphinx 문서 빌더를 다룬다.
Sphinx 는 reStructuredText_ 소스 파일을 다양한 출력 포맷으로 변환하는 도구로
상호 레퍼런스, 인덱스 등을 자동 생성한다.
이로 인해 디렉토리(하위 디렉토리도 가능)가 reST 포맷 문서 묶음을 포함할 때
Sphinx는 체계적으로 배열된 HTML 파일들을 다른 디렉토리에 생성 브라우징과 네비게이션을 용이하게 한다.
동일한 소스로부터 LaTeX, `rinohtype`_, `rst2pdf`_ 를 사용해 PDF 또한 생성할 수도 있다.

수동으로 작성한 문서를 주로 처리하지만 자동 생성 API 문서와 같이
다른 타입의 문서들도 지원하여 수동으로 작성한 문서들과 쉽게 혼합할 수 있게 고안되었다.
순수 API 문서를 다뤄야 한다면 reST를 지원하는 `Epydoc <http://epydoc.sourceforge.net/>`_ 를 참고하라.

문서 작성 전반에 대한 좋은 소개(목적과 방법)을 위해 Eric Holscher 가 작성한
`Write the docs <http://write-the-docs.readthedocs.org/>`_ 를 참고하라.

.. _rinohtype: https://github.com/brechtm/rinohtype
.. _rst2pdf: https://github.com/rst2pdf/rst2pdf

다른 시스템으로부터 변환
-----------------------------

이 섹션은 다른 문서 시스템에서 reStructuredText/Sphinx 로 전향하고자 하는 사람들에게
좋은 길잡이가 될 수 있는 문서들을 모았다.

* Gerard Flanagan 는 순수 HTML을 reST로 변환하는 스크립트를 작성했다.
  `Python Package Index <https://pypi.python.org/pypi/html2rest>`_ 에서 볼 수 있다.

* 오래된 파이썬 문서를 Sphinx 로 변환하기 위해 작성된 컨버터를
  `the Python SVN repository <http://svn.python.org/projects/doctools/converter>`_ 에서 확인할 수 있다.
  이 컨버터는 파이썬 문서 스타일 LaTex 마크업을 Sphinx reST 로 변환하는 범용 코드를 포함한다.

* Marcin Wojdyr 는 Docbook 을 reST(Sphinx 마크업) 으로 변환하는 스크립트를 작성했다.
  `Google Code <https://github.com/wojdyr/db2rst>`_.

* Christophe de Vienne 는 Open/LibreOffice 문서를 Sphinx 로 변화하는 툴을 작성했다.
  `odt2sphinx <https://pypi.python.org/pypi/odt2sphinx/>`_.

* 다양한 마크업을 변환할 때 `Pandoc <http://pandoc.org/>`_ 이 유용한 툴이다.


다른 시스템과 사용하기
----------------------

:ref:`pertinent section in the FAQ list <usingwith>` 를 보라.


전제
-------------

Sphinx는 구동을 위해 최소한 **Python 2.7** 또는 **Python 3.4** 를
필요로 하며, docutils_ 과 Jinja2_ 라이브러리 또한 필요하다.
Sphinx는 docutils 버전 0.10 또는 손상되지 않은 SVN 트렁크 스냅샷과 함께 사용해야 한다.
소스코드 강조 지원을 원한다면 Pygments_ 라이브러리를 설치해야 한다.

.. _reStructuredText: https://veranostech.github.io/docs-korean-docutils/docutils/docs/ref/rst/introduction_ko.html
.. _docutils: https://veranostech.github.io/docs-korean-docutils/docutils/docs/index.html
.. _Jinja2: http://jinja.pocoo.org/
.. _Pygments: http://pygments.org/


사용
-----

소개를 위해 :doc:`tutorial_ko` 를 보라. 이 문서는 다루고 있는 주제에 대한 심화된 섹션으로의 링크를 포함한다.
