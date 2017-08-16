.. highlight:: rst

.. _domains:

Sphinx Domains
==============

.. versionadded:: 1.0

도메인이란 무엇인가?
----------------------

원래, 스핑크스는 하나의 프로젝트와 파이썬 문서화를 위해 만들어졌다. 그 후 문서화 tool 로써 모든 사람이 사용 가능하게 만들어졌다. 하지만, 파이썬 모듈의 문서화는 빌트인 상태로 남게 되었고, ``function`` 같은 가장 기초적인 directives는 파이썬 object를 위해 고안되어졌다. 스핑크스가 유명해진 후에, C/C++, 자바스크립트, reStructuredText 마크업 등의 다양한 목적으로 개발되어지고고 있다. 

이제는 모든 목적에 맞는 **domain** 을 제공함으로써 다른 프로그래밍 언어를 사용하는 프로젝트 또는 스핑크스에서 지원하지 않는 프로젝트의 문서화를 더 쉽게 지원할 수 있다. 

도메인은 :term:`object`\ 를 설명하고 연결하기 위한 markup(reStructuredText :term:`directive`\ s 과:term:`role`\ s) 컬렉션이다. (예를 들면, 프로그래밍 언어의 요소) 
도메인에서 directive와 role names는 ``domain:name`` 과 같은 이름을 가진다. (예를 들면, ``py:function``) 도메인은 또한 파이썬 모듈 인덱스 같은 커스텀 인덱스들을 제공할 수 있다.

도메인을 가지는 것은 예를 들어 문서화된 한 세트가 C++, 파이썬 클래스를 참조하고자 할 때, 네이밍 문제가 없다는 것을 의미한다. 또한 새로운 언어의 문서화를 지원해주는 확장을 작성하기가 훨씬 더 쉬워졌다는 것을 의미한다.

이 섹션은 스핑크스에 포함된 도메인이 무엇을 제공하는지를 설명해준다. 도메인 API는 문서화가 잘되어있다 :ref:`domain-api`.


.. _basic-domain-markup:

Basic Markup
------------

대부분의 도메인은 많은 :dfn:`object description directives` 를 제공하고, 이러한 것들은 모듈에 의해 제공된 구체적인 object를 설명하기 위해 사용되어 진다. 
각각의 directive는 설명할 것들에 대해 기본적인 정보를 제공하기 위해 하나 이상의 signature를 요구한다. 그리고 내용을 설명해야 한다. 기본 버젼은 일반적인 인덱스 내에서 엔트리를 만든다. 인덱스가 없는 엔트리가 요구되면, directive option flag 인 ``:noindex:`` 를 줄 수 있다. 아래는 파이썬 도메인 directive를 사용한 예이다. ::

   .. py:function:: spam(eggs)
                    ham(eggs)

      Spam or ham the foo.

이는 두 개의 파이썬 함수 ``spam`` 과 ``ham`` 을 설명한다. signature가 너무 길어질 때, 다음 줄까지 이어지는 줄에 backslash를 추가한다면 signature를 깰 수 있다. 예를 들면::

   .. py:function:: filterwarnings(action, message='', category=Warning, \
                                   module='', lineno=0, append=False)
      :noindex:

(이 예는 또한 ``:noindex:`` flag를 사용하는 방법을 보여준다.)

도메인은 또한 이러한 object 설명들로 돌아갈 수 있게 연결해주는 역할을 한다.
예를 들면, 위의 예에서 설명된 함수중 하나에 연결하기 위해, 이렇게 할 수 있다. ::

   The function :py:func:`spam` does a similar thing.

위에서 볼 수 있듯이, directives와 role 이름 모두는 그 도메인 이름과 그 directive 네임을 포함 한다.

.. rubric:: Default Domain

하나의 도메인으로부터 object를 설명하는 문서화를 위해서, 저자는 각각의 directive, role, 기타 등에 대해서 구체적인 디폴트를 지정한 후에는 다시 명시할 필요가 없다.
이는 config value :confval:`primary_domain` 또는 directive를 통해 수행할 수 있다.


.. rst:directive:: .. default-domain:: name

   새로운 디폴트 도메인을 선택해라. :confval:`primary_domain` 가 전역 디폴트를 선택하는 반면, 이는 같은 파일 내에서만 효과를 가진다.

다른 디폴트가 선택되지 않는다면, 대부분 스핑크스의 이전 버젼용으로 작성된 문서와의 호환성을 위해 파이썬 도메인 (named ``py``) 가 디폴트가 된다.

디폴트 도메인에 속하는 Directives와 roles은 도메인 이름을 주지 않고 언급되어질 수 있다. 예를 들면 ::

   .. function:: pyfunc()

      Describes a Python function.

   Reference to :func:`pyfunc`.


Cross-referencing syntax
~~~~~~~~~~~~~~~~~~~~~~~~

도메인에 의해 제공된 상호 참조 role의 경우, 일반적인 상호 참조를 위해 같은 기능이 존재한다. :ref:`xref-syntax` 를 보자.

간략하게 :

* 명시적인 제목과 참조 타겟을 제공할 수도 있다. : ``:role:`title
  <target>``` 는 *target* 을 참조할 것이지만, 연결문은 *title* 일 것이다.

* ``!`` 가 있는 컨텐츠를 앞에 놓는다면, 어떠한 참조나 하이퍼링크도 만들어지지 않을 것이다.

* ``~`` 가 있는 컨텐츠를 앞에 놓는다면, 연결문은 단지 타겟의 마지막 구성요소일 것이다. 예를 들면, ``:py:meth:`~Queue.Queue.get`` 는 ``Queue.Queue.get`` 를 참조할 것이지만, 단지 연결문으로서 ``get`` 을 보여준다.


The Python Domain
-----------------

파이썬 도메인(name **py**)은 모듈 선언을 위해 다음의 directives를 제공한다.:

.. rst:directive:: .. py:module:: name

   
   이 directive는 모듈(또는 패키지 이름을 포함하고 이름이 완전한 패키지 서브 모듈)에 대한 설명을 시작을 나타낸다. 이는 content를 만들지 않는다. (예를 들면 :rst:dir:`py:class` )

   또한 이 directive는 전역 모듈 인덱스 내에서 인덱스의 원인이 된다.

   ``platform`` 옵션은 모듈이 이용 가능한 플랫폼의 콤마로 구분 가능한 리스트이다.(    모든 플랫폼에서 사용이 가능한 경우, 옵션은 생략한다.)
   키들은 짧은 식별자이다. 사용중인 예는 "IRIX", "Mac", "Windows", 와 "Unix"를 포함   한다. 적용이 가능할 때, 이미 사용된 키를 사용하는 것은 중요하다. 

   ``synopsis`` 옵션은 모듈의 목적을 설명하는 하나의 문장으로 구성 되어야 한다. 이는 현재 전역 모듈 인덱스로 사용되어 진다.

   ``deprecated`` 옵션은 module이 deprecated이라는 것을 나타내기 위해 사용할 수 있다. 그리고 다양한 위치에 지정 되어질 것이다.


.. rst:directive:: .. py:currentmodule:: name

   이 directive는 클래스, 함수, 문서화 등이 주어진 모듈에 있는지를 스핑크스에게 알려준다. ( :rst:dir:`py:module`), 그러나 그것은 :rst:role:`py:mod` 에 대한 인덱스 엔트리, 전역 모듈 인덱스 내의 엔트리 도는 연결 타겟을 만들지 않을 것이다. 이는 모듈에 있는 것들에 대한 문서화가 여러 개의 파일 또는 섹션을 통해 확산되는 상황에서 도움이 된다. 하나의 위치는 :rst:dir:`py:module` directive를 가지고, 다른 위치들은 단지
   :rst:dir:`py:currentmodule` 를 가진다.


다음의 directive들은 모듈과 클래스 컨텐츠를 위해 제공 되어 진다.

.. rst:directive:: .. py:function:: name(parameters)

   모듈 레벨의 함수를 설명한다. signature는 파이썬 함수 정의에서 주어진 파라미터를 포함 해야한다. :ref:`signatures` 를 보자.
   예시::

      .. py:function:: Timer.repeat(repeat=3, number=1000000)

   사용해야하는 메소드의 경우 :rst:dir:`py:method`.

   일반적으로 설명은 요구되어지는 파라미터, 어떻게 그들이 사용 되어지는지 , 부작용그리고  가능한 예외에 대한 정보를 포함해야 한다. 

   이 정보는 (in any ``py`` directive) 선택적으로 구조화된 형태로 주어진다. :ref:`info-field-lists` 를 봐라.

.. rst:directive:: .. py:data:: name

   모듈에 있는 "dfined constants"로 사용되는 변수와 값을 모두 포함하는 전역 데이터를 설명한다. 클래스와 object 특징들은 이 환경을 사용해서 문서화 되지 않는다.

.. rst:directive:: .. py:exception:: name

   예외 클래스를 설명한다. Signature는 constructor arguments를 가진 괄호를 포함할 필요는 없다.

.. rst:directive:: .. py:class:: name
                   .. py:class:: name(parameters)

   클래스를 설명한다. Signature는 선택적으로 constructor arguments로 보여지는 파라미터를 가진 괄호를 포함할 수 있다.
   :ref:`signatures` 를 보자.

   클래스에 속하는 메소드와 특성은 directive의body에 위치 해야한다. 그들이 바깥에 위치한다면, 제공되어진 이름은 상호 참조가 여전히 작동하게 하기 위해서 클래스 이름을 포함 해야한다. 예::

      .. py:class:: Foo

         .. py:method:: quux()

      -- or --

      .. py:class:: Bar

      .. py:method:: Bar.quux()

   첫번째 방법이 선호되는 방법이다.

.. rst:directive:: .. py:attribute:: name

   object 데이터 특성을 설명한다. 설명은 데이터 타입에 대한 정보를 포함해야하고, 데이터가 변경 여부에 대한 정보 또한 포함해야한다.

.. rst:directive:: .. py:method:: name(parameters)

   Object 메소드를 설명한다. 파라미터는 ``self`` 를 포함하지 않아도 된다. 설명은 ``function``에 대한 설명과 유사한 정보를 포함해야한다.  :ref:`signatures` 와 :ref:`info-field-lists` 를 보자.

.. rst:directive:: .. py:staticmethod:: name(parameters)

   :rst:dir:`py:method` 와 같지만, 메소드가 static 메소드이다.

   .. versionadded:: 0.4

.. rst:directive:: .. py:classmethod:: name(parameters)

   :rst:dir:`py:method` 와 같지만, 메소드가 클래스 메소드이다.

   .. versionadded:: 0.6

.. rst:directive:: .. py:decorator:: name
                   .. py:decorator:: name(parameters)

   Decorator 함수를 설명한다. Signature는 decorator의 사용법을 표시해야한다. 예를 들면, 주어진 함수에서

   .. code-block:: python

      def removename(func):
          func.__name__ = ''
          return func

      def setnewname(name):
          def decorator(func):
              func.__name__ = name
              return func
          return decorator

   설명은 이렇게 보여야 한다.::

      .. py:decorator:: removename

         Decorated 함수의 이름을 제거해라.

      .. py:decorator:: setnewname(name)

         Decorated function의 이름을 *name*로 설정한다.

   (as opposed to ``.. py:decorator:: removename(func)``.)

   이 directive와 함께 나타내는 decorator에 연결하는 ``py:deco`` role이 없다. 오히려 :rst:role:`py:func` role를 사용한다.

.. rst:directive:: .. py:decoratormethod:: name
                   .. py:decoratormethod:: name(signature)

   :rst:dir:`py:decorator` 와 같지만, 메소드인 decorator를 위한 것이다.

   :rst:role:`py:meth` role를 사용해서 decorator 메소드를 참조해라.


.. _signatures:

Python Signatures
~~~~~~~~~~~~~~~~~

함수, 메소드와 클래스 constuctor의 Signatures는 그들이 파이썬에서 쓰여지는 것처럼 주어질 수 있다.

선택적인 argument에 대한 기본값은 주어질 수 있다.(하지만 콤마를 포함한다면, signature parser를 혼동할 것이다. 파이썬의 3가지 스타일의 주석 또한 주어질 것이고 또한 타입 주석을 리턴할 수 있다.::

   .. py:function:: compile(source : string, filename, symbol='file') -> ast object

기본값이 없는 선택적인 파라미터를 가진 함수(일반적으로 키워드 argument 지원없이 C의 확장 모듈에서 구현되어진 함수)에 대해 선택적인 부분을 명확히 하기 위해 괄호를 사용할 수 있다.:

   .. py:function:: compile(source[, filename[, symbol]])

콤마 전에 여는 괄호를 넣는 것이 일반적인 관례이다.


.. _info-field-lists:

Info field lists
~~~~~~~~~~~~~~~~

.. versionadded:: 0.4

파이썬 object description directives 내에서, 이러한 field list를 가진 reST field lists는 잘 인식되어지고 포매팅 되어진다.
are recognized and formatted nicely:

* ``param``, ``parameter``, ``arg``, ``argument``, ``key``, ``keyword``:
  파라미터에 대한 설명.
* ``type``: 파라미터의 타입. 가능하면 링크를 만든다.
* ``raises``, ``raise``, ``except``, ``exception``: 특정 에러가 발생.
* ``var``, ``ivar``, ``cvar``: 변수에 대한 설명.
* ``vartype``: 변수의 타입. 가능하면 링크를 만든다.
* ``returns``, ``return``: 반환값에 대한 설명.
* ``rtype``: 반환값의 타입. 가능하면 링크를 만든다.

.. note::

   현재 버젼에서, 모든 ``var``, ``ivar`` 와 ``cvar`` 는 "Variable"로 표현된다.
   그리고 차이점은 전혀 없다.

The field names은 이러한 키워드 중 하나와 argument(argument를 필요로 하지 않는 ``returns`` 나 ``rtype`` 은 제외)로 구성되어야 한다. 이는 가장 잘 설명된 예시이다.::

   .. py:function:: send_message(sender, recipient, message_body, [priority=1])

      Send a message to a recipient

      :param str sender: The person sending the message
      :param str recipient: The recipient of the message
      :param str message_body: The body of the message
      :param priority: The priority of the message, can be a number 1-5
      :type priority: integer or None
      :return: the message id
      :rtype: int
      :raises ValueError: if the message_body exceeds 160 characters
      :raises TypeError: if the message_body is not a basestring

이는 이렇게 될 것이다.:

   .. py:function:: send_message(sender, recipient, message_body, [priority=1])
      :noindex:

      Send a message to a recipient

      :param str sender: The person sending the message
      :param str recipient: The recipient of the message
      :param str message_body: The body of the message
      :param priority: The priority of the message, can be a number 1-5
      :type priority: integer or None
      :return: the message id
      :rtype: int
      :raises ValueError: if the message_body exceeds 160 characters
      :raises TypeError: if the message_body is not a basestring

타입이 하나의 단어라면, 파라미터의 타입과 설명을 결합하는 것 또한 가능하다. 
::

   :param int priority: The priority of the message, can be a number 1-5


.. versionadded:: 1.5

리스트와 딕셔너리 같은 컨테이너 타입은 다음의 syntax를 통해 자동으로 연결될 수 있다.::

   :type priorities: list(int)
   :type priorities: list[int]
   :type mapping: dict(str, int)
   :type mapping: dict[str, int]
   :type point: tuple(float, float)
   :type point: tuple[float, float]

타입 필드에서 다중 타입들은 "or"에 의해 구분 되어 진다면, 자동으로 연결될 수 있다.::

   :type an_arg: int or None
   :vartype a_var: str or int
   :rtype: float or str

.. _python-roles:

Cross-referencing Python objects
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

다음의 role은 모듈 내에서 object를 참조하고 매칭이 되는 식별자를 발견한다면 하이퍼링크의 기능도 할 수 있다.:

.. rst:role:: py:mod

   모듈 참조; 점이 있는 이름을 사용할 수도 있다.  이는 또한 패키지 이름으로도 사용되어져야 한다.

.. rst:role:: py:func

   파이썬 함수 참조; 점이 있는 이름을 사용할 수도 있다. Role 텍스트는 가독성을 향상 시키기 위해 괄호를 추적하는 것을 할 필요가 없다.  :confval:`add_function_parentheses` config value 가 ``True`` (기본값)이라면 스핑크스에 의해 자동적으로 더해질 것이기 때문이다.

.. rst:role:: py:data

   모듈 레벨의 변수 참조.

.. rst:role:: py:const

   "defined" constant 참조. 이는 변경되지 않는 파이썬 변수이다.

.. rst:role:: py:class

   클래스 참조; 점이 포함된 이름이 사용 가능하다.

.. rst:role:: py:meth

   Object의 메소드 참조.  Role text는 타입 이름과 메소드 이름을 포함할 수 있다. 타입의 설명 중에 발생하는 경우, 타입 이름은 생략되어 질 수 있다. 점이 있는 이름 또한 사용 가능하다.

.. rst:role:: py:attr

   Object의 데이터 특성 참조.

.. rst:role:: py:exc

   예외 참조.  점이 있는 이름도 사용 가능하다.

.. rst:role:: py:obj

   구체화 되지 않은 타입의 object 참조. 유용한 예
   :confval:`default_role`.

   .. versionadded:: 0.4


이 마크업에 둘러쌓인 이름은 모듈 이름과 클래스의 이름을 포함할 수 있다.
예를 들면, ``:py:func:`filter``` 는 현재 모듈 또는 그 이름의 빌트인 함수 내에서 ``filter`` 라는 이름의 함수를 참조할 수 있다. 반대로
``:py:func:`foo.filter``` 는 명확하게 ``foo`` 모듈 내에 있는 ``filter`` 함수를 참조한다. 

일반적으로, 이러한 role에 있는 이름들은 더 필요한 조건 없이 처음에 검색된다. 그리고 나서, 현재 모듈 이름 앞에 추가된다. 그리고 현재 모듈의 이름이나 클래스의 이름 앞에 추가된다. 만약 점을 포함한 이름을 앞에 둔다면, 이 순서는 반대가 된다. 예를 들면,   파이썬 도큐멘테이션에서 :mod:`codecs` module, ``:py:func:`open```  는 항상 빌트인 함수를 참조하는 반면, ``:py:func:`.open``` 는 :func:`codecs.open` 를 참조한다.

유사한 휴리스틱은 이름이 현재 문서화된 클래스의 특성인지 아닌지 여부를 결정하는데 사용된다.

또한, 점이 이름 앞에 오고, 정확한 짝을 찾지 못하는 경우, 타겟은 접미사처럼 취급되어 지고 접미사를 가진 모든 object의 이름이 검색되어진다. 예를 들면 현재 모듈이 ``tarfile`` 이 아닌데 ``:py:meth:`.TarFile.close``` 가
``tarfile.TarFile.close()`` 함수를 참조한다.  이는 모호함을 발생시키기 때문에, 하나 이상의 가능한 짝이 있다면, 스핑크스로가 경고를 보여줄 것이다.

``~`` 와 ``.`` 를 접두사로 결합할 수 있다.:
``:py:meth:`~.TarFile.close``` 는  ``tarfile.TarFile.close()``
메소드를 참조할 것이다. 하지만 눈에 보이는 링크 캡션은 단지 ``close()`` 일 것이다.


.. _c-domain:

The C Domain
------------

C 도메인 (이름 **c**)은 C API의 문서화에 적합하다.

.. rst:directive:: .. c:function:: type name(signature)

   C 함수를 설명한다. Signature는 C에서 제공되어진다. , 예::

      .. c:function:: PyObject* PyType_GenericAlloc(PyTypeObject *type, Py_ssize_t nitems)

   이는 또한 Preprocessor macros 같은 함수를 설명한다. Argument의 이름은 그들이 설명 내에서 사용되어지게 하기 위해서 주어진다.

   reST 인라이너에 의해 parse되지 않기 때문에, Signature에서 백슬래시 이스케이프 asterisk를 사용하지 마라. 

.. rst:directive:: .. c:member:: type name

   Describes a C struct member. Example signature::

      .. c:member:: PyObject* PyTypeObject.tp_bases

   설명하는 텍스트는 허락되어진 값의 범위와, 어떻게 값이 해석되어져야 하는지, 값이 변경되는지 여부에 대한 정보를 포함해야한다. 텍스트에서 구조 멤버에 대한 참조는 ``member`` role을 사용해야한다.

.. rst:directive:: .. c:macro:: name

   간단한 C 매크로를 설명한다.  간단한 매크로는 코드의 확장을 위해 사용된다. 그러나 함수로 설명될 수 없는 argument는 사용하지 않는다. ``#define`` 는 간단한 C언어이다. 파이썬 문서에서 그것의 사용에 대한 예는 :c:macro:`PyObject_HEAD` 과
   :c:macro:`Py_BEGIN_ALLOW_THREADS` 를 포함한다.

.. rst:directive:: .. c:type:: name

   C 타입을 설명한다.(typedef 또는 struct에 의해 정의 되어 졌는지 여부). Signature
   는 타입 이름이어야 한다.

.. rst:directive:: .. c:var:: type name

   전역 C 변수를 설명한다.  Signature는 다음과 같은 타입을 포함해야한다.::

      .. c:var:: PyObject* PyClass_Type


.. _c-roles:

Cross-referencing C constructs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

다음의 role은 문서에서 정의되어졌다면,  C 언어 구조에서 상호 참조를 생성한다.:

.. rst:role:: c:data

   C 언어 변수 참조.

.. rst:role:: c:func

   C 언어 함수 참조. 괄호를 따라가는 것을 포함한다.

.. rst:role:: c:macro

   간단한 C 매크로 참조.

.. rst:role:: c:type

   C 언어 타입 참조.

.. _cpp-domain:

The C++ Domain
--------------

C++ 도메인 (name **cpp**)은 C++ 프로젝트를 문서화 하는 것을 지원한다.


Directives
~~~~~~~~~~

다음의 directive가 가능하다. 모든 선언은 ``public``, ``private`` 또는 ``protected``와 함께 시작할 수 있다.

.. rst:directive:: .. cpp:class:: class specifier

   클래스와 구조를 설명한다.(가능하면 상속에 대한 설명도 함께) 예,::

      .. cpp:class:: MyClass : public MyBase, MyOtherBase

   클래스는 중첩된 범위 안에서 직접적으로 선언된다. 예,::

      .. cpp:class:: OuterScope::MyClass : public MyBase, MyOtherBase

   클래스 템플릿이 선언되어 질 수 있다.::

      .. cpp:class:: template<typename T, std::size_t N> std::array

   줄 바꿈과 함께 사용할 수도 있다.::

      .. cpp:class:: template<typename T, std::size_t N> \
                     std::array

   완전한 템플릿과 부분적인 템플릿 전문화가 선언되어 질 수 있다.::

      .. cpp:class:: template<> \
                      std::array<bool, 256>

      .. cpp:class:: template<typename T> \
                      std::array<T, 42>


.. rst:directive:: .. cpp:function:: (member) function prototype

   함수 또는 멤버 함수를 설명한다. 예,::

      .. cpp:function:: bool myMethod(int arg1, std::string arg2)

         파라미터와 타입을 가진 함수.

      .. cpp:function:: bool myMethod(int, double)

         이름이 없는 파라미터를 가진 함수.

      .. cpp:function:: const T &MyClass::operator[](std::size_t i) const

         인덱싱 오퍼레이터에 대한 오버로드.

      .. cpp:function:: operator bool() const

         캐스팅 오퍼레이터.

      .. cpp:function:: constexpr void foo(std::string &bar[2]) noexcept

         constexpr 함수.

      .. cpp:function:: MyClass::MyClass(const MyClass&) = default

         디폴트 구현의 생성자 복사.

   함수 템플릿 또한 설명되어질 수 있다.::

      .. cpp:function:: template<typename U> \
                        void print(U &&u)

   함수 템플릿 전문화::

      .. cpp:function:: template<> \
                        void print(int i)


.. rst:directive:: .. cpp:member:: (member) variable declaration
                   .. cpp:var:: (member) variable declaration

   변수와 멤버 변수를 설명한다. 예,::

      .. cpp:member:: std::string MyClass::myMember

      .. cpp:var:: std::string MyClass::myOtherMember[N][M]

      .. cpp:member:: int a = 42

   변수 템플릿 또한 설명되어 질 수 있다.::

      .. cpp:member:: template<class T> \
                      constexpr T pi = T(3.1415926535897932385)


.. rst:directive:: .. cpp:type:: typedef declaration
                   .. cpp:type:: name
                   .. cpp:type:: type alias declaration

   typedef 선언, type 별명 선언,
   또는 단순히 지정되지 않은 타입의 타입 이름으로 타입을 설명한다. e.g.,::

      .. cpp:type:: std::vector<int> MyList

         typedef 방식의 타입 선언

      .. cpp:type:: MyContainer::const_iterator

         지정되지 않은 타입의 타입 별명 선언.

      .. cpp:type:: MyType = std::unordered_map<int, std::string>

         타입 별명 선언.

   타입 별명 또한 템플릿화 되어 질 수 있다.::

      .. cpp:type:: template<typename T> \
                    MyContainer = std::vector<T>

   다음 예시는 렌더링이 되어 진다.

   .. cpp:type:: std::vector<int> MyList

      typedef 방식의 타입 선언.

   .. cpp:type:: MyContainer::const_iterator

      지정되지 않은 타입의 타입 별명 선언.

   .. cpp:type:: MyType = std::unordered_map<int, std::string>

      타입 별명 선언.

   .. cpp:type:: template<typename T> \
                 MyContainer = std::vector<T>


.. rst:directive:: .. cpp:enum:: unscoped enum declaration
                   .. cpp:enum-struct:: scoped enum declaration
                   .. cpp:enum-class:: scoped enum declaration

   가능하면 지정된 기본 타입과 함께 (scoped) 열거를 설명한다.
   범위가 지정되지 않은 열거 내에서 선언된 enumerator는 enum scope와 parent scope 양 쪽 모두에서 선언된다.

   예시::

      .. cpp:enum:: MyEnum

         범위가 지정되지 않은 열거.

      .. cpp:enum:: MySpecificEnum : long

         지정된 기본 타입이 있고 범위가 지정되지 않은 열거.

      .. cpp:enum-class:: MyScopedEnum

         범위가 지정된 열거.

      .. cpp:enum-struct:: protected MyScopedVisibilityEnum : std::underlying_type<MySpecificEnum>::type

         지정된 기본 타입이 있고 디폴트가 아닌 visibility가 있으며 범위가 지정된 열거.

.. rst:directive:: .. cpp:enumerator:: name
                   .. cpp:enumerator:: name = constant

   임의로 정의된 값을 가진 Enumerator를 설명한다. 예,::

      .. cpp:enumerator:: MyEnum::myEnumerator

      .. cpp:enumerator:: MyEnum::myOtherEnumerator = 42


.. rst:directive:: .. cpp:concept:: template-parameter-list name
                   .. cpp:concept:: template-parameter-list name()

   .. 경고:: 컨셉에 대한 지원은 실험적이고,
      Concepts Technical Specification에 기초를 두고 있다. 특징은 TS의 발전에 따라 변할 수 있다.

   변수 컨셉과 함수 컨셉을 설명한다. 둘 다 정확하게 하나의 템플릿 파라미터 리스트를 가져야 한다. 이름은 중첩된 이름이어도 된다. 예시::

      .. cpp:concept:: template<typename It> std::Iterator

         비교되거나 증가될 수 있는 개념적인 시퀀스의 요소에 대한 Proxy.

         **Notation**

         .. cpp:var:: It r

            An lvalue.

         **Valid Expressions**

         - :cpp:expr:`*r`, when :cpp:expr:`r` is dereferenceable.
         - :cpp:expr:`++r`, with return type :cpp:expr:`It&`, when :cpp:expr:`r` is incrementable.

      .. cpp:concept:: template<typename Cont> std::Container()

         :cpp:concept:`Iterator` s 를 통해 접근할 수 있는 요소의 Holder

   다음의 예시는 렌더링 되어 진다.:

   .. cpp:concept:: template<typename It> std::Iterator

      비교되거나 증가될 수 있는 개념적인 시퀀스의 요소에 대한 Proxy.

      **Notation**

      .. cpp:var:: It r

         An lvalue.

      **Valid Expressions**

      - :cpp:expr:`*r`, when :cpp:expr:`r` is dereferenceable.
      - :cpp:expr:`++r`, with return type :cpp:expr:`It&`, when :cpp:expr:`r` is incrementable.

   .. cpp:concept:: template<typename Cont> std::Container()

      :cpp:concept:`Iterator` s 를 통해 접근할 수 있는 요소의 Holder.

Options
.......

몇몇의 directive는 옵션을 지원한다.:

- ``:noindex:``, see :ref:`basic-domain-markup`.
- ``:tparam-line-spec:``, for templated declarations.
  If specified, each template parameter will be rendered on a separate line.


Constrained Templates
~~~~~~~~~~~~~~~~~~~~~

.. warning:: The support for constrained templates is experimental. It is based on the
  Concepts Technical Specification, and the features may change as the TS evolves.

.. note:: Sphinx does not currently support ``requires`` clauses.

Placeholders
............

Declarations may use the name of a concept to introduce constrained template
parameters, or the keyword ``auto`` to introduce unconstrained template parameters::

   .. cpp:function:: void f(auto &&arg)

      A function template with a single unconstrained template parameter.

   .. cpp:function:: void f(std::Iterator it)

      A function template with a single template parameter, constrained by the
      Iterator concept.

Template Introductions
......................

Simple constrained function or class templates can be declared with a
`template introduction` instead of a template parameter list::

   .. cpp:function:: std::Iterator{It} void advance(It &it)

       A function template with a template parameter constrained to be an Iterator.

   .. cpp:class:: std::LessThanComparable{T} MySortedContainer

       A class template with a template parameter constrained to be LessThanComparable.

They are rendered as follows.

.. cpp:function:: std::Iterator{It} void advance(It &it)

   A function template with a template parameter constrained to be an Iterator.

.. cpp:class:: std::LessThanComparable{T} MySortedContainer

   A class template with a template parameter constrained to be LessThanComparable.

Note however that no checking is performed with respect to parameter
compatibility. E.g., ``Iterator{A, B, C}`` will be accepted as an introduction
even though it would not be valid C++.


Inline Expressions and Tpes
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. rst:role:: cpp:expr

   A role for inserting a C++ expression or type as inline text.
   For example::

      .. cpp:var:: int a = 42

      .. cpp:function:: int f(int i)

      An expression: :cpp:expr:`a * f(a)`.
      A type: :cpp:expr:`const MySortedContainer<int>&`.

   will be rendered as follows:

  .. cpp:var:: int a = 42

  .. cpp:function:: int f(int i)

  An expression: :cpp:expr:`a * f(a)`.
  A type: :cpp:expr:`const MySortedContainer<int>&`.

Namespacing
~~~~~~~~~~~~~~~~~

Declarations in the C++ domain are as default placed in global scope.
The current scope can be changed using three namespace directives.
They manage a stack declarations where ``cpp:namespace`` resets the stack and
changes a given scope.
The ``cpp:namespace-push`` directive changes the scope to a given inner scope
of the current one.
The ``cpp:namespace-pop`` directive undos the most recent ``cpp:namespace-push``
directive.

.. rst:directive:: .. cpp:namespace:: scope specification

   Changes the current scope for the subsequent objects to the given scope,
   and resets the namespace directive stack.
   Note that the namespace does not need to correspond to C++ namespaces,
   but can end in names of classes, e.g.,::

      .. cpp:namespace:: Namespace1::Namespace2::SomeClass::AnInnerClass

   All subsequent objects will be defined as if their name were declared with the scope
   prepended. The subsequent cross-references will be searched for starting in the current scope.

   Using ``NULL``, ``0``, or ``nullptr`` as the scope will change to global scope.

   A namespace declaration can also be templated, e.g.,::

      .. cpp:class:: template<typename T> \
                     std::vector

      .. cpp:namespace:: template<typename T> std::vector

      .. cpp:function:: std::size_t size() const

   declares ``size`` as a member function of the class template ``std::vector``.
   Equivalently this could have been declared using::

      .. cpp:class:: template<typename T> \
                     std::vector

         .. cpp:function:: std::size_t size() const

   or:::

      .. cpp:class:: template<typename T> \
                     std::vector


.. rst:directive:: .. cpp:namespace-push:: scope specification

   Change the scope relatively to the current scope. For example, after::

      .. cpp:namespace:: A::B

      .. cpp:namespace-push:: C::D

   the current scope will be ``A::B::C::D``.

.. rst:directive:: .. cpp:namespace-pop::

   Undo the previous ``cpp:namespace-push`` directive (*not* just pop a scope).
   For example, after::

      .. cpp:namespace:: A::B

      .. cpp:namespace-push:: C::D

      .. cpp:namespace-pop::

   the current scope will be ``A::B`` (*not* ``A::B::C``).

   If no previous ``cpp:namespace-push`` directive has been used, but only a ``cpp:namespace``
   directive, then the current scope will be reset to global scope.
   That is, ``.. cpp:namespace:: A::B`` is equivalent to::

      .. cpp:namespace:: nullptr

      .. cpp:namespace-push:: A::B


Info field lists
~~~~~~~~~~~~~~~~~

The C++ directives support the following info fields (see also :ref:`info-field-lists`):

* `param`, `parameter`, `arg`, `argument`: Description of a parameter.
* `tparam`: Description of a template parameter.
* `returns`, `return`: Description of a return value.
* `throws`, `throw`, `exception`: Description of a possibly thrown exception.


.. _cpp-roles:

Cross-referencing
~~~~~~~~~~~~~~~~~

These roles link to the given declaration types:

.. rst:role:: cpp:any
              cpp:class
              cpp:func
              cpp:member
              cpp:var
              cpp:type
              cpp:concept
              cpp:enum
              cpp:enumerator

   Reference a C++ declaration by name (see below for details).
   The name must be properly qualified relative to the position of the link.

.. admonition:: Note on References with Templates Parameters/Arguments

   Sphinx's syntax to give references a custom title can interfere with
   linking to class templates, if nothing follows the closing angle
   bracket, i.e. if the link looks like this: ``:cpp:class:`MyClass<int>```.
   This is interpreted as a link to ``int`` with a title of ``MyClass``.
   In this case, please escape the opening angle bracket with a backslash,
   like this: ``:cpp:class:`MyClass\<int>```.

.. admonition:: Note on References to Overloaded Functions

   It is currently impossible to link to a specific version of an
   overloaded method.  Currently the C++ domain is the first domain
   that has basic support for overloaded methods and until there is more
   data for comparison we don't want to select a bad syntax to reference a
   specific overload.  Currently Sphinx will link to the first overloaded
   version of the method / function.

Declarations without template parameters and template arguments
.................................................................

For linking to non-templated declarations the name must be a nested name,
e.g., ``f`` or ``MyClass::f``.

Templated declarations
......................

Assume the following declarations.

.. cpp:class:: Wrapper

   .. cpp:class:: template<typename TOuter> \
                  Outer

      .. cpp:class:: template<typename TInner> \
                     Inner

In general the reference must include the template paraemter declarations, e.g.,
``template\<typename TOuter> Wrapper::Outer`` (:cpp:class:`template\<typename TOuter> Wrapper::Outer`).
Currently the lookup only succeed if the template parameter identifiers are equal strings. That is,
``template\<typename UOuter> Wrapper::Outer`` will not work.

The inner class template can not be directly referenced, unless the current namespace
is changed or the following shorthand is used.
If a template parameter list is omitted, then the lookup will assume either a template or a non-template,
but not a partial template specialisation.
This means the following references work.

- ``Wrapper::Outer`` (:cpp:class:`Wrapper::Outer`)
- ``Wrapper::Outer::Inner`` (:cpp:class:`Wrapper::Outer::Inner`)
- ``template\<typename TInner> Wrapper::Outer::Inner`` (:cpp:class:`template\<typename TInner> Wrapper::Outer::Inner`)

(Full) Template Specialisations
................................

Assume the following declarations.

.. cpp:class:: template<typename TOuter> \
               Outer

  .. cpp:class:: template<typename TInner> \
                 Inner

.. cpp:class:: template<> \
               Outer<int>

  .. cpp:class:: template<typename TInner> \
                 Inner

  .. cpp:class:: template<> \
                 Inner<bool>

In general the reference must include a template parameter list for each template argument list.
The full specialisation above can therefore be referenced with ``template\<> Outer\<int>`` (:cpp:class:`template\<> Outer\<int>`)
and ``template\<> template\<> Outer\<int>::Inner\<bool>`` (:cpp:class:`template\<> template\<> Outer\<int>::Inner\<bool>`).
As a shorthand the empty template parameter list can be omitted, e.g., ``Outer\<int>`` (:cpp:class:`Outer\<int>`)
and ``Outer\<int>::Inner\<bool>`` (:cpp:class:`Outer\<int>::Inner\<bool>`).


Partial Template Specialisations
.................................

Assume the following declaration.

.. cpp:class:: template<typename T> \
               Outer<T*>

References to partial specialisations must always include the template parameter lists, e.g.,
``template\<typename T> Outer\<T*>`` (:cpp:class:`template\<typename T> Outer\<T*>`).
Currently the lookup only succeed if the template parameter identifiers are equal strings.


Configuration Variables
~~~~~~~~~~~~~~~~~~~~~~~

See :ref:`cpp-config`.


The Standard Domain
-------------------

The so-called "standard" domain collects all markup that doesn't warrant a
domain of its own.  Its directives and roles are not prefixed with a domain
name.

The standard domain is also where custom object descriptions, added using the
:func:`~sphinx.application.Sphinx.add_object_type` API, are placed.

There is a set of directives allowing documenting command-line programs:

.. rst:directive:: .. option:: name args, name args, ...

   Describes a command line argument or switch.  Option argument names should be
   enclosed in angle brackets.  Examples::

      .. option:: dest_dir

         Destination directory.

      .. option:: -m <module>, --module <module>

         Run a module as a script.

   The directive will create cross-reference targets for the given options,
   referencable by :rst:role:`option` (in the example case, you'd use something
   like ``:option:`dest_dir```, ``:option:`-m```, or ``:option:`--module```).

   ``cmdoption`` directive is a deprecated alias for the ``option`` directive.

.. rst:directive:: .. envvar:: name

   Describes an environment variable that the documented code or program uses or
   defines.  Referencable by :rst:role:`envvar`.

.. rst:directive:: .. program:: name

   Like :rst:dir:`py:currentmodule`, this directive produces no output.
   Instead, it serves to notify Sphinx that all following :rst:dir:`option`
   directives document options for the program called *name*.

   If you use :rst:dir:`program`, you have to qualify the references in your
   :rst:role:`option` roles by the program name, so if you have the following
   situation ::

      .. program:: rm

      .. option:: -r

         Work recursively.

      .. program:: svn

      .. option:: -r revision

         Specify the revision to work upon.

   then ``:option:`rm -r``` would refer to the first option, while
   ``:option:`svn -r``` would refer to the second one.

   The program name may contain spaces (in case you want to document subcommands
   like ``svn add`` and ``svn commit`` separately).

   .. versionadded:: 0.5


There is also a very generic object description directive, which is not tied to
any domain:

.. rst:directive:: .. describe:: text
               .. object:: text

   This directive produces the same formatting as the specific ones provided by
   domains, but does not create index entries or cross-referencing targets.
   Example::

      .. describe:: PAPER

         You can set this variable to select a paper size.


The JavaScript Domain
---------------------

The JavaScript domain (name **js**) provides the following directives:

.. rst:directive:: .. js:module:: name

   This directive sets the module name for object declarations that follow
   after. The module name is used in the global module index and in cross
   references. This directive does not create an object heading like
   :rst:dir:`py:class` would, for example.

   By default, this directive will create a linkable entity and will cause an
   entry in the global module index, unless the ``noindex`` option is specified.
   If this option is specified, the directive will only update the current
   module name.

   To clear the current module, set the module name to ``null`` or ``None``

   .. versionadded:: 1.6

.. rst:directive:: .. js:function:: name(signature)

   Describes a JavaScript function or method.  If you want to describe
   arguments as optional use square brackets as :ref:`documented
   <signatures>` for Python signatures.

   You can use fields to give more details about arguments and their expected
   types, errors which may be thrown by the function, and the value being
   returned::

      .. js:function:: $.getJSON(href, callback[, errback])

         :param string href: An URI to the location of the resource.
         :param callback: Gets called with the object.
         :param errback:
             Gets called in case the request fails. And a lot of other
             text so we need multiple lines.
         :throws SomeError: For whatever reason in that case.
         :returns: Something.

   This is rendered as:

      .. js:function:: $.getJSON(href, callback[, errback])

        :param string href: An URI to the location of the resource.
        :param callback: Gets called with the object.
        :param errback:
            Gets called in case the request fails. And a lot of other
            text so we need multiple lines.
        :throws SomeError: For whatever reason in that case.
        :returns: Something.

.. rst:directive:: .. js:method:: name(signature)

   This directive is an alias for :rst:dir:`js:function`, however it describes a
   function that is implemented as a method on a class object.

   .. versionadded:: 1.6

.. rst:directive:: .. js:class:: name

   Describes a constructor that creates an object.  This is basically like
   a function but will show up with a `class` prefix::

      .. js:class:: MyAnimal(name[, age])

         :param string name: The name of the animal
         :param number age: an optional age for the animal

   This is rendered as:

      .. js:class:: MyAnimal(name[, age])

         :param string name: The name of the animal
         :param number age: an optional age for the animal

.. rst:directive:: .. js:data:: name

   Describes a global variable or constant.

.. rst:directive:: .. js:attribute:: object.name

   Describes the attribute *name* of *object*.

.. _js-roles:

These roles are provided to refer to the described objects:

.. rst:role:: js:mod
          js:func
          js:meth
          js:class
          js:data
          js:attr


The reStructuredText domain
---------------------------

The reStructuredText domain (name **rst**) provides the following directives:

.. rst:directive:: .. rst:directive:: name

   Describes a reST directive.  The *name* can be a single directive name or
   actual directive syntax (`..` prefix and `::` suffix) with arguments that
   will be rendered differently.  For example::

      .. rst:directive:: foo

         Foo description.

      .. rst:directive:: .. bar:: baz

         Bar description.

   will be rendered as:

      .. rst:directive:: foo

         Foo description.

      .. rst:directive:: .. bar:: baz

         Bar description.

.. rst:directive:: .. rst:role:: name

   Describes a reST role.  For example::

      .. rst:role:: foo

         Foo description.

   will be rendered as:

      .. rst:role:: foo

         Foo description.

.. _rst-roles:

These roles are provided to refer to the described objects:

.. rst:role:: rst:dir
              rst:role


More domains
------------

The sphinx-contrib_ repository contains more domains available as extensions;
currently Ada_, CoffeeScript_, Erlang_, HTTP_, Lasso_, MATLAB_, PHP_, and Ruby_
domains. Also available are domains for `Chapel`_, `Common Lisp`_, dqn_, Go_,
Jinja_, Operation_, and Scala_.


.. _sphinx-contrib: https://bitbucket.org/birkenfeld/sphinx-contrib/

.. _Ada: https://pypi.python.org/pypi/sphinxcontrib-adadomain
.. _Chapel: https://pypi.python.org/pypi/sphinxcontrib-chapeldomain
.. _CoffeeScript: https://pypi.python.org/pypi/sphinxcontrib-coffee
.. _Common Lisp: https://pypi.python.org/pypi/sphinxcontrib-cldomain
.. _dqn: https://pypi.python.org/pypi/sphinxcontrib-dqndomain
.. _Erlang: https://pypi.python.org/pypi/sphinxcontrib-erlangdomain
.. _Go: https://pypi.python.org/pypi/sphinxcontrib-golangdomain
.. _HTTP: https://pypi.python.org/pypi/sphinxcontrib-httpdomain
.. _Jinja: https://pypi.python.org/pypi/sphinxcontrib-jinjadomain
.. _Lasso: https://pypi.python.org/pypi/sphinxcontrib-lassodomain
.. _MATLAB: https://pypi.python.org/pypi/sphinxcontrib-matlabdomain
.. _Operation: https://pypi.python.org/pypi/sphinxcontrib-operationdomain
.. _PHP: https://pypi.python.org/pypi/sphinxcontrib-phpdomain
.. _Ruby: https://bitbucket.org/birkenfeld/sphinx-contrib/src/default/rubydomain
.. _Scala: https://pypi.python.org/pypi/sphinxcontrib-scaladomain
