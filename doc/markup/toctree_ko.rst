.. highlight:: rst
.. _toctree-directive:

TOC tree
============

.. index:: pair: table of; contents

reST에는 여러 문서를 서로 연결하거나 문서를 여러 출력 파일로 분할 할 수 있는 기능이 없기 때문에 Sphinx는 custom directive를 사용하여 문서들 관의 관계와 목차를 추가한다. ``toctree`` directive는 이를 위한 핵심 요소이다.

.. note::

   하나의 파일에 다른 파일을 간단하게 "포함"시키는 것은 :dudir:`include` directive로 가능하다.

.. rst:directive:: toctree

   이 directive는 directive 본문에 지정된 문서의 개별 TOC("sub-TOC tree" 포함)를 사용하여 현재 위치에 "TOC tree"를 삽입한다. 문서의 상대적 이름(슬래시로 시작하지 않음)은 directive가 있는 문서에 대해 상대적이며, 절대적 이름은 소스 디렉토리에 대해 상대적이다. 숫자 ``maxdepth`` 옵션은 트리의 깊이를 나타내기 위해 지정할 수 있다. 기본적으로는 모든 레벨이 포함된다. [#]_

   다음 예제를 고려해보자(Python 문서의 library reference index에서 가져옴)::

      .. toctree::
         :maxdepth: 2

         intro
         strings
         datatypes
         numeric
         (many more documents listed here)

   이것은 다음 두 가지를 수행한다 :

   * 연관된 문서들에 있는 ``toctree`` directive 또한 고려하여, 모든 문서의 깊이 2까지의 목차를 삽입한다.
* Sphinx는 문서의 ``intro``, ``string`` 등의 상대적인 순서를 알고 있으며, 현재 문서(library index)의 하위 항목임을 알고 있다. 이 정보를 이용하여 "다음 장", "이전 장" 및 "상위 장" 링크를 생성한다.

   **Entries**

   Document titles in the :rst:dir:`toctree` will be automatically read from the
   title of the referenced document. If that isn't what you want, you can
   specify an explicit title and target using a similar syntax to reST
   hyperlinks (and Sphinx's :ref:`cross-referencing syntax <xref-syntax>`). This
   looks like::

       .. toctree::

          intro
          All about strings <strings>
          datatypes

   The second line above will link to the ``strings`` document, but will use the
   title "All about strings" instead of the title of the ``strings`` document.

   You can also add external links, by giving an HTTP URL instead of a document
   name.

   **Section numbering**

   If you want to have section numbers even in HTML output, give the
   **toplevel** toctree a ``numbered`` option.  For example::

      .. toctree::
         :numbered:

         foo
         bar

   Numbering then starts at the heading of ``foo``.  Sub-toctrees are
   automatically numbered (don't give the ``numbered`` flag to those).

   Numbering up to a specific depth is also possible, by giving the depth as a
   numeric argument to ``numbered``.

   **Additional options**

   You can use ``caption`` option to provide a toctree caption and you can use
   ``name`` option to provide implicit target name that can be referenced by
   using :rst:role:`ref`::

      .. toctree::
         :caption: Table of Contents
         :name: mastertoc

         foo

   If you want only the titles of documents in the tree to show up, not other
   headings of the same level, you can use the ``titlesonly`` option::

      .. toctree::
         :titlesonly:

         foo
         bar

   You can use "globbing" in toctree directives, by giving the ``glob`` flag
   option.  All entries are then matched against the list of available
   documents, and matches are inserted into the list alphabetically.  Example::

      .. toctree::
         :glob:

         intro*
         recipe/*
         *

   This includes first all documents whose names start with ``intro``, then all
   documents in the ``recipe`` folder, then all remaining documents (except the
   one containing the directive, of course.) [#]_

   The special entry name ``self`` stands for the document containing the
   toctree directive.  This is useful if you want to generate a "sitemap" from
   the toctree.

   You can use the ``reversed`` flag option to reverse the order of the entries
   in the list. This can be useful when using the ``glob`` flag option to
   reverse the ordering of the files.  Example::

      .. toctree::
         :glob:
         :reversed:

         recipe/*

   You can also give a "hidden" option to the directive, like this::

      .. toctree::
         :hidden:

         doc_1
         doc_2

   This will still notify Sphinx of the document hierarchy, but not insert links
   into the document at the location of the directive -- this makes sense if you
   intend to insert these links yourself, in a different style, or in the HTML
   sidebar.

   In cases where you want to have only one top-level toctree and hide all other
   lower level toctrees you can add the "includehidden" option to the top-level
   toctree entry::

      .. toctree::
         :includehidden:

         doc_1
         doc_2

   All other toctree entries can then be eliminated by the "hidden" option.

   In the end, all documents in the :term:`source directory` (or subdirectories)
   must occur in some ``toctree`` directive; Sphinx will emit a warning if it
   finds a file that is not included, because that means that this file will not
   be reachable through standard navigation.

   Use :confval:`exclude_patterns` to explicitly exclude documents or
   directories from building completely.  Use :ref:`the "orphan" metadata
   <metadata>` to let a document be built, but notify Sphinx that it is not
   reachable via a toctree.

   The "master document" (selected by :confval:`master_doc`) is the "root" of
   the TOC tree hierarchy.  It can be used as the documentation's main page, or
   as a "full table of contents" if you don't give a ``maxdepth`` option.

   .. versionchanged:: 0.3
      Added "globbing" option.

   .. versionchanged:: 0.6
      Added "numbered" and "hidden" options as well as external links and
      support for "self" references.

   .. versionchanged:: 1.0
      Added "titlesonly" option.

   .. versionchanged:: 1.1
      Added numeric argument to "numbered".

   .. versionchanged:: 1.2
      Added "includehidden" option.

   .. versionchanged:: 1.3
      Added "caption" and "name" option.

Special names
-------------

Sphinx reserves some document names for its own use; you should not try to
create documents with these names -- it will cause problems.

The special document names (and pages generated for them) are:

* ``genindex``, ``modindex``, ``search``

  These are used for the general index, the Python module index, and the search
  page, respectively.

  The general index is populated with entries from modules, all index-generating
  :ref:`object descriptions <basic-domain-markup>`, and from :rst:dir:`index`
  directives.

  The Python module index contains one entry per :rst:dir:`py:module` directive.

  The search page contains a form that uses the generated JSON search index and
  JavaScript to full-text search the generated documents for search words; it
  should work on every major browser that supports modern JavaScript.

* every name beginning with ``_``

  Though only few such names are currently used by Sphinx, you should not create
  documents or document-containing directories with such names.  (Using ``_`` as
  a prefix for a custom template directory is fine.)

.. warning::

   Be careful with unusual characters in filenames.  Some formats may interpret
   these characters in unexpected ways:

   * Do not use the colon ``:`` for HTML based formats.  Links to other parts
     may not work.

   * Do not use the plus ``+`` for the ePub format.  Some resources may not be
     found.

.. rubric:: Footnotes

.. [#] The LaTeX writer only refers the ``maxdepth`` option of first toctree
       directive in the document.

.. [#] A note on available globbing syntax: you can use the standard shell
       constructs ``*``, ``?``, ``[...]`` and ``[!...]`` with the feature that
       these all don't match slashes.  A double star ``**`` can be used to match
       any sequence of characters *including* slashes.
