# RUN: nixd --lit-test < %s | FileCheck %s

Check basic handshake with the server, i.e. "initialize"

<-- initialize(0)

```json
{
   "jsonrpc":"2.0",
   "id":0,
   "method":"initialize",
   "params":{
      "processId":123,
      "rootPath":"",
      "capabilities":{
      },
      "trace":"off"
   }
}
```

<-- reply:initialize(0)

```
     CHECK: {
CHECK-NEXT:   "id": 0,
CHECK-NEXT:   "jsonrpc": "2.0",
CHECK-NEXT:   "result": {
CHECK-NEXT:     "capabilities": {
CHECK-NEXT:       "completionProvider": {
CHECK-NEXT:         "triggerCharacters": [
CHECK-NEXT:           "."
CHECK-NEXT:         ]
CHECK-NEXT:       },
CHECK-NEXT:       "declarationProvider": true,
CHECK-NEXT:       "definitionProvider": true,
CHECK-NEXT:       "documentFormattingProvider": true,
CHECK-NEXT:       "hoverProvider": true,
CHECK-NEXT:       "textDocumentSync": {
CHECK-NEXT:         "change": 2,
CHECK-NEXT:         "openClose": true,
CHECK-NEXT:         "save": true
CHECK-NEXT:       }
CHECK-NEXT:     },
CHECK-NEXT:     "serverInfo": {
CHECK-NEXT:       "name": "nixd",
CHECK-NEXT:       "version": {{.*}}
CHECK-NEXT:     }
CHECK-NEXT:   }
CHECK-NEXT: }
```

<-- initialized

```json
{
   "jsonrpc":"2.0",
   "method":"initialized",
   "params":{

   }
}
```


```json
{"jsonrpc":"2.0","method":"exit"}
```
