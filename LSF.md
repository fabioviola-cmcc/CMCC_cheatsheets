# LSF

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
  
- Set the walltime (hh:mm)
  ```
  #BSUB -W <hh:mm>
  ```

- Impose dependencies
  ```
  #BSUB -w "<condition>(<JOBID>)"
  ```
  An example condition is *ended*. See the documentation for more.

## Checking/Moving/Killing a job

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

## History

## Service classes

## References
