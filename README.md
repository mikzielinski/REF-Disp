### Documentation is included in the Documentation folder ###


### REFrameWork Template ###
**Robotic Enterprise Framework**

* Built on top of *Transactional Business Process* template
* Uses *State Machine* layout for the phases of automation project
* Offers high level logging, exception handling and recovery
* Keeps external settings in *Config.json* file and Orchestrator assets
* Pulls credentials from Orchestrator assets and *Windows Credential Manager*
* Gets transaction data from Orchestrator queue and updates back status
* Takes screenshots in case of system exceptions


### How It Works ###

1. **INITIALIZE PROCESS**
 + ./Framework/*InitiAllSettings* - Load configuration data from Config.json file and from assets
 + ./Framework/*GetAppCredential* - Retrieve credentials from Orchestrator assets or local Windows Credential Manager
 + ./Framework/*InitiAllApplications* - Open and login to applications used throughout the process

2. **GET TRANSACTION DATA**
 + Dispatcher mode: transaction data is prepared and queued inside *Process* workflow (no *GetTransactionData* step)

3. **PROCESS TRANSACTION**
 + *Process* - Process trasaction and invoke other workflows related to the process being automated 
 + ./Framework/*SetTransactionStatus* - Updates the status of the processed transaction (Orchestrator transactions by default): Success, Business Rule Exception or System Exception

4. **END PROCESS**
 + ./Framework/*CloseAllApplications* - Logs out and closes applications used throughout the process


### For New Project ###

1. Check the Config.json file and add/customize any required fields and values
2. Implement InitiAllApplications.xaml and CloseAllApplicatoins.xaml workflows, linking them in the Config.json fields
3. Implement Process.xaml as dispatcher logic (source read + enqueue) and keep SetTransactionStatus only if needed for performer scenarios
4. Implement Process.xaml workflow and invoke other workflows related to the process being automated

### AI Provider Configuration

The project supports a provider-agnostic AI configuration under the `AI` section in `Data/Config.json`.

- Set `AI.Enabled` to `true` only when AI integration is needed.
- Set `AI.Provider` to one of the configured providers (for example: `openai`, `anthropic`, `azureOpenAI`).
- Keep provider-specific details in `AI.Providers.<providerName>`.
- Store secrets in Orchestrator Assets and reference them via `APIKeyAsset`.
- If `AI` is missing or invalid, the runtime falls back to `AIEnabled=false` and `AIProvider=none`.

### Runtime Mode

Use `Settings.RuntimeMode` in `Data/Config.json` to describe intended development/runtime style:

- `framework` (default): standard XAML REFramework-style dispatcher runtime.
- `coded`: indicates coded-robot oriented development mode while keeping the same workflow runtime path.

If the value is missing or invalid, the process automatically falls back to `framework`.

You can also override environment classification at run start using input argument `in_EnvironmentType` (for example `DEV`, `TEST`, `UAT`, `PROD`), which updates `logF_Environment` for that run.
