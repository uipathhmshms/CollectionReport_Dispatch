### HmsTeam.DispatchQueueItem.s Template ###
**DispatchQueueItem.s**

* Built on top of *hms GeneralProcess Template* template
* Uses *State Machine* layout for the phases of automation project
* Offers high level logging, exception handling and recovery
* Keeps external settings in Orchestrator assets
* Pulls credentials from Orchestrator assets
* Gets records data from custom source like DB, business app, etc.
* Creats queue items in Orchestrator queue
* Takes screenshots in case of system exceptions


### How It Works ###

1. **BEGINNINGACTIONS PROCESS**
 + *BeginningActions* - Filling ProcessSetup variable with Assets, Job details, Environment properties etc.
 + *CheckForDownTime* - Checking if process may now run or not, depends on it's downtimes and it's business apps downtimes.

2. **BUILD DATATABLE**
 + ./Framework/*BuildDataTable* - Custom actions for creating the Datatable of the records that will be dispatched into Orchestartor queue.

3. **BULK ADD QUEUE ITEMS**
 + *BulkAddQueueItems* - Using Bulk add queue items activity for dispatching the items. Erros is exist, will be reflected in the following variables: boo_BulkHadErrors, boo_AllErrorsUniqueRef, dt_QueueItemsFailed

4. **BEFORE END PROCESS**
 + ./Framework/*BeforeEndProcess* - Performing actions that must run only once, at the end of the process, like sending email, report, etc.


### For New Project ###

1. Ensure existness of the Orchestrator required folders of Root\BusinessDepartment, Root\BusinessDepartment\General and Root\BusinessDepartment\BusinessProcess
2. In case you are using action center, ensure existness of a catalog in the relevant Orchestrator folder
3. Fill the queue name in argument in_str_QueueName
4. Implement ExecuteTransaction.xaml workflow and invoke other workflows related to the process being automated

## UseCases  ##
* Robot runs a DB query, populates the result in a Datatable and sent the rows to Orchestrator queue.