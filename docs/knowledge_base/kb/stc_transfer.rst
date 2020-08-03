
.. _trnsf-smartcontract:

Transfer - smart contract
***********************

"The transfer of asset from one account to another is one operation and a smart contract."

In this section, we examine how worker proposal  smart contract was constructed. There are items and helpful steps to construct a smart contract. We want to list those items by examining DNA blockchain components.

* **Smart contract items**

  - Object
  - Object Index and Call
  - Operations
  - Validations
  - Initialize Evaluators and Index
  - Evaluators
  - API call


-------------------

Items
========================

Object
---------------------------------------------

- account_object
- account_balance_object
- asset_object
- asset_dynamic_data_object



Object Index and Search Call
---------------------------------------------
- generic_index
- get_asset()
- get_account()

Operations
---------------------------------------------
- transfer_operation 
- override_transfer_operation
- blind_transfer_operation
- transfer_from_blind_operation 
- transfer_to_blind_operation 


Validations
---------------------------------------------

.. code-block:: cpp 



Initialize Evaluators and Index
---------------------------------------------

.. code-block:: cpp 

	void database::initialize_evaluators()
	{
	   _operation_evaluators.resize(255);
	   register_evaluator<account_create_evaluator>()
	   .......
	}   


.. code-block:: cpp 
	
	void database::initialize_indexes()
	{
	   reset_indexes();
	   _undo_db.set_max_size( GRAPHENE_MIN_UNDO_HISTORY );    

	   .....   
	}  


	void database::init_genesis(const genesis_state_type& genesis_state)
	{ try {
	.....

	}}	
	
   
Evaluators
---------------------------------------------

- transfer_evaluator 
- override_transfer_evaluator 
- generic_evaluator
- op_evaluator



CLI Wallet Calls
---------------------------------------------
- 

API calls
---------------------------------------------
- set_fees_on_builder_transaction



------------------------


|

