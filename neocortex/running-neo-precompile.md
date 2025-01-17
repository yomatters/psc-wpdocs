
     
### Neocortex
This section provides a brief description about how you can run and monitor your jobs on the Neocortex system. Sample commands are as follows.

First, you need to login to the system via ssh. Use your PSC credentials.
```
ssh PSC_USERNAME@neocortex.psc.edu
```

#### How to pre-compile your model
You can pre-compile your model on the CPU while CS-2 is busy with another job. Navigate to your model directory and follow these instructions:
* Reserve CPU node and run Cerebras singularity container in interactive mode.
     ```
      srun --pty --cpus-per-task=28 --kill-on-bad-exit singularity shell --cleanenv --bind /local1/cerebras/data,/local2/cerebras/data,/local3/cerebras/data,/local4/cerebras/data,$PROJECT 
     /ocean/neocortex/cerebras/cbcore_latest.sif
    ```
    To avoid typing this long command every time on the terminal, you can save it in a file, such as `salloc_node` and run that. This commands starts the shell with:<br />
    &bullet; 1 sdf node: 28 cores, 2 threads per core<br />
    &bullet;  --bind is for binding the folders so that they are accessible from inside the singularity shell.<br />
    &bullet; The .sif container here is the symlink to the latest version of the container provided by the Cerebras team. Please use `ll` for more details about this container.

* From inside the singularity shell, for validation only mode:
    ```
    python run.py --mode train --validate_only --model_dir validate
    ```
    where <br />
      &bullet; -o is the output directory<br />
      &bullet; --mode is the mode i.e., `compile_only`, `validate_only`, `train`, `eval`

* From inside the singularity shell, for compile only mode:
    ```
     python run.py --mode train --compile_only --model_dir compile
    ```

    **Note:** You can also start an interactive session on the SDF nodes with the following command:
    ```
    interact
    ```

    `interact` will start an interactive job on one of the available SDF nodes (sdf-1 or sdf-2). You can also specify which node to log into with the `srun` command and `--nodelist` parameter:
    ```
    srun --nodelist=sdf-1 --pty bash -i
    ```
