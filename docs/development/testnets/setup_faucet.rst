
.. _how-to setup-faucet:

How to Set up the Faucet
===================================

.. contents:: Table of Contents
   :local:
   
-------

In order to allow people who do not have funds yet to enter the system, we need to setup a faucet.

Tapin
----------

If you are interested Python-based faucet, check out `Tapin <https://github.com/bitshares/tapin>`_ . Tapin is a python-based faucet for Graphene-based blockchains (e.g. DNA).


----------------
 
Here, we will also use *mina* as our **deployment tool** for a production installation.

1. Installation of Dependencies
--------------------------------------------

Install every other dependency that is needed and not yet installed::

    sudo apt-get install mysql-server libmysqlclient-dev
    # put a master password for mysql

Also install a decently recent version of Ruby::

    cd
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    exec $SHELL

    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL

    git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

    sudo rbenv install 2.2.3
    sudo rbenv global 2.2.3
    sudo gem install bundle

	
2. Get the Source 
--------------------------------------------

::

    git clone https://github.com/cryptonomex/faucet
    cd faucet
    sudo bundle   # ignore warnings


3. Configuration
--------------------------------------------


- 1 ) the faucet itself,
- 2 ) rails API secrets,
- 3 ) database access,
- 4 ) and the deployment tool, mina.

**1) Faucet**

All required settings are in config/faucet.yml which has an example file::

    cp config/faucet-example.yml config/faucet.yml
    vim config/faucet.yml

**2) Rails API**

Rails needs to know a secret for their internals, we can get a new one with rake secret. Put it into the corresponding lines in the config file.::

    cp config/secrets-example.yml config/secrets.yml
    rake secret
    vim config/secrets.yml

**3a) Database Access**

The default configuration for the database access expects an empty root password for MySQL. Hence, if you have set a different password, you need to provide it here.::

    vim config/database.yml

We generate our databases with::

    rake db:create; rake db:migrate; rake db:seed
    RAILS_ENV=production bundle exec rake db:create db:schema:load

**3b) Database Settings**

We also need to add an entry to the database so that page loads and referrals work properly::

   1. Go to `/www/current`
   2. execute: `rails db`, a mysql console will open
   3.  Execute: `insert into widgets set allowed_domains='testnet.bitshares.eu'`; (replace the domain with your domain)


**4) Mina deployment**

Mina is used to deploy the faucet properly and move all the required files over to the production machine (may be the same machine).::

    cp config/deploy-example.yml config/deploy.yml
    vim config/deploy.yml

Make sure to add your ssh-pub key to your authorized file so that mina can deploy to the corrseponding machine.

Create the public directly that is served and copy/link the configuration file to the deployment’s shared files.

::

    sudo mkdir /www
    sudo chown -R gph:gph /www

    mina setup
    ln -s $HOME/faucet/config/faucet.yml /www/shared/config/
    ln -s $HOME/faucet/config/secrets.yml /www/shared/config/
    ln -s $HOME/faucet/config/database.yml /www/shared/config/

Deploy mina and the wallet with::

    ln -s $HOME/graphene-ui/web/dist $HOME/faucet/public/wallet
    mina deploy
    mina wallet



|

