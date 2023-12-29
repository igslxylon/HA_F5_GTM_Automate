# Project HA_LTMGTMAutomate
## Overview
The program will perform below tasks
1. Read GTM Excel, defualt `ltm-gtm-template.xlsx` and Output to several Ansible Variables YML file.
   Input: Excel File, default `REL_PATH_INPUT_Excel_File = ltm-gtm-template.xlsx`
   Output: YML Files under Output Folder, default `REL_PATH_OUTPUT_DIR = './output'`
2. Commit the Output YML Files for related `<tsr_no>` to Github
## Quick Start
1. Clone the project .zip file and extract to a folder, `{project_home}`
2. Install the python module
```
pip install pandas
pip install pyyaml
pip install PyGithub
pip install ruamel.yaml
```
3. Select or Setup and init a Github repository, `{RepositoryName}`
4. Generate a Github token, `{Token}`
5. Revise paramters in `config/GTMConfig.py`, replace the {RepositoryName}, {Token}
```
REL_PATH_ServiceDCMapping = 'config/Service_DC_Mapping.csv'
REL_PATH_INPUT_Excel_File = 'ltm-gtm-template.xlsx'
REL_PATH_OUTPUT_DIR = './output'
GITHUB_REPOSITORY_NAME = '{RepositoryName}'
GITHUB_TOKEN = '{Token}'
...
GTM_DEVICE_LIST_PRD = ['dc6-cloud-gtm01','dc7-cloud-gtm01','dc8-cloud-gtm01']
GTM_DEVICE_LIST_TST = ['hkch-cloudnp-gtm01','dc7-cloudnp-gtm01']
...
```
6. Execute Script
```
cd {project_home}
python ./GTMTaskAutomate.py
```
Sample Result
```
> python .\GTMTaskAutomate.py
2023-12-22 16:46:31 - root - INFO - No Duplicates FQDN
2023-12-22 16:46:31 - root - INFO - Completed Manupulate Fields for record 0
...
2023-12-22 16:46:31 - root - INFO - Completed Manupulate Fields for record 17
2023-12-22 16:46:31 - root - INFO - Preparing YML Template ...
2023-12-22 16:46:31 - root - INFO - Generating yml files for record 0 ...
...
2023-12-22 16:46:33 - root - INFO - Generating yml files for record 17 ...
2023-12-22 16:46:33 - root - INFO - Start Commit the files ...
2023-12-22 16:47:32 - root - INFO - Commit created successfully! id: bce0d7422287a2959d88b8fa612ce1af3562a238
```
## Folder Structure
```
.
├── config                                  # Config Folder
    ├── GTMConfig.py                        # File, Config Variables stored for Input, Output and Github parameters
    ├── logging.conf                        # File, Logging Conf File
    ├── Service_DC_Mapping.csv              # File, CSV storing the Serivce, Server Info and Data Center Mapping
    └── ...         
├── output                                  # Destination Folder for Output Ansible variables yml files
    └── <tsr_no>                            # List of Folders, naming with <tsr_no>, e.g. 'RID-00001', 'RID-00002'
        └── <device_hostname>               # List of Folders, naming with <device_hostname>, which depends on the Environment, either 'tst' or 'prd'
                                            #  e.g. 'dc6-cloud-gtm01','dc7-cloud-gtm01','dc8-cloud-gtm01'
            └── <service_type>+<wideip>.yml # Output YML file, with pattern <service_type>+<wideip>.yml, e.g. 'nonwebservice+aiops-chatops-redis-aiops-corp-chatops-sit.tstcld1.ha.org.hk.yml'
├── service                                 # Service Folder for Service module storing Ansible Variable Patterns
├── template                                # Template Folder for YML Template of Ansible Variable
├── util                                    # Folder for Python module, util
├── ltm-gtm-template.xlsx                   # File, The GTM Excel File provided by User
├── GTMTaskAutomate.py                      # File, Main Python File to start the program
├── app.log                                 # Log File
├── GTM_Audit.log                           # Audit Log
└── README.md
```

## Output File
Output File is default set to under `./output`. Could set `REL_PATH_OUTPUT_DIR`
### Example
For a GTM record with below variables
```
tsr_no = 'RID_00001'
service_type = 'nonwebservice'
wideip = 'aiops-chatops-redis-aiops-corp-chatops-sit.tstcld1.ha.org.hk'
```
The Output Files with default are
```
./output/RID_00001/hkch-cloudnp-gtm01/nonwebservice+aiops-chatops-redis-aiops-corp-chatops-sit.tstcld1.ha.org.hk.yml
./output/RID_00001/dc7-cloudnp-gtm01/nonwebservice+aiops-chatops-redis-aiops-corp-chatops-sit.tstcld1.ha.org.hk.yml
```


## GTM Excel File
To be input by User

Accepted Values in `Service Type` in the Excel
- Web service
- Non-web service
- HA remote
- Internet security gateway
- MLLP
- .home
- Cluster resilience * Only PRD
- DMZ cloud

