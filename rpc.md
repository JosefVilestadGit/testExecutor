# Colonies RPC API Reference

This document provides a comprehensive reference for all RPC calls in the Colonies system. Each RPC call follows a consistent pattern with a unique PayloadType and corresponding message structure.

## RPC Message Structure

All RPC requests are wrapped in the generic `RPCMsg` structure:

```json
{
  "signature": "cryptographic_signature",
  "payloadtype": "specific_payload_type",
  "payload": "base64_encoded_json_payload"
}
```

All RPC responses are wrapped in the generic `RPCReplyMsg` structure:

```json
{
  "payloadtype": "reply_payload_type",
  "payload": "base64_encoded_json_payload",
  "error": false
}
```

## Colony Operations

Operations for managing colonies (distributed runtime environments).

### Add Colony
- **PayloadType**: `addcolonymsg`
- **Struct**: `AddColonyMsg`
- **Required Fields**:
  - `colony` (*core.Colony): Colony object to add
  - `msgtype` (string): Message type identifier

### Get Colony
- **PayloadType**: `getcolonymsg`
- **Struct**: `GetColonyMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony to retrieve
  - `msgtype` (string): Message type identifier

### Get Colonies
- **PayloadType**: `getcoloniesmsg`
- **Struct**: `GetColoniesMsg`
- **Required Fields**:
  - `msgtype` (string): Message type identifier

### Remove Colony
- **PayloadType**: `removecolonymsg`
- **Struct**: `RemoveColonyMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony to remove
  - `msgtype` (string): Message type identifier

### Change Colony ID
- **PayloadType**: `changecolonyidmsg`
- **Struct**: `ChangeColonyIDMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `colonyid` (string): New colony ID
  - `msgtype` (string): Message type identifier

### Get Colony Statistics
- **PayloadType**: `getcolonystatsmsg`
- **Struct**: `GetColonyStatisticsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

## Executor Operations

Operations for managing executors (distributed workers that execute processes).

### Add Executor
- **PayloadType**: `addexecutormsg`
- **Struct**: `AddExecutorMsg`
- **Required Fields**:
  - `executor` (*core.Executor): Executor object to add
  - `msgtype` (string): Message type identifier

### Get Executor
- **PayloadType**: `getexecutormsg`
- **Struct**: `GetExecutorMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor
  - `msgtype` (string): Message type identifier

### Get Executors
- **PayloadType**: `getexecutorsmsg`
- **Struct**: `GetExecutorsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Get Executor By ID
- **PayloadType**: `getexecutorbyidmsg`
- **Struct**: `GetExecutorByIDMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorid` (string): ID of the executor
  - `msgtype` (string): Message type identifier

### Approve Executor
- **PayloadType**: `approveexecutormsg`
- **Struct**: `ApproveExecutorRPC`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor to approve
  - `msgtype` (string): Message type identifier

### Reject Executor
- **PayloadType**: `rejectexecutormsg`
- **Struct**: `RejectExecutorMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor to reject
  - `msgtype` (string): Message type identifier

### Remove Executor
- **PayloadType**: `removeexecutormsg`
- **Struct**: `RemoveExecutorMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor to remove
  - `msgtype` (string): Message type identifier

### Change Executor ID
- **PayloadType**: `changeexecutoridmsg`
- **Struct**: `ChangeExecutorIDMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorid` (string): New executor ID
  - `msgtype` (string): Message type identifier

### Report Allocations
- **PayloadType**: `reportallocationmsg`
- **Struct**: `ReportAllocationsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor
  - `allocations` (core.Allocations): Resource allocation information
  - `msgtype` (string): Message type identifier

## Process Operations

Operations for managing processes (computational workloads).

### Submit Function Spec
- **PayloadType**: `submitfuncspecmsg`
- **Struct**: `SubmitFunctionSpecMsg`
- **Required Fields**:
  - `spec` (*core.FunctionSpec): Function specification to submit
  - `msgtype` (string): Message type identifier

### Assign Process
- **PayloadType**: `assignprocessmsg`
- **Struct**: `AssignProcessMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `timeout` (int): Timeout in seconds
  - `availablecpu` (string): Available CPU resources
  - `availablememory` (string): Available memory resources
  - `msgtype` (string): Message type identifier

### Close Successful
- **PayloadType**: `closesuccessfulmsg`
- **Struct**: `CloseSuccessfulMsg`
- **Required Fields**:
  - `processid` (string): ID of the process
  - `output` ([]interface{}): Process output data
  - `msgtype` (string): Message type identifier

### Close Failed
- **PayloadType**: `closefailedmsg`
- **Struct**: `CloseFailedMsg`
- **Required Fields**:
  - `processid` (string): ID of the process
  - `errors` ([]string): Error messages
  - `msgtype` (string): Message type identifier

### Get Process
- **PayloadType**: `getprocessmsg`
- **Struct**: `GetProcessMsg`
- **Required Fields**:
  - `processid` (string): ID of the process
  - `msgtype` (string): Message type identifier

### Get Processes
- **PayloadType**: `getprocessesmsg`
- **Struct**: `GetProcessesMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of processes to retrieve
  - `state` (int): Process state filter
  - `executortype` (string): Executor type filter
  - `label` (string): Label filter
  - `initiator` (string): Initiator filter
  - `msgtype` (string): Message type identifier

### Remove Process
- **PayloadType**: `removeprocessmsg`
- **Struct**: `RemoveProcessMsg`
- **Required Fields**:
  - `processid` (string): ID of the process to remove
  - `all` (bool): Remove all related processes
  - `msgtype` (string): Message type identifier

### Remove All Processes
- **PayloadType**: `removeallprocessesmsg`
- **Struct**: `RemoveAllProcessesMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `state` (int): State filter for processes to remove
  - `msgtype` (string): Message type identifier

### Set Output
- **PayloadType**: `setoutputmsg`
- **Struct**: `SetOutputMsg`
- **Required Fields**:
  - `processid` (string): ID of the process
  - `output` ([]interface{}): Output data to set
  - `msgtype` (string): Message type identifier

### Add Child
- **PayloadType**: `addchildmsg`
- **Struct**: `AddChildMsg`
- **Required Fields**:
  - `processgraphid` (string): ID of the process graph
  - `parentprocessid` (string): ID of the parent process
  - `childprocessid` (string): ID of the child process
  - `spec` (*core.FunctionSpec): Function specification for the child
  - `insert` (bool): Whether to insert the child
  - `msgtype` (string): Message type identifier

### Subscribe Process
- **PayloadType**: `subscribeprocessmsg`
- **Struct**: `SubscribeProcessMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `processid` (string): ID of the process to subscribe to
  - `executortype` (string): Executor type filter
  - `state` (int): State filter
  - `timeout` (int): Subscription timeout
  - `msgtype` (string): Message type identifier

### Subscribe Processes
- **PayloadType**: `subscribeprocessesmsg`
- **Struct**: `SubscribeProcessesMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executortype` (string): Executor type filter
  - `state` (int): State filter
  - `timeout` (int): Subscription timeout
  - `msgtype` (string): Message type identifier

### Get Process Graph
- **PayloadType**: `getprocessgraphmsg`
- **Struct**: `GetProcessGraphMsg`
- **Required Fields**:
  - `processgraphid` (string): ID of the process graph
  - `msgtype` (string): Message type identifier

### Get Process Graphs
- **PayloadType**: `getprocessgraphsmsg`
- **Struct**: `GetProcessGraphsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of graphs to retrieve
  - `state` (int): State filter
  - `msgtype` (string): Message type identifier

### Remove Process Graph
- **PayloadType**: `removeprocessgraphmsg`
- **Struct**: `RemoveProcessGraphMsg`
- **Required Fields**:
  - `processgraphid` (string): ID of the process graph to remove
  - `all` (bool): Remove all related graphs
  - `msgtype` (string): Message type identifier

### Remove All Process Graphs
- **PayloadType**: `removeallprocessgraphsmsg`
- **Struct**: `RemoveAllProcessGraphsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `state` (int): State filter for graphs to remove
  - `msgtype` (string): Message type identifier

### Get Process History
- **PayloadType**: `getprocesshistmsg`
- **Struct**: `GetProcessHistMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorid` (string): ID of the executor
  - `seconds` (int): Time range in seconds
  - `state` (int): State filter
  - `msgtype` (string): Message type identifier

### Pause Assignments
- **PayloadType**: `pauseassignmentsmsg`
- **Struct**: `PauseAssignmentsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Resume Assignments
- **PayloadType**: `resumeassignmentsmsg`
- **Struct**: `ResumeAssignmentsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Get Pause Status
- **PayloadType**: `getpausestatusmsg`
- **Struct**: `GetPauseStatusMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

## Function Operations

Operations for managing executor functions (capabilities offered by executors).

### Add Function
- **PayloadType**: `addfunctionmsg`
- **Struct**: `AddFunctionMsg`
- **Required Fields**:
  - `function` (*core.Function): Function object to add
  - `msgtype` (string): Message type identifier

### Get Functions
- **PayloadType**: `getfunctionsmsg`
- **Struct**: `GetFunctionsMsg`
- **Required Fields**:
  - `executorname` (string): Name of the executor
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Remove Function
- **PayloadType**: `removefunctionmsg`
- **Struct**: `RemoveFunctionMsg`
- **Required Fields**:
  - `functionid` (string): ID of the function to remove
  - `msgtype` (string): Message type identifier

## File Operations

Operations for managing files in the distributed file system.

### Add File
- **PayloadType**: `addfilemsg`
- **Struct**: `AddFileMsg`
- **Required Fields**:
  - `file` (*core.File): File object to add
  - `msgtype` (string): Message type identifier

### Get File
- **PayloadType**: `getfilemsg`
- **Struct**: `GetFileMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `fileid` (string): ID of the file
  - `label` (string): Label filter
  - `name` (string): Name filter
  - `latest` (bool): Retrieve latest version
  - `msgtype` (string): Message type identifier

### Get Files
- **PayloadType**: `getfilesmsg`
- **Struct**: `GetFilesMsg`
- **Required Fields**:
  - `label` (string): Label filter
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Remove File
- **PayloadType**: `removefilemsg`
- **Struct**: `RemoveFileMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `fileid` (string): ID of the file
  - `label` (string): Label filter
  - `name` (string): Name filter
  - `msgtype` (string): Message type identifier

### Get File Labels
- **PayloadType**: `getfilelabelsmsg`
- **Struct**: `GetFileLabelsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Name filter
  - `exact` (bool): Exact match flag
  - `msgtype` (string): Message type identifier

## Cron Operations

Operations for managing scheduled jobs (cron-like functionality).

### Add Cron
- **PayloadType**: `addcronmsg`
- **Struct**: `AddCronMsg`
- **Required Fields**:
  - `cron` (*core.Cron): Cron object to add
  - `msgtype` (string): Message type identifier

### Get Cron
- **PayloadType**: `getcronmsg`
- **Struct**: `GetCronMsg`
- **Required Fields**:
  - `cronid` (string): ID of the cron job
  - `msgtype` (string): Message type identifier

### Get Crons
- **PayloadType**: `getcronsmsg`
- **Struct**: `GetCronsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of crons to retrieve
  - `msgtype` (string): Message type identifier

### Remove Cron
- **PayloadType**: `removecronmsg`
- **Struct**: `RemoveCronMsg`
- **Required Fields**:
  - `cronid` (string): ID of the cron job to remove
  - `all` (bool): Remove all related crons
  - `msgtype` (string): Message type identifier

### Run Cron
- **PayloadType**: `runcronmsg`
- **Struct**: `RunCronMsg`
- **Required Fields**:
  - `cronid` (string): ID of the cron job to run
  - `msgtype` (string): Message type identifier

## Generator Operations

Operations for managing process generators (dynamic process creation).

### Add Generator
- **PayloadType**: `addgeneratormsg`
- **Struct**: `AddGeneratorMsg`
- **Required Fields**:
  - `generator` (*core.Generator): Generator object to add
  - `msgtype` (string): Message type identifier

### Get Generator
- **PayloadType**: `getgeneratormsg`
- **Struct**: `GetGeneratorMsg`
- **Required Fields**:
  - `generatorid` (string): ID of the generator
  - `msgtype` (string): Message type identifier

### Get Generators
- **PayloadType**: `getgeneratorsmsg`
- **Struct**: `GetGeneratorsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of generators to retrieve
  - `msgtype` (string): Message type identifier

### Remove Generator
- **PayloadType**: `removegeneratormsg`
- **Struct**: `RemoveGeneratorMsg`
- **Required Fields**:
  - `generatorid` (string): ID of the generator to remove
  - `all` (bool): Remove all related generators
  - `msgtype` (string): Message type identifier

### Resolve Generator
- **PayloadType**: `resolvegeneratormsg`
- **Struct**: `ResolveGeneratorMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `generatorname` (string): Name of the generator
  - `msgtype` (string): Message type identifier

### Pack Generator
- **PayloadType**: `packgeneratormsg`
- **Struct**: `PackGeneratorMsg`
- **Required Fields**:
  - `generatorid` (string): ID of the generator
  - `arg` (string): Argument to pack
  - `msgtype` (string): Message type identifier

## Blueprint Operations

Operations for managing blueprints (infrastructure as code definitions).

### Add Blueprint
- **PayloadType**: `addblueprintmsg`
- **Struct**: `AddBlueprintMsg`
- **Required Fields**:
  - `blueprint` (*core.Blueprint): Blueprint object to add
  - `msgtype` (string): Message type identifier

### Add Blueprint Definition
- **PayloadType**: `addblueprintdefinitionmsg`
- **Struct**: `AddBlueprintDefinitionMsg`
- **Required Fields**:
  - `blueprintdefinition` (*core.BlueprintDefinition): Blueprint definition to add
  - `msgtype` (string): Message type identifier

### Get Blueprint
- **PayloadType**: `getblueprintmsg`
- **Struct**: `GetBlueprintMsg`
- **Required Fields**:
  - `namespace` (string): Blueprint namespace
  - `name` (string): Blueprint name
  - `msgtype` (string): Message type identifier

### Get Blueprints
- **PayloadType**: `getblueprintsmsg`
- **Struct**: `GetBlueprintsMsg`
- **Required Fields**:
  - `namespace` (string): Blueprint namespace filter
  - `kind` (string): Blueprint kind filter
  - `msgtype` (string): Message type identifier

### Get Blueprint Definition
- **PayloadType**: `getblueprintdefinitionmsg`
- **Struct**: `GetBlueprintDefinitionMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Definition name
  - `msgtype` (string): Message type identifier

### Get Blueprint Definitions
- **PayloadType**: `getblueprintdefinitionsmsg`
- **Struct**: `GetBlueprintDefinitionsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Get Blueprint History
- **PayloadType**: `getblueprinthistorymsg`
- **Struct**: `GetBlueprintHistoryMsg`
- **Required Fields**:
  - `blueprintid` (string): ID of the blueprint
  - `limit` (int): Maximum number of history entries
  - `msgtype` (string): Message type identifier

### Remove Blueprint
- **PayloadType**: `removeblueprintmsg`
- **Struct**: `RemoveBlueprintMsg`
- **Required Fields**:
  - `namespace` (string): Blueprint namespace
  - `name` (string): Blueprint name
  - `msgtype` (string): Message type identifier

### Remove Blueprint Definition
- **PayloadType**: `removeblueprintdefinitionmsg`
- **Struct**: `RemoveBlueprintDefinitionMsg`
- **Required Fields**:
  - `namespace` (string): Definition namespace
  - `name` (string): Definition name
  - `msgtype` (string): Message type identifier

### Update Blueprint
- **PayloadType**: `updateblueprintmsg`
- **Struct**: `UpdateBlueprintMsg`
- **Required Fields**:
  - `blueprint` (*core.Blueprint): Updated blueprint object
  - `msgtype` (string): Message type identifier

## Node Operations

Operations for managing nodes in the distributed system.

### Get Node
- **PayloadType**: `getnodemsg`
- **Struct**: `GetNodeMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `nodename` (string): Name of the node
  - `msgtype` (string): Message type identifier

### Get Nodes
- **PayloadType**: `getnodesmsg`
- **Struct**: `GetNodesMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Get Nodes By Location
- **PayloadType**: `getnodesbylocationmsg`
- **Struct**: `GetNodesByLocationMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `location` (string): Location filter
  - `msgtype` (string): Message type identifier

## User Operations

Operations for managing users and authentication.

### Add User
- **PayloadType**: `addusermsg`
- **Struct**: `AddUserMsg`
- **Required Fields**:
  - `user` (*core.User): User object to add
  - `msgtype` (string): Message type identifier

### Get User
- **PayloadType**: `getusermsg`
- **Struct**: `GetUserMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Username
  - `msgtype` (string): Message type identifier

### Get User By ID
- **PayloadType**: `getuserbyidmsg`
- **Struct**: `GetUserByIDMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `userid` (string): ID of the user
  - `msgtype` (string): Message type identifier

### Get Users
- **PayloadType**: `getusersmsg`
- **Struct**: `GetUsersMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Remove User
- **PayloadType**: `removeusermsg`
- **Struct**: `RemoveUserMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Username to remove
  - `msgtype` (string): Message type identifier

### Change User ID
- **PayloadType**: `changeuseridmsg`
- **Struct**: `ChangeUserIDMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `userid` (string): New user ID
  - `msgtype` (string): Message type identifier

## Attribute Operations

Operations for managing custom attributes.

### Add Attribute
- **PayloadType**: `addattributemsg`
- **Struct**: `AddAttributeMsg`
- **Required Fields**:
  - `attribute` (core.Attribute): Attribute object to add
  - `msgtype` (string): Message type identifier

### Get Attribute
- **PayloadType**: `getattributemsg`
- **Struct**: `GetAttributeMsg`
- **Required Fields**:
  - `attributeid` (string): ID of the attribute
  - `msgtype` (string): Message type identifier

## Log Operations

Operations for managing process logs.

### Add Log
- **PayloadType**: `addlogmsg`
- **Struct**: `AddLogMsg`
- **Required Fields**:
  - `processid` (string): ID of the process
  - `message` (string): Log message
  - `msgtype` (string): Message type identifier

### Get Logs
- **PayloadType**: `getlogsmsg`
- **Struct**: `GetLogsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `processid` (string): ID of the process
  - `executorname` (string): Name of the executor
  - `count` (int): Number of logs to retrieve
  - `since` (int64): Timestamp filter
  - `msgtype` (string): Message type identifier

### Search Logs
- **PayloadType**: `searchlogsmsg`
- **Struct**: `SearchLogsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `text` (string): Search text
  - `days` (int): Number of days to search
  - `count` (int): Maximum number of results
  - `msgtype` (string): Message type identifier

## Snapshot Operations

Operations for managing file system snapshots.

### Create Snapshot
- **PayloadType**: `createsnapshotmsg`
- **Struct**: `CreateSnapshotMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `label` (string): Snapshot label
  - `name` (string): Snapshot name
  - `msgtype` (string): Message type identifier

### Get Snapshot
- **PayloadType**: `getsnapshotmsg`
- **Struct**: `GetSnapshotMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `snapshotid` (string): ID of the snapshot
  - `name` (string): Snapshot name
  - `msgtype` (string): Message type identifier

### Get Snapshots
- **PayloadType**: `getsnapshotsmsg`
- **Struct**: `GetSnapshotsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

### Remove Snapshot
- **PayloadType**: `removesnapshotmsg`
- **Struct**: `RemoveSnapshotMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `snapshotid` (string): ID of the snapshot
  - `name` (string): Snapshot name
  - `msgtype` (string): Message type identifier

### Remove All Snapshots
- **PayloadType**: `removeallsnapshotmsg`
- **Struct**: `RemoveAllSnapshotsMsg`
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `msgtype` (string): Message type identifier

## Cluster Operations

Operations for managing cluster information.

### Get Cluster
- **PayloadType**: `getclustermsg`
- **Struct**: `GetClusterMsg`
- **Required Fields**:
  - `msgtype` (string): Message type identifier

## Workflow Operations

Operations for managing workflow specifications.

### Submit Workflow Spec
- **PayloadType**: `submitworkflowspecmsg`
- **Struct**: `SubmitWorkflowSpecMsg`
- **Required Fields**:
  - `workflowspec` (*core.WorkflowSpec): Workflow specification to submit
  - `msgtype` (string): Message type identifier

## System Operations

System-level operations for server management.

### Version
- **PayloadType**: `versionmsg`
- **Struct**: `VersionMsg`
- **Required Fields**:
  - `buildversion` (string): Build version string
  - `buildtime` (string): Build timestamp
  - `msgtype` (string): Message type identifier

### Get Server Info
- **PayloadType**: `getserverinfomsg`
- **Struct**: `GetServerInfoMsg`
- **Required Fields**:
  - `msgtype` (string): Message type identifier

### Get Statistics
- **PayloadType**: `getstatisticsmsg`
- **Struct**: `GetStatisticsMsg`
- **Required Fields**:
  - `msgtype` (string): Message type identifier

### Change Server ID
- **PayloadType**: `changeserveridmsg`
- **Struct**: `ChangeServerIDMsg`
- **Required Fields**:
  - `serverid` (string): New server ID
  - `msgtype` (string): Message type identifier

## Notes

- All RPC messages include a `msgtype` field that matches the PayloadType constant
- Fields use Go type notation where `*core.Type` indicates a pointer to a core domain object
- The actual payload is base64-encoded JSON within the RPCMsg wrapper
- All communications are cryptographically signed for zero-trust security
- Field names in JSON payloads follow Go's JSON marshaling conventions (typically lowercase)
