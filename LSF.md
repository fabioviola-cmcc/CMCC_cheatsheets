# LSF

- [Submitting a job](#submitting-a-job)
- [Most common BSUB directives](#most-common-bsub-directives)
- [Checking/Moving/Killing a job](#checking-moving-or-killing-a-job)
- [Queues](#queues)
- [History](#history)
- [Service classes](#service-classes)
- [Status of the cluster](#status-of-the-cluster)
- [References](#references)


## Submitting a job

1. Create a script including the BSUB directives (see below)
2. Invoke the script with:
   ```
   $ bsub < script.sh
   ```


## Most common BSUB Directives

- Set the name of a job
  ```
  #BSUB -J <NAME>
  ```

- Set the file for standard output log
  ```
  #BSUB -o <file.out>
  ```

- Set the file for standard error log
  ```
  #BSUB -e <file.err>
  ```
  
- Select the queue
  ```
  #BSUB -q <queue>
  ```
  To check the available queues use `bqueues -u <user>`

- Request exclusive access to nodes
  ```
  #BSUB -x
  ```

- Set the project ID (for accounting)
  ```
  #BSUB -P <projectID>
  ```
  To see the existing project codes, use the command `bprojects`.

- Set the total number of cores (total across the nodes)
  ```
  #BSUB -n <number>
  ```
  
- Run all processes on the same host
  ```
  #BSUB -R "span[hosts=1]"
  ```
  Combine this option with `-n` to specify the number of cores to request.
  
- Set the number of processors on each host
  ```
  #BSUB -R "span[ptile=<cores_per_host>]"
  ```
  Combine this option with `-n` to define the full configuration
  
- Set the walltime, i.e. the runtime limit of the job (hh:mm):
  ```
  #BSUB -W <hh:mm>
  ```
  The queues already have a walltime defined. To see the walltime for a queue, use: `bqueues -l <queue_name>`.
  

- Impose dependencies
  ```
  #BSUB -w "<condition>(<JOBID>)"
  ```
  An example condition is *done*, to request that the job start after successful completion of the specified job. The main conditions are `done`, `ended`, `exit`, `started`. Boolean operators are supported: ``&&`` (and), `||` (or), `!` (not). See the [documentation](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=o-w-2) for more.

## Checking, moving or killing a job

- Verify the status of a job:
  ```
  $ bjobs
  ```
  Instead of torturing your shell to compulsively check the status of a job, I would suggest using watch:
  ```
  $ watch -n 1 bjobs
  ```

- Killing a job
  ```
  $ bkill <JOBID>
  ```

- Moving a job to another queue
  ```
  $ bswitch <new_queue> <JOBID>
  ```

- Suspend/Resume a job
  ```
  $ bstop <JOBID>
  $ bresume <JOBID>
  ```

## Queues

- To get an overview of the status of the queues:
  ```
  $ bqueues
  ```
  
- To restrict the list to the queues where a user is allowed to submit:
  ```
  $ bqueues -u <user_name>
  ```

- Details on a queue can be read with:
  ```
  $ bqueues -l <queue_name>
  ```

## History

- To see the list of all* jobs finished, type:
  ```
  $ bjobs -a
  ```
  The term _all_ refers to all the jobs finished in the last `CLEAN_PERIOD` seconds.

- To see details on a job:
  ```
  $ bjobs -l <JOB_ID>
  ```
  
## Service classes

- To see the list of available service classes:
  ```
  $ bsla
  ```

- To see details of a service class:
  ```
  $ bsla <SC_NAME>
  ```

- To use a service class, add the following directive:
  ```
  #BSUB -app <SC_NAME>
  ```
  NOTE: `-sla` is the parameter to use a service class, while `-app` is the parameter to enable a specific application profile. Using `-app` implies using also `-sla`.

## Status of the cluster

- Check the status of the queues:
  ```
  $ bqueues
  ```
  
- Check the status of the nodes:
  ```
  $ bhosts
  ```

- Check the load of the nodes:
  ```
  $ lsload
  ```
  
- Connect to a node to check its status:
  ```
  $ bsub -Is -q s_short -P R000 /bin/bash
  ```

## References

- [**bhosts manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bhosts)
- [**bkill manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bkill)
- [**bqueues manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bqueues)
- [**bresume manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bresume)
- [**bstop manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bstop)
- [**bsub manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bsub)
- [**bswitch manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-bswitch)
- [**lsload manual**](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=reference-lsload)

