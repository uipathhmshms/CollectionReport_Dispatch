# Collection Report Dispatch

## Project Overview
This UiPath automation project processes payroll data from a company's payroll system and distributes reports to managers based on their unique identifiers. <br>
The automation segregates employee data by manager and creates individual reports that are then queued for processing.

### Quick Overview
![Sample Data Structure](/Documentation/dataFlow1.png)
![Sample Data Structure](/Documentation/dataFlow2.png)
*Quick overview of the process steps*

## Main Workflow Components

### 1. Data Collection and Processing
- Extracts payroll data from the company's database into a tabular data structure
- Sorts the data by manager ID and removes irrelevant columns
- Creates individual data tables for each manager containing their relevant data

### 2. Queue Management
The workflow processes data in batches by manager and creates queue items with the following properties:
- ReportData: JSON serialized data containing manager-specific information
- ReportRowsNo: Number of rows in the manager's report
- EmailAddress: Manager's email address (currently set to a test address)
- Reference: Unique identifier combining manager name, ID, and date

### 3. Main Process Flow
1. Initializes output data table for queue items
2. Sets initial manager ID and position from first row
3. Iterates through data rows:
   - Detects manager changes in the data
   - Creates temporary data tables for current manager
   - Converts data to dictionary format
   - Adds items to the queue with proper references
   - Updates tracking variables for next iteration

## Technical Details

### Input Arguments
- `in_dt_data`: DataTable
  - Contains raw payroll data
  - Must include columns:
    - מזהה מנהל פרויקט (Manager ID)
    - שם מנהל פרויקט (Manager Name)
    - Additional employee data columns

### Output Arguments
- `io_dt_dataForBulkAddQueueITems`: DataTable
  - Contains processed queue items
  - Columns:
    - ReportData (string, max length: 1000000)
    - ReportRowsNo (integer)
    - EmailAddress (string, max length: 100)
    - Reference (string, max length: 80)
    - Priority (string, max length: 20, default: "Low")

### Queue Configuration
- Queue Path: Root/Finance/CollectionReport
- Queue Type: Test
- Priority: Low
- Reference Format: `{ManagerName}_{ManagerID}_{CurrentDate}`

## Dependencies
- System.Data
- Newtonsoft.Json
- UiPath.Excel.Activities
- UiPath.System.Activities

## Setup Instructions
1. Ensure all required dependencies are installed
2. Configure database connection settings
3. Verify queue paths and permissions

## Notes
- Current implementation uses a test email address (LidorM@hms.co.il)
- TODO: Add manager email functionality (marked in workflow)
- The process handles data batching automatically based on manager changes
- All dates are formatted as dd/MM/yyyy
