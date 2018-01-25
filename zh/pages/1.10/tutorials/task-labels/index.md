---
layout: layout.pug
navigationTitle: Labeling Tasks and Jobs
title: Labeling Tasks and Jobs
menuWeight: 6
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

本教程将演示如何使用DC / OS Web界面和Marathon HTTP API定义标签，以及如何根据标签值标准查询正在运行的应用程序和作业的相关信息。

在DC / OS群集中部署应用程序，容器或作业时，可以将标记或标签与部署的组件关联，以跟踪和报告这些组件的使用情况。 在DC / OS群集中部署应用程序，容器或作业时，可以将标记或标签与部署的组件关联，以跟踪和报告这些组件的使用情况。...

# 为应用程序和任务分配标签

您可以通过DC / OS Web界面的**Services**选项卡或从DC / OS CLI将标签附加到任务。 您可以指定多个标签，但每个标签只能有一个值。

## 从DC / OS Web界面为应用程序或任务分配标签

在DC / OS Web界面中，点击**Services**标签。 您可以在部署新服务时添加标签，也可以从**Labels**标签中编辑现有标签。

## 从DC / OS CLI为应用程序或任务分配一个标签

您也可以在应用程序定义的` labels `参数中指定标签值。

    vi myapp.json
    
    {
        "id": "myapp",
        "cpus": 0.1,
        "mem": 16.0,
        "ports": [
            0
        ],
        "cmd": "/opt/mesosphere/bin/python3 -m http.server $PORT0",
        "instances": 2,
        "labels": {
            "COST_CENTER": "0001"
        }
    }
    

然后，从DC / OS CLI进行部署：

```bash
dcos marathon app add <myapp>.json
```

# 为作业分配标签

您可以通过DC / OS Web界面的**Jobs**选项卡或从DC / OS CLI将作业标签附加到作业中。 您可以指定多个标签，但每个标签只能有一个值。

## 从DC / OS Web界面为作业分配一个标签

在DC / OS网络界面中，点击**Jobs**标签。 您可以在部署新作业时添加标签，也可以从**Labels**标签中编辑现有标签。

![Job label](/1.10/img/job-label.png)

## 从DC / OS CLI为作业分配一个标签

您还可以在作业定义的` labels `参数中指定标签值。

    vi myjob.json
    
     ```json
        {
          "id": "my-job",
          "description": "A job that sleeps",
          "labels": {
            "department": "marketing"
          },
          "run": {
            "cmd": "sleep 1000",
            "cpus": 0.01,
            "mem": 32,
            "disk": 0
          }
        }
     ```
    

然后，从DC / OS CLI进行部署：

```bash
dcos job add <myjob>.json
```

# 显示标签信息

部署和启动应用程序后，可以按照DC / OS UI的**Services**选项卡中的标签进行过滤。

You can also use the Marathon HTTP API from the DC/OS CLI to query the running applications based on the label value criteria.

The code snippet below shows an HTTP request issued to the Marathon HTTP API. The curl program is used in this example to submit the HTTP GET request, but you can use any program that is able to send HTTP GET/PUT/DELETE requests. You can see the HTTP end-point is `https://52.88.210.228/marathon/v2/apps` and the parameters sent along with the HTTP request include the label criteria `?label=COST_CENTER==0001`:

    curl --insecure \
    > https://52.88.210.228/marathon/v2/apps?label=COST_CENTER==0001 \
    > | python -m json.tool | more
    

You can also specify multiple label criteria like so: `?label=COST_CENTER==0001,COST_CENTER==0002`

In the example above, the response you receive will include only the applications that have a label `COST_CENTER` defined with a value of `0001`. The resource metrics are also included, such as the number of CPU shares and the amount of memory allocated. At the bottom of the response, you can see the date/time this application was deployed, which can be used to compute the uptime for billing or charge-back purposes.