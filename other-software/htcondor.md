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





### Some differences between CERN and CNAF HTCondor jobs

HTCondor for CERN lxplus are submitted without should\_transfer\_file (they have a  shared filesytem?), and there is only one log file for the whole cluster (log/neutrino.$(ClusterId).log). The jobs are removed automatically from condor after completion

Instead, HTCondor jobs at CNAF are launched with should\_transfer\_file activated. It means the jobs are not removed automatically after completion, and I need to launch `condor_transfer_data -all` and then `condor_rm jobnumber`afterwards. If the log name is the same, it is overwritten and not appended. We need a separate log file for each job (still, it may be a good idea to append them together afterwards): log/corsika.$(ClusterId).$(ProcId).log

