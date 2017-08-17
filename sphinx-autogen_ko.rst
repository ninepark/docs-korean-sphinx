sphinx-autogen
==============

시놉시스
--------

**sphinx-autogen** [*options*] <sourcefile> ...

기술
-----------

:program:`sphinx-autogen` 은 Sphinx 소스를 자동생성하는 툴이다.
생성된 소스는 :rst:dir:`autodoc` 확장을 사용하며
:rst:dir:`autosummary` 리스트에 포함된 항목들을 문서화 한다.

*sourcefile* 은 :rst:dir:`autosummary` 항목과 ``:toctree::`` 옵션 세트를 포함하는
하나 이상의 reStructuredText 문서로의 경로이다.
*sourcefile* 은 :py:module:`fnmatch` 스타일 패턴이 될 수 있다.

옵션
-------

.. program:: sphinx-autogen

.. option:: -o <outputdir>

   출력파일을 저장할 디렉토리. 존재하지 않는 디렉토리면 생성된다.
   디폴트는 ``:toctree:`` 옵션으로 전달된 값이다.

.. option:: -s <suffix>, --suffix <suffix>

   생설될 소스 파일의 확장자. 디폴트는 ``rst``.

.. option:: -t <templates>, --templates <templates>

   사용자 지정 템플릿의 디렉토리. 디폴트는 ``None``.

.. option:: -i, --imported-members

   가져온 멤버를 문서화 한다.

예시
-------

아래와 같은 디렉토리 구조가 주어졌다.::

    docs
    ├── index.rst
    └── ...
    foobar
    ├── foo
    │   └── __init__.py
    └── bar
        ├── __init__.py
        └── baz
            └── __init__.py

``docs/index.rst`` 이 아래 모듈을 포함한다고 하자.:

.. code-block:: rst

    Modules
    =======

    .. autosummary::
       :toctree: modules

       foobar.foo
       foobar.bar
       foobar.bar.baz

아래 코드를 실행하면:

.. code-block:: bash

    $ sphinx-autodoc doc/index.rst

``docs`` 에 아래 스텁 파일들이 생성된다.::

    docs
    ├── index.rst
    └── modules
        ├── foobar.bar.rst
        ├── foobar.bar.baz.rst
        └── foobar.foo.rst

각각의 생성된 파일들은 :rst:dir:`autodoc` 명령어와 다른 몇가지 정보를 포함한다.

참고
--------

:manpage:`sphinx-build(1)`, :manpage:`sphinx-apidoc(1)`
