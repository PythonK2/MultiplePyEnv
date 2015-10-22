Here we will use Homebrew as my package manager for Unix utilities on OSX.
Homebrew seems to install less dependencies than Macports or Fink.
Also, most of my Python development has settled on Python 2.7 and occasionally Python 3 for tasks that don’t require external libraries that only run on Python 2.x. So for me, an easier install with less gunk on my hard drive is a fair trade for losing the latest updates to Python 2.4, 2.5, and 2.6.

In this post, I’ll describe my setup.

The first thing to do is uninstall Macports if present.
Homebrew has fairly up-to-date versions all of the Unix packages I use on a daily basis, including git, subversion, bash_completion, Python, Qt, PyQt, and their supporting libraries. As such, I don’t need Macports taking up additional hard drive space and potentially causing conflicts with Homebrew’s packages.

Next, I installed (or update) pip, virtualenv, and virtualenvwrapper in my Mac’s default Python (2.7.10 on Mac Capitain 11) via easy_install (or pip itself). These are usually the only 3rd-party Python packages I install in the system Python’s site-packages folder.

```brew update```
(If the update fails due to read only access on /usr/local folder, add the following code to .bash_profile file:
alias fix_brew='sudo chown -R $USER /usr/local/'
Source the bash_profile file and call: source .bash_profile
Now on terminal call : fix_brew )



Next, I remove the Macports configuration from my bash ~/.bash_profile and replace it with the following:

```
# virtualenvwrapper
export WORKON_HOME=$HOME/VirtualEnvs
if [ -f /usr/local/bin/virtualenvwrapper.sh ]; then
    source /usr/local/bin/virtualenvwrapper.sh
fi

# bash completion
if [ -f `brew --prefix`/etc/bash_completion ]; then
     . `brew --prefix`/etc/bash_completion
fi

# Python 2.6.1
alias mkve26='mkvirtualenv --no-site-packages'
# Python 2.7.1
alias mkve='mkvirtualenv --no-site-packages --python=/usr/local/Cellar/python/2.7.1/bin/python'
alias mkveqt='mkvirtualenv --python=/usr/local/Cellar/python/2.7.1/bin/python'
alias designer='open /usr/local/Cellar/qt/4.7.2/bin/Designer.app'
# Python 3.5
alias mkve3='mkvirtualenv --no-site-packages --python=/usr/local/Cellar/python3/3.5.0/bin/python3'

# Ammend python path for Homebrew PyQt
export PYTHONPATH=/usr/local/lib/python:$PYTHONPATH
```
The first couple of sections set up the Terminal for bash completion and virtualenvwrapper commands. The third section sets up my virtualenv aliases: one for the default system python, one for Python 2.7, one for Python 2.7 and PyQt, and another for Python 3.2. Python 3 support was added to virtualenv in version 1.6, so you no longer need to jump through hoops to get it working with Pythyon 2.x.

Now using the virtualenv:
```
$ mkve27 py27
Running virtualenv with interpreter /opt/local/bin/python2.7
New python executable in py27/bin/python
Installing setuptools............................done.
virtualenvwrapper.user_scripts Creating /Users/daledavis/VirtualEnvs/py27/bin/predeactivate
virtualenvwrapper.user_scripts Creating /Users/daledavis/VirtualEnvs/py27/bin/postdeactivate
virtualenvwrapper.user_scripts Creating /Users/daledavis/VirtualEnvs/py27/bin/preactivate
virtualenvwrapper.user_scripts Creating /Users/daledavis/VirtualEnvs/py27/bin/postactivate
virtualenvwrapper.user_scripts Creating /Users/daledavis/VirtualEnvs/py27/bin/get_env_details
(py27)$ mkve31 py31
New python executable in /Users/daledavis/VirtualEnvs/py31/bin/python
Installing setuptools..............................................................................................................................................................................................................................................................................................................................done.
(py27)$ python --version
Python 2.7
(py27)$ workon py31
(py31)$ python --version
Python 3.1.2
```
