
.. _private-testnet-genesis-example:

Private Testnet Genesis file
===================================

As you know, each blockchain starts with a genesis block that definitions are in a genesis file.

If you want to test the feature set of current mainnet without connecting to mainnet, you must start a new (**private**) network.

In a private testnet, you **MUST** create and configure own genesis file by creating a subdirectory named **genesis** and create a file within it named ``my-genesis.json``. This file dictates the initial state of the network.


For the use of private testnet, you must put an account into the genesis file with keys that you have generated yourself. In a configuration ``config.ini`` file, you can set a genesis.json path parameter value (i.e.,  genesis-json = genesis/my-genesis.json ).  Before you start a ``witness_node``, make sure if you set your **my-genesis.json** file name in a ``config.ini`` file.


.. tip:: If you want to generate new keys pairs, you can use
   - ``suggest_brain_key``  in cli_wallet. Or  ``./get_dev_key``  in the ``programs/genesis_util/`` by using the corresponding network prefix("DNA", "TEST", "MY", etc).


* :ref:`how-to-get-key-pairs`

|

-----

Sample private testnet genesis file
------------------------------------------

**Notes and Tips::**

- For the private testnet, each public key (key pairs) should be generated. You use different keys to set up the private testnet genesis file ``active_key`` and ``owner_key`` for each **initX** account. You can use your own "name" settings.

- "initial_accounts": The names "initx"..."initxx" can be chosen freely. **Note::** the "initial_witness_candidates"- ``owner_name`` and "initial_committee_candidates"- ``owner_name`` refer the the "initial_account" by ``name``.

- ``name``: There is no direct connection between the account ``name`` and keys, so you can change the name and insert keys from ``get_dev_key`` results. If you import the corresponding private keys into cli_wallet, then cli_wallet will recognize the accounts as its own.

- The UI generates keys by hashing name, password and a key type suffix. Therefore, only if you want to use the UI, there is a connection between name, password and keys.

- "max_core_supply" refers to **TEST** in testnet, and to **DNA** in mainnet.

- "initial_balances": the ``owner`` is an **address** displayed by ``get_dev_key``; which is basically a shortened hash of a public key. These initial balances can be claimed into an account using the private key corresponding to the owner address.

- For example, if you create twelve ``name`` for ``initial_accounts``, you should generate 12 accounts * 2 different keys = 24 key pairs (owner_key and active_key) to set up the genesis file parameters.

- The memo key is always the same as the active key. The “active_key equals to the memo_key” applies to genesis accounts, it’s not a general rule.

- "initial_witness_candidates": ``owner_name`` must refer to the initial_accounts. The ``block_signing_key`` can be different from owner or active key.

- The ``block_signing_key`` is the one you must use with the ``--private-key`` of witness node, or in the configuration file.


**Must generate and fill each key to use**

::

	{
	  "initial_timestamp": "2016-01-18T09:18:25",
	  "max_core_supply": "1000000000000000",
	  "initial_parameters": {
		"current_fees": {
		  "parameters": [[
			  0,{
				"fee": 2000000,
				"price_per_kbyte": 1000000
			  }
			],[
			  1,{
				"fee": 500000
			  }
			],[
			  2,{
				"fee": 0
			  }
			],[
			  3,{
				"fee": 2000000
			  }
			],[
			  4,{}
			],[
			  5,{
				"basic_fee": 500000,
				"premium_fee": 200000000,
				"price_per_kbyte": 100000
			  }
			],[
			  6,{
				"fee": 2000000,
				"price_per_kbyte": 100000
			  }
			],[
			  7,{
				"fee": 300000
			  }
			],[
			  8,{
				"membership_annual_fee": 200000000,
				"membership_lifetime_fee": 1000000000
			  }
			],[
			  9,{
				"fee": 50000000
			  }
			],[
			  10,{
				"symbol3": "50000000000",
				"symbol4": "30000000000",
				"long_symbol": 500000000,
				"price_per_kbyte": 10
			  }
			],[
			  11,{
				"fee": 50000000,
				"price_per_kbyte": 10
			  }
			],[
			  12,{
				"fee": 50000000
			  }
			],[
			  13,{
				"fee": 50000000
			  }
			],[
			  14,{
				"fee": 2000000,
				"price_per_kbyte": 100000
			  }
			],[
			  15,{
				"fee": 2000000
			  }
			],[
			  16,{
				"fee": 100000
			  }
			],[
			  17,{
				"fee": 10000000
			  }
			],[
			  18,{
				"fee": 50000000
			  }
			],[
			  19,{
				"fee": 100000
			  }
			],[
			  20,{
				"fee": 500000000
			  }
			],[
			  21,{
				"fee": 2000000
			  }
			],[
			  22,{
				"fee": 2000000,
				"price_per_kbyte": 10
			  }
			],[
			  23,{
				"fee": 2000000,
				"price_per_kbyte": 10
			  }
			],[
			  24,{
				"fee": 100000
			  }
			],[
			  25,{
				"fee": 100000
			  }
			],[
			  26,{
				"fee": 100000
			  }
			],[
			  27,{
				"fee": 2000000,
				"price_per_kbyte": 10
			  }
			],[
			  28,{
				"fee": 0
			  }
			],[
			  29,{
				"fee": 500000000
			  }
			],[
			  30,{
				"fee": 2000000
			  }
			],[
			  31,{
				"fee": 100000
			  }
			],[
			  32,{
				"fee": 100000
			  }
			],[
			  33,{
				"fee": 2000000
			  }
			],[
			  34,{
				"fee": 500000000
			  }
			],[
			  35,{
				"fee": 100000,
				"price_per_kbyte": 10
			  }
			],[
			  36,{
				"fee": 100000
			  }
			],[
			  37,{}
			],[
			  38,{
				"fee": 2000000,
				"price_per_kbyte": 10
			  }
			],[
			  39,{
				"fee": 500000,
				"price_per_output": 500000
			  }
			],[
			  40,{
				"fee": 500000,
				"price_per_output": 500000
			  }
			],[
			  41,{
				"fee": 500000
			  }
			],[
			  42,{}
			],[
			  43,{
				"fee": 2000000
			  }
			]
		  ],
		  "scale": 10000
		},
		"block_interval": 5,
		"maintenance_interval": 86400,
		"maintenance_skip_slots": 3,
		"committee_proposal_review_period": 1209600,
		"maximum_transaction_size": 2048,
		"maximum_block_size": 2048000000,
		"maximum_time_until_expiration": 86400,
		"maximum_proposal_lifetime": 2419200,
		"maximum_asset_whitelist_authorities": 10,
		"maximum_asset_feed_publishers": 10,
		"maximum_witness_count": 1001,
		"maximum_committee_count": 1001,
		"maximum_authority_membership": 10,
		"reserve_percent_of_fee": 2000,
		"network_percent_of_fee": 2000,
		"lifetime_referrer_percent_of_fee": 3000,
		"cashback_vesting_period_seconds": 31536000,
		"cashback_vesting_threshold": 10000000,
		"count_non_member_votes": true,
		"allow_non_member_whitelists": false,
		"witness_pay_per_block": 1000000,
		"worker_budget_per_day": "50000000000",
		"max_predicate_opcode": 1,
		"fee_liquidation_threshold": 10000000,
		"accounts_per_fee_scale": 1000,
		"account_fee_scale_bitshifts": 4,
		"max_authority_depth": 2,
		"extensions": []
	  },
	  "initial_accounts": [{
		  "name": "init0",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init1",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init2",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init3",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init4",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init5",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init6",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init7",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init8",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init9",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "init10",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": true
		},{
		  "name": "nathan-test",
		  "owner_key": "--- set a public key ---",
		  "active_key": "--- set a public key ---",
		  "is_lifetime_member": false
		}
	  ],
	  "initial_assets": [],
	  "initial_balances": [{
		  "owner": "--- set an address ---",
		  "asset_symbol": "TEST",
		  "amount": "1000000000000000"
		}
	  ],
	  "initial_vesting_balances": [],
	  "initial_active_witnesses": 11,
	  "initial_witness_candidates": [{
		  "owner_name": "init0",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init1",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init2",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init3",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init4",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init5",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init6",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init7",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init8",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init9",
		  "block_signing_key": "--- set a public key ---"
		},{
		  "owner_name": "init10",
		  "block_signing_key": "--- set a public key ---"
		}
	  ],
	  "initial_committee_candidates": [{
		  "owner_name": "init0"
		},{
		  "owner_name": "init1"
		},{
		  "owner_name": "init2"
		},{
		  "owner_name": "init3"
		},{
		  "owner_name": "init4"
		},{
		  "owner_name": "init5"
		},{
		  "owner_name": "init6"
		},{
		  "owner_name": "init7"
		},{
		  "owner_name": "init8"
		},{
		  "owner_name": "init9"
		},{
		  "owner_name": "init10"
		}
	  ],
	  "initial_worker_candidates": [],
	  "immutable_parameters": {
		"min_committee_member_count": 11,
		"min_witness_count": 11,
		"num_special_accounts": 0,
		"num_special_assets": 0
	  }
	}

