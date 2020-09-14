# Manage multiple projects

## Resources

[YouTube video](https://www.youtube.com/watch?v=cY2NXB_Tqq0&t=10s)


## Manage virtual environments 


1. Create a new project

``` mkdir <project_name>```


2. Create a conda virtual environment

```conda create --name test_project_env flask sqlalchemy numpy pandas```

3. Activate the environment

```conda activate test_project_env```

4. List the dependencies

```pip list```

5. Export the dependencies in a YAML file

```conda env export > environment.yaml```

6. Make a replicate environment from the created YAML file

```conda env create --name replicate_env -f environment.yaml```

## Manage environment variables

How to separate the environment variables for different projects. It is possible with **Anaconda**.

1. List conda environments

```conda env list```

2. Change location to the virtual environment

```cd <virtual_env_path>```

3. Create two directory trees to set environment variables

    - Create an activate directory:

    ``` mkdir -p etc/conda/activate.d```

    - Create an deactivate directory:

    ``` mkdir -p etc/conda/deactivate.d```

4. Create a bash files in the two directories

    - In the activate directory

    ``` touch etc/conda/activate.d/env_vars.sh ```

    - In the deactivate directory

    ``` touch etc/conda/deactivate.d/env_vars.sh ```

5. Edit your bash files

    - Create an environnement variable in the `activate.d/env_vars.sh` bash file

    ``` export <env_var_name> = <env_var_value>```

    - Remove this environnement variable in the `deactivate.d/env_vars.sh` bash file

    ``` unset <env_var_name>```

6. Test the result

    - Get back to the project directory

    ```cd <project_dir>```

    - Activate the virtual environment

    ```conda activate test_project_env```

    - Echo the created environment variable, it should display its value in the terminal

    ```echo $<env_var_name>```

    - Leave the virtual environment

    ```conda deactivate```

    - Try to echo the environment variable, nothing should be displayed


## Automate the process

When working with different projects, one may forget to deactivate one project environment before working on another one.

The solution proposed here is to have something in place that checks when we navigate to our projects. If the project has an `environment.yaml` file and activate this virtual environment if it exists.

### How it works

It runs a bash function that checks for the virtual environment and automatically activate it if it finds one.

Then, we need to set an environment variable called `PROMPT_COMMAND` that will run this custom function before each bash prompt.

1. Navigate in Home Directory to to where the "global" environment variables are set

``` code ~/.bash_profile```

2. Copy paste the `conda_auto_env` function in this file. You can find this function [here](https://github.com/CoreyMSchafer/code_snippets/blob/master/conda_auto_env.sh)  and save the results

3. Test the process, the conda environment should be activated automatically

```cd <project_name>```


## Manage environment variables more generally

To activate environment variables from a `.env` file present automatically when navigating to a folder where this `.env` file is present, follow these steps

1. Navigate in Home Directory to to where the "global" environment variables are set

``` code ~/.bash_profile```

2. Copy paste the following function and save

```
# Export environment variable from .env
function set_env_vars() {
  if [ -e ".env" ]; then
    set -a
    source .env
    set +a
  fi
}

```

3. Restart the terminal

4. Test the process:
    - Create a new `.env` file in a folder

    ```touch <folder_name>/.env```
    - In the ```.env``` file, initialize an environment variable

    ```<env_var_name>=<env_var_value>```
    - Echo it the terminal, and it should be displayed
    
    ```echo $<env_var_name>```







    



