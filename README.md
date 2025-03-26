# up-status-render


* Standard XR, no status in spec
```sh
up composition render apis/xrs/composition.yaml examples/xr.yaml --function-credentials=examples/azure-creds.yaml
  ✓   Checking dependencies
  ✓   Generating language schemas
  ✓   Building functions
  ✓   Building configuration package
  ✓   Pushing embedded functions to local daemon
▄  Rendering (4s)---
apiVersion: example.crossplane.io/v1
kind: XR
metadata:
  name: example-xr
status:
  azResourceGraphQueryResult:
  - Count: 255
  conditions:
  - lastTransitionTime: "2024-01-01T00:00:00Z"
    reason: Available
    status: "True"
    type: Ready
  - lastTransitionTime: "2024-01-01T00:00:00Z"
    reason: Success
    status: "True"
    type: FunctionSuccess
  ✓   Rendering
```

* XR with existing status produces error

```
up composition render apis/xrs/composition.yaml examples/xr-with-existing-status.yaml --function-credentials=examples/azure-creds.yaml
  ✓   Checking dependencies
  ✓   Generating language schemas
  ✓   Building functions
  ✓   Building configuration package
  ✓   Pushing embedded functions to local daemon
▄  Rendering (2s)W0326 13:13:22.743328   32508 logging.go:55] [core] [Channel #2 SubChannel #5]grpc: addrConn.createTransport failed to connect to {Addr: "127.0.0.1:61679", ServerName: "127.0.0.1:61679", }. Err: connection error: desc = "error reading server preface: EOF"
 ▀ Rendering (3s)---
apiVersion: example.crossplane.io/v1
kind: XR
metadata:
  name: example-xr
status:
  azResourceGraphQueryResult: alreadyExist
  conditions:
  - lastTransitionTime: "2024-01-01T00:00:00Z"
    reason: Available
    status: "True"
    type: Ready
  - lastTransitionTime: "2024-01-01T00:00:00Z"
    message: Target already has data, skipped query to avoid throttling
    reason: SkippedQuery
    status: "True"
    type: FunctionSkip
  - lastTransitionTime: "2024-01-01T00:00:00Z"
    reason: Success
    status: "True"
    type: FunctionSuccess
  ✓   Rendering
```

At the same time status conditions are properly exposed in both cases
