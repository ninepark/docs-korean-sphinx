.. _builders:

Available builders
==================

.. module:: sphinx.builders
   :synopsis: Available built-in builder classes.

이것들은 빌트인(built-in) 스핑크스(Sphinx) 빌더(builders)이다.
:ref:`extensions <extensions>` 를 통해서 빌더를 추가할 수 있다.

빌더를 선택하기 위해서는 빌더의 이름이 :program:`sphinx-build` 의 **-b**
커맨드라인 옵션에 주어져야 한다.


.. module:: sphinx.builders.html
.. class:: StandaloneHTMLBuilder

   이것은 표준 HTML 빌더다. 이 빌더의 출력물은 스타일 시트와(style sheets)와
   추가적인 reST 소스가 완비된 HTML 파일이 있는 디렉토리다. 빌더의 출력물을
   커스터마이즈할 수 있는 설정 값들이 꽤 많이 존재한다. 자세한 내용은
   :ref:`html-options` 챕터를 참고하라.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

.. class:: DirectoryHTMLBuilder

   이것은 표준 HTML 빌더의 하위 클래스다. 이것의 출력물은 페이지 이름과
   같은 이름의 하위 디렉토리에 있으면서 ``index.html`` 로 불리는 HTML
   파일들이 있는 디렉토리다. 예를 들어 ``markup/rest.rst`` 문서는
   출력 파일``markup/rest.html``을 생성하지 않고 ``markup/rest/index.html``
   을 생성한다. 페이지 간의 링크를 생성할 때 ``index.html`` 는 제외 돼서
   URL은 ``markup/rest/`` 같이 보인다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 0.6

.. class:: SingleFileHTMLBuilder

   이것은 모든 프로젝트를 하나의 출력 파일로 결합시켜주는 HTML 빌더다.
   (이 것은 오로지 작은 프로젝트로 이루어 졌을 때만 작동한다. 파일 이름은
   마스터 문서와 같다. 색인은 생성되지 않는다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.0

.. module:: sphinx.builders.htmlhelp
.. class:: HTMLHelpBuilder

   이 빌더는 자립형(standalone) HTML 빌더와 동일한 출력물을 생성하지만
   Microsoft HTML Help Workshop이 출력물을 CHM 파일로
   컴파일 할 수 있게 해주는 HTML Help 지원 파일도 함께 생성한다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

.. module:: sphinx.builders.qthelp
.. class:: QtHelpBuilder

   이 빌더는 자립형 HTML 빌더와 같은 출력물을 생성하지만
   Qt collection generator가 출력물을 컴파일 하게 해주는 `Qt help`_
   collection 지원 파일도 함께 생성한다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. _Qt help: https://doc.qt.io/qt-4.8/qthelp-framework.html

.. module:: sphinx.builders.applehelp
.. class:: AppleHelpBuilder

   이 빌더는 자립형 HTML 빌더와 같은 출력물을 기반으로하는 Apple Help Book
   을 생성한다.

   만약 소스 디렉토리가 ``.lproj`` 폴더를 포함하고 있다면, 선택한 언어에
   해당하는 것의 내용이 생성된 출력물과 병합될 것이다. 이 폴더는 다른 모든
   도큐먼테이션 타입에서 무시된다.

   유효한 help book을 생성하기 위해서 이 빌더는 커맨드 라인 툴(tool)
   :program:`hiutil` 을 필요로 하며 이 툴은 Mac OS X 10.6 이상의
   버전에서만 사용 가능하다. :confval:`applehelp_disable_external_tools`
   을 ``True``로 설정해서 인덱싱 단계를 비활성화할 수 있으며 이 경우
   출력물은 번들에 있는 ``.lproj`` 폴더의 모든 파일에 대해 :program:`hiutil`
   이 실행되기 전까지는 유효하지 않다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.3

.. module:: sphinx.builders.devhelp
.. class:: DevhelpBuilder

   이 빌더는 자립형 HTML 빌더와 같은 출력물을 생성하지만 GNOME Devhelp reader로
   볼 수 있게 해주는 <https://wiki.gnome.org/Apps/Devhelp>`__ 지원
   파일도 함께 생성한다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

.. module:: sphinx.builders.epub3
.. class:: Epub3Builder

   이 빌더는 자립형 HTML 빌더와 같은 출력물을 생성하지만 ebook reader을 위한
   *epub* 파일도 함께 생성한다. 자세한 내용은 :ref:`epub-faq` 를 참고하라.
   epub 포맷의 정의에 대해서는 `<http://idpf.org/epub>`_ 나
   `<https://en.wikipedia.org/wiki/EPUB>`_ 를 참고하라.
   이 빌더는 *EPUB 3* 파일을 생성한다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.4

   .. versionchanged:: 1.5

      Sphinx-1.5 이후부터, epub3 빌더는 epub의 기본 빌더로 사용된다.

.. module:: sphinx.builders.latex
.. class:: LaTeXBuilder

   이 빌더는 출력물 디렉토리에 LaTeX을 생성한다. :confval:`latex_documents`
   설정 값을 통해서 LaTeX 파일에 포함되어야 하는 문서를 지정해야 한다.
   몇가지 빌더의 출력물을 커스터마이즈 할 수 있는 설정값이 존재한다.
   자세한 사항은 :ref:`latex-options` 챕터를 확인하라.

   .. note::

      생성된 LaTeX 파일은 "최소" Tex 배포 설치 파일에 존재하지 않는
      여러 LaTeX 패키지를 사용한다. 예를 들어, Ubuntu에서는 성공적인 PDF
      빌드를 위해 아래의 패키지를 설치해야 될 필요가 있다:

      * texlive-latex-recommended
      * texlive-fonts-recommended
      * texlive-latex-extra
      * latexmk (for ``make latexpdf``)

      Sphinx는 존재한다면 ``xcolor.sty`` 를 사용할 것이다: 최근의 Ubuntu
      배포판에는 latex-recommended에 ``xcolor.sty`` 이 포함되어 있고
      이전의 배포판에는 latex-xcolor에 포함되어 있다. 유니코드 인젠은
      texlive-luatex 또는 texlive-xetex를 필요로 한다.

      Sphinx LaTeX의 테스트는 위에 언급된 패키지로 우분투 trusty에서 시행되었고
      위의 패키지들은 2014년 2월의 TeXLive 2013 스냅샷(snapshot)이다.

      .. versionchanged:: 1.6
         이전에는, 몇 년 동안 TeXLive 2009에 기반한 Ubuntu precise 에서
         테스트가 이루어졌다.
      .. versionchanged:: 1.6
         GNU/Linux와 Mac OS X에서 ``make latexpdf`` 를 위해 ``latexmk``
         사용

      1.6 이후로, ``make latexpdf`` (또는 ``sphinx-build`` 실행 이후
      ``make -C "<builddir>/latex"``) 는 ``latexmk`` 를 사용한다
      (윈도우는 아님). ``LATEXMKOPTS``Makefile 변수를 통해서 ``latexmk``
      options에 전달할 수 있다:

      .. code-block:: console

         make latexpdf LATEXMKOPTS="-silent"

      위 명령은 콘솔 출력을 최소로 줄여준다. 또한 ``latexmk`` 버전이 4.52b
      이상이고 ``xelatex`` 이 :confval:`latex_engine` 이면,
      ``LATEXMKOPTS="-xelatex"`` 는 PDF 빌드 속도를 향상시킬 것이다.

      옵션을 ``(pdf|xe|lua)latex`` 실행파일에 직접 전달하고 싶다면,
      ``LATEXOPTS`` 변수를 사용하라. (예,
      ``LATEXOPTS="--interaction=nonstopmode"``)

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

직접적인 PDF 빌더는 `rinohtype`_ 에 의해 제공된다는 점을 참고하라. 빌더의
이름은 ``rinoh`` 이다. 자세한 사항은 `rinohtype manual`_ 를 참고하라.
0.12 이상의 `rst2pdf`_ 에 있는 ReportLab을 사용하는 빌더도 있다.
그러나, rst2pdf는 더이상 적극적으로 유지되지 않으며 최근의 Sphinx 버전에서
사용될 때 몇 가지 문제가 있다. 사용 방법은 `rst2pdf manual`_ 를 참고하라.

.. _rinohtype: https://github.com/brechtm/rinohtype
.. _rinohtype manual: http://www.mos6581.org/rinohtype/quickstart.html#sphinx-builder
.. _rst2pdf: https://github.com/rst2pdf/rst2pdf
.. _rst2pdf manual: http://ralsina.me/static/manual.pdf

.. module:: sphinx.builders.text
.. class:: TextBuilder

   이 빌더는 각 reST파일에 대한 텍스트 파일을 생성한다. 파일은 reST 원본과
   거의 동일하지만 가독성을 위해 대부분의 마크업이 제거되어 있다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 0.4

.. module:: sphinx.builders.manpage
.. class:: ManualPageBuilder

   이 빌더는 groff 포맷의 manual pages를 생성한다. :confval:`man_pages`
   설정 값을 통해서 어떤 문서가 어떤 manual page에 포함되어야 할지를
   지정해야 한다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.0


.. module:: sphinx.builders.texinfo
.. class:: TexinfoBuilder

   이 빌더는 :program:`makeinfo` 프로그램에 의해 Info 파일로 처리되는
   Texinfo 파일을 생성한다. :confval:`texinfo_documents` 설정 값을 통해서
   어떤 문서가 어떤 Texinfo 파일에 포함되어야 하는지 지정해야 한다.

   Info 포맷은 GNU Emacs와 터미널 기반 프로그램 :program:`info` 에서
   사용되는 온라인 도움말 시스템의 기본이다. 자세한 내용은 :ref:`texinfo-faq`
   를 참고하라. Texinfo 포맷은 GNU 프로젝트에서 사용되는 공식 문서화 시스템이다.
   Texinfo에 대핸 자세한 내용은 `<https://www.gnu.org/software/texinfo/>`_
   에서 찾아볼 수 있다.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.1


.. currentmodule:: sphinx.builders.html
.. class:: SerializingHTMLBuilder

   This builder uses a module that implements the Python serialization API
   (`pickle`, `simplejson`, `phpserialize`, and others) to dump the generated
   HTML documentation.  The pickle builder is a subclass of it.

   A concrete subclass of this builder serializing to the `PHP serialization`_
   format could look like this::

        import phpserialize

        class PHPSerializedBuilder(SerializingHTMLBuilder):
            name = 'phpserialized'
            implementation = phpserialize
            out_suffix = '.file.phpdump'
            globalcontext_filename = 'globalcontext.phpdump'
            searchindex_filename = 'searchindex.phpdump'

   .. _PHP serialization: https://pypi.python.org/pypi/phpserialize

   .. attribute:: implementation

      A module that implements `dump()`, `load()`, `dumps()` and `loads()`
      functions that conform to the functions with the same names from the
      pickle module.  Known modules implementing this interface are
      `simplejson`, `phpserialize`, `plistlib`, and others.

   .. attribute:: out_suffix

      The suffix for all regular files.

   .. attribute:: globalcontext_filename

      The filename for the file that contains the "global context".  This
      is a dict with some general configuration values such as the name
      of the project.

   .. attribute:: searchindex_filename

      The filename for the search index Sphinx generates.


   See :ref:`serialization-details` for details about the output format.

   .. versionadded:: 0.5

.. class:: PickleHTMLBuilder

   This builder produces a directory with pickle files containing mostly HTML
   fragments and TOC information, for use of a web application (or custom
   postprocessing tool) that doesn't use the standard HTML templates.

   See :ref:`serialization-details` for details about the output format.

   .. autoattribute:: name

      The old name ``web`` still works as well.

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   The file suffix is ``.fpickle``.  The global context is called
   ``globalcontext.pickle``, the search index ``searchindex.pickle``.

.. class:: JSONHTMLBuilder

   This builder produces a directory with JSON files containing mostly HTML
   fragments and TOC information, for use of a web application (or custom
   postprocessing tool) that doesn't use the standard HTML templates.

   See :ref:`serialization-details` for details about the output format.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   The file suffix is ``.fjson``.  The global context is called
   ``globalcontext.json``, the search index ``searchindex.json``.

   .. versionadded:: 0.5

.. module:: sphinx.builders.gettext
.. class:: MessageCatalogBuilder

   This builder produces gettext-style message catalogs.  Each top-level file or
   subdirectory grows a single ``.pot`` catalog template.

   See the documentation on :ref:`intl` for further reference.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.1

.. module:: sphinx.builders.changes
.. class:: ChangesBuilder

   This builder produces an HTML overview of all :rst:dir:`versionadded`,
   :rst:dir:`versionchanged` and :rst:dir:`deprecated` directives for the
   current :confval:`version`.  This is useful to generate a ChangeLog file, for
   example.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

.. module:: sphinx.builders.dummy
.. class:: DummyBuilder

   This builder produces no output.  The input is only parsed and checked for
   consistency.  This is useful for linting purposes.

   .. autoattribute:: name

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.4

.. module:: sphinx.builders.linkcheck
.. class:: CheckExternalLinksBuilder

   This builder scans all documents for external links, tries to open them with
   :mod:`urllib2`, and writes an overview which ones are broken and redirected
   to standard output and to :file:`output.txt` in the output directory.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

.. module:: sphinx.builders.xml
.. class:: XMLBuilder

   This builder produces Docutils-native XML files.  The output can be
   transformed with standard XML tools such as XSLT processors into arbitrary
   final forms.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.2

.. class:: PseudoXMLBuilder

   This builder is used for debugging the Sphinx/Docutils "Reader to Transform
   to Writer" pipeline. It produces compact pretty-printed "pseudo-XML", files
   where nesting is indicated by indentation (no end-tags). External
   attributes for all elements are output, and internal attributes for any
   leftover "pending" elements are also given.

   .. autoattribute:: name

   .. autoattribute:: format

   .. autoattribute:: supported_image_types

   .. versionadded:: 1.2


Built-in Sphinx extensions that offer more builders are:

* :mod:`~sphinx.ext.doctest`
* :mod:`~sphinx.ext.coverage`


.. _serialization-details:

Serialization builder details
-----------------------------

All serialization builders outputs one file per source file and a few special
files.  They also copy the reST source files in the directory ``_sources``
under the output directory.

The :class:`.PickleHTMLBuilder` is a builtin subclass that implements the pickle
serialization interface.

The files per source file have the extensions of
:attr:`~.SerializingHTMLBuilder.out_suffix`, and are arranged in directories
just as the source files are.  They unserialize to a dictionary (or dictionary
like structure) with these keys:

``body``
   The HTML "body" (that is, the HTML rendering of the source file), as rendered
   by the HTML translator.

``title``
   The title of the document, as HTML (may contain markup).

``toc``
   The table of contents for the file, rendered as an HTML ``<ul>``.

``display_toc``
   A boolean that is ``True`` if the ``toc`` contains more than one entry.

``current_page_name``
   The document name of the current file.

``parents``, ``prev`` and ``next``
   Information about related chapters in the TOC tree.  Each relation is a
   dictionary with the keys ``link`` (HREF for the relation) and ``title``
   (title of the related document, as HTML).  ``parents`` is a list of
   relations, while ``prev`` and ``next`` are a single relation.

``sourcename``
   The name of the source file under ``_sources``.

The special files are located in the root output directory.  They are:

:attr:`.SerializingHTMLBuilder.globalcontext_filename`
   A pickled dict with these keys:

   ``project``, ``copyright``, ``release``, ``version``
      The same values as given in the configuration file.

   ``style``
      :confval:`html_style`.

   ``last_updated``
      Date of last build.

   ``builder``
      Name of the used builder, in the case of pickles this is always
      ``'pickle'``.

   ``titles``
      A dictionary of all documents' titles, as HTML strings.

:attr:`.SerializingHTMLBuilder.searchindex_filename`
   An index that can be used for searching the documentation.  It is a pickled
   list with these entries:

   * A list of indexed docnames.
   * A list of document titles, as HTML strings, in the same order as the first
     list.
   * A dict mapping word roots (processed by an English-language stemmer) to a
     list of integers, which are indices into the first list.

``environment.pickle``
   The build environment.  This is always a pickle file, independent of the
   builder and a copy of the environment that was used when the builder was
   started.

   .. todo:: Document common members.

   Unlike the other pickle files this pickle file requires that the ``sphinx``
   package is available on unpickling.
