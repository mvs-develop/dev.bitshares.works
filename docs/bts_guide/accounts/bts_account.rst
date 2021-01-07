
.. _bts-accounts:


Account
========================


In DNA, you can create readable and memorable own unique account name for your DNA account. You do not need to write down a long random number as your account name. The DNA account name would be helpful to identify other DNA accounts.


.. contents:: Table of Contents

-----------


When you first time connect to one of DNA UI wallets, you will find **Welcome to DNA** page. As a default, you will create a Cloud Wallet. You can imagine the Cloud Wallet smiler to your normal bank account. You remember your account name and a password and the bank manage and save your funds for you.

A Local Wallet, as opposed to a Cloud Wallet, is another type of Wallet. You create your account name and a password and also you have to manage your funds. That means you have to create your account backup files and keep them in a safe place.  The Local Wallet creation form is the advanced form. You can find the link on the same "Welcome to DNA" page.

It's important to understand that **only** you know your password, and no one can recover it.  The account information is registered to the blockchain with private/public keys of the account. Only the account creator knows the password and finds out the private keys. We will look into three types of keys (i.e., active, owner, and memo key) in anther section.


DNA UI Wallet
-----------------------

If you create an account at one of DNA partners, the account name will be available among DNA partners.  So, you will be able to use the DNA account name to communicate (e.g., sending fund) with **other DNA account holders** like sending an email. The advantage of using account name is you, and other people can identify the account holder.

- If you want to see the DNA wallet, go to this link. It opens `DNA UI wallet <https://mvsdna.info>`_


Identifier
-------------

When you create a DNA account, DNA 2.0 registers the account name to the blockchain and also assigns an incrementing identifier (account id) during the registration. You will not need to remember the account_id while you use the DNA wallet operations (i.e., send fund, place order, etc).

If you want to know your acount_id. go to a `cryptofresh <https://www.cryptofresh.com/>`_ and search your account name. You will find your account id under your account name.


*The identifier (account_id) comes with many advantages:*
Besides improved scalability, we have separated the identity from the transaction authorizing signature. In practice, owning an account name is autonomous from being able to spend its funds. Furthermore, both rights (we call them permissions) can split among an arbitrary complex relation of people (we call them authorities) using weights and required thresholds.

To separating authorities from identities, DNA 2.0 can be much faster in processing delay while having much smaller transaction sizes. All participants are forced to have a named account on the blockchain. Also, most transactions are tied to the account name and can be linked to individuals (this includes transfers, trades, shorts, etc. but not stealthed transactions).

|

Accounts and keys
------------------------

.. image:: ../../_static/structures/ui_create_account_keys.png
        :alt: UI Account create
        :width: 700px
        :align: center


|
