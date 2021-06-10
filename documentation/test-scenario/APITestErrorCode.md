<!--
 Copyright (c) 2021 Microsoft Corporation

 This software is released under the MIT License.
 https://opensource.org/licenses/MIT
-->

# API testing error code

OAV api testing define some rules and check whether actual service response match with example. Example is very important for downstream SDK test code generation, code samples and doc. The goal to detect example quality issues and check service behaviors consistent with example.

## Rule descriptions

#### INCORRECT_PROVISIONING_STATE

If service return 200 status code,it means the operation is succeed and finished. So the provisioning state should be one of the terminal states ["succeeded", "failed", "canceled", "ready", "created", "deleted"].

The provisioning state is very important for downstream terraform or cli to manage resource status. For more details about provisioning state, please refer to this [ARM RPC provisioning state](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/Addendum.md#provisioningstate-property)

#### RESPONSE_MISSING_VALUE

**Error message**: The response value is missing. Path: {}. Expected: {}. Actual: undefined

The example has defined response value, but actually the in api testing server doesn't return that value.

Example:

```diff
{
   "properties":{
      "targetType":"blobNfs",
      "junctions":[
         {
            "namespacePath":"/blobnfs"
         }
      ],
      "blobNfs":{
         "target":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/scgroup/providers/Microsoft.Storage/storageAccounts/blofnfs/blobServices/default/containers/blobnfs",
-         "usageModel":"WRITE_WORKLOAD_15"
      }
   }
}
```

#### RESPONSE_ADDITIONAL_VALUE

**Error message** Return additional response value. Path: {}. Expected: undefined. Actual: {}

Example:

```diff
{
   "properties":{
      "targetType":"blobNfs",
      "junctions":[
         {
            "namespacePath":"/blobnfs"
         }
      ],
      "blobNfs":{
         "target":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/scgroup/providers/Microsoft.Storage/storageAccounts/blofnfs/blobServices/default/containers/blobnfs",
         "usageModel":"WRITE_WORKLOAD_15"
+        "enableFeature": true
      }
   }
}
```

#### RESPONSE_INCONSISTENT_VALUE

**Error message** The actual response value is different from example. Path: {}. Expected: {}. Actual: {}

Example:

```diff
{
   "properties":{
      "targetType":"blobNfs",
      "junctions":[
         {
            "namespacePath":"/blobnfs"
         }
      ],
      "blobNfs":{
         "target":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/scgroup/providers/Microsoft.Storage/storageAccounts/blofnfs/blobServices/default/containers/blobnfs",
-         "usageModel":"WRITE_WORKLOAD_15"
+         "usageModel":"WORK_LOAD_14"
      }
   }
}
```
