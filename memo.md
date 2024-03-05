





```
type Query {
  """
  Block fetches an Ethereum block by number or by hash. If neither is
  supplied, the most recent known block is returned.
  """
  block(number: Long, hash: Bytes32): Block

  """
  Blocks returns all the blocks between two numbers, inclusive. If
  to is not supplied, it defaults to the most recent known block.
  """
  blocks(from: Long, to: Long): [Block!]!

  """Pending returns the current pending state."""
  pending: Pending!

  """Transaction returns a transaction specified by its hash."""
  transaction(hash: Bytes32!): Transaction

  """Logs returns log entries matching the provided filter."""
  logs(filter: FilterCriteria!): [Log!]!

  """
  GasPrice returns the node's estimate of a gas price sufficient to
  ensure a transaction is mined in a timely fashion.
  """
  gasPrice: BigInt!

  """
  MaxPriorityFeePerGas returns the node's estimate of a gas tip sufficient
  to ensure a transaction is mined in a timely fashion.
  """
  maxPriorityFeePerGas: BigInt!

  """Syncing returns information on the current synchronisation state."""
  syncing: SyncState

  """
  ChainID returns the current chain ID for transaction replay protection.
  """
  chainID: BigInt!
}
```






```
curl -X POST -H "Content-Type: application/json" -d'{ "query" : "query getLogs($filter: FilterCriteria!) {logs(filter: $filter ) {index}}", "variables" : {"filter" : {"fromBlock" : 0, "toBlock" : 700, "addresses" : ["fe3b557e8fb62b89f4916b721be55ceb828dbd73"], "topics" : []}}}' http://localhost:8547/graphql
```



```
{
  "errors" : [ {
    "message" : "Exception while fetching data (/logs) : class java.lang.Integer cannot be cast to class java.lang.Long (java.lang.Integer and java.lang.Long are in module java.base of loader 'bootstrap')",
    "locations" : [ {
      "line" : 1,
      "column" : 42
    } ],
    "path" : [ "logs" ],
    "extensions" : {
      "classification" : "DataFetchingException"
    }
  } ],
  "data" : null
}
```

