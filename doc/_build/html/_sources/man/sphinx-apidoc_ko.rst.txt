sphinx-apidoc
=============

시놉시스
--------

**sphinx-apidoc** [*options*] -o <*outputdir*> <*sourcedir*> [*pathnames* ...]

기술
-----------

:program:`sphinx-apidoc` 은 Sphinx 소스를 자동생성하는 툴이다.
생성된 소스는 :rst:dir:`autodoc` 확장을 사용하며 다른 자동 API 문서화툴과 같은 스타일로 전체 패키지를 문서화 한다.

*sourcedir* 는 문서화 할 파이썬 패키지로의 경로이고 *outputdir* 은 생성된 소스가 저장될 경로이다.
주어진 *pathnames* 은 생성시 제외될 경로들이다.

.. warning::

   ``sphinx-apidoc`` 이 생성한 소스파일은 :mod:`sphinx.ext.autodoc` 을 사용해
   발견된 모든 소스파일을 문서화 한다. 만약 발견된 모듈 중 가져올 때 부작용이 있는 모듈이 있다면
   ``sphinx-build`` 가 실행될 때 ``autodoc`` 에 의해 부작용이 실행된다.

   스크립트를 문서화 한다면 (라이브러리 모듈과 반대로)
   스크립트의 메인 루틴이 ``if __name__ == '__main__'`` 설정으로 보호되어야 한다.

옵션
-------

.. program:: sphinx-apidoc

.. option:: -o <outputdir>

   출력 파일을 저장할 디렉토리. 존재하지 않는 디렉토리면 생성된다.

.. option:: -f, --force

   이미 존재하는 생성 파일에 강제 덮어쓰기.

.. option:: -l, --follow-links

   심볼릭 링크를 따라간다.

.. option:: -n, --dry-run

   어떤 파일도 만들지 않는다.

.. option:: -s <suffix>

   생설될 소스 파일의 확장자. 디폴트는 ``rst``.

.. option:: -d <maxdepth>

   생성될 컨텐츠 파일 테이블의 최대 깊이.

.. option:: -T, --no-toc

   컨텐츠 파일 테이블을 생성하지 않는다.
   :option:`--full` 이면 무시된다.

.. option:: -F, --full

   완전한 Sphinx 프로젝트(``conf.py``, ``Makefile`` 등)을 생성한다.
   :program:`sphinx-quickstart` 과 동일한 메카니즘을 사용한다.

.. option:: -e, --separate

   각각의 모듈이 갖는 페이지에 문서를 저장한다.

   .. versionadded:: 1.2

.. option:: -E, --no-headings

   모듈과 패키지의 제목을 생성하지 않는다.
   이 옵션은 docstring이 이미 제목을 포함할 때 유용하다.

.. option:: -P, --private

   "_private" 모듈을 포함한다.

   .. versionadded:: 1.2

.. option:: --implicit-namespaces

   디폴트에 의해 sphinx-apidoc은 모듈을 검색할 때만 sys.path를 실행한다.
   파이썬 3.3은 내포된 :pep:`420` 네임스페이스를 도입해
   ``foo/bar/module.py``나 ``foo/bar/baz/__init__.py``와 같은 모듈 경로 구조를 허용한다.
   (``bar``와 ``foo``는 네임스페이스이고 모듈이 아님을 숙지한다.)

   PEP-0420에 따라 경로는 재귀적으로 해석된다.

.. option:: -M

   서브 모듈 문서 이전에 모듈 문서를 저장한다.

아래 옵션들은 :option:`--full` 이 설정된 경우에만 사용된다.:

.. option:: -a

   sys.path로 module_path를 추가한다.

.. option:: -H <project>

   생성될 파일에 들어갈 프로젝트명을 설정한다. (:confval:`project` 를 보라.)

.. option:: -A <author>

   생성될 파일에 들어갈 저자명을 설정한다. (:confval:`copyright` 를 보라.)

.. option:: -V <version>

   생성될 파일에 들어갈 프로젝트 버전을 설정한다. (:confval:`version` 를 보라.)

.. option:: -R <release>

   생성될 파일에 들어갈 배포를 설정한다. (:confval:`release` 를 보라.)

참고
--------

:manpage:`sphinx-build(1)`, :manpage:`sphinx-autogen(1)`
