.. highlight:: rest

.. _code-examples:

코드 예제 표시
---------------------

.. index:: pair: code; examples
           single: sourcecode

Python 소스 코드나 인터랙티브 세션의 예제는 표준 reST literal block을 사용하여
표현된다. 그것들은 앞 단락의 끝에 ``::`` 에 의해 시작되고 들여 쓰기로 구분된다.

인터랙티브 세션을 나타내는 것은 프롬프트와 Python 코드와 함께 출력을 포함해야 한다.
인터랙티브 세션에는 특별한 마크업이 필요하지 않다. 입력 또는 출력의 마지막 줄이
표시된 후에 "사용되지 않은" 기본 프롬프트가 없어야 한다. 이것은 *하면 안되는* 예이다::

   >>> 1 + 1
   2
   >>>

Syntax 강조는 `Pygments <http://pygments.org>`_ 와 함께 현명게 처리된다:

* 각 소스 파일에는 "강조 표시 언어"가 있다. 파일의 대부분은 Python 코드를
  강조해야 하기 때문에 기본값은 ``'python'`` 이지만 문서 전체 기본값을
  :confval:`highlight_language` 설정 값으로 설정할 수 있다.

* Python 강조 모드에서 인터랙티브 세션은 자동으로 인식되고 적절히 강조가 된다.
  일반 Python 코드는 parsing이 가능한 경우에만 강조된다. (따라서 Python을
  기본값으로 사용할 수 있지만 여기저기 산재된 shell command나 다른 코드 블록의
  경우 강조되지 않는다.)

* 강조 언어는 다음과 같이 사용되는``highlight`` directive를 사용하여 변경 할 수 있다:

  .. rst:directive:: .. highlight:: language

     예::

        .. highlight:: c

     이 언어는 다음 ``highlight`` directive가 나올 때까지 사용된다.

* 다른 언어의 코드를 표시해야하는 문서의 경우에는 강조 언어가 직접 지정되는
  :rst:dir:`code-block' directive가 있다.

  .. rst:directive:: .. code-block:: language

     다음과 같이 사용::

        .. code-block:: ruby

           Some Ruby code.

     이 directive의 alias인 :rst:dir:`sourcecode` 도 사용 가능하다.

* 강조 언어의 유효한 값은 다음과 같다.

  * ``none`` (no highlighting)
  * ``python`` (:confval:`highlight_language` 가 지정되지 않을 경우의 기본값)
  * ``guess`` (Pygments 내용을 기반으로 추측. 잘 알려진 특정 언어들에 대해서만 작동)
  * ``rest``
  * ``c``
  * ... `Pygments가 지원하는 기타 lexer alias <http://pygments.org/docs/lexers/>`_.

* 선택한 언어로 강조 표시가 실패한 경우 (즉, Pygments가 "오류" 토큰을 내보낼 경우)
  블록은 강조 표시되지 않는다.

줄 번호
^^^^^^^^^^^^

Pygments는 코드 블록의 줄 번호를 생성 할 수 있다. 자동으로 강조되는 블록
(``::`` 에 의해 시작된 블록)의 경우, 줄 번호는 :rst:dir:`highlight` directive에
``linenothreshold`` 옵션과 함께 사용해서 활성화 해야 한다::

   .. highlight:: python
      :linenothreshold: 5

이렇게 하면 5줄 보다 긴 모든 코드 블록에 대해 줄 번호가 생성된다.

:rst:dir:`code-block` 블록의 경우, ``linenos`` 플래그 옵션을 사용하여 개별 블록의
줄 번호를 활성화 할 수 있다 ::

   .. code-block:: ruby
      :linenos:

      Some more Ruby code.

첫 번째 줄의 번호는 ``lineno-start`` 옵션으로 지정할 수 있다. 이 경우
``linenos`` 는 자동으로 활성화 된다::

   .. code-block:: ruby
      :lineno-start: 10

      Some more Ruby code, with line numbering starting at 10.

또한, ``emphasize-lines`` 옵션은 Pygments가 특정 행을 강조하도록 할 수 있다::

    .. code-block:: python
       :emphasize-lines: 3,5

       def some_function():
           interesting = False
           print 'This line is highlighted.'
           print 'This one is not...'
           print '...but this one is.'

.. versionchanged:: 1.1
   ``emphasize-lines`` has been added.

.. versionchanged:: 1.3
   ``lineno-start`` has been added.


포함
^^^^^^^^

.. rst:directive:: .. literalinclude:: filename

   긴 예제 텍스트의 표시는 외부 파일에 plaintext로 저장하여 포함시킬 수 있다. 이 파일은
   ``literalinclude`` directive를 사용하여 포함 할 수 있다. [1]_ 예를 들어, Python
   소스 파일 :file:`example.py` 를 사용하려면 다음과 같이 쓴다::

      .. literalinclude:: example.py

   파일 이름은 대개 현재 파일의 경로를 기준으로합니다. 그러나 절대 경로
   (``/`` 로 시작하는 경로)는 최상위 소스 디렉토리에 상대적이다.

   ``tab-width`` 옵션과 함께 원하는 탭의 크기를 지정하면 input에서의 탭이 확장된다.

   :rst:dir:`code-block` 과 같이, 이 directive는 ``linenos``, ``lineno-start``,
   ``emphasize-lines`` 옵션들을 지원하고 현재 파일에서 지정된 언어와 다른
   언어를 지정하기 위한 ``language`` 옵션도 지원한다. 예::

      .. literalinclude:: example.rb
         :language: ruby
         :emphasize-lines: 12,15-18
         :linenos:

   포함되는 파일은 :confval:`source_encoding` 에 인코딩되어 있다고 가정합니다.
   파일의 인코딩이 다른 경우 ``encoding`` 옵션으로 지정할 수 있다::

      .. literalinclude:: example.py
         :encoding: latin-1

   이 directive는 파일의 일부만을 포함하는 것도 지원한다. 그것이 Python module인
   경우, ``pyobject`` 옵션을 사용하여 포함 할 class, function, method를 선택할 수 있다::

      .. literalinclude:: example.py
         :pyobject: Timer.start

   이 경우 파일 내의 ``Timer`` class에 있는 ``start ()`` method에 속한 코드만을 포함한다.

   다른 방법으로는 ``lines`` 옵션을 사용하여 포함 할 행들만 정확히 지정할 수도 있다::

      .. literalinclude:: example.py
         :lines: 1,3,5-10,20-

   이 경우 1, 3, 5 ~ 10 행과 20 ~ 마지막 행까지만 포함된다.

   파일의 어느 부분이 포함는지를 제어하는 ​​또 다른 방법은 ``start-after`` 와
   ``end-before`` 옵션(또는 그 중 하나만)을 사용하는 것이다. ``start-after`` 가 string
   옵션으로 주어지면, 그 string을 포함하는 첫 번째 라인 다음의 라인들만 포함된다.
   ``end-before`` 가 string 옵션으로 주어지면, 그 string을 포함하는 첫 번째 줄 이전
   줄들만 포함된다.

   ``start-after`` 를 사용하였더라도 여전히 ``lines`` 를 사용 가능하다. 이 경우
   허용되는 첫번째 행은 규칙에 따라 줄 번호 ``1`` 을 가진다.

   위에서 설명한 방법으로 행들을 선택했을 때, ``emphasize-lines`` 의 줄 번호는
   선택된 텍스트에 대한 상대적 줄번호를 의미한다. 즉, 선택된 내용의 첫번째 줄의
   번호는 ``1`` 이다.

   표시 할 파일의 특정 부분을 지정할 때, 원래의 줄 번호를 표시하는 것이 유용 할 수도 있다.
   이 경우 ``lineno-match`` 옵션을 사용하면 된다. 다만 이는 선택 항목이 연속적인 행들로
   구성되어 있을 때만 허용된다.

   ``prepend`` 와 ``append`` 옵션을 사용하여 포함 된 코드 앞뒤에 행을 추가 할 수 있다.
   이는 예를 들면 ``<?php``/``?>`` 마커를 포함하지 않는 PHP 코드를 강조 할 때 유용하다.


   코드의 diff를 보여주고 싶다면 ``diff`` 옵션을 주어 이전 버전의 파일을 지정할 수 있다::

      .. literalinclude:: example.py
         :diff: example.py.orig

   이 예제는 example.py와 example.py.orig의 diff를 unified diff format으로 보여준다.

   .. versionadded:: 0.4.3
      The ``encoding`` option.
   .. versionadded:: 0.6
      The ``pyobject``, ``lines``, ``start-after`` and ``end-before`` options,
      as well as support for absolute filenames.
   .. versionadded:: 1.0
      The ``prepend`` and ``append`` options, as well as ``tab-width``.
   .. versionadded:: 1.3
      The ``diff`` option.
      The ``lineno-match`` option.
   .. versionchanged:: 1.6
      With both ``start-after`` and ``lines`` in use, the first line as per
      ``start-after`` is considered to be with line number ``1`` for ``lines``.

캡션과 이름
^^^^^^^^^^^^^^^^^^^

.. versionadded:: 1.3

``caption`` 옵션은 코드 블록 앞에 그 이름을 보여줄 수 있습니다. ``name`` 옵션은
:rst:role:`ref` 를 사용하여 참조 할 수 있는 암시적인 대상의 이름이 제공 될 수 있다.
예를 들면::

   .. code-block:: python
      :caption: this.py
      :name: this-py

      print 'Explicit is better than implicit.'


:rst:dir:`literalinclude` 또한 ``caption`` 과 ``name`` 옵션을 지원한다. 이 경우
``caption`` 은 값을 비워두면 표시된 파일 이름이 argument로 주어진다.


Dedent
^^^^^^^^^^

.. versionadded:: 1.3

코드 블록에서 indent 문자들을 제거하기 위해서 ``dedent`` 옵션을 사용 할 수 있다.
예를 들면::

   .. literalinclude:: example.rb
      :language: ruby
      :dedent: 4
      :lines: 10-15

:rst:dir:`code-block` 또한 ``dedent`` 옵션을 지원한다.


.. rubric:: Footnotes

.. [1] 표준 ``.. include`` directive가 있지만 파일이 발견되지 않을 경우 에러를
내보낸다. 여기에서 사용하는 directive는 경고만 발생시킨다.
