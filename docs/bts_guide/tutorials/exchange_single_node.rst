
.. _exchange-single-node:

***************************************************************
How to prepare DNA Exchange Guide (Single Node Edition)
***************************************************************

The purpose of this article is to assist third-party exchanges to go online for DNA transactions.

The scheme described in this section is a single node scheme. Compared to the two-node scenario described in another document, the single-node solution can save memory, hard disk, and synchronization time.

.. contents:: Table of Contents
   :local:

-------


1. Basic Concept
==============================================

**1.1 Consensus**

The DNA uses the DPOS consensus mechanism to vote for the block forge by the person holding the DNA. The standard block interval is 3 seconds.

**1.2 Account**

1) In DNA, funds are stored in an account, unlike Bitcoin where there is an address. For the exchange, it is necessary to open an account for users to deposit.

  - You can use a web wallet or light wallet to register a new account.

.. Note:: For the exchange, please use the wallet mode (Local Wallet) instead of the account mode  (Cloud Wallet) for the registered account, because the exchange needs to use some advanced functions and there will be problems in the account mode.

  - `DNA-UI Release <https://github.com/bitshares/bitshares-ui/releases>`_
  - BitShare UI wallet:  https://wallet.bitshares.org
  - `Create an Account Guide <http://how.bitshares.works/en/latest/user_guide/create_account.html>`_


- Not all accounts can be registered for free. Generally, a flat or numeric account can be registered for free, such as my-exchange, or myexchange2017.

- In the light wallet account page, a number is displayed below the account number. This number is the built-in ID of the account in the DNA system, which will be used below.

.. Hint:: You can go to the blockchain browser https://cryptofresh.com/ and enter the account name to get the account ID.

- You can also use the command to obtain the ID in the wallet after the synchronization of the self-built node is completed, and you can obtain the command reference section *Checking the name of the account for withdrawal*.

.. Attention:: For the sake of fund safety, the exchange can use one account to deposit (charge) the money, and the another account can be used to withdraw money.

2) User's deposit is transferred from other accounts to the open account of the exchange.

- The account name is the payment address.

- Each remittance can carry one Memo (note). The exchange uses this Memo to distinguish which user's deposit.

- The specific Memo are related to the exchange user's relationship, and the exchange must set it up.

- Memo are encrypted and only the memo (note) key with the sender or receiver can be decrypted.

3) User withdrawals are transferred from the exchange account to the user account. The destination account name is provided by the user.

- Since there are users who need to transfer funds directly from one exchange to another exchange, and another exchange is based on Memo, the cash withdrawal function can best take Memo (notes).

4) The account registered using the web wallet is the basic account. Can be upgraded to a lifetime membership (LTM) account, after the upgrade, the subsequent transaction fee to save 80%.

- Lifetime members can create new accounts.

- The current fee rate standard can be viewed in the wallet. From the interface, click [Explorer] - [Fee Schedule] tab to access.

5) Each account has 3 pairs of keys by default, which can be viewed on a Permissions page from the side menu. They are: **Active**, **Owner**, and **Memo** keys.

- Among them, the *active* authority key is used for daily operations such as transfer; the *owner* authority key is used to modify the key; the *memo* key is used to encrypt and decrypt the transfer Memo.

- By default, the active permission key is the same as the memo key, but it can be modified to be different.

- The above 3 pairs of keys can be modified. Among them, the owner (account privilege) is the highest privilege, and all keys can be modified; using the owner (active privilege) key, the owner (account privilege) key cannot be modified, but the other two keys can be modified.


**1.3 Assets**

1) There are various assets in the DNA system, of which the core asset is the DNA. The approach of the exchange to other assets in the DNA system is similar to that of the DNA.
2) Each account can have multiple assets at the same time.

**1.4 BlockChain Structure**

- Each block has an ID, block_id, which is the hash value of the block contents.
- Each block contains the ID of the previous block, stored in the *previous* field, thus forming a chain;
- Each block contains multiple transactions, stored in the *transactions* field and stored in order;
  - When using the API to get block information, it also returns the *transaction_ids* field, which is the list of transaction IDs. It is the serialized hash value of the transaction (without signature).
- Each transaction can contain multiple operations, stored in the *operations* field and stored in order;
  - Each operation also has an ID, which is a global number, generated internally during program execution, not a hash value.

  Read: :ref:`Block Component information <lib-block>`

-----------------------

2. Basic Software and Hardware Requirements
==============================================

- Stand-alone server or VPS
- 8G memory (more and better)
- 50G hard disk

Install 64-bit Ubuntu 16.04 LTS (it will not work on 32-bit Ubuntu), or 64-bit Ubuntu 14.04 LTS, or Windows Server.

> See Also, :ref:`System Requirements <system-requirements-node>`

------------------

3. Program Preparation
==============================================

To start-off the DNA system, you need to run these programs: normal node ``witness_node``, command line wallet ``cli_wallet``.

**3.1 Architecture Description**

- The ``witness_node`` is connected to the DNA network in a P2P manner, receives the latest block from the network, and broadcasts the locally signed transaction packet to the network.
- The ``witness_node`` provides an API for other program calls (hereafter referred to as node APIs) via websocket + HTTP RPC.
- ``Cli_wallet`` connects to ``witness_node`` via websocket.
- ``Cli_wallet`` manages wallet files, which contain an encrypted user private key, and a wallet file can contain multiple private keys.
- You can run multiple ``cli_wallet`` processes at the same time and connect to the ``witness_node`` to manage multiple wallet files at the same time.
- ``Cli_wallet`` provides the transaction signature function, which is broadcast after being signed by the ``witness_node``.
- ``Cli_wallet`` provides APIs for other program calls (hereafter referred to as wallet APIs) via HTTP RPC.

.. Attention:: The recommended exchange uses a ``cli_wallet`` to monitor user deposit and another ``cli_wallet`` to handle user withdrawal requests.


**3.2 Windows**

The compiled Windows executable is available for download on Github, at https://github.com/mvs-org/dna-core/releases/latest ,

The file is `DNA-Core-2.0.xxxxxx-x64-cli-tools.zip` and it can be unzipped. It contains three exe files and two dll files. Here is the :ref:`installation guide: CLI-Wallet on Windows (x64) <cli-tool>`


**3.3 Linux**

If you are using a Linux system, you need to compile several of these programs yourself. Ubuntu 16.04 LTS (64 bit) is recommended. The compilation steps are as follows:

::

	sudo apt-get update
	sudo apt-get install autoconf cmake git libboost-all-dev libssl-dev doxygen g++ libcurl4-openssl-dev

	git clone https://github.com/bitshares/bitshares-core.git
	cd bitshares-core
	git checkout <LATEST_RELEASE_TAG>
	git submodule update --init --recursive
	mkdir build
	cd build
	cmake -DCMAKE_BUILD_TYPE=Release ..
	make witness_node cli_wallet


Read also: :ref:`Installation Guide <installation-guide>`

.. Note:: In the above steps, replace <LATEST_RELEASE_TAG> with the latest release number.

After the compilation is complete, two executable programs are:

* build/programs/witness_node/witness_node
* build/programs/cli_wallet/cli_wallet

The above program can be copied to other directories or other servers for execution. By default, the program is considered to be in the current directory.

.. Note:: When copying to other servers for execution, if the server operating system or other hardware and software environments are different, they may not be used.

If you use Ubuntu 14.04 LTS (64 bit), you need to compile and install the Boost library before performing the above steps.

Please note that currently only Boost libraries from 1.57.0 to 1.65.0 are supported.

The steps to compile and install the Boost library are:

::

	sudo apt-get install cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git autotools-dev build-essential g++ libbz2-dev libicu-dev python-dev doxygen

	wget -c 'http://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.bz2/download' -O boost_1_57_0.tar.bz2
	tar xjf boost_1_57_0.tar.bz2
	cd boost_1_57_0
	./bootstrap.sh
	sudo ./b2 install

It is also possible to compile with other Linux distributions, which is beyond the scope of this article.

-------------------------

4. Environmental Preparation
==============================================

To ensure the normal operation of the system, you need to ensure that the **server system time** is correct. Inaccurate times can cause problems such as the failure of synchronization of blockchains and the failure of funds to be sent.

Ubuntu system is recommended to install NTP server by::

	Sudo timedatectl set-ntp false
	Sudo apt-get install ntp

Depending on the deployment environment, you may need to modify the default ntp server address.

If it is a Windows system, set the system time synchronization.

---------------------------

5. Synchronize Data
==============================================

Since it is necessary to run multiple programs at the same time, Ubuntu recommends starting the program on *screen* or *tmux*.

The following description is mainly for Ubuntu, so the command comes with ``./``. For Windows, after the command line interface cd to the program directory, ``./`` is not required for execution.


**5.1 witness_node**

You can use ``./witness_node --help`` to see the command parameters.

5.1.1 Initial Implementation::

	./witness_node -d witness_node_data_dir

Then press ``Ctrl+C`` to end it.

This will generate a data directory in the current directory, ``witness_node_data_dir``, which contains the blockchain directory for the data store and a configuration file ( :ref:`config.ini <bts-config-ini-eg>` ).

For exchanges, some modifications to the ``config.ini`` configuration file are recommended.

1) You can close the p2p log to reduce disk storage pressure by finding the ``filename=logs/p2p/p2p.log`` line and adding the # sign to the beginning of the line. Or change ``level=info below [logger.p2p]`` to level=error

2) Consider saving the console log to a file at the same time by using the following sections::

		[logger.default]
		level=info
		appenders=stderr

  change into::

		[log.file_appender.default]
		filename=logs/default/default.log

		[logger.default]
		level=info
		appenders=stderr,default

After this, the console log for the last 24 hours is kept under the ``witness_node_data_dir/logs/default/`` directory.

3) The following parameters will reduce the memory required for operation. The principle is that the historical transaction record index of the DNA built-in transaction engine is not saved because the exchange does not normally use this data.::

	   history-per-size = 0

  If it is 2.0.171105a and later, you need to set this parameter::

	   plugins = witness account_history


- Read more: :ref:`memory-nodes`


**Note:**

* The default plugins in ``config.ini`` have a "#" symbol and need to be deleted;
* The default plugins configuration is `witness account_history market_history`, where `market_history` is actually removed;
* If the configuration item is not found in `config.ini`, for example, it will not update the existing configuration file when    upgrading from an old version.

  * You can add a line to the front of `config.ini` (don't add it to the end of the file),
  * You can also find an empty directory to generate a `config.ini` file and edit it again.

4) The following parameters indicate how many history records are kept for each account. The default value is 1000.

  For exchanges, if you have more depossit and withdrawal records, consider setting a larger value, such as::

		max-ops-per-account = 1000

  change into::

		max-ops-per-account = 1000000

It will retain one million data. Earlier data is deleted from memory and cannot be queried quickly (but still recorded on the chain).

5) The following two parameters will greatly reduce the memory required for the operation, the principle is not to save the historical data index which is not related to the exchange account.::

		track-account = "1.2.12345"
		partial-operations = true

Please replace 12345 with your account's digital ID. The "1.2." before the number indicates that the type is an account.

If you need to monitor multiple accounts, use multiple ``track-account`` configurations, such as::

	track-account = "1.2.12345"
	track-account = "1.2.12346"
	partial-operations = true

The configuration of multiple track-accounts will cause the above log changes to not take effect.

The way to get around this problem is not to move config.ini , but to start witness_node, add the --track-account parameter after the command line, for example::

	./witness_node --track-account "\"1.2.12345\"" --track-account "\"1.2.12346\""

**Note:**

* The first and last quotes of the parameter need to be preserved, so escape using `\`. Linux can use double quotation marks plus a single quotation mark, which does not require escaping.
* If you need to add, modify or delete the tracking account, after the modification, you need to rebuild the index to take effect.

To do this, press ``Ctrl + C`` to end the program and restart it with the ``--replay-blockchain`` parameter, such as::

	./witness_node -d witness_node_data_dir --track-account "\"1.2.12345\"" --track-account "\"1.2.12346\"" --replay-blockchain

* Read more information about :ref:`memory-nodes`

5.1.2 Re-execution

Start ``witness_node`` again and start synchronizing data. Depending on the network conditions and server hardware conditions, initial synchronization may take several hours to several days.::

	./witness_node -d witness_node_data_dir --rpc-endpoint 127.0.0.1:8090
	                                        --track-account "\"1.2.12345\""
						--track-account "\"1.2.12346\""
						--partial-operations true
						--max-ops-per-account 1000000
						--replay-blockchain

In the above command, use ``--rpc-endpoint`` to enable the node API service so that you can use ``cli_wallet`` to connect with other programs.

.. Note::  When you need to restart the `witness_node` later, generally do not add the ``--replay-blockchain`` parameter, otherwise startup will be slow


**5.2 Running a ``cli_wallet`` to Process Withdraw**

::

	./cli_wallet -w wallet_for_withdrawal.json -s ws://127.0.0.1:8090 -H 127.0.0.1:8091

The above command uses the ``-w`` parameter to specify the wallet file, the ``-s`` parameter connects to the witness_node, and the ``-H`` parameter opens the wallet API service, listening on port 8091

**Note:**

- You can use ``./cli_wallet --help`` to see the command parameters.
- The data communicated between the ``cli_wallet`` and the ``witness_node`` does not contain private data. Generally, no encryption is needed and no deliberate protection is required for the RPC port of the node (it is not necessary to add a layer of protection).
- However, the communication between ``cli_wallet`` and the `deposit program` is in plain text and may need to include the password. If the deployment is a multi-machine architecture, you need to pay attention to encryption and use `SSH tunneling`.
- In addition, when the ``cli_wallet`` is in the `unlocked` state, the funds in the wallet account can be transferred through the RPC port. Care must be taken to prevent unauthorized access, and it is strongly recommended that the wallet RPC directly open public network access.
- The practice of configuring certificates or passwords for the cli_wallet's RPC has not been studied and is therefore not described.

Successful execution will show::

	Please use the set_password method to initialize a new wallet before continuing
	new >>>


* **For more detailed instructions, see the tutorial on** :ref:`How to Set a password and Unlock a Cli Wallet <cli-wallet-setpwd-unlock>`



Use the info command to view the current synchronization::

	unlocked >>> info
	info
	{
	  "head_block_num": 17249870,
	  "head_block_id": "0107364e2bf1c4ed1331ece4ad7824271e563fbb",
	  "head_block_age": "23 seconds old",
	  "next_maintenance_time": "31 minutes in the future",
	  "chain_id": "4018d7844c78f6a6c41c6a552b898022310fc5dec06da467ee7905a8dad512c8",
	  "participation": "96.87500000000000000",
	  ...
	}





**5.3 Run another cli_wallet to handle deposit**

::

	./cli_wallet -w wallet_for_deposit.json -s ws://127.0.0.1:8090 -H 127.0.0.1:8093

This cli_wallet also opens the wallet API service listening on port 8093

Please refer to the previous section to set a password and unlock it.

----------------------------

6. Account Settings
==============================================

Considering security, you can use **two accounts** to handle the deposit and withdrawal respectively. Here assume that *deposit-account* is used for depositing, and *withdrawal-account* is used for withdrawal.

**6.1 Modifying the Remark Key of the Deposit Account**

   Performing ``suggest_brain_key`` in any of the above ``cli_wallets`` will result in a pair of keys, an example of which is as follows:

::

	unlocked >>> suggest_brain_key
	suggest_brain_key
	{
	  "brain_priv_key": ".....",
	  "wif_priv_key": "5JxyJx2KyDmAx5kpkMthWEpqGjzpwtGtEJigSMz5XE1AtrQaZXu",
	  "pub_key": "DNA69uKRvM8dAPn8En4SCi2nMTHKXt1rWrohFbwaPwv2rAbT3XFzf"
	}

In the light wallet, on the permission page, modify the memo (comment) key to `pub_key` in the above result.

**Note:**

1. Please pay attention to the backup light wallet after the change, otherwise the light wallet may not be able to decrypt the pre-modification comment.
2. After the change, if you still need to use a light wallet to make a transfer with a memo (note), or read a new transfer/transfer transfer note,

 - You need to import the above ``wif_priv_key`` into the light wallet.
 - You can make a new backup after importing.

3. This method can also be used to modify the account's active authority key and owner (account authority) key, which can be used when needed.


**6.2 Import Memo Key of Deposit Account into Cli_wallet Connected to record deposit**

If the wallet is locked, you need to unlock it with the ``unlock`` command.

  Here we need to use ``wif_priv_key`` in the above ``suggest_brain_key`` result::

	unlocked >>> import_key deposit-account 5JxyJx2KyDmAx5kpkMthWEpqGjzpwtGtEJigSMz5XE1AtrQaZXu

  Cli_wallet will automatically generate one or two backup files when it is imported and can be deleted.

Then you can press ``Ctrl + D`` to exit the wallet, back up the wallet file ``wallet_for_deposit.json``, and restart ``cli_wallet``.

If compile time does not introduce the readline library, you need to exit with ``Ctrl + C``

Since no active authority key was imported, the ``cli_wallet`` responsible for handling the deposit cannot use the funds of the deposit account and can only view the history record.


**6.3 Obtaining the Active Authority Key of the Withdrawal Account from the Light Wallet**

Reference: :ref:`User Guide - Permissions <acc-permission>`

**6.4 Import the withdrawal account's active authorization key into cli_wallet responsible for withdrawal**

::

	unlocked >>> import_key withdrawal-account 5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

You can also make a backup of your wallet file.


.. Note:: Check whether the active authority key and the memo key of the withdrawal account are the same. If they are different, the memo key must also be imported. Otherwise, withdrawals with memo cannot be processed.

The import command is still::

	Unlocked >>> import_key withdrawal-account 5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

-----------------------

7. Wallet Command
==============================================

Cli_wallet,

* Use the ``help`` command to list command lists and parameters
* If there is doxygen at compile time, use the ``ethelp`` command to get the parameter description and example of the specific command, such as::

		unlocked >>> gethelp get_account

------------------

8. Wallet API
==============================================

When the wallet opens the HTTP RPC API service, all **wallet commands can be invoked via http RPC. The effect is the same as entering the command in the wallet.**

Example::

	curl -d '{"jsonrpc": "2.0", "method": "get_block", "params": [1], "id": 1}' http://127.0.0.1:8093/rpc

That is: method incoming command name, params array passed to the list of parameters.

Return:

.. code-block:: json

	{"id":1,
	   "result":{
		  "previous":"0000000000000000000000000000000000000000",
		  "timestamp":"2015-10-13T14:12:24",
		  "witness":"1.6.8",
		  "transaction_merkle_root": "0000000000000000000000000000000000000000",
		  "extensions": [],
		  "witness_signature":"1f53542bb60f1f7a653bac70d6b1613e73b9adc952031e30e591e601dd60d493ba5c9a832e155ff0c40ea1dd53512e9f93bf65a8191497ea67d701bc2502f93af7",
		  "transactions": [],
		  "block_id": "00000001b656820f72f6b28cda811778632d4998",
		  "signing_key": "DNA6ZQEFsPmG6jWspNDdZHkehNmPpG7gkSHkphmRZQWaJ2LrcaVSi",
		  "transaction_ids": []
		  }
	}




If the execution is successful, the result will be result, otherwise there will be an error.

**Note:**

* The HTTP RPC request URI is /RPC .
* Enter the command in the wallet and return the result is beautified; when using the HTTP RPC request, the original data in json format is returned. With regard to raw data, there are some things to note:
* The amount is **{"amount":467116432,"asset_id":"1.3.0"}** format, where

  * The ``asset_id`` can be found by the ``get_asset`` command. The ``asset_id`` of the DNA is 1.3.0. Other assets have other ids.
  * amount is the value after the decimal point is removed. For example, DNA is `5 decimal` places. In the above example, it is actually 4671.16432 DNA.

* The account is in the form of 1.2.xxxxx. Get account information via ``get_account``
* Operation type (op) is a numeric format, such as 0 for transfer operation

-------------------

9. Deal with Deposit
==============================================

**9.1 Obtaining the Current *Unable to Return Block* Number**

The possibility of using Bitcoin and others to use the confirmation number to reduce the probability of a fallback in the transaction is different. In the DNA, the **unreturnable block** number can be used to determine whether the transaction can be rolled back.

**Unable to roll back** transactions in blocks and earlier blocks can be guaranteed not to roll back.

Use the command ``get_dynamic_global_properties`` in `cli_wallet` to get the ``block number`` that cannot be rolled back. Such as:

.. code-block:: json

	get_dynamic_global_properties
	{
	  "id": "2.1.0",
	  "head_block_number": 21955727,
	  ...
	  "last_irreversible_block_num": 21955709
	}

Among them, ``head_block_number`` is the latest block number, and ``last_irreversible_block_num`` is the block number that cannot be rollback.

**9.2 Querying Deposit Account History**

Use the ``get_relative_account_history`` command to query the history of the deposit account and check for new deposits.

Such as::

	unlocked >>> get_relative_account_history deposit-account 1 100 100

	unlocked >>> get_relative_account_history deposit-account 101 100 200

	curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": ["deposit-account",1,100,100], "id": 1}' http://127.0.0.1: 8093/rpc


The four parameters are: **account name, minimum number, maximum return number, and maximum number.** Numbering starts with 1.

**Note:**
When the maximum number of cli_wallets returned in a certain version exceeds 100, resulting in inaccurate results. Please avoid using the limit to exceed 100.

The result is an array, sorted in reverse chronological order, with the most recent record at the top.

* If there is no new deposit (charge), the length of the array is 0.
* If there is a new record, where the Nth data is result[N], the format may be:

.. code-block:: json

	{
	"memo":"",
	"description":"Transfer 1 DNA from a to b -- Unlock wallet to see memo. (Fee: 0.22941 DNA)",
	"op":{
	"id":"1.11.1234567",
	"op":[
	 0,
	 {
			"fee":{
				 "amount": 22941,
				 "asset_id":"1.3.0"
			},
			"from":"1.2.12345",
			"to":"1.2.45678",
			"amount":{
				 "amount": 100000,
				 "asset_id":"1.3.0"
			},
			"memo":{
				 "from":"DNA7NLcZJzqq3mvKfcqoN52ainajDckyMp5SYRgzicfbHD6u587ib",
				 "to":"DNA7SakKqZ8HamkTr7FdPn9qYxYmtSh2QzFNn49CiFAkdFAvQVMg6",
				 "nonce": "5333758904325274680",
				 "message": "0b809fa8169453422343434366514a153981ea"
			},
			"extensions":[
			]
	 }
	],
	"result":[
	 0,
	 {
	 }
	],
	"block_num":1234567,
	"trx_in_block":7,
	"op_in_trx":0,
	"virtual_op":1234
	}
	}



Visible, the result does not explicitly include the number of each record, the need for the program to calculate, record. It is usually reversed the order of the array, and then it is more appropriate to deal with one by one.

First of all, we must determine whether the block where the transaction is located cannot be rolled back.

- Take result[N]["op"]["block_num"] is compared with ``last_irreversible_block_num``. If it can't be rolled back, continue processing. If you can roll back, skip skipping.

.. Note:: When the transaction does not enter the block, it may still appear in ``get_relative_account_history``, and the block number where it is located will always change, making it difficult to determine the status.

So use ``last_irreversible_block_num`` to determine.

- Result[N]["op"]["op"] is an array format, taking the first element of the array result[N]["op"]["op"][0] , and if it is 0, it means transfer ;
- Then you can use the ``to`` field in the second element (i.e., result[N]["op"]["op"][1]["to"]) to determine if it is the same as the deposit-account ID. Whether to transfer;
- If yes, then take the ``asset_id`` field result[N]["op"]["op"][1]["amount"]["asset_id"] in the ``amount`` field in the second field to determine if the asset is correct Types of,
- Then take ``amount`` in `amount`, that is, result[N]["op"]["op"][1]["amount"]["amount"], add the decimal places, and get the amount of recharge ;
- Take the outermost ``memo`` field, which is result[N]["memo"] , to get the `user's ID` on the exchange and enter it.
- Result[N]["op"]["id"] is the unique ID of this transfer and can be recorded for future reference.
- At the same time, it is recommended to record several data of ``block_num``, ``trx_in_block``, and ``op_in_trx`` in the result, which means the number of the block, the number of transactions in the block, and the number of operations in the transaction.

  In addition, due to other transfers, it may only record the transaction ID (hashed value), or transaction signature, without recording the operation ID or block number.

  In order to facilitate the inspection of the problem, it is recommended to record the `transaction ID` and `transaction signature` corresponding to the operation at the time of recharge detection as follows:

  According to the above ``block_num``, call the ``get_block`` command to get the contents of the block, such as::

	unlocked >>> get_block 16000000

	curl -d '{"jsonrpc": "2.0", "method": "get_block", "params": [160000], "id": 1}' http://127.0.0.1:8093/rpc


Let the result block be result , according to the above `trx_in_block`,

- Take result["transaction_ids"][trx_in_block] is the corresponding `transaction ID`;
- Take result["transactions"][trx_in_block]["signatures"] , which is a signature for the transaction. It is an array because multi-signature account transfers may contain multiple signatures.

**Note:**

1) The wallet must be unlocked before decrypting the memo.
2) If it is detected that there are refills with incorrect Memo, or if the asset type is incorrect, be careful not to return it simply because it may have been transferred from other exchanges and it will be very troublesome for the other party to deal with it after repatriation.
3) There may be more than one deposit in a block. The result is that ``block_num`` is the same. It may even be the same for ``trx_in_block`` and ``op_in_trx``, but ``virtual_op`` is different.

  - It is certain that the combination of **blocknum + trx_in_block + op_in_trx + vitrual_op** is unique.
  - It is also worth noting here that the data of ``virtual_op``. If the parameters are not the same and replay every time you restart the device, and you check the historical data again, you will find that this value will be inconsistent.

4) Due to the "Proposal" function, it is possible to postpone execution. When using ``get_block`` and then positioning with ``trx_in_block``, the corresponding transaction may not be available, or the acquired transaction does not correspond to the recharge operation.

  - Delayed execution function is rarely used, but theoretically, please pay attention to error handling.

--------------

10. Processing Withdrawals
==============================================

**10.1 Network Status Check**

For security reasons, withdrawals are processed only when the ``witness_node`` network is normal.

Use the info command in the ``cli_wallet`` responsible for withdrawal to check the network status.::

	unlocked >>> info
	info
	{
	  "head_block_num": 17249870,
	  "head_block_id": "0107364e2bf1c4ed1331ece4ad7824271e563fbb",
	  "head_block_age": "23 seconds old",
	  "next_maintenance_time": "31 minutes in the future",
	  "chain_id": "4018d7844c78f6a6c41c6a552b898022310fc5dec06da467ee7905a8dad512c8",
	  "participation": "96.87500000000000000",
	  ...
	}

The fields to check are:

  * ``head_block_age`` is best within 1 minute
  * The best participation is 80 or more, which means that 80% of the out-of-block nodes in the network connected to the ``witness_node`` are working properly

In addition, when the network is normal, the difference between ``last_irreversible_block_num`` and ``head_block_num`` is not too large (generally less than 30);
This can be used as a reference.


**10.2 Cash withdrawal account balance check**

Use the ``list_account_balances`` command to check whether the withdrawal account balance is sufficient (pay attention to the asset type and calculate the fee)::

	unlocked >>> list_account_balances withdrawal-account


**Note:**

1) Pay attention to asset type
2) Pay attention to handling fees. Because the Memo are based on length, the handling fee with Memo will be higher than without Memo.


**10.3 Checking a validation of the Name of the Account**

Use the ``get_account_id command`` to check whether the customer’s withdrawal account is valid::

	locked >>> get_account_id test-123
	get_account_id test-123
	"1.2.96698"

	locked >>> get_account_id test-124
	get_account_id test-124
	10 assert_exception: Assert Exception
	rec && rec->name == account_name_or_id:
	    {}
	    th_a  wallet.cpp:597 get_account


**10.4 Sending withdrawal**

Use the ``transfer2`` command to send a withdrawal transaction. Such as::

	unlocked >>> transfer2 withdrawal-account to-account 100 DNA "some memo"

The parameters are: **source account name, destination account name, amount, currency, Memo**

The command will sign and broadcast the transaction and return an array. The first element is the transaction id and the second element is the detailed transaction content.

**Note:**

1) If the currency is DNA, the number of decimal places is up to 5 digits. If it is other assets, you can view the decimal places of the asset with the ``precision`` field with the ``get_asset`` command.
2) You can also use the transfer command, but this does not directly return the transaction ID. Instead, it needs to call another API to calculate it, so it is not recommended.
3) Memo are usually encoded in UTF-8
4) It is recommended to record relevant data for future reference, such as transaction id, detailed transaction content in json format, etc.


**10.5 Withdrawal Results Check**

Use the ``get_relative_account_history`` command to obtain withdrawal history of withdrawal-account. Refer to the deposit processing section. If new records are found,

And the transaction's block number is earlier than ``last_irreversible_block_num``, indicating that the transaction has entered the block and cannot be rolled back;

**Note:**

When the transaction does not enter the block, it may still appear in ``get_relative_account_history``, and the block number where it is located will always change, making it difficult to determine the status. So use ``last_irreversible_block_num`` to determine.

Use the ``get_block`` command to query details based on the block_num field of the record::

	unlocked >>> get_block 12345

In the returned result, the ``transaction_ids`` field data should contain the previous ``transaction id``.

It is recommended to record the ``id (1.11.x)``, ``block_num``, and ``trx_in_block`` in the above result of ``get_relative_account_history`` for future reference.


**10.6 About Resend Failure**

In some cases, after the transaction may be sent, it is not packaged into the block in time.

Unlike Bitcoin, there is a timeout in the DNA transaction.

When using ``cli_wallet`` to sign a broadcast transaction, this field value defaults to the local system time plus 2 minutes.

When the number of local deals is particularly high, the timeout period will increase.

If, after the network time reaches the timeout, the transaction is still not packed into the block, the transaction is discarded by all network nodes and is no longer likely to be packed.

Therefore, if a transaction broadcast appears but does not appear in the account history, first check if the local system time logs.

* If the block time corresponding to ``last_irreversible_block_num`` has passed the timeout of the transaction, retransmission is safe.
* If the transaction has already appeared in history, check if the block number of the transaction is fixed, instead of always updating with the latest block number.
* If the exchange's block number is updated, it indicates that the transaction has not been packed into the block and it needs to wait patiently for being packaged or timeout
* If the block number is fixed, the ``last_irreversible_block_num`` will soon exceed the block number when the network is normal over time.
* If ``head_block_num`` keeps growing, and ``last_irreversible_block_num`` doesn't grow,

  * It is likely that the ``witness_node`` has entered a short branching chain, or there is a problem with the network, and the transaction cannot be fully confirmed.
  * In this case, check if there is a new version of the ``witness_node`` to be upgraded or contact the development team.

* If the retransmission still cannot be packaged, you may encounter network abnormalities or congestion. This is relatively rare. Please contact the development team.


11. Others
==============================================

* ``cli_wallet`` has a parameter ``--daemon`` , which will run in the background when started with this parameter

* When you need to close the ``witness_node``, press ``Ctrl C`` once and wait for the program to exit.

  * After the normal exit, when restarting, no need to rebuild the index, the startup will be faster
  * After the normal exit, the data directory ``witness_node_data_dir`` can be backed up, and can be used directly when needed.
  * If you quit abnormally, when restarting, it is very likely that you need to rebuild the index and start slower.

* If the ``witness_node`` is abnormal, generally try to restart it first. If it is not, you can try to restart it with the ``--replay-blockchain`` parameter, that is, manually trigger rebuilding the index.

  * If not resolved, use backup recovery
  * If there is no backup, re-synchronization may take longer

* **Multi-signature**: DNA natively supports account-level multi-signature, and there is a proposal-approval mechanism that can initiate multi-signature requests online and then confirm the completion of multi-signature transactions.  Read about: :ref:`bts-multi-sign`
* Hardware Wallet: No Support
* **Cold Storage**: Can be implemented, the steps are somewhat complicated, examples:

  * On offline machines, start ``witness_node`` and ``cli_wallet`` and use the ``suggest_brain_key`` command to generate the key pair offline;
  * Then use the light wallet to change the account key to the above key, then the account goes into cold storage
  * When you need to use a cold deposit account,

    * You can use the temporary heating method, that is, import the private key into the light wallet, use it and then replace it with a new one
    * Pure cold mode can also be achieved, but the current ``cli_wallet`` support is not good, if necessary, please contact alone


12. Related information
==============================================

* Graphic tutorial http://jc.btsabc.org/
  * Self-built node tutorial http://btsabc.org/article-477-1.html
  * Get account private key http://btsabc.org/article-761-1.html
* English Docking Documents http://docs.bitshares.org/integration/exchanges/step-by-step.html
* English API Documentation http://docs.bitshares.org/api/index.html


--------------

- Contributor: @abit

(ref)

This information originates [abitmore/bts-cn-docs](https://github.com/abitmore/bts-cn-docs/blob/master/DNA%E4%BA%A4%E6%98%93%E6%89%80%E5%AF%B9%E6%8E%A5%E6%8C%87%E5%8D%97%EF%BC%88%E5%8D%95%E8%8A%82%E7%82%B9%E7%89%88%EF%BC%89.txt) repository.

*(Translated by an application and adjusted by human. Some words might be not accurate.)*

|

|