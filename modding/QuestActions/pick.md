# pick

> Selecting tasks randomly

```
pick one of aTaskName1 aTaskName2 aTaskName3 aTaskName4
```

This action randomly selects one of the tasks for activation. The choice is evenly selected over the tasks, but you can skew the selection by repeating the same task.


## Examples

### Equal odds

```
pick one of task1 task2 task3
```

Gives the PC equal odds of getting _task1_, _task2_ or _task3_.


### Weighted odds

```
pick one of task1 task2 task2 task2
```

Weights the odds 75% in favor of _task2_.
