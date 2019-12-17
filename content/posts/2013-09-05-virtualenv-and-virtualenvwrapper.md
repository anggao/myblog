---
title: "Virtualenv and Virtualenvwrapper"
date: "2013-09-05"
template: "post"
draft: false
slug: "virtualenv-and-virtualenvwrapper"
category: "python"
tags:
  - "python"
description: ""
socialImage: ""
---

### virtualenv

- virtualenv is software for isolating python package
  environments (e.g. import paths).

- [site-packages should mostly be for system-installed python
  and not python that you are installing as a user.][1] \[1\]

- `virtualenv ENV`

- `source ENV/bin/active`

- Once the virtualenv is activated, the virtualenv's python
  (and other executables) will be on your PATH and you will
  have a new environment variable, `VIRTUAL_ENV`,
  that points to the path of the virtualenv, as well as a
  deactivate function for deactivating the virtualenv \[2\].

* `ENV/bin/deactive`

* build with `virtualenv --system-site-packages ENV`,
  virtual environment will inherit packages from
  `/usr/lib/python2.7/site-packages`

* Environment variables and configuration files
  - Environment variable: `VIRTUAL_<UPPER_NAME>`
    eg: export VIRTUAL_PYTHON=/opt/python-3.3/bin/python
  - Config files: `~/.virtualenv/virtualenv.ini`

```shell
[virtualenv]
python=/opt/python-3.3/bin/python
```

- bootstrap example:

```python
import virtualenv
script = virtualenv.create_bootstrap_script('''
    def after_install(options, home_dir):
        print options, home_dir
''')
open('my_bootstrap.py', 'w').write(script)
```

### virtualenvwrapper

- a set of extensions to `virtualenv`

- initialization steps:

  - `export WORKON_HOME="$HOME/.virtualenvs"`
  - source `/usr/local/bin/virtualenvwrapper.sh`

- `mkvirtualenv foo`

- `cdvirtualenv`

- `workon` list all virtualenv

- `workon env1` switch between environements with `workon`

- `deactive`

- `lssitepackages`

- `cdsitepackages`

- `setvirtualenvproject` bind an existing virtualenv to an existing project

\[1\]: [http://blog.ianbicking.org/site-packages-considered-harmful.html][1]

\[2\]: [https://developer.mozilla.org/en-US/docs/Python/Virtualenv][2]

[1]: http://blog.ianbicking.org/site-packages-considered-harmful.html
[2]: https://developer.mozilla.org/en-US/docs/Python/Virtualenv
