.. _cli:
.. _commands:

======================
Command Line Interface
======================

.. _completion:

Completion
----------

In bash (``~/.bashrc``):

.. code-block:: sh

    eval "$(_TMUXP_COMPLETE=source tmuxp)"

In zsh (``~/.zshrc``):

.. code-block:: sh

    eval "$(_TMUXP_COMPLETE=source_zsh tmuxp)"

.. _cli_shell:

Shell
-----

::

    tmuxp shell

    tmuxp shell <session_name>

    tmuxp shell <session_name> <window_name>

    tmuxp shell -c 'python code'

Launch into a python console with `libtmux`_ objects. Compare to django's shell.

  .. image:: _static/tmuxp-shell.gif
     :width: 100%

Automatically preloads current tmux :class:`server <libtmux.Server>`,
:class:`session <libtmux.Session>`, :class:`window <libtmux.Window>` 
:class:`pane <libtmux.Pane>`. Pass additional arguments to select a
specific one of your choice::

    (Pdb) server
    <libtmux.server.Server object at 0x7f7dc8e69d10>
    (Pdb) server.sessions
    [Session($1 your_project)]
    (Pdb) session
    Session($1 your_project)
    (Pdb) session.name
    'your_project'
    (Pdb) window
    Window(@3 1:your_window, Session($1 your_project))
    (Pdb) window.name
    'your_window'
    (Pdb) window.panes
    [Pane(%6 Window(@3 1:your_window, Session($1 your_project)))
    (Pdb) pane
    Pane(%6 Window(@3 1:your_window, Session($1 your_project)))

Python 3.7 supports `PEP 553`_'s ``PYTHONBREAKPOINT`` and supports
compatible debuggers, for instance `ipdb`_:

.. code-block:: sh

   $ pip install ipdb
   $ env PYTHONBREAKPOINT=ipdb.set_trace tmuxp shell

You can also pass in python code directly, similar to ``python -c``, do
this via ``tmuxp -c``:

.. code-block:: shell

   $ tmuxp shell -c 'print(session.name); print(window.name)'
   my_server
   my_window

   $ tmuxp shell my_server -c 'print(session.name); print(window.name)'
   my_server
   my_window

   $ tmuxp shell my_server my_window -c 'print(session.name); print(window.name)'
   my_server
   my_window

   $ tmuxp shell my_server my_window -c 'print(window.name.upper())'
   MY_WINDOW

   # Assuming inside a tmux pane or one is attached on default server
   $ tmuxp shell -c 'print(pane.id); print(pane.window.name)'
   %2
   my_window

.. _PEP 553: https://www.python.org/dev/peps/pep-0553/
.. _ipdb: https://pypi.org/project/ipdb/
.. _libtmux: https://libtmux.git-pull.com

.. _cli_freeze:

Freeze sessions
---------------

::

    tmuxp freeze <session_name>

You can save the state of your tmux session by freezing it.

Tmuxp will offer to save your session state to ``.json`` or ``.yaml``.

.. _cli_load:

Load session
------------

You can load your tmuxp file and attach the vim session via a few
shorthands:

1. The directory with a ``.tmuxp.{yaml,yml,json}`` file in it
2. The name of the project file in your `$HOME/.tmuxp` folder
3. The direct path of the tmuxp file you want to load

::

    # path to folder with .tmuxp.{yaml,yml,json}
    tmuxp load .
    tmuxp load ../
    tmuxp load path/to/folder/
    tmuxp load /path/to/folder/

    # name of the config, assume $HOME/.tmuxp/myconfig.yaml
    tmuxp load myconfig

    # direct path to json/yaml file
    tmuxp load ./myfile.yaml
    tmuxp load /abs/path/to/myfile.yaml
    tmuxp load ~/myfile.yaml

Absolute and relative directory paths are supported.

.. code-block:: bash

    $ tmuxp load <filename>

Files named ``.tmuxp.yaml`` or ``.tmuxp.json`` in the current working
directory may be loaded with:

.. code-block:: bash

    $ tmuxp load .

Multiple sessions can be loaded at once. The first ones will be created
without being attached. The last one will be attached if there is no
``-d`` flag on the command line.

.. code-block:: bash

    $ tmuxp load <filename1> <filename2> ...

A session name can be provided at the terminal. If multiple sessions 
are created, the last session is named from the terminal.

.. code-block:: bash

    $ tmxup load -s <new_session_name> <filename1> ...

.. _cli_import:

Import
------

.. _import_teamocil:

From teamocil
~~~~~~~~~~~~~

::

    tmuxp import teamocil /path/to/file.{json,yaml}

.. _import_tmuxinator:

From tmuxinator
~~~~~~~~~~~~~~~

::

    tmuxp import tmuxinator /path/to/file.{json,yaml}

.. _convert_config:

Convert between YAML and JSON
-----------------------------

::

    tmuxp convert /path/to/file.{json,yaml}

tmuxp automatically will prompt to convert ``.yaml`` to ``.json`` and
``.json`` to  ``.yaml``.
