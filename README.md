# Project HA_GTMAutomate
## Overview
The program will perform below tasks
1. Read GTM Excel and Output to several Ansible Variables YML file.
   Input: An Excel file path in 1st parameters
   Output: YML Files under Output Folder, default `REL_PATH_OUTPUT_DIR = './output'`
2. Commit the Output YML Files for related `<tsr_no>` to Github
## Python Version
`3.12`
## Tested Python Module Version
```
Package            Version
------------------ ------------
numpy              1.26.2
openpyxl           3.1.2
pandas             2.1.4
pip                23.3.2
PyGithub           2.1.1
PyJWT              2.8.0
python-dateutil    2.8.2
PyYAML             6.0.1
requests           2.31.0
ruamel.yaml        0.18.5
ruamel.yaml.clib   0.2.8
six                1.16.0
urllib3            2.1.0
```

## Quick Start
1. Clone the project .zip file and extract to a folder, `{project_home}`
2. Install python version `3.12` and Install the python module
```
pip install openpyxl
pip install pandas
pip install pyyaml
pip install PyGithub
pip install ruamel.yaml
```
3. Select or Setup and init a Github repository, `{GITHUB_REPOSITORY_NAME}` and create a Branch, `{GITHUB_BRANCH_NAME}`
4. Generate a Github token, `{GITHUB_TOKEN}`
5. Revise paramters in `config/GTMConfig.py`, replace the {GITHUB_REPOSITORY_NAME}, {GITHUB_TOKEN}
```
REL_PATH_ServiceDCMapping = 'config/Service_DC_Mapping.csv'
REL_PATH_CRPRDDomainHospitalMapping = 'config/Cluster_Cloud_Domain_Hospital_Mapping.csv'
REL_PATH_OUTPUT_DIR = './output'
REL_PATH_OUTPUT_AUDIT_FILE_PREFIX = 'GTM_Audit'
...
GITHUB_REPOSITORY_NAME = '{RepositoryName}'
GITHUB_TOKEN = '{GITHUB_TOKEN}'
GITHUB_BASE_URL = 'https://hagithub.home/api/v3'
GITHUB_BRANCH_NAME = 'main' #{GITHUB_BRANCH_NAME}
...
```
6. Execute Script
```
cd {project_home}
python ./GTMTaskAutomate.py gtm-template_nonprd.xlsx
```
Sample Result
```
> python .\GTMTaskAutomate.py .\240214_ansible_nonprd.xlsx
2024-02-21 17:25:39 - root - INFO - Processing file: F:\My Documents\HA\F5Ansible\Dev\HA_GTMAutomate_xw\.\240214_ansible_nonprd.xlsx
2024-02-21 17:25:40 - root - INFO - No Duplicate FQDN
2024-02-21 17:25:40 - root - INFO - Completed Manupulate Fields for record 0
2024-02-21 17:25:40 - root - INFO - Completed Manupulate Fields for record 1
...
2024-02-21 17:25:40 - root - INFO - Completed Manupulate Fields for record 22
2024-02-21 17:25:40 - root - INFO - Preparing YML Template ...
2024-02-21 17:25:40 - root - INFO - Generating yml files for record 0 ...
2024-02-21 17:25:40 - root - INFO - Generating yml files for record 1 ...
...
2024-02-21 17:25:41 - root - INFO - Generating yml files for record 21 ...
2024-02-21 17:25:41 - root - INFO - Generating yml files for record 22 ...
2024-02-21 17:25:41 - root - INFO - Start Commit files ... ['./output/tst/RID-240215_webservice_1/webservice+t7-automation-test-ga-sit.tstcld1.ha.org.hk.yml', 
...
'./output/tst/RID-240215_dmzinternet/dmzinternet+t7-automation-dmz-dev.svc.hadev.org.hk.yml']
...
2024-02-21 17:26:20 - root - INFO - Commit created successfully! id: 19d006e296157c8cffac70f9553b9996adc82676
2024-02-21 17:26:20 - root - INFO - Start Commit files ... ['F:\\My Documents\\HA\\F5Ansible\\Dev\\HA_GTMAutomate_xw\\GTM_Audit_nonprd.csv']
2024-02-21 17:26:27 - root - INFO - Commit created successfully! id: e12ac5f3cb88f471e88e7320419f1e27378dda94
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
    └── <env>                               # List of Folders, naming with <Env>, <tst|prd>
        └── <tsr_no>                        # List of Folders, naming with <tsr_no>, e.g. 'RID-00001', 'RID-00002'
            └── <service_type>+<wideip>.yml # Output YML file, with pattern <service_type>+<wideip>.yml, e.g. 'nonwebservice+aiops-chatops-redis-aiops-corp-chatops-sit.tstcld1.ha.org.hk.yml'
├── service                                 # Service Folder for Service module storing Ansible Variable Patterns
├── template                                # Template Folder for YML Template of Ansible Variable
├── util                                    # Folder for Python module, util
├── gtm-template_nonprd.xlsx                # Template File, The GTM Excel File provided by User
├── <anyname>_<tst|prd>.xlsx                # Import Excel, with naming including env <tst|prd>
├── GTMTaskAutomate.py                      # File, Main Python File to start the program
├── app.log                                 # Log File
├── GTM_Audit_nonprd.csv                    # Audit CSV for GTM non-production
├── GTM_Audit_prd.csv                       # Audit CSV for GTM production
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
./output/tst/RID_00001/nonwebservice+aiops-chatops-redis-aiops-corp-chatops-sit.tstcld1.ha.org.hk.yml
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
- DMZ to Intranet
- DMZ cloud Internet

