.. _intl:

Internationalization
====================

.. versionadded:: 1.1

네비게이션 바(navigation bars) 같이 스핑크스 생성 메세지에 제공되는 번역을 보완하는
스핑크스는 문서 번역을 용이하게 하는 메커니즘을 제공한다. 설정에 대한 자세한 사항은
:ref:`intl-options` 를 참고하라.

.. figure:: translation.png
   :width: 100%

   스핑크스 번역 작업 흐름(workflow) 시각화. (`XKCD comic <http://xkcd.com/779/>`_
   에서 참고함.)

.. contents::
   :local:

Sphinx internationalization details
-----------------------------------

**gettext** [1]_ 는 국제화와 지역화를 위한 확정된 표준이다. 이것은 단순하게 프로그램에 있는
메세지를 번역된 스트링에 매핑한다. 스핑크스는 모든 문서를 번역할 때 이 기능을 사용한다.

처음에 프로젝트 관리자는 번역기에 알려줄 수 있도록 번역가능한 모든 스트링(또는 메세지)을 수집해야
한다. 스핑크스는 ``sphinx-build -b gettext`` 를 실행해서 스트링을 추출한다.

독트리에 있는 각각의 단일 요소는 단일 메세지가 되며 큰 단락들은 원본 문서에서처럼 거칠게
(coarsely-grained) 남아있는 반면에 리스트는 다른 덩어리들로(chunks) 동일하게 분할된다.
이것은 자유 텍스트(free-text) 단계에 있는 번역기에 약간의 컨텍스트를 제공하면서 원활한 문서
업데이트를 가능하게 한다. 자동화 된 방법이 없기 때문에 아주 큰 단락을 분리하는 일은 관리자가
해야 한다.

스핑크스가 성공적으로
:class:`~sphinx.builders.gettext.MessageCatalogBuilder` 를 실행하면 출력 디렉토리에
``.pot`` 파일들이 모여있는 것을 볼 수 있다. 이것은 **카탈로그 템플릿** 이며 원본 언어로만 된
메세지를 포함하고 있다.

그것들은 번역기에 전달될 수 있고 번역기는 그것들을 원본 메세지를 외국어 스트링으로 매핑하는
``.po`` 로 변환시킨다.

Gettext는 효율성을 위해 :program:`msgfmt` 를 통해서 파일들을 바이너리 카탈로그 라고 알려진
바이너리 포맷으로 컴파일한다. 만약 이 파일들을 :confval:`language` 를 위해
:confval:`locale_dirs` 로 검색 가능하게 하면 스핑크스가 자동적으로 그것들을 선택할 것이다.

예: 당신은 스핑크스 프로젝트에 ``usage.rst`` 문서를 가지고 있다. gettext 빌더는 문서의
메세지를 ``usage.pot`` 에 넣을 것이다. ``usage.po`` 에 스페인어 번역 [2]_ 내용이 이
있다고 상상해보자. 당신의 빌드가 번역되기 위해서는 아래의 지시 사항을 따라야 한다:

* 당신의 메세지 카탈로그를 로케일(locale) 디렉토리로 컴파일하라. ``locale`` 이라 하면
  당신의 소스 디렉토리에서 ``./locale/es/LC_MESSAGES/usage.mo`` 가 된다.
  (``es`` 는 스페인어의 언어 코드다.) ::

        msgfmt "usage.po" -o "locale/es/LC_MESSAGES/usage.mo"

* :confval:`locale_dirs` 를 ``["locale/"]`` 로 설정하라.
* :confval:`language` 를 ``es`` (:option:`-D <sphinx-build -D>` 를 통해서도 가능)
  로 설정하라.
* 원하는 빌드를 실행하라.


Translating with sphinx-intl
----------------------------

Quick guide
^^^^^^^^^^^

`sphinx-intl`_ 는 스핑크스 번역 플로우(flow)를 다루는 유용한 도구다.
이 섹선은 sphix-intl로 번역하는 쉬운 방법을 설명한다.

#. :command:`pip install sphinx-intl` 나 :command:`easy_install sphinx-intl`
   명령을 통해서 `sphinx-intl`_ 을 설치하라.

#. `conf.py` 에 설정을 추가하라::

      locale_dirs = ['locale/']   # 예시지만 이 경로를 추천한다.
      gettext_compact = False     # 선택적인 부분이다.

   이 가이드는 :confval:`locale_dirs` 가 'locale/' 로 설정되어 있고
   :confval:`gettext_compact` 가 `False` 로 설정되어 있다.(스핑크스 문서도
   마찬가지로 구성되어 있다.)

#. 문서의 번역 가능한 메제세를 pot 파일로 추출하라:

   .. code-block:: console

      $ make gettext

   결과적으로, pot 파일들이 ``_build/gettext`` 디렉토리 아래에 많이 생성될 것이다.

#. `locale_dir`를 설정/업데이트 하라:

   .. code-block:: console

      $ sphinx-intl update -p _build/gettext -l de -l ja

   실행하면 po 파일을 포함하는 아래의 디렉토리들을 얻게 된다:

   * `./locale/de/LC_MESSAGES/`
   * `./locale/ja/LC_MESSAGES/`

#. `./locale/<lang>/LC_MESSAGES/` 에 있는 po 파일들을 번역하라.

#. 번역된 문서를 만들어라.

   ``conf.py``에 있는 :confval:`language` 파라미터를 변경하거나 커맨드라인에서
   파라미터를 지정할 수 있다:

   .. code-block:: console

      $ make -e SPHINXOPTS="-D language='de'" html

축하한다! 당신은 ``_build/html`` 디렉토리에 번역된 문서를 얻게 되었다.

.. versionadded:: 1.3

   make 커맨드에 의해 호출된 스핑크스 빌드는 po 파일을 mo 파일로 빌드 한다.

   만약 1.2.x 또는 이번의 버전을 사용하면 make 커맨드 이전에 ``sphinx-intl build`` 커맨드를
   호출하라.


Translating
^^^^^^^^^^^

``./locale/de/LC_MESSAGES`` 디렉토리에 있는 po 파일을 번역하라.
스핑크스 문서에 있는 builders.po 파일의 경우:

.. code-block:: po

   # a5600c3d2e3d48fc8c261ea0284db79b
   #: ../../builders.rst:4
   msgid "Available builders"
   msgstr "<FILL HERE BY TARGET LANGUAGE>"

다른 경우, msgid는 reStructuredText 신택스를 포함하는 여러 줄의 텍스트일 수 있다:

.. code-block:: po

   # 302558364e1d41c69b3277277e34b184
   #: ../../builders.rst:9
   msgid ""
   "These are the built-in Sphinx builders. More builders can be added by "
   ":ref:`extensions <extensions>`."
   msgstr ""
   "FILL HERE BY TARGET LANGUAGE FILL HERE BY TARGET LANGUAGE FILL HERE "
   "BY TARGET LANGUAGE :ref:`EXTENSIONS <extensions>` FILL HERE."

reST 형식을 위반하지 않도록 주의하라. 대부분의 po 에디터가 위반하지 않도록 도와줄 것이다.


Update your po files by new pot files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

문서가 업데이트 되었다면 업데이트된 pot 파일을 생성하고 번역된 po 파일에 변경점을 적용해야 한다.
업데이트된 pot 파일의 변경점을 po 파일에 적용시키기 위해서는
:command:`sphinx-intl update` 커맨드를 사용하라.

.. code-block:: console

   $ sphinx-intl update -p _build/locale


Using Transifex service for team translation
--------------------------------------------

Transifex_ 는 웹 인터페이스를 통해서 공동 번역을 할 수 있게 해주는 서비스 중의 하나이다.
이것은 번역을 쉽게 fetch 하고 push 하게 만들어주는 뛰어난 파이선 기반 커맨드라인 클라이언트다.

.. TODO: why use transifex?


#. `transifex-client`_ 를 설치하라

   리소스(pot 파일)를 업로드를 위해서 :command:`tx` 커맨드를 사용하라.

   .. code-block:: console

      $ pip install transifex-client

   .. seealso:: `Transifex Client v0.8 &mdash; Transifex documentation`_


#. transifex_ 계정을 생성하고 당신의 문서를 위한 새로운 프로젝트를 생성하라.

   현재, transifex는 번역 프로젝트가 여러 버전의 문서를 버전을 보유하는 것을 허용하지 않는다.
   따라서 프로젝트 이름에 버전 숫자를 써두는 것이 좋다.

   예시:

   :Project ID: ``sphinx-document-test_1_0``
   :Project URL: ``https://www.transifex.com/projects/p/sphinx-document-test_1_0/``


#. tx 커맨드를 위한 설정 파일들.

   이 과정은 현재 디렉토리에 ``.tx/config`` 와 작성자 정보가 있는 ``~/.transifexrc`` 파일을
   생성한다.

   .. code-block:: console

      $ tx init
      Creating .tx folder...
      Transifex instance [https://www.transifex.com]:
      ...
      Please enter your transifex username: <transifex-username>
      Password: <transifex-password>
      ...
      Done.

#. pot 파일을 transifex 서비스에 업로드하라.

   pot 파일을 ``.tx/config`` 파일에 등록하라:

   .. code-block:: console

      $ cd /your/document/root
      $ sphinx-intl update-txconfig-resources --pot-dir _build/locale \
        --transifex-project-name sphinx-document-test_1_0

   그리고 pot 파일을 업로드하라:

   .. code-block:: console

      $ tx push -s
      Pushing translations for resource sphinx-document-test_1_0.builders:
      Pushing source file (locale/pot/builders.pot)
      Resource does not exist.  Creating...
      ...
      Done.


#. 번역본을 transifex에 forward 하라.

   .. TODO: write this section


#. 번역된 po 파일을 pull 해서 번역된 html 을 만들어라.

   번역된 카탈로그를 얻고 mo 파일을 빌드하라('de'인 경우):

   .. code-block:: console

      $ cd /your/document/root
      $ tx pull -l de
      Pulling translations for resource sphinx-document-test_1_0.builders (...)
       -> de: locale/de/LC_MESSAGES/builders.po
      ...
      Done.

   make html을 호출하라:

   .. code-block:: console

      $ make -e SPHINXOPTS="-D language='de'" html


과정이 끝났다!


.. tip:: Transifex나 로컬에서 번역하기

   만약 모든 언어의 po 파일을 push 하고 싶다면, :command:`tx push -t` 커맨드를
   사용하면 된다. 이 작업은 transifex에 있는 번역본들을 덮어쓰기 때문에 주의해야 한다.

   바꿔 말하면, 만약 당신이 서비스와 로컬에 있는 po 파일을 각각 업데이트 했으면
   통합할 때 굉장히 많은 시간과 노력이 필요하다.


Contributing to Sphinx reference translation
--------------------------------------------

새로운 컨트리뷰터(contributor)가 스핑크스 레퍼런스를 번역할 때 권장하는 방법은 Transifex에
있는 번역 팀에 가입시키는 것이다.

Sphinx-1.3 도큐먼테이션에 `sphinx translation page`_ 가 있다.

1. transifex_ 서비스에 로그인 하라.
2. `sphinx translation page`_ 로 이동하라.
3. ``Request language`` 을 클릭하고 양식을 채워라.
4. transifex 스핑크스 번역 관리자의 승인을 기다려라. Wait acceptance by  sphinx translation maintainers.
5. (승인 후) transifex로 번역하라.


.. rubric:: Footnotes

.. [1] 소프트웨어 suite에 대한 자세한 내용은 `GNU gettext utilities
       <http://www.gnu.org/software/gettext/manual/gettext.html#Introduction>`_
       을 참고하라.
.. [2] 아무도 스페인 종교 재판을 원하지 않기 때문이다!


.. _`transifex-client`: https://pypi.python.org/pypi/transifex-client
.. _`sphinx-intl`: https://pypi.python.org/pypi/sphinx-intl
.. _Transifex: https://www.transifex.com/
.. _`sphinx translation page`: https://www.transifex.com/sphinx-doc/sphinx-doc-1_3/
.. _`Transifex Client v0.8 &mdash; Transifex documentation`: http://docs.transifex.com/developer/client/
