How to import an existing delegate as witness in DNA 2.0
=========================================================================

Preparations in DNA 0.9 network
--------------------------------------------

We need to Extract the signing public and private key from DNA 0.9.

Let's obtain the <publickey>::

    >>> get_account <delegatename>
    [...]
    Block Signing Key: <publickey>
    [...]

Remark: Public keys in the DNA network have the prefix BTS. Hence, in the case of the Graphene testnet you should replace BTS by GPH.

and the corresponding <wifkey>::

    >>> wallet_dump_account_private_key <delegatename> signing_key
    "<wifkey>"
