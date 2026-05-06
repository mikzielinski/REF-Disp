# REF-Disp (Dispatcher) #

## What this project is ##

`REF-Disp` is a **dispatcher robot**.  
Its job is to read source data and create queue items in Orchestrator for a performer robot.

This is not a full classic REFramework performer flow with `GetTransactionData` + `SetTransactionStatus` per transaction.

## Main flow ##

1. **Initialize settings**
* `Framework/InitAllSettings.xaml` loads values from `Data/Config.json`.
* Configuration is exposed through `Config` dictionary.

2. **Initialize applications**
* `Framework/InitAllApplications.xaml` starts required apps/connections (if needed in your scenario).

3. **Dispatch data to queue**
* `Framework/Process.xaml` prepares source rows and enqueues them.
* `AddQueueItem` uses:
  * `QueueType` from `Config("OrchestratorQueueName")`
  * `FolderPath` from `Config("OrchestratorQueueFolder")`
  * `ItemInformation` dictionary (business payload)

4. **Close applications**
* `Framework/CloseAllApplications.xaml` closes resources.

5. **Write run report**
* `Main.xaml` writes summary JSON to:
  * `Data/Output/RunReport_{RunId}.json` (when `RunReportEnabled=true`)

## Inputs ##

`Main.xaml` supports input arguments:

* `in_OrchestratorQueueName` - optional queue override
* `in_OrchestratorQueueFolder` - optional folder override
* `in_EnvironmentType` - optional environment override (`DEV`, `TEST`, `UAT`, `PROD`)

## Configuration ##

Core config is in `Data/Config.json`:

* Queue settings (`OrchestratorQueueName`, `OrchestratorQueueFolder`)
* Runtime mode (`Settings.RuntimeMode`)
* Run report flags/path (`RunReportEnabled`, `RunReportOutputPath`)
* Optional AI section (`AI`)


## Additional docs ##

* Base REFramework PDF: `Documentation/REFramework Documentation-EN.pdf`
* Local implementation notes: `Documentation/LOCAL_PROJECT_NOTES.md`
