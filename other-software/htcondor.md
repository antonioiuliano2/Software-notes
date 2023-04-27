# HTCondor

Always remember to create 3 folders, named **error**, **log** and **output** before launching the job



Check if a job is in hold for which reason with

condor\_q -hold 16.0&#x20;

(replace 16.0 with number of your job)

condor\_q -analyze

Useful to check why a job is in idle for so long...

condor\_q -l 16.0

Get attribute details of the job

condor\_tail 16.0

Check std\_output of the job (seems only from latest command in the .sh file)
