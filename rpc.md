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

## Authentication

All RPC messages are authenticated using cryptographic signatures. The signature is created using the caller's private key and verified by the server using ECDSA signature recovery. The system supports four types of private keys:

- **Server Owner Key**: Controls server-level operations
- **Colony Owner Key**: Controls colony administration operations
- **Executor Key**: Used by executors for process execution
- **User Key**: Used by users for submitting work and accessing resources

## Colony Operations

Operations for managing colonies (distributed runtime environments).

### Add Colony
- **PayloadType**: `addcolonymsg`
- **Credentials**: Server Owner Private Key
- **Required Fields**:
  - `colony` (*core.Colony): Colony object to add

### Get Colony
- **PayloadType**: `getcolonymsg`
- **Credentials**: Executor or User Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony to retrieve

### Get Colonies
- **PayloadType**: `getcoloniesmsg`
- **Credentials**: Server Owner Private Key
- **Required Fields**: None

### Remove Colony
- **PayloadType**: `removecolonymsg`
- **Credentials**: Server Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony to remove

### Change Colony ID
- **PayloadType**: `changecolonyidmsg`
- **Credentials**: Server Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `colonyid` (string): New colony ID

### Get Colony Statistics
- **PayloadType**: `getcolonystatsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

## Executor Operations

Operations for managing executors (distributed workers that execute processes).

### Add Executor
- **PayloadType**: `addexecutormsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `executor` (*core.Executor): Executor object to add

### Get Executor
- **PayloadType**: `getexecutormsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor

### Get Executors
- **PayloadType**: `getexecutorsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Get Executor By ID
- **PayloadType**: `getexecutorbyidmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorid` (string): ID of the executor

### Approve Executor
- **PayloadType**: `approveexecutormsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor to approve

### Reject Executor
- **PayloadType**: `rejectexecutormsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor to reject

### Remove Executor
- **PayloadType**: `removeexecutormsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor to remove

### Change Executor ID
- **PayloadType**: `changeexecutoridmsg`
- **Credentials**: Executor Private Key (self-update only)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorid` (string): New executor ID

### Report Allocations
- **PayloadType**: `reportallocationmsg`
- **Credentials**: Executor Private Key (approved)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorname` (string): Name of the executor
  - `allocations` (core.Allocations): Resource allocation information

## Process Operations

Operations for managing processes (computational workloads).

### Submit Function Spec
- **PayloadType**: `submitfuncspecmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `spec` (*core.FunctionSpec): Function specification to submit

### Assign Process
- **PayloadType**: `assignprocessmsg`
- **Credentials**: Executor Private Key (approved)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `timeout` (int): Timeout in seconds
  - `availablecpu` (string): Available CPU resources
  - `availablememory` (string): Available memory resources

### Close Successful
- **PayloadType**: `closesuccessfulmsg`
- **Credentials**: Executor Private Key (must be assigned to the process)
- **Required Fields**:
  - `processid` (string): ID of the process
  - `output` ([]interface{}): Process output data

### Close Failed
- **PayloadType**: `closefailedmsg`
- **Credentials**: Executor Private Key (must be assigned to the process)
- **Required Fields**:
  - `processid` (string): ID of the process
  - `errors` ([]string): Error messages

### Get Process
- **PayloadType**: `getprocessmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `processid` (string): ID of the process

### Get Processes
- **PayloadType**: `getprocessesmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of processes to retrieve
  - `state` (int): Process state filter
  - `executortype` (string): Executor type filter
  - `label` (string): Label filter
  - `initiator` (string): Initiator filter

### Remove Process
- **PayloadType**: `removeprocessmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `processid` (string): ID of the process to remove
  - `all` (bool): Remove all related processes

### Remove All Processes
- **PayloadType**: `removeallprocessesmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `state` (int): State filter for processes to remove

### Set Output
- **PayloadType**: `setoutputmsg`
- **Credentials**: Executor Private Key (must be assigned to the process)
- **Required Fields**:
  - `processid` (string): ID of the process
  - `output` ([]interface{}): Output data to set

### Add Child
- **PayloadType**: `addchildmsg`
- **Credentials**: Executor Private Key (must be assigned to parent process)
- **Required Fields**:
  - `processgraphid` (string): ID of the process graph
  - `parentprocessid` (string): ID of the parent process
  - `childprocessid` (string): ID of the child process
  - `spec` (*core.FunctionSpec): Function specification for the child
  - `insert` (bool): Whether to insert the child

### Subscribe Process
- **PayloadType**: `subscribeprocessmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `processid` (string): ID of the process to subscribe to
  - `executortype` (string): Executor type filter
  - `state` (int): State filter
  - `timeout` (int): Subscription timeout

### Subscribe Processes
- **PayloadType**: `subscribeprocessesmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executortype` (string): Executor type filter
  - `state` (int): State filter
  - `timeout` (int): Subscription timeout

### Get Process Graph
- **PayloadType**: `getprocessgraphmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `processgraphid` (string): ID of the process graph

### Get Process Graphs
- **PayloadType**: `getprocessgraphsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of graphs to retrieve
  - `state` (int): State filter

### Remove Process Graph
- **PayloadType**: `removeprocessgraphmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `processgraphid` (string): ID of the process graph to remove
  - `all` (bool): Remove all related graphs

### Remove All Process Graphs
- **PayloadType**: `removeallprocessgraphsmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `state` (int): State filter for graphs to remove

### Get Process History
- **PayloadType**: `getprocesshistmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `executorid` (string): ID of the executor
  - `seconds` (int): Time range in seconds
  - `state` (int): State filter

### Pause Assignments
- **PayloadType**: `pauseassignmentsmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Resume Assignments
- **PayloadType**: `resumeassignmentsmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Get Pause Status
- **PayloadType**: `getpausestatusmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

## Function Operations

Operations for managing executor functions (capabilities offered by executors).

### Add Function
- **PayloadType**: `addfunctionmsg`
- **Credentials**: Executor Private Key (approved)
- **Required Fields**:
  - `function` (*core.Function): Function object to add

### Get Functions
- **PayloadType**: `getfunctionsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `executorname` (string): Name of the executor
  - `colonyname` (string): Name of the colony

### Remove Function
- **PayloadType**: `removefunctionmsg`
- **Credentials**: Executor Private Key (approved)
- **Required Fields**:
  - `functionid` (string): ID of the function to remove

## File Operations

Operations for managing files in the distributed file system.

### Add File
- **PayloadType**: `addfilemsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `file` (*core.File): File object to add

### Get File
- **PayloadType**: `getfilemsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `fileid` (string): ID of the file
  - `label` (string): Label filter
  - `name` (string): Name filter
  - `latest` (bool): Retrieve latest version

### Get Files
- **PayloadType**: `getfilesmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `label` (string): Label filter
  - `colonyname` (string): Name of the colony

### Remove File
- **PayloadType**: `removefilemsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `fileid` (string): ID of the file
  - `label` (string): Label filter
  - `name` (string): Name filter

### Get File Labels
- **PayloadType**: `getfilelabelsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Name filter
  - `exact` (bool): Exact match flag

## Cron Operations

Operations for managing scheduled jobs (cron-like functionality).

### Add Cron
- **PayloadType**: `addcronmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `cron` (*core.Cron): Cron object to add

### Get Cron
- **PayloadType**: `getcronmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `cronid` (string): ID of the cron job

### Get Crons
- **PayloadType**: `getcronsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of crons to retrieve

### Remove Cron
- **PayloadType**: `removecronmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `cronid` (string): ID of the cron job to remove
  - `all` (bool): Remove all related crons

### Run Cron
- **PayloadType**: `runcronmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `cronid` (string): ID of the cron job to run

## Generator Operations

Operations for managing process generators (dynamic process creation).

### Add Generator
- **PayloadType**: `addgeneratormsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `generator` (*core.Generator): Generator object to add

### Get Generator
- **PayloadType**: `getgeneratormsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `generatorid` (string): ID of the generator

### Get Generators
- **PayloadType**: `getgeneratorsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `count` (int): Number of generators to retrieve

### Remove Generator
- **PayloadType**: `removegeneratormsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `generatorid` (string): ID of the generator to remove
  - `all` (bool): Remove all related generators

### Resolve Generator
- **PayloadType**: `resolvegeneratormsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `generatorname` (string): Name of the generator

### Pack Generator
- **PayloadType**: `packgeneratormsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `generatorid` (string): ID of the generator
  - `arg` (string): Argument to pack

## Blueprint Operations

Operations for managing blueprints (infrastructure as code definitions).

### Add Blueprint
- **PayloadType**: `addblueprintmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `blueprint` (*core.Blueprint): Blueprint object to add

### Add Blueprint Definition
- **PayloadType**: `addblueprintdefinitionmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `blueprintdefinition` (*core.BlueprintDefinition): Blueprint definition to add

### Get Blueprint
- **PayloadType**: `getblueprintmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `namespace` (string): Blueprint namespace
  - `name` (string): Blueprint name

### Get Blueprints
- **PayloadType**: `getblueprintsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `namespace` (string): Blueprint namespace filter
  - `kind` (string): Blueprint kind filter

### Get Blueprint Definition
- **PayloadType**: `getblueprintdefinitionmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Definition name

### Get Blueprint Definitions
- **PayloadType**: `getblueprintdefinitionsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Get Blueprint History
- **PayloadType**: `getblueprinthistorymsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `blueprintid` (string): ID of the blueprint
  - `limit` (int): Maximum number of history entries

### Remove Blueprint
- **PayloadType**: `removeblueprintmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `namespace` (string): Blueprint namespace
  - `name` (string): Blueprint name

### Remove Blueprint Definition
- **PayloadType**: `removeblueprintdefinitionmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `namespace` (string): Definition namespace
  - `name` (string): Definition name

### Update Blueprint
- **PayloadType**: `updateblueprintmsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `blueprint` (*core.Blueprint): Updated blueprint object

## Node Operations

Operations for managing nodes in the distributed system.

### Get Node
- **PayloadType**: `getnodemsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `nodename` (string): Name of the node

### Get Nodes
- **PayloadType**: `getnodesmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Get Nodes By Location
- **PayloadType**: `getnodesbylocationmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `location` (string): Location filter

## User Operations

Operations for managing users and authentication.

### Add User
- **PayloadType**: `addusermsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `user` (*core.User): User object to add

### Get User
- **PayloadType**: `getusermsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Username

### Get User By ID
- **PayloadType**: `getuserbyidmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `userid` (string): ID of the user

### Get Users
- **PayloadType**: `getusersmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Remove User
- **PayloadType**: `removeusermsg`
- **Credentials**: Colony Owner Private Key
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `name` (string): Username to remove

### Change User ID
- **PayloadType**: `changeuseridmsg`
- **Credentials**: User Private Key (self-update only)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `userid` (string): New user ID

## Attribute Operations

Operations for managing custom attributes.

### Add Attribute
- **PayloadType**: `addattributemsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `attribute` (core.Attribute): Attribute object to add

### Get Attribute
- **PayloadType**: `getattributemsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `attributeid` (string): ID of the attribute

## Log Operations

Operations for managing process logs.

### Add Log
- **PayloadType**: `addlogmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `processid` (string): ID of the process
  - `message` (string): Log message

### Get Logs
- **PayloadType**: `getlogsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `processid` (string): ID of the process
  - `executorname` (string): Name of the executor
  - `count` (int): Number of logs to retrieve
  - `since` (int64): Timestamp filter

### Search Logs
- **PayloadType**: `searchlogsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `text` (string): Search text
  - `days` (int): Number of days to search
  - `count` (int): Maximum number of results

## Snapshot Operations

Operations for managing file system snapshots.

### Create Snapshot
- **PayloadType**: `createsnapshotmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `label` (string): Snapshot label
  - `name` (string): Snapshot name

### Get Snapshot
- **PayloadType**: `getsnapshotmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `snapshotid` (string): ID of the snapshot
  - `name` (string): Snapshot name

### Get Snapshots
- **PayloadType**: `getsnapshotsmsg`
- **Credentials**: Executor or User Private Key (colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

### Remove Snapshot
- **PayloadType**: `removesnapshotmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony
  - `snapshotid` (string): ID of the snapshot
  - `name` (string): Snapshot name

### Remove All Snapshots
- **PayloadType**: `removeallsnapshotmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `colonyname` (string): Name of the colony

## Cluster Operations

Operations for managing cluster information.

### Get Cluster
- **PayloadType**: `getclustermsg`
- **Credentials**: None (public endpoint)
- **Required Fields**: None

## Workflow Operations

Operations for managing workflow specifications.

### Submit Workflow Spec
- **PayloadType**: `submitworkflowspecmsg`
- **Credentials**: Executor or User Private Key (approved colony member)
- **Required Fields**:
  - `workflowspec` (*core.WorkflowSpec): Workflow specification to submit

## System Operations

System-level operations for server management.

### Version
- **PayloadType**: `versionmsg`
- **Credentials**: None (public endpoint)
- **Required Fields**:
  - `buildversion` (string): Build version string
  - `buildtime` (string): Build timestamp

### Get Server Info
- **PayloadType**: `getserverinfomsg`
- **Credentials**: None (public endpoint)
- **Required Fields**: None

### Get Statistics
- **PayloadType**: `getstatisticsmsg`
- **Credentials**: Server Owner Private Key
- **Required Fields**: None

### Change Server ID
- **PayloadType**: `changeserveridmsg`
- **Credentials**: Server Owner Private Key
- **Required Fields**:
  - `serverid` (string): New server ID

## Notes

- All RPC messages use a zero-trust security model with cryptographic signatures
- The signature is created using ECDSA and verified using signature recovery
- Each private key generates a deterministic 64-character hexadecimal ID
- Field names in JSON payloads follow Go's JSON marshaling conventions (typically lowercase)
- Fields use Go type notation where `*core.Type` indicates a pointer to a core domain object
- The actual payload is base64-encoded JSON within the RPCMsg wrapper
