---
title: "sp_help_job (Transact-SQL)"
description: Returns information about jobs that are used by SQL Server Agent to perform automated activities in SQL Server.
author: markingmyname
ms.author: maghan
ms.reviewer: randolphwest
ms.date: 05/14/2024
ms.service: sql
ms.subservice: system-objects
ms.topic: "reference"
f1_keywords:
  - "sp_help_job_TSQL"
  - "sp_help_job"
helpviewer_keywords:
  - "sp_help_job"
dev_langs:
  - "TSQL"
monikerRange: ">=sql-server-2016 || >=sql-server-linux-2017 || =azuresqldb-mi-current"
---
# sp_help_job (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Returns information about jobs that are used by [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] Agent to perform automated activities in [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)].

:::image type="icon" source="../../includes/media/topic-link-icon.svg" border="false"::: [Transact-SQL syntax conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## Syntax

```syntaxsql
sp_help_job
    [ [ @job_id = ] 'job_id' ]
    [ , [ @job_name = ] N'job_name' ]
    [ , [ @job_aspect = ] 'job_aspect' ]
    [ , [ @job_type = ] 'job_type' ]
    [ , [ @owner_login_name = ] N'owner_login_name' ]
    [ , [ @subsystem = ] N'subsystem' ]
    [ , [ @category_name = ] N'category_name' ]
    [ , [ @enabled = ] enabled ]
    [ , [ @execution_status = ] execution_status ]
    [ , [ @date_comparator = ] 'date_comparator' ]
    [ , [ @date_created = ] date_created ]
    [ , [ @date_last_modified = ] date_last_modified ]
    [ , [ @description = ] N'description' ]
[ ; ]
```

## Arguments

#### [ @job_id = ] '*job_id*'

The job identification number. *@job_id* is **uniqueidentifier**, with a default of `NULL`.

To view a specific job, either *@job_id* or *@job_name* must be specified. Omit both *@job_id* and *@job_name* to return information about all jobs.

#### [ @job_name = ] N'*job_name*'

The name of the job. *@job_name* is **sysname**, with a default of `NULL`.

To view a specific job, either *@job_id* or *@job_name* must be specified. Omit both *@job_id* and *@job_name* to return information about all jobs.

#### [ @job_aspect = ] '*job_aspect*'

The job attribute to display. *@job_aspect* is **varchar(9)**, and can be one of these values.

| Value | Description |
| --- | --- |
| `ALL` | Job aspect information |
| `JOB` | Job information |
| `SCHEDULES` | Schedule information |
| `STEPS` | Job step information |
| `TARGETS` | Target information |

#### [ @job_type = ] '*job_type*'

The type of jobs to include in the report.*@job_type* is **varchar(12)**, with a default of `NULL`. *@job_type* can be `LOCAL` or `MULTI-SERVER`.

#### [ @owner_login_name = ] N'*owner_login_name*'

The login name of the owner of the job. *@owner_login_name* is **sysname**, with a default of `NULL`.

#### [ @subsystem = ] N'*subsystem*'

The name of the subsystem. *@subsystem* is **nvarchar(40)**, with a default of `NULL`.

#### [ @category_name = ] N'*category_name*'

The name of the category. *@category_name* is **sysname**, with a default of `NULL`.

#### [ @enabled = ] *enabled*

A number indicating whether information is shown for enabled jobs or disabled jobs. *@enabled* is **tinyint**, with a default of `NULL`.

- `1` indicates enabled jobs.
- `0` indicates disabled jobs.

#### [ @execution_status = ] *execution_status*

The execution status for the jobs. *@execution_status* is **int**, and can be one of these values.

| Value | Description |
| --- | --- |
| `0` | Returns only those jobs that aren't idle or suspended. |
| `1` | Executing. |
| `2` | Waiting for thread. |
| `3` | Between retries. |
| `4` | Idle. |
| `5` | Suspended. |
| `7` | Performing completion actions. |

#### [ @date_comparator = ] '*date_comparator*'

The comparison operator to use in comparisons of *@date_created* and *@date_last_modified*. *@date_comparator* is **char(1)**, and can be `=`, `<`, or `>`.

#### [ @date_created = ] *date_created*

The date the job was created. *@date_created* is **datetime**, with a default of `NULL`.

#### [ @date_last_modified = ] *date_last_modified*

The date the job was last modified. *@date_last_modified* is **datetime**, with a default of `NULL`.

#### [ @description = ] N'*description*'

The description of the job. *@description* is **nvarchar(512)**, with a default of `NULL`. *@description* can include the [wildcard characters](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md) for pattern matching.

## Return code values

`0` (success) or `1` (failure).

## Result set

If no arguments are specified, `sp_help_job` returns this result set.

| Column name | Data type | Description |
| --- | --- | --- |
| `job_id` | **uniqueidentifier** | Unique ID of the job. |
| `originating_server` | **nvarchar(30)** | Name of the server from which the job came. |
| `name` | **sysname** | Name of the job. |
| `enabled` | **tinyint** | Indicates whether the job is enabled, so that it can execute. |
| `description` | **nvarchar(512)** | Description for the job. |
| `start_step_id` | **int** | ID of the step in the job where execution should begin. |
| `category` | **sysname** | Job category. |
| `owner` | **sysname** | Job owner. |
| `notify_level_eventlog` | **int** | A bitmask indicating circumstances in which a notification event should be logged to the Microsoft Windows application log. Can be one of these values:<br /><br />`0` = Never<br />`1` = When a job succeeds<br />`2` = When the job fails<br />`3` = Whenever the job completes (regardless of the job outcome) |
| `notify_level_email` | **int** | A bitmask indicating circumstances in which a notification e-mail should be sent when a job completes. Possible values are the same as for `notify_level_eventlog`. |
| `notify_level_netsend` | **int** | A bitmask indicating circumstances in which a network message should be sent when a job completes. Possible values are the same as for `notify_level_eventlog`. |
| `notify_level_page` | **int** | A bitmask indicating circumstances in which a page should be sent when a job completes. Possible values are the same as for `notify_level_eventlog`. |
| `notify_email_operator` | **sysname** | E-mail name of the operator to notify. |
| `notify_netsend_operator` | **sysname** | Name of the computer or user used when sending network messages. |
| `notify_page_operator` | **sysname** | Name of the computer or user used when sending a page. |
| `delete_level` | **int** | A bitmask indicating circumstances in which the job should be deleted when a job completes. Possible values are the same as for `notify_level_eventlog`. |
| `date_created` | **datetime** | Date the job was created. |
| `date_modified` | **datetime** | Date the job was last modified. |
| `version_number` | **int** | Version of the job (automatically updated each time the job is modified). |
| `last_run_date` | **int** | Date the job last started execution. |
| `last_run_time` | **int** | Time the job last started execution. |
| `last_run_outcome` | **int** | Outcome of the job the last time it ran:<br /><br />`0` = Failed<br />`1` = Succeeded<br />`3` = Canceled<br />`5` = Unknown |
| `next_run_date` | **int** | Date the job is scheduled to run next. |
| `next_run_time` | **int** | Time the job is scheduled to run next. |
| `next_run_schedule_id` | **int** | Identification number of the next run schedule. |
| `current_execution_status` | **int** | Current execution status:<br /><br />`1` = Executing<br />`2` = Waiting For Thread<br />`3` = Between Retries<br />`4` = Idle<br />`5` = Suspended<br />`6` = Obsolete<br />`7` = PerformingCompletionActions |
| `current_execution_step` | **sysname** | Current execution step in the job. |
| `current_retry_attempt` | **int** | If the job is running and the step was retried, this is the current retry attempt. |
| `has_step` | **int** | Number of job steps the job has. |
| `has_schedule` | **int** | Number of job schedules the job has. |
| `has_target` | **int** | Number of target servers the job has. |
| `type` | **int** | Type of the job.<br /><br />`1` = Local job.<br />`2` = Multiserver job.<br />`0` = Job has no target servers. |

If *@job_id* or *@job_name* is specified, `sp_help_job` returns these additional result sets for job steps, job schedules, and job target servers.

This is the result set for job steps.

| Column name | Data type | Description |
| --- | --- | --- |
| `step_id` | **int** | Unique (for this job) identifier for the step. |
| `step_name` | **sysname** | Name of the step. |
| `subsystem` | **nvarchar(40)** | Subsystem in which to execute the step command. |
| `command` | **nvarchar(3200)** | Command to execute. |
| `flags` | **nvarchar(4000)** | A bitmask of values that control step behavior. |
| `cmdexec_success_code` | **int** | For a **CmdExec** step, this is the process exit code of a successful command. |
| `on_success_action` | **nvarchar(4000)** | What to do if the step succeeds:<br /><br />`1` = Quit with success.<br />`2` = Quit with failure.<br />`3` = Go to next step.<br />`4` = Go to step. |
| `on_success_step_id` | **int** | If `on_success_action` is `4`, this indicates the next step to execute. |
| `on_fail_action` | **nvarchar(4000)** | Action to take if the step fails. Values are the same as for `on_success_action`. |
| `on_fail_step_id` | **int** | If `on_fail_action` is `4`, this indicates the next step to execute. |
| `server` | **sysname** | Reserved. |
| `database_name` | **sysname** | For a [!INCLUDE [tsql](../../includes/tsql-md.md)] step, this is the database in which the command executes. |
| `database_user_name` | **sysname** | For a [!INCLUDE [tsql](../../includes/tsql-md.md)] step, this is the database user context in which the command executes. |
| `retry_attempts` | **int** | Maximum number of times the command should be retried (if it's unsuccessful) before the step is deemed to have failed. |
| `retry_interval` | **int** | Interval (in minutes) between any retry attempts. |
| `os_run_priority` | **varchar(4000)** | Reserved. |
| `output_file_name` | **varchar(200)** | File to which command output should be written ([!INCLUDE [tsql](../../includes/tsql-md.md)] and **CmdExec** steps only). |
| `last_run_outcome` | **int** | Outcome of the step the last time it ran:<br /><br />`0` = Failed<br />`1` = Succeeded<br />`3` = Canceled<br />`5` = Unknown |
| `last_run_duration` | **int** | Duration (in seconds) of the step the last time it ran. |
| `last_run_retries` | **int** | Number of times the command was retried the last time the step ran. |
| `last_run_date` | **int** | Date the step last started execution. |
| `last_run_time` | **int** | Time the step last started execution. |
| `proxy_id` | **int** | Proxy for the job step. |

This is the result set for job schedules.

| Column name | Data type | Description |
| --- | --- | --- |
| `schedule_id` | **int** | Identifier of the schedule (unique across all jobs). |
| `schedule_name` | **sysname** | Name of the schedule (unique for this job only). |
| `enabled` | **int** | Whether the schedule is active (`1`) or not (`0`). |
| `freq_type` | **int** | Value indicating when the job is to be executed:<br /><br />`1` = Once<br />`4` = Daily<br />`8` = Weekly<br />`16` = Monthly<br />`32` = Monthly, relative to the `freq_interval`<br />`64` = Run when [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] Agent service starts. |
| `freq_interval` | **int** | Days when the job is executed. The value depends on the value of `freq_type`. For more information, see [sp_add_schedule](sp-add-schedule-transact-sql.md) |
| `freq_subday_type` | **int** | Units for `freq_subday_interval`. For more information, see [sp_add_schedule](sp-add-schedule-transact-sql.md) |
| `freq_subday_interval` | **int** | Number of `freq_subday_type` periods to occur between each execution of the job. For more information, see [sp_add_schedule](sp-add-schedule-transact-sql.md) |
| `freq_relative_interval` | **int** | Scheduled job's occurrence of the `freq_interval` in each month. For more information, see [sp_add_schedule](sp-add-schedule-transact-sql.md) |
| `freq_recurrence_factor` | **int** | Number of months between the scheduled execution of the job. |
| `active_start_date` | **int** | Date to begin execution of the job. |
| `active_end_date` | **int** | Date to end execution of the job. |
| `active_start_time` | **int** | Time to begin the execution of the job on `active_start_date.` |
| `active_end_time` | **int** | Time to end execution of the job on `active_end_date`. |
| `date_created` | **datetime** | Date the schedule is created. |
| `schedule_description` | **nvarchar(4000)** | An English description of the schedule (if requested). |
| `next_run_date` | **int** | Date the schedule next causes the job to run. |
| `next_run_time` | **int** | Time the schedule next causes the job to run. |
| `schedule_uid` | **uniqueidentifier** | Identifier for the schedule. |
| `job_count` | **int** | Returns the number of jobs that reference this schedule. |

This is the result set for job target servers.

| Column name | Data type | Description |
| --- | --- | --- |
| `server_id` | **int** | Identifier of the target server. |
| `server_name` | **nvarchar(30)** | Computer name of the target server. |
| `enlist_date` | **datetime** | Date the target server enlisted into the master server. |
| `last_poll_date` | **datetime** | Date the target server last polled the master server. |
| `last_run_date` | **int** | Date the job last started execution on this target server. |
| `last_run_time` | **int** | Time the job last started execution on this target server. |
| `last_run_duration` | **int** | Duration of the job the last time it ran on this target server. |
| `last_run_outcome` | **tinyint** | Outcome of the job the last time it ran on this server:<br /><br />`0` = Failed<br />`1` = Succeeded<br />`3` = Canceled<br />`5` = Unknown |
| `last_outcome_message` | **nvarchar(1024)** | Outcome message from the job the last time it ran on this target server. |

## Permissions

[!INCLUDE [msdb-execute-permissions](../../includes/msdb-execute-permissions.md)]

Other users must be granted one of the following [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] Agent fixed database roles in the `msdb` database:

- **SQLAgentUserRole**
- **SQLAgentReaderRole**
- **SQLAgentOperatorRole**

For details about the permissions of these roles, see [SQL Server Agent Fixed Database Roles](../../ssms/agent/sql-server-agent-fixed-database-roles.md).

Members of **SQLAgentUserRole** can only view jobs that they own. Members of **sysadmin**, **SQLAgentReaderRole**, and **SQLAgentOperatorRole** can view all local and multiserver jobs.

## Examples

### A. List information for all jobs

The following example executes the `sp_help_job` procedure with no parameters to return the information for all of the jobs currently defined in the `msdb` database.

```sql
USE msdb;
GO

EXEC dbo.sp_help_job;
GO
```

### B. List information for jobs matching a specific criteria

The following example lists job information for the multiserver jobs owned by `françoisa` where the job is enabled and executing.

```sql
USE msdb;
GO

EXEC dbo.sp_help_job
   @job_type = N'MULTI-SERVER',
   @owner_login_name = N'françoisa',
   @enabled = 1,
   @execution_status = 1;
GO
```

### C. List all aspects of information for a job

The following example lists all aspects of the information for the job `NightlyBackups`.

```sql
USE msdb;
GO

EXEC dbo.sp_help_job
    @job_name = N'NightlyBackups',
    @job_aspect = N'ALL';
GO
```

## Related content

- [sp_add_job (Transact-SQL)](sp-add-job-transact-sql.md)
- [sp_delete_job (Transact-SQL)](sp-delete-job-transact-sql.md)
- [sp_update_job (Transact-SQL)](sp-update-job-transact-sql.md)
- [System stored procedures (Transact-SQL)](system-stored-procedures-transact-sql.md)
