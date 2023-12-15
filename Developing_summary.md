# Developing Summary
## Issue 1
**Description**: On Windows, when activating a Python virtual env, on some machine there is a permission issue and the error message is "xxx is not digitally signed. You cannot run this script on the current system. For more information about running scripts and setting execution policy, see about_Execution_Policies at xxx".

**Solution**: Run "**Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass**" in the terminal, then activate.

## Issue 2
**Description**: When using Jupyter notebook in e.g. VS code, JupyterLab, you want to load a kernel but the kernel resides in a different location and cannot be found in the list when you "Select Kernel".

**Solution**: Open a terminal, activate the venv desired, run the command "**python -m ipykernel install --user --name \<venv name\> --display-name \<any name you want\>**", then restart the IDE and the kernel should appear in the list. More detailed steps can be found [here](https://srinivas1996kumar.medium.com/adding-custom-kernels-to-a-jupyter-notebook-in-visual-studio-53e4d595208c)

## Miscellaneous

**Set the cache dir for Pytorch hub**: Usually happen for cluster users, Pytorch models are stored at "/home/\<usr_name\>/.cache/torch/hub/checkpoints" and it will quickly exceed the disk quota. To fix this issue, one may set the default cache dir by adding the following code.

```python
import torch
torch.hub.set_dir('/work/<any dir with ample space>/<better to use full path>')
```

**Import a custom python file that is not in current folder**: You write some awesome functions and wrap them in a python file and put it in at location A. You are doing a seperate project at location B and you decide to import that awesome python file but could not. Here is the way to fix this issue.

```python
import sys
sys.path.append('/path/to/location/A')
import the_awesome_python_file

# able to run this then
the_awesome_python_file.awesome_function1()
```

**Check cuBLAS version**: (Debian) Open a terminal, run "**cat /usr/local/cuda/include/cublas_api.h | grep CUBLAS_VER**". More details can be found [here](https://stackoverflow.com/questions/52337791/verify-that-cublas-is-installed)

**Flush GPU memory**: Other implementations can be found [here](https://huggingface.co/blog/optimize-llm). Below a simpliest version is provided.
```python 
import cuda, gc

# Your code

torch.cuda.empty_cache()
gc.collect()

# GPU memory is reset
```

**Timing GPU computation time**: E.g. I want to record the wall-clock time of matrix multiplication on a GPU. I will run the multiplication for multiple times and run a flush to clear the GPU memory (maybe because GPU cannot fit multiple matrices), and calculate the average wall-clock time as the result. If you implement this experiment using a simple for loop, you will find some run consumes a lot more time than the others, and wall-clock times are not stable. This is usually caused by GPU trying to do the flushing and loading the new data simultaneously. A simple fix is do the flush, then wait a few second and load the matrices to do the multiplication. Here is a simple code example.

```python
from time import time, sleep

def wall_clock_timer(all_trys):
    wall_clock_times = []
    for each_try in all_trys:
        # flush GPU memory
        torch.cuda.empty_cache()
        gc.collect()
        # wait for 2 seconds
        sleep(2)

        t1 = time()

        # run matrix multiplication

        t_consumed = (time() - t1)*1000
        wall_clock_times.append(t_consumed)
        
    return sum(wall_clock_times)/len(wall_clock_times)
```

**Enable/Disable tqdm progress bar**: for i in tqdm(range(10), disable=False/True)

**TF OD model zoo**: [link](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md)

**TF OD tutorials**: [link1](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2.md), [link2](https://neptune.ai/blog/how-to-train-your-own-object-detector-using-tensorflow-object-detection-api)

