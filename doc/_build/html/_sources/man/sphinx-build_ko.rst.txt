sphinx-build
============

Synopsis
--------

**sphinx-build** [*options*] <*sourcedir*> <*outputdir*> [*filenames* ...]

Description
-----------

:program:`sphinx-build` 는 ``<sourcedir>`` 의 파일들로부터 문서를 생성하고
이를 ``<outputdir>`` 에 저장한다.

:program:`sphinx-build` 는 ``<sourcedir>/conf.py`` 를 보고 배열 설정을 참고한다.
``conf.py`` 를 포함한 템플릿 파일을 생성하는데 :manpage:`sphinx-quickstart(1)` 가 사용될 수 있다.

:program:`sphinx-build` 는 다른 포맷으로 문서를 생성할 수 있다.
생성할 포맷은 커맨드 라인에 지정된 빌더에 따라 결정된다. 디폴트는 HTML 이다.
빌더는 문서 처리 과정에 따라 다른 작업도 수행할 수 있다.

디폴트에 의해 만료된 것까지 모두 빌드된다.
개별 파일명을 명시하면 선택된 파일만 빌드되게 할 수 있다.

가능한 옵션에 대한 리스트는 :option:`sphinx-build -b` 로 볼 수 있다.

Options
-------

.. program:: sphinx-build

.. option:: -b buildername

   가장 중요한 옵션으로 빌더를 선택한다. 가장 일반적인 빌더들은 아래와 같다.:

   **html**
      HTML 페이지를 빌드한다.  디폴트 빌더이다.

   **dirhtml**
      HTML 페이지를 빌드하지만 문서당 하나의 디렉토리를 생성한다는 점에서 **html** 과 다르다.
      웹서버에 의해 지원된다면 ``.html`` 이 아닌 더 보기 좋은 URL을 만들 수 있다.

   **singlehtml**
      전체 컨텐츠를 하나의 HTML로 빌드한다.

   **htmlhelp**, **qthelp**, **devhelp**, **epub**
      위 포맷들 중 하나로 HTML 파일을 빌드한다.
      빌드 된 파일은 문서 컬렉션을 빌드하기 위한 추가정보를 포함한다.

   **applehelp**
      Apple Help Book을 빌드한다.
      :program:`hiutil` 과 :program:`codesign` 를 요구하며
      이 프로그램들은 현재 맥 OS X 10.6 이상만 지원하고 오픈소스가 아니다.

   **latex**
      LaTex 소스를 빌드한다.
      :program:`pdflatex` 를 사용해 PDF 문서로 컴파일 될 수 있다.

   **man**
      groff 포맷으로 유닉스 시스템의 매뉴얼 페이지를 빌드한다.

   **texinfo**
      Texinfo 파일을 빌드한다.
      :program:`makeinfo` 를 사용해 Info 파일로 처리될 수 있다.

   **text**
      플레인 텍스트 파일을 빌드한다.

   **gettext**
      gettext 스타일 메시지 카탈로그를 빌드한다. (``.pot`` 파일)

   **doctest**
      :mod:`~sphinx.ext.doctest` 확장이 가능할 때 문서 내부의 모든 doctests를 실행한다.

   **linkcheck**
      모든 외부 링크의 완전성을 확인한다.

   **xml**
     Docutils 네이티브 XML 파일을 빌드한다.

   **pseudoxml**
     잘 프린트 된 압축 "pseudo-XML" 파일을 빌드해 중간 문서 계층의 내부구조를 보여준다.

   :ref:`builders` 에서 Sphinx와 함께 제공되는 모든 빌더의 리스트를 볼 수 있다.
   확장이 자체 빌더를 추가할 수 있다.

.. option:: -a

   위 옵션이 주여지면 항상 모든 아웃풋 파일을 작성한다.
   디폴트에서는 변경 되었거나 새로운 소스파일만을 작성한다.
   (빌더에 따라 적용되지 않을 수 있다.)

.. option:: -E

   저장된 :term:`environment` (모든 상호 레퍼런스를 캐싱하는 구조)를 사용하지 않고
   완전히 다시 빌드한다. 디폴트에서는 지난 실행 이후로 변경되거나 새로 나타난 소스 파일만을 읽고 파싱한다.

.. option:: -t tag

   *tag* 태그를 설정한다. 태그가 설정된 내용만을 포함한다는 점에서
   :rst:dir:`only` 명령어와 유사하다.

   .. versionadded:: 0.6

.. option:: -d path

   Sphinx가 출력 파일을 작성하려면 모든 소스파일을 읽고 파싱해야 하기 때문에
   파싱된 소스는 "doctree pickles" 로 캐시된다.
   일반적으로 이 파일들은 빌드 디렉토리 하에 :file:`.doctrees` 디렉토리에 저장된다.
   이 옵션을 사용하면 다른 캐시 디렉토리를 설정할 수 있다.
   (doctrees 는 모든 빌더들 간에 공유될 수 있다.)

.. option:: -j N

   빌드를 *N* 프로세스들 상에 동시에 배포하여 멀티 프로세서 머신의 빌드를 보다 효과적으로 만든다.
   Sphinx의 모든 빌드와 모든 파트가 동시에 가능한 것은 아니니 주의한다.

   .. versionadded:: 1.2
      이 옵션은 *실험적* 이다.

.. option:: -c path

   소스 디렉토리의 :file:`conf.py` 를 보지 않고 주어진 배열 디렉토리를 사용한다.
   배열값에 의해 주어진 다른 파일들과 경로들이 배열 디렉토리 아래에 있을 것으로 예상되므로
   주어진 파일들도 동일한 배열 디렉토리에 있어야 한다.

   .. versionadded:: 0.3

.. option:: -C

   배열 파일을 보지 않는다.; ``-D`` 옵션을 통해서만 사용한다.

   .. versionadded:: 0.5

.. option:: -D setting=value

   :file:`conf.py` 파일에 설정된 배열값을 무시한다.
   값은 반드시 숫자, 문자열, 리스트, 딕셔너리 값이어야 한다.

   리스트라면 ``-D html_theme_path=path1,path2`` 와 같이 콤마로 요소를 구별할 수 있다.

   딕셔너리라면 ``-D latex_elements.docclass=scrartcl`` 와 같이 세팅명과 키를 제공한다.

   부올리언이라면 ``0`` 또는 ``1`` 을 사용한다.

   .. versionchanged:: 0.6
      딕셔너리값이 가능해졌다.

   .. versionchanged:: 1.3
      리스트값이 가능해졌다.

.. option:: -A name=value

   HTMl 템플릿에서 *name* 에 지정될 *value* 를 설정한다.

   .. versionadded:: 0.5

.. option:: -n

   nit-picky 모드를 실행한다. 현재 이 옵션은 모든 레퍼런스 손실에 대해 경고를 생성한다.
   :confval:`nitpick_ignore` 배열값에서 이미 알고 있는 손실을 제외할 수 있다.

.. option:: -N

   컬러 출력을 하지 않는다.

.. option:: -v

   verbosity (로그레벨)을 증가시킨다.
   이 옵션은 디버그 기록 출력을 위해 세번까지 줄 수 있다.
   :option:`-T` 를 포함한다..

   .. versionadded:: 1.2

.. option:: -q

   표준 출력 상의 출력은 출력하지 않는다. 경고와 표준 오류의 오류만을 작성한다.

.. option:: -Q

   표준 출력 상의 출력은 출력하지 않고 경고 또한 무시한다.
   표준 오류의 오류만 작성한다.

.. option:: -w file

   주어진 파일과 표준 오류의 경고(오류 포함)을 작성한다.

.. option:: -W

   경고를 오류로 변환한다. 이 때 빌드는 첫번째 경고에서 멈추고
   ``sphinx-build`` 가 exit status 1로 종료된다.

.. option:: -T

   처리되지 않은 예외가 발생했을 때 모든 트레이스백(traceback)을 보여준다.
   또는 트레이스백 정보는 추가 분석을 위해 파일로 저장되고 요약만을 보여준다.

   .. versionadded:: 1.2

.. option:: -P

   (디버깅에서만 유용.) 빌딩 중 처리되지 않은 예외가 발생했을 때 파이썬 디버거(:mod:`pdb`) 를 실행한다.

.. option:: -h, --help, --version

   Sphinx 버전 또는 사용법 요약을 보여준다.

   .. versionadded:: 1.2

소스와 빌드 디렉토리 다음 커맨드 라인에 하나 이상의 파일명을 부여할 수 있다.
이 때 Sphinx는 부여된 출력 파일과 그 종속 파일 만을 빌드하려 할 것이다.

환경 변수
---------------------

:program:`sphinx-build` 는 아래 환경 변수를 적용한다.

.. describe:: MAKE

   make 커맨드로의 경로. 커맨드명도 허용됨.
   :program:`sphinx-build` 는 이 변수를 사용해 make-mode 상의 하위 빌드 프로세스를 불러온다.

.. rubric:: Makefile Options

:program:`sphinx-quickstart` 에 의해 생성된 :file:`Makefile` 과 :file:`make.bat` 은
일반적으로 :option:`-b`, :option:`-d` 옵션에서만 :program:`sphinx-build` 를 실행한다.
그러나 위 파일들은 사용자 지정에 따라 아래 변수들을 지원한다.

.. describe:: PAPER

   :confval:`latex_elements` 의 '"papersize"` 키가 갖는 값.

.. describe:: SPHINXBUILD

   ``sphinx-build`` 대신 사용되는 커맨드.

.. describe:: BUILDDIR

   :program:`sphinx-quickstart` 에서 지정된 것 대신 사용할 빌드 디렉토리.

.. describe:: SPHINXOPTS

   :program:`sphinx-build` 의 추가 옵션.

.. _when-deprecation-warnings-are-displayed:

반대 경고
--------------------

사용자의 문서를 빌드할 때 ``RemovedInSphinxXXXWarning`` 와 같은 반대 경고가 나타난다면
Sphinx 확장이 미래엔 배제될 기능을 사용하는 것이다. 그러한 상황에서는 확장 저자에게 문의하길 바란다.

사용자의 환경에서 ``PYTHONWARNINGS=`` 환경 변수를 설정해 반대 경고를 비활성화 할 수 있다. 예시:

* ``PYTHONWARNINGS= make html`` (Linux/Mac)
* ``export PYTHONWARNINGS=`` and do ``make html`` (Linux/Mac)
* ``set PYTHONWARNINGS=`` and do ``make html`` (Windows)
* modify your Makefile/make.bat and set the environment variable

참고
--------

:manpage:`sphinx-quickstart(1)`
