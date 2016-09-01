# ChronosInvestigation

Chronos is a distributed and fault-tolerant scheduler which runs on top of Mesos.

# Overview

## Big picture

Chronos allows users to view execution history of jobs in unified place. This section is actually a main dashboard of Chronos. You are able to filter jobs by **name** or **status** (for example to view only completed or running jobs in list).

![Screenshot 1](http://mesos.github.io/chronos/img/chronos_ui-1.png)

## Dependencies pipeline

You are able to view dependencies pipeline of jobs that you've created under `graph` section.

![Screenshot 2](https://s14.postimg.org/gb3tpbce9/Screenshot_at_Sep_01_12_23_28.png)

## Create new job

Creating job in Chronos is quite simple. To create new job, click `+ New job` button from main dashboard. Then specify all necessary field values. In order to manage dependencies, so particular job will be executed after job it depends will be executed successfully fill out "parents" box. One job can have more than one parent, so it will start execution only after all parent jobs will finish completely.

![Screenshot 4](https://s10.postimg.org/5vkxckkop/Screenshot_at_Sep_01_12_41_25.png)

## Job management

If you want to **re-run** some failed job, you have to select it in list and click `force run` button.

![Screenshot 3](https://s17.postimg.org/6iy5vs5cv/Screenshot_at_Sep_01_12_27_59.png)



Although Chronos provides handy enough web UI, some of it's features are available only under CLI.
For instance, to view execution history with execution ID's, you have to execute following command (using DC/OS for testing):
`dcos task --completed ct*`.

The `--completed` argument includes tasks that have completed their execution. Chronos uses the prefix `ct` for all its tasks, so `ct*` filters only Chronos tasks.

To view the output of a task, copy one of the values under the ID column in the output of the previous command, and use it as the argument to `dcos task log`. For example: `dcos task log --completed ct:1460844479000:0:Date:`.

The output should look similar to this:

    Registered executor on 10.0.0.252
    Starting task ct:1460844479000:0:Date:
    Forked command at 15344
    sh -c '/bin/date'
    Some output
    Command exited with status 0 (pid: 15344)
    
## Execution environment

Chronos runs on top of Apache Mesos. It supports custom Mesos executors as well as the default command executor.
In this example Chronos was installed on DS/OS that is on AWS.

## Comparing Chronos and Azkaban

**Supported types of actions out of the box**

- Chronos: by default executes only .sh scripts.
- Azkaban: java, javaprocess and pig, command scripts.

**Regular Scheduling**

- Chronos: interval job scheduling is time based
- Azkaban: interval job scheduling is time based as well, but with a possibility to make workflows dependent on each other

**Parameterization of workflows**
- Chronos: doesn't support variables
- Azkaban: supports variables, i.e.: ${input}

**Job management**

- Chronos: filter jobs by status or name.
- Azkaban: advanced filtering by project, flow, user, status, time period etc.

**Workflow management**

- Chronos: all jobs are being executed in single default workflow.
- Azkaban: user can combine jobs into multiple workflows, workflows can be combined into multiple projects.

**Visual representation of job's dependencies**

- Chronos: provides job dependencies visualization as graphs.
- Azkaban: provides job dependencies visualization as graphs with possibility to re-run perticular job or group of jobs directly from canvas, enable or disable groups etc.


## Personal opinion

After comparing Azkaban and Chronos, launching some basic jobs and scheduling them, i've highlighted some noteworthy differences that Azkaban is leading over Chronos. 

- First of all, Azkaban allows users to create isolated workflows that in conjunction with other workflows are projects. This allows better control over jobs, as we can combine some huge job pipelines into groups(workflows) causing better understanding of pipeline. Futhermore, it's possible to make dependent execution of one workflow on another in Azkaban (as workflow is just another job type).
- Azkaban has much more job types out of the box, other jobtypes can be included into Azkaban working instance via jobtype plugins. Chronos supports only .sh (bash on most systems) scripts, and doesn't support plugins.
- Azkaban's UI is much more flexible, with a lot of functionality on the board, and much more sections to use. In Azkaban you can view view executions page for every workflow or even for every job. Canvas of Azkaban is very interactive in comparison to Chronos one, as there you can not only view dependencies, but also enable or disable some jobs in order to re-run them.
- Chronos is expedient to use when cluster manager is Mesos, as it is running on the top of it.
- Azkaban's community is much more bigger and widespread in the world. If you face some difficulties in a particular case, it's more likely to find ready solution or at least ask some question at some forum and receive a feedback.















