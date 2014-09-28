scientific-python-quick-primer
==============================

A quick hands-on Scientific Python primer

## About

This primer is a mini course for a small-to-medium sized group of computational
scientists, mainly physicists.
Its aim is to introduce the SciPy stack and in particular, the IPython
notebook, to attendees who have a computational science background, but not
necessarily experienced Python before.
For a quick IPython notebook experience, we cater for serving the notebook to
participants.
This hopefully saves time since participants do not need to have to tediously
set up everything first.
Plus you will have a defined environment, more specifically, an environment
defined by you.
Of course, this needs your machine to be accessible by the participants over
the network.

## Presentation: The Wonderful World of Scientific Python

    ipython nbconvert --profile-dir ipython-scipy-primer the-wonderful-world-of-scientific-python.ipynb --to slides --post serve

## Setup

### Set up the Python virtual environment and the IPython profile

To set up the Python virtual environment with Python 3.4:

    pyvenv-3.4 pyvenv-scipy-primer

    # activate virtual environment
    source pyvenv-scipy-primer/bin/activate

    # install scipy stack
    pip install ipython[all] numpy scipy matplotlib numexpr

    # test scipy stack
    iptest
    python -c "import numpy; numpy.test('full')"
    python -c "import scipy; scipy.test('full')"
    python -c "import matplotlib; matplotlib.test()"
    python -c "import numexpr; numexpr.test()"

    # clean up matplotlib test
    rm -Rf result_images

    # create IPython profile
    ipython profile create --profile-dir=ipython-scipy-primer

    # install MathJax
    python -m IPython.external.mathjax

    # install version_information extension
    ipython --profile-dir ipython-scipy-primer -c "%install_ext http://raw.github.com/jrjohansson/version_information/master/version_information.py"

    # install networkx
    pip install pyyaml
    pip install -e git+https://github.com/pygraphviz/pygraphviz#egg=pygraphviz
    pip install networkx

    # test networkx
    python -c "import pygraphviz; pygraphviz.test()"
    python -c "import networkx; networkx.test()"

    # install basemap
    pip install -e git+https://github.com/matplotlib/basemap#egg=basemap
    

The following set-up is only needed for the "normal user", not the "ipnb" user:

    # install nbviewer
    sudo apt-get libmemcached-dev libcurl4-openssl-dev pandoc libevent-dev
    
    # install pylibmc from source due to Python 3 complications
    pip install -e git+https://github.com/lericson/pylibmc#egg=pylibmc
    pip install -r https://raw.githubusercontent.com/ipython/nbviewer/master/requirements.txt
    pip install -e git+https://github.com/ipython/nbviewer#egg=nbviewer

    # install further dependencies
    pip install netifaces runipy

### Set up a local user for serving the notebook to participants

In order to serve the notebook to participants, think twice before you do it
under your own user.
Hint: IPython grants full shell access.
You might want to secure your local home directory, and prune the ``/tmp``
directory.
Bottom line: do this only if you trust people and their machines on the
network...

    # create new user
    sudo useradd --create-home --user-group ipnb

    # login as new user
    sudo su ipnb
    cd ~

    # clone the git repository
    mkdir -p repos
    cd repos
    git clone https://github.com/andsor/scientific-python-quick-primer.git
    cd scientific-python-quick-primer

Now repeat the steps to set up Python virtual environment and IPython profile.

## Serving the notebook

### Start nbviewer for participants

Do this as the normal user:

    # activate virtual environment
    source pyvenv-scipy-primer/bin/activate

    python -m nbviewer --localfiles=participants-viewer --no-cache


The link to the index noteboook:

    # activate virtual environment
    source pyvenv-scipy-primer/bin/activate

    python -c "import netifaces; print( \
    'http://{}:5000/localfile/index.ipynb'.format( \
    netifaces.ifaddresses('wlan0')[netifaces.AF_INET][0]['addr'] \
    ))" > nbviewer-url
    python -c "import netifaces; print( \
    'http://{}:8887'.format( \
    netifaces.ifaddresses('wlan0')[netifaces.AF_INET][0]['addr'] \
    ))" > notebook-url

    runipy participants-index.ipynb participants-viewer/index.ipynb

    echo; echo "Send this link to participants: "; cat nbviewer-url
    

### Serve the notebook to participants

Better serve the notebook as another user for security reasons!
(IPython notebook grants full shell access, and file access!)

    # login as ipnb user
    sudo su ipnb
    cd $HOME/repos/scientific-python-quick-primer

    # activate virtual environment
    source pyvenv-scipy-primer/bin/activate
    ipython notebook --profile-dir $PWD/ipython-scipy-primer --notebook-dir \
        participants --ip '*' --no-browser --port 8887

### Serve the notebook to instructor
   
As "normal" user:

    # activate virtual environment
    source pyvenv-scipy-primer/bin/activate
    ipython notebook --profile-dir $PWD/ipython-scipy-primer

## License

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">A quick hands-on Scientific Python primer</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/andsor/scientific-python-quick-primer" property="cc:attributionName" rel="cc:attributionURL">Andreas Sorge</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
