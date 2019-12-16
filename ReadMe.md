# Dependency Analysis Tool

## Copyright

Copyright (c) 2017, Microsoft Corporation

SPDX-License-Identifier: BSD-2-Clause-Patent

## About

This tool parses a UEFI Edk2 code tree and generates a HTML report that allows a developer to evaluate code dependencies. The user can review each dependency using a fully searchable and filterable table and/or generate "network" graphs of the dependencies. Dependencies can be grouped by module, package, and type to allow complete review. On any graph the nodes can be moved around to produce a better
visualization. Nodes can also be removed by right-clicking the node. This will remove any node and its edges.

### DependencyAnalysisTool.py

This is the parsing tool where all the UEFI code is inspected. This python code relies on the MS Core UEFI Python library for file parsing and path management. The tool will collapse all UEFI data into JSON and will then insert it into a HTML file based on the template.

### Dep_template.html

This is a single HTML file that allows the user to interact with the parsed data. This template will be read by the python code and parsed data will be inserted as JSON into the predetermined spot. The whole contents will then be written to the output report as specified. This template file follows the MS Core UEFI HTML template design pattern and contains common tabs such as basic info, errors, and about. The About tab contains attribution to all other Javascript and CSS libraries used to make the report. It also contains custom tabs for this report.

## Prerequisite

1. Python: Tool has been validated on Python 3.7.x
2. Pip Dependencies:

   * It is suggested to create a clean python virtual environment to run the tool in.
   * `pip install -r requirements.txt` file at the root of the repo

## Command line options

| command line Syntax | What it is | Required | Extra Help
| --- | --- | --- | --- |
| -h, --help | Show help message | NO | |
| --Workspace *path* | absolute path to Edk2 workspace | YES | Should be the root workspace
| --OutputReport *reportfile* | Path to output report html | YES ||
| --InputDir *dir*| Folder to start parsing INFs | NO | Default will be same as workspace|
| --BuildReport *path* | Path to edk2 build report | NO | If not set the "as built" section of the report will be empty
|--CodebaseName *name* | Meaningful name for user.| NO | Will show up in report
|--CodebaseVersion *ver* | Meaningful version for user| NO | Will show up in report
|--PackagesPathCsvString *string* | a CSV string containing any Packages Path arguments used for the Edk2 build | NO|if your build uses package path you should set this the same so parsing is correct
|--IgnoreFolderCsvString *string* | a CSV string containing any folders you don't want parsed | NO | For example Build Output, version control folders, or directories you just want to ignore
|--debug | Turn on debug logging level for the file logger | NO ||
|-l *outputlogfile* | Create an output log for the parsing tool | NO ||

## Example Usage

### Run on edk2 workspace

1. Assume git repo at c:\src\edk2
1. Assume user doesn't want to parse some of the code that is less commonly used.
1. No as built info.  If you wanted that you would need to turn on the edk2 build report and then add the --BuildReport *path* to the tool

``` cmd

DependencyAnalysisTool.py --Workspace c:\src\edk2 --OutputReport Edk2_core.html --CodebaseName "TianoCore Edk2 Master" --CodebaseVersion bfb141cf19dd6f9b8df8b9d0914a5b3b15e1a798 -l Edk2DepTool.log --debug
 ```

### Run on multi-repo platform tree

 Same as above except use the `--PackagesPathCsvString` to add your packages path
