.. highlight:: rest

문단 레벨 마크업
----------------------

.. index:: note, warning
           pair: changes; in version

아래의 directive들은 짧은 문단을 만들며, 일반 텍스트 뿐만 아니라 정보 단위 내에서도
사용할 수 있습니다.

.. rst:directive:: .. note::

   메모와 관련된 API를 사용할 때 사용자가 알아야 할 API에 대한 중요한 내용이다.
   이 directive의 내용은 구두점을 포함한 완전한 문장으로 작성되어야 한다.

   예::

      .. note::

         This function is not suitable for sending spam e-mails.

.. rst:directive:: .. warning::

   경고에 관련된 API를 사용할 때 사용자가 알아야하는 API에 대한 특별히 중요한
   정보이다. 내용은 구두법을 포함한 완전한 문장으로 작성되어야 한다. 이는 보안과
   관련하여 :rst:dir:`note` 이상으로 권장되는 점에서 :rst:dir:`note`와는 다르다.

.. rst:directive:: .. versionadded:: version

   설명 된 기능을 라이브러리 또는 C API에 추가 한 프로젝트 버전을 기록한다. 이것이
   전체 module에 적용될 때, module의 맨 위에 위치해야 한다.

   첫 번째 argument는 주어져야 하며 버전 숫자이다. 변경 사항에 대한 *간단한*
   설명으로 구성된 두 번째 argument를 추가 할 수도 있다.

   예::

      .. versionadded:: 2.5
         The *spam* parameter.

   Directive head와 설명 사이에는 blank like이 없어야 한다는 점에 주의한다. 이는
   마크업에서 이러한 블록을 시각적으로 연속으로 보이게 만들기 위해서이다.

.. rst:directive:: .. versionchanged:: version

   :rst:dir:`versionadded` 와 유사하지만, 명시된 기능에서 언제 무엇이(새로운
   parameter, 변경된 부작용 등) 변경되었는지 설명한다.

.. rst:directive:: .. deprecated:: version

   :rst:dir:`versionchanged` 와 유사하지만 언제 기능이 deprecate되었는지 설명한다.
   사용자에게 대신에 무엇을 사용해야 하는지 알려주는 설명이 주어질 수도 있다. 예::

      .. deprecated:: 3.1
         Use :func:`spam` instead.


--------------

.. rst:directive:: seealso

   대부분의 섹션에는 module documentation 또는 외부 문서에 대한 참조 목록이
   포함되어 있다. 이 목록은 :rst:dir:`seealso` directive를 사용하여 생성된다.

   :rst:dir:`seealso` directive는 일반적으로 서브섹션 바로 이전에 섹션 안에 배치된다.
   HTML로 출력할 경우, 텍스트의 기본 흐름 밖에 있는 상자안에 표시된다.

   :rst:dir:`seealso` directive의 내용은 reST 정의 목록이어야 한다. 예::

      .. seealso::

         Module :py:mod:`zipfile`
            Documentation of the :py:mod:`zipfile` standard module.

         `GNU tar manual, Basic Tar Format <http://link>`_
            Documentation for tar archive files, including GNU tar extensions.

   다음과 같은 "짧은 형식"도 허용된다::

      .. seealso:: modules :py:mod:`zipfile`, :py:mod:`tarfile`

   .. versionadded:: 0.5
      The short form.

.. rst:directive:: .. rubric:: title

   이 directive는 목차 노드를 만드는 데 사용되지 않는 문단 제목을 만든다.

   .. note::

      지시문의 *title*이 "Footnotes"(또는 특정 언어의 동등한 것)인 경우 LaTeX 작성기는
      지시문을 무시한다. 이는 각주 정의만 포함된 것으로 간주하기 때문에 빈 제목을 생성하기
      때문이다.


.. rst:directive:: centered

   이 directive는 가운데에 굵게 표시된 텍스트를 만든다. 다음과 같이 사용하면 된다::

      .. centered:: LICENSE AGREEMENT

   .. deprecated:: 1.1
      이 프리젠테이션 전용 directive는 구형 버전의 legacy이다. 대신 :rst:dir:`rst-class`
      directive를 사용하고 적절한 스타일을 추가해라.


.. rst:directive:: hlist

   이 directive에는 bullet list가 있어야 한다. 빌더에 따라 두 개 이상의 항목을
   가로로 분배하거나 항목 간 간격을 줄여 좀 더 컴팩트한 목록으로 변환한다.

   수평 분배를 지원하는 빌더의 경우, 컬럼 수를 지정하는``columns`` 옵션이 있다.
   기본값은 2이다. 예::

      .. hlist::
         :columns: 3

         * A list of
         * short items
         * that should be
         * displayed
         * horizontally

   .. versionadded:: 0.6


목차 마크업
------------------------

하위문서의 목차를 생성하는 :rst:dir:`toctree` directive는 ref:`toctree-directive`
에 설명되어 있다.

로컬 목차의 경우, 표준 reST :dudir:`contents directive <table-of-contents>` 를
사용한다.


.. _glossary-directive:

용어집
----------

.. rst:directive:: .. glossary::

   이 directive는 용어와 정의가 있는 reST 정의 목록과 유사한 마크업을 포함해야
   한다. 정의는 :rst:role:`term` role을 통해 참조 가능하다. 예::

      .. glossary::

         environment
            A structure where information about all documents under the root is
            saved, and used for cross-referencing.  The environment is pickled
            after the parsing stage, so that successive runs only need to read
            and parse new and changed documents.

         source directory
            The directory which, including its subdirectories, contains all
            source files for one Sphinx project.

   일반 정의 목록과 달리 항목 당 *여러* 용어가 허용되며 용어 안에 인라인 마크업도
   허용된다. 모든 용어에는 링크가 가능하다. 예::

      .. glossary::

         term 1
         term 2
            Definition of both terms.

   (용어집이 정렬 될 때, 첫 번째 용어가 정렬 순서를 결정합니다.)

   일반 색인 항목에 대해 "그룹화 key"를 지정하려면 "key"를 "용어 : key" 형태로
   넣으면 된다. 예::

      .. glossary::

         term 1 : A
         term 2 : B
            Definition of both terms.

   "key"는 그룹화하는 데 그대로 사용된다. "key"는 정규화 되지 않는다. "A"와 "a"는 다른
   그룹이 된다. 또한 "key"의 전체 문자가 사용된다. "Combining Character Sequence
   (문자 시퀀스 조합)" 및 "Surrogate Pair(대리 쌍)" 그룹화 키에 사용된다.

   i18n 상황에서는 원본 텍스트에 "용어"부분 만 있더라도 "현지화 용어 : key"를
   지정할 수 있다. 이 경우, 번역 된 "현지화 용어"는 "key" 그룹으로 분류된다.

   .. versionadded:: 0.6
      You can now give the glossary directive a ``:sorted:`` flag that will
      automatically sort the entries alphabetically.

   .. versionchanged:: 1.1
      Now supports multiple terms and inline markup in terms.

   .. versionchanged:: 1.4
      Index key for glossary term should be considered *experimental*.

Grammar production 표시
---------------------------

Formal grammar의 production을 표시하기 위해 특수한 마크업이 사용 가능하다.
이 마크업은 간단하며, BNF(또는 BNF에서 파생 된 형식)의 모든 측면을 모델링
하지는 않지만, 심볼 사용을 정의에 대한 하이퍼링크로 렌더링하는 방식으로
context-free grammar를 표시 할 수 있도록 충분한 기능을 제공한다. 이를 위해서는
다음 directive가 있다:

.. rst:directive:: .. productionlist:: [name]

   이 directive는 production group을 묶는 데 사용된다. 각 production은 한 줄로
   주어지며, 콜론으로 구분 된 이름과 정의로 구성된다. 정의가 여러 행에 걸쳐있는 경우,
   각 연속되는 행은 첫​​째 행의 콜론에서와 동일한 column에서 콜론으로 시작해야 한다.

   :rst:dir:`productionlist` 의 argument는 서로 다른 grammar에 속하는 서로 다른
   production list의 집합들을 구별하는 역할을 한다.

   Blank line은 ``productionlist`` directive arguments 내에서 허용되지 않는다.

   정의는 interpreted text로 표시된 토큰 이름을 포함 할 수 있다. (예: ``sum ::= `integer`
   "+" `integer```) 이는 토큰의 production에 대한 cross-reference를 생성한다.
   Production 목록 외부에서는 :rst:role:`token` 을 사용하여 토큰 production을
   참조 할 수 있다.

   Production에서는 더이상 reST parsing이 수행되지 않으므로, ``*`` or ``|`` 문자들을
   escape 할 필요가 없다.

다음은 Python Reference Manual에서 가져온 예제이다::

   .. productionlist::
      try_stmt: try1_stmt | try2_stmt
      try1_stmt: "try" ":" `suite`
               : ("except" [`expression` ["," `target`]] ":" `suite`)+
               : ["else" ":" `suite`]
               : ["finally" ":" `suite`]
      try2_stmt: "try" ":" `suite`
               : "finally" ":" `suite`
