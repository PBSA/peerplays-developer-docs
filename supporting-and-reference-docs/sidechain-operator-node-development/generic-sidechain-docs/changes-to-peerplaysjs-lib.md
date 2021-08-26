---
description: >-
  Peerplaysjs-lib is a javascript library that provides an easy way to connect
  to the peerplays blockchain. This page reflects the changes that have to be
  made to the peerplaysjs-lib to support SONs.
---

# Changes to Peerplaysjs-lib

Following changes have been made to the peerplaysjs-lib:

1. **ChainTypes.js**
   1. Add the following missing object types to the `object_type` constant: `son: 27,  son_proposal: 28,  son_wallet: 29,  son_wallet_deposit: 30,  son_wallet_withdraw: 31,  sidechain_address: 32,  sidechain_transaction: 33`
   2. Add the following missing implementation object types to the `impl_object_type` constant: `son_statistics: 24,  son_schedule: 25`
   3. Add the following missing operations in `operations` constant: `son_create: 82,  son_update: 83,  son_delete: 84,  son_heartbeat: 85,  son_report_down: 86,  son_maintenance: 87,  son_wallet_recreate: 88,  son_wallet_update: 89,  son_wallet_deposit_create: 90,  son_wallet_deposit_process: 91,  son_wallet_withdraw_create: 92,  son_wallet_withdraw_process: 93,  sidechain_address_add: 94,  sidechain_address_update: 95,  sidechain_address_delete: 96,  sidechain_transaction_create: 97,  sidechain_transaction_sign: 98,  sidechain_transaction_send: 99,  sidechain_transaction_settle: 100`
2. operations.js 1. Add the following missing fee parameters:

   `const son_create_operation_fee_parameters = new Serializer(    
   'son_create_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_update_operation_fee_parameters = new Serializer(    
   'son_update_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_delete_operation_fee_parameters = new Serializer(    
   'son_delete_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_heartbeat_operation_fee_parameters = new Serializer(    
   'son_heartbeat_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_report_down_operation_fee_parameters = new Serializer(    
   'son_report_down_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_maintenance_operation_fee_parameters = new Serializer(    
   'son_maintenance_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_wallet_recreate_operation_fee_parameters = new Serializer(    
   'son_wallet_recreate_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_wallet_update_operation_fee_parameters = new Serializer(    
   'son_wallet_update_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const son_wallet_deposit_create_operation_fee_parameters = new Serializer(    
   'son_wallet_deposit_create_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const son_wallet_deposit_process_operation_fee_parameters = new Serializer(    
   'son_wallet_deposit_process_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const son_wallet_withdraw_create_operation_fee_parameters = new Serializer(    
   'son_wallet_withdraw_create_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const son_wallet_withdraw_process_operation_fee_parameters = new Serializer(    
   'son_wallet_withdraw_process_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );    
   const sidechain_address_add_operation_fee_parameters = new Serializer(    
   'sidechain_address_add_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const sidechain_address_update_operation_fee_parameters = new Serializer(    
   'sidechain_address_update_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const sidechain_address_delete_operation_fee_parameters = new Serializer(    
   'sidechain_address_delete_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const sidechain_transaction_create_operation_fee_parameters = new Serializer(    
   'sidechain_transaction_create_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const sidechain_transaction_sign_operation_fee_parameters = new Serializer(    
   'sidechain_transaction_sign_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const sidechain_transaction_send_operation_fee_parameters = new Serializer(    
   'sidechain_transaction_send_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   `const sidechain_transaction_settle_operation_fee_parameters = new Serializer(    
   'sidechain_transaction_settle_operation_fee_parameters',    
   {    
   fee: uint64    
   }    
   );`

   1. Add these fee parameters to the `fee_parameters` constant: `son_create_operation_fee_parameters, son_update_operation_fee_parameters, son_delete_operation_fee_parameters, son_heartbeat_operation_fee_parameters, son_report_down_operation_fee_parameters, son_maintenance_operation_fee_parameters, son_wallet_recreate_operation_fee_parameters, son_wallet_update_operation_fee_parameters, son_wallet_deposit_create_operation_fee_parameters, son_wallet_deposit_process_operation_fee_parameters, son_wallet_withdraw_create_operation_fee_parameters, son_wallet_withdraw_process_operation_fee_parameters, sidechain_address_add_operation_fee_parameters, sidechain_address_update_operation_fee_parameters, sidechain_address_delete_operation_fee_parameters, sidechain_transaction_create_operation_fee_parameters, sidechain_transaction_sign_operation_fee_parameters, sidechain_transaction_send_operation_fee_parameters, sidechain_transaction_settle_operation_fee_parameters`
   2. Add missing fields to `parameter_extensions` constant: `gpos_period: optional(uint32),  gpos_subperiod: optional(uint32),  gpos_period_start: optional(uint32),  gpos_vesting_lockin_period: optional(uint32),  son_vesting_amount: optional(uint32),  son_vesting_period: optional(uint32),  son_pay_max: optional(uint32),  son_pay_time: optional(uint32),  son_deregister_time: optional(uint32),  son_heartbeat_frequency: optional(uint32),  son_down_time: optional(uint32),  son_bitcoin_min_tx_confirmations: optional(uint16),  son_account: optional(protocol_id_type('account')),  btc_asset: optional(protocol_id_type('asset'))`
   3. Add a new serializer for `dormant_vesting_balance_initializer` and add it to the constant `vesting_policy_initializer`: `const dormant_vesting_policy_initializer = new Serializer('dormant_vesting_policy_initializer', {});`
   4. Add 'son' to `vesting_balance_type` enum serializer: `const vesting_balance_type = enumeration([ 'normal', 'gpos', 'son' ]);`
   5. Add the following missing operation serializers:  
      `const sidechain_type = enumeration([    
      'unknown',    
      'bitcoin',    
      'ethereum',    
      'eos',    
      'peerplays'    
      ]);`

      `const son_create = new Serializer('son_create', {    
      fee: asset,    
      owner_account: protocol_id_type('account'),    
      url: string,    
      deposit: protocol_id_type('vesting_balance'),    
      signing_key: public_key,    
      sidechain_public_keys: map(sidechain_type, string),    
      pay_vb: protocol_id_type('vesting_balance')    
      });`

      `const son_update = new Serializer('son_update', {    
      fee: asset,    
      son_id: protocol_id_type('son'),    
      owner_account: protocol_id_type('account'),    
      new_url: optional(string),    
      new_deposit: optional(protocol_id_type('vesting_balance')),    
      new_signing_key: optional(public_key),    
      new_sidechain_public_keys: optional(map(sidechain_type, string)),    
      new_pay_vb: optional(protocol_id_type('vesting_balance'))    
      });`

      `const son_delete = new Serializer('son_delete', {    
      fee: asset,    
      son_id: protocol_id_type('son'),    
      payer: protocol_id_type('account'),    
      owner_account: protocol_id_type('account')    
      });`

      `const son_heartbeat = new Serializer('son_heartbeat', {    
      fee: asset,    
      son_id: protocol_id_type('son'),    
      owner_account: protocol_id_type('account'),    
      ts: time_point_sec    
      });`

      `const son_report_down = new Serializer('son_report_down', {    
      fee: asset, son_id: protocol_id_type('son'),    
      payer: protocol_id_type('account'),    
      down_ts: time_point_sec    
      });`

      `const son_maintenance_request_type = enumeration([    
      'request_maintenance',    
      'cancel_request_maintenance'    
      ]);`

      `const son_maintenance = new Serializer('son_maintenance', {    
      fee: asset,    
      son_id: protocol_id_type('son'),    
      owner_account: protocol_id_type('account'),    
      request_type: son_maintenance_request_type    
      });`

      `const son_info = new Serializer('son_info', {    
      son_id: protocol_id_type('son'),    
      weight: uint16,    
      signing_key: public_key,    
      sidechain_public_keys: map(sidechain_type, string)    
      });`

      `const son_wallet_recreate = new Serializer('son_wallet_recreate', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      sons: set(son_info) });`

      `const son_wallet_update = new Serializer('son_wallet_update', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      son_wallet_id: protocol_id_type('son_wallet'),    
      sidechain: sidechain_type,    
      address: string    
      });`

      `const son_wallet_deposit_create = new Serializer('son_wallet_deposit_create', {    
      fee: asset, payer: protocol_id_type('account'),    
      son_id: protocol_id_type('son'),    
      timestamp: time_point_sec,    
      block_num: uint32,    
      sidechain: sidechain_type,    
      sidechain_uid: string,    
      sidechain_transaction_id: string,    
      sidechain_from: string,    
      sidechain_to: string,    
      sidechain_currency: string,    
      sidechain_amount: int64,    
      peerplays_from: protocol_id_type('account'),    
      peerplays_to: protocol_id_type('account'),    
      peerplays_asset: asset    
      });`

      `const son_wallet_deposit_process = new Serializer('son_wallet_deposit_process', {    
      fee: asset, payer: protocol_id_type('account'),    
      son_wallet_deposit_id: protocol_id_type('son_wallet_deposit')    
      });`

      `const son_wallet_withdraw_create = new Serializer('son_wallet_withdraw_create', {    
      fee: asset, payer: protocol_id_type('account'),    
      son_id: protocol_id_type('son'),    
      timestamp: time_point_sec,    
      block_num: uint32,    
      sidechain: sidechain_type,    
      peerplays_uid: string,    
      peerplays_transaction_id: string,    
      peerplays_from: protocol_id_type('account'),    
      peerplays_asset: asset,    
      withdraw_sidechain: sidechain_type,    
      withdraw_address: string,    
      withdraw_currency: string,    
      withdraw_amount: int64    
      });`

      `const son_wallet_withdraw_process = new Serializer('son_wallet_withdraw_process', {    
      fee: asset, payer: protocol_id_type('account'),    
      son_wallet_withdraw_id: protocol_id_type('son_wallet_withdraw')    
      });`

      `const sidechain_address_add = new Serializer('sidechain_address_add', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      sidechain_address_account: protocol_id_type('account'),    
      sidechain: sidechain_type,    
      deposit_public_key: string,    
      deposit_address: string,    
      deposit_address_data: string,    
      withdraw_public_key: string,    
      withdraw_address: string    
      });`

      `const sidechain_address_update = new Serializer('sidechain_address_update', {    
      fee: asset, payer: protocol_id_type('account'),    
      sidechain_address_id: protocol_id_type('sidechain_address'),    
      sidechain_address_account: protocol_id_type('account'),    
      sidechain: sidechain_type,    
      deposit_public_key: optional(string),    
      deposit_address: optional(string),    
      deposit_address_data: optional(string),    
      withdraw_public_key: optional(string),    
      withdraw_address: optional(string)    
      });`

      `const sidechain_address_delete = new Serializer('sidechain_address_delete', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      sidechain_address_id: protocol_id_type('sidechain_address'),    
      sidechain_address_account: protocol_id_type('account'),    
      sidechain: sidechain_type    
      });`

      `const sidechain_transaction_create = new Serializer('sidechain_transaction_create', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      sidechain: sidechain_type,    
      object_id: protocol_id_type('object'),    
      transaction: string, signers: set(son_info)    
      });`

      `const sidechain_transaction_sign = new Serializer('sidechain_transaction_sign', {    
      fee: asset,    
      signer: protocol_id_type('son'),    
      payer: protocol_id_type('account'),    
      sidechain_transaction_id: protocol_id_type('sidechain_transaction'),    
      signature: string    
      });`

      `const sidechain_transaction_send = new Serializer('sidechain_transaction_send', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      sidechain_transaction_id: protocol_id_type('sidechain_transaction'),    
      signature: string    
      });`

      `const sidechain_transaction_settle = new Serializer('sidechain_transaction_settle', {    
      fee: asset,    
      payer: protocol_id_type('account'),    
      sidechain_transaction_id: protocol_id_type('sidechain_transaction')    
      });`

   6. Add the missing operations in `st_operations`: `son_create, son_update, son_delete, son_heartbeat, son_report_down, son_maintenance, son_wallet_recreate, son_wallet_update, son_wallet_deposit_create, son_wallet_deposit_process, son_wallet_withdraw_create, son_wallet_withdraw_process, sidechain_address_add, sidechain_address_update, sidechain_address_delete, sidechain_transaction_create, sidechain_transaction_sign, sidechain_transaction_send, sidechain_transaction_settle`

