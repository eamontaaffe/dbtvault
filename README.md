# dbtvault Developer Quick Setup

## Prerequisites

- Python 3.9.6
- Pipenv

## Install dependencies

Run `pipenv install --dev` 

## Setting up your development environment

### 1. Create environment files from templates
##### NOTE: If you are an internal contributor (e.g. Datavault employee) then you may skip this step

In the `<project_root>/env/templates` folder you will find two files: `db.tpl.env` and `profiles.tpl.yml`.

#### profiles.tpl.yml

This file is a template which can be used as a drop-in replacement for your dbt `profiles.yml` file, or added to an existing `profiles.yml`.

- Copy and paste `profiles.tpl.yml` to your local dbt profiles directory, (by default, a `.dbt` directory in your home/user directory)
- Rename `profiles.tpl.yml` to `profiles.yml`. This file can be used as-is.

#### db.tpl.env

This file holds key-value pairs in YAML format. Each key is the key for an environment variable. You will 
replace the empty quotes with your own credentials. 

**WARNING: `db.env` IS IN THE GIT IGNORES, BUT PLEASE TAKE CARE NOT TO COMMIT THIS FILE.**

- Copy and paste `db.tpl.yml` to your to the root of the `<project_root>/env/` directory.
- Rename `db.tpl.yml` to `db.yml`. 
- Remove the sections for the platforms you are not developing. For example, if you are developing only for snowflake, 
  your file should resemble the following:

    ```yaml
    # Snowflake
    SNOWFLAKE_DB_ACCOUNT: ""
    SNOWFLAKE_DB_USER: ""
    SNOWFLAKE_DB_PW: ""
    SNOWFLAKE_DB_ROLE: ""
    SNOWFLAKE_DB_DATABASE: ""
    SNOWFLAKE_DB_WH: ""
    SNOWFLAKE_DB_SCHEMA: ""
    ```

- Next, Replace the empty quotes with your own credentials.


### 2. Run the setup command

### Without 1Password integration
##### Recommended for External Contributors

Inside the virtual environment, run `inv setup -p <platform> -d`

e.g. `inv setup -p snowflake -d`

### With 1Password integration
##### Recommended for Datavault Employees

Inside the virtual environment, run `inv setup -p <platform>`

e.g. `inv setup -p snowflake`

#### Options:

`-p, --platform` Sets the development platform environment which the dbtvault test harness will be executed under.

`platform` must be one of:

- snowflake
- bigquery
- sqlserver

`-d, --disable-op` Disables 1Password CLI integration, which is used by Datavault Developers internally for secrets management. 

A successful run should produce something similar to the following output:

```shell
(dbtvault) INFO: Defaults set.
(dbtvault) INFO: Project: test
(dbtvault) INFO: Platform: snowflake
(dbtvault) INFO: Platform set to 'snowflake'
(dbtvault) INFO: Project set to 'test'
(dbtvault) INFO: Checking dbt connection... (running dbt debug)
(dbtvault) INFO: Project 'test' is available at: '.../dbtvault/test/dbtvault_test'
Running with dbt=0.20.0
dbt version: 0.20.0
python version: 3.9.0
python path: ~/venvs/dbtvault/bin/python
os info: Linux-5.4.0-81-generic-x86_64-with-glibc2.31
Using profiles.yml file at ~/.dbt/profiles.yml
Using dbt_project.yml file at .../dbtvault/test/dbtvault_test/dbt_project.yml

Configuration:
  profiles.yml file [OK found and valid]
  dbt_project.yml file [OK found and valid]

Required dependencies:
 - git [OK found]

Connection:
  account: <redacted>
  user: <redacted>
  database: <redacted>
  schema: <redacted>
  warehouse: <redacted>
  role: <redacted>
  client_session_keep_alive: False
  Connection test: OK connection ok

(dbtvault) INFO: Installing dbtvault-dev in test project...
(dbtvault) INFO: Project 'test' is available at: '.../dbtvault/test/dbtvault_test'
Running with dbt=0.20.0
Installing ../../dbtvault-dev
  Installed from <local @ ../../dbtvault-dev>
Installing dbt-labs/dbt_utils@0.7.0
  Installed from version 0.7.0
(dbtvault) INFO: Setup complete!
```