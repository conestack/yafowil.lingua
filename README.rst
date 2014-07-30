Documentation
=============

This package hooks up a lingua extractor for ``yafowil.yaml`` based forms.


Installation
------------

Install to system python for global availability::

    sudo pip install yafowil.lingua


Usage
-----

A ``xpot-create`` console script gets created which must be used instead of
``pot-create`` script created by ``lingua``.


Helper script
-------------

Add the following helper script to the packages you want to translate and
adopt ``DOMAIN``, ``SEARCH_PATH`` and ``LOCALES_PATH``::

    #!/bin/bash

    # configuration
    DOMAIN="mydomain"
    SEACH_PATH=src/mypackage
    LOCALES_PATH=src/mypackage/locales
    # end configuration

    # create locales folder if not exists
    if [ ! -d "$LOCALES_PATH" ]; then
        echo "Locales directory not exists, create"
        mkdir -p $LOCALES_PATH
    fi

    # create pot if not exists
    if [ ! -f $LOCALES_PATH/$DOMAIN.pot ]; then
       echo "Create pot file"
       touch $LOCALES_PATH/$DOMAIN.pot
    fi

    # no arguments, extract and update
    if [ $# -eq 0 ]; then
        echo "Extract messages"
        xpot-create $SEACH_PATH -o $LOCALES_PATH/$DOMAIN.pot

        echo "Update translations"
        for po in $LOCALES_PATH/*/LC_MESSAGES/$DOMAIN.po; do
            msgmerge -o $po $po $LOCALES_PATH/$DOMAIN.pot
        done

        echo "Compile message catalogs"
        for po in $LOCALES_PATH/*/LC_MESSAGES/*.po; do
            msgfmt -o ${po%.*}.mo $po
        done

    # first argument represents language identifier, create catalog
    else
        cd $LOCALES_PATH
        mkdir -p $1/LC_MESSAGES
        msginit -i $DOMAIN.pot -o $1/LC_MESSAGES/$DOMAIN.po -l $1
    fi


Source Code
===========

The sources are in a GIT DVCS with its main branches at 
`github <http://github.com/bluedynamics/yafowil.lingua>`_.

We'd be happy to see many forks and pull-requests ;).


Contributors
============

- Robert Niederreiter <rnix@squarewave.at>
