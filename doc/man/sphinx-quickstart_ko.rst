sphinx-quickstart
=================

시놉시스
--------

**sphinx-quickstart**

기술
-----------

:program:`sphinx-quickstart` 는 대화형 툴로 사용자의 프로젝트에 대한 몇가지 질문을 한다.
질문을 마치면 완성된 문서 디렉토리와 :manpage:`sphinx-build(1)` 에 사용될 샘플 Makefile을 생성한다.

옵션
-------

.. program:: sphinx-quickstart

.. option:: -q, --quiet

   Quiet 모드는 옵션을 명시하는 대화 마법사를 생략한다.
   이 옵션은 `-p`, `-a`, `-v` 옵션을 필요로 한다.

.. option:: -h, --help, --version

   Sphinx 버전 또는 사용법 요약을 보여준다.

.. rubric:: Structure Options

.. option:: --sep

   이 옵션이 명시되면 소스를 분리하고 디렉토리를 빌드한다.

.. option:: --dot=DOT

   루트 디렉토리 내부에 2개의 디렉토리가 더 생성된다.
   사용자 정의 HTML 템플릿을 위한 "_templates" 과
   사용자 스타일시트와 다른 정적 파일들을 위한 "_static" 이다.
   디렉토리 앞에 붙는 밑줄("_")을 구두점(".")과 같은 다른 접두사로 대체할 수 있다.

.. rubric:: Project Basic Options

.. option:: -p PROJECT, --project=PROJECT

   프로젝트명을 설정한다. (:confval:`project` 를 보자.).

.. option:: -a AUTHOR, --author=AUTHOR

   저자명. (:confval:`copyright` 를 보자.).

.. option:: -v VERSION

   프로젝트 버전.  (:confval:`version` 를 보자.).

.. option:: -r RELEASE, --release=RELEASE

   프로젝트 배포. (:confval:`release` 를 보자.).

.. option:: -l LANGUAGE, --language=LANGUAGE

   문서 언어. (:confval:`language` 를 보자.).

.. option:: --suffix=SUFFIX

   소스 파일 접미사. (:confval:`source_suffix` 를 보자.).

.. option:: --master=MASTER

   마스터 문서명. (:confval:`master_doc` 를 보자.).

.. option:: --epub

   epub 을 사용한다.

.. rubric:: Extension Options

.. option:: --ext-autodoc

   `sphinx.ext.autodoc` 확장을 사용한다.

.. option:: --ext-doctest

   `sphinx.ext.doctest` 확장을 사용한다.

.. option:: --ext-intersphinx

   `sphinx.ext.intersphinx` 확장을 사용한다.

.. option:: --ext-todo

   `sphinx.ext.todo` 확장을 사용한다.

.. option:: --ext-coverage

   `sphinx.ext.coverage` 확장을 사용한다.

.. option:: --ext-imgmath

   `sphinx.ext.imgmath` 확장을 사용한다.

.. option:: --ext-mathjax

   `sphinx.ext.mathjax` 확장을 사용한다.

.. option:: --ext-ifconfig

   `sphinx.ext.ifconfig` 확장을 사용한다.

.. option:: --ext-viewcode

   `sphinx.ext.viewcode` 확장을 사용한다.

.. option:: --extensions=EXTENSIONS

   임의의 확장을 사용한다.

.. rubric:: Makefile and Batchfile Creation Options

.. option:: --use-make-mode, --no-use-make-mode

   이 옵션에서 지정하는대로 Makefile/make.bat 이 make-mode 를 사용하거나 사용하지 않는다.
   디폴트에서는 make-mode 를 사용한다.

   .. versionchanged:: 1.5
      make-mode 가 디폴트이다.

.. option:: --makefile, --no-makefile

   이 옵션에서 지정하는대로 makefile 을 생성하거나 생성하지 않는다.

.. option:: --batchfile, --no-batchfile

   이 옵션에서 지정하는대로 batchfile 을 생성하거나 생성하지 않는다.

.. rubric:: Project templating

.. versionadded:: 1.5
   sphinx-quickstart 를 위한 프로젝트 템플릿화 옵션.

.. option:: -t, --templatedir=TEMPLATEDIR

   템플릿 파일을 위한 템플릿 디렉토리.
   quickstart에 의해 생성된 sphinx 프로젝트 파일의 템플릿을 수정할 수 있다.
   아래 Jinja2 파일들이 허용된다.:

   * ``master_doc.rst_t``
   * ``conf.py_t``
   * ``Makefile_t``
   * ``Makefile.new_t``
   * ``make.bat_t``
   * ``make.bat.new_t``

   세부적으로는 Sphinx 가 제공하는 시스템 템플릿 파일을 따른다.
   (``sphinx/templates/quickstart``)

.. option:: -d NAME=VALUE

   템플릿 변수를 정의한다.

참고
--------

:manpage:`sphinx-build(1)`
