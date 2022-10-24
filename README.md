# intervene-job-manager
Help tools to use CSC SD Connect service to manage INTERVENE jobs. 

This site contais two software components that interact with the central data repository hosted by INTERVENE Data Coordination Center.  The software components are:

   * iv-request.py (INTERVENE requests). A tool that a researcher can use to upload an analysis request to the SD-Connect to be processed
   * iv-analyst.py (INTERVENE analyst). A tool that is used by the biobank analysts to download the requests for processing and to upload the results to the SD-Connect so that the original requester can retrieve the results. 

## Requirements

iv-request.py and iv-analyst are command line python tools.
They can be used by any computer that has Python3 installed with the following python libraries: 
json, datetime, time, re, os, swiftclient and getpass.

## 1. Task submission and result retrieval by INTERVENE researcher using iv-request.py

When iv-request is started, it asks for the CSC username and password to authenticate the user. This means that the user must have a CSC user account, which can be provided to all INTERVENE analysts. 
After that  the user can choose one of the four operations:
   * Listing submitted requests
   * Submitting a new analysis request
   * Downloading a processed request
   * Deleting a request

The analysis request submission is the main functionality of this tool. During submission, user must define following items:
   * task name
   * requestor e-mail
   * biobanks included in the analysis
   * analysis type
   * input files 
   * output file
At the moment iv-request includes only two pre-defined analysis types: test tasks for prspipe and pgsc_calc pipelines. If a user wants to submit some other type of task, she must provide a singularity container that contains the software tools needed to run the task and instructions how to perform the analysis.

Based on the information given, iv-request constructs a job description file that is uploaded to the DCC data repository together with the input files.  

The researcher can have several jobs submitted for processing. The researcher can use the task listing functionality of iv-request.py to check the status of all her tasks in all biobanks (waiting, processing, or ready). When all the requested biobanks have uploaded the results to the central repository, the researcher uses the download function to retrieve the results to her local computer. The result files will be stored into task and biobank specific folders for further analysis.



## 2. Task processing by biobank analyst using iv-analyst.py

Biobank analysts use the iv-analyst.py tool in their local environments to interact with the tasks loaded to DCC central job repository. When the script is started it asks for the CSC username and password to authenticate to the DCC repository. After that the analyst selects her biobank and the task to be executed. The tasks are:
   * biobank specific task list including status of each task
   * display job description file of a task
   * downloading a task to start the analyses
   * upload the results of the task to the data repository
   
When a biobank analyst selects to start to process the analysis, the interface downloads the pipeline container together with other input data needed. 
It also downloads an instruction document. Typically this is a text document that describes what biobank data that needs to be collected for the analysis and the commands that need to be executed, but in principle it could be a command script too.

When the job ready the iv-analyst.pl tool can be used to upload the results to the central data repository.
