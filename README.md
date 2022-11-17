# This repo allows to reproduce environment build issue in Azure ML Studio
Steps to reproduce:
1. Make sure that you have `az ml` CLI installed. My version is:
<details>
  <summary>az --version</summary>

```bash
azure-cli                         2.42.0

core                              2.42.0
telemetry                          1.0.8

Extensions:
storage-preview                    0.8.3
ml                                2.11.0

Dependencies:
msal                              1.20.0
azure-mgmt-resource             21.1.0b1

Python location '/usr/local/Cellar/azure-cli/2.42.0/libexec/bin/python'
Extensions directory '/Users/marcin/.azure/cliextensions'

Python (Darwin) 3.10.8 (main, Oct 13 2022, 10:17:43) [Clang 14.0.0 (clang-1400.0.29.102)]

Legal docs and information: aka.ms/AzureCliLegal


Your CLI is up-to-date.
```
</details>


2. Run
```bash
az ml environment create --name my-environment -w <workspace-name> -g <resource-group> --build-context . -d Dockerfile
```

3. In Azure ML Studio, the build will not event start, no `Build Logs` will be visible:
```
Environment image build status
❌ Failed
```
**Expected output**: enviroinment builds without problems.


4. Change the `.amlignore` to *exclude* the data folder:
```
!Dockerfile
data
```

5. Run the build again. Now the build will proceed and finish successfuly, which means that something in the data folder breaks the build context in the Azure ML Studio, but those are just plain CSV / XLSX files. I've tested the process and including ANY of them, break the platform with no logs.

----

Additional notes
The example code comes from `kedro new --starter=spaceflights`.
