The goal of this workshop is to get familiar with all the capabilities Cartographer provides.

VMware Tanzu Application Platform (TAP) uses **Supply Chain Choreographer** which is based on the open source **Cartographer** to **allow App Operators create pre-approved paths to production** by integrating Kubernetes resources with the elements of their existing toolchains.

```dashboard:open-url
url: https://cartographer.sh
```

Each pre-approved supply chain creates a paved road to production. Orchestrating supply chain resources - test, build, scan, and deploy - allows developers to focus on delivering value to their users and provides App Operators the assurance that all code in production has passed through all the steps of an approved workflow.

##### Design and Philosophy

Cartographer **allows operators** via the **Supply Chain** abstraction **to define all of the steps** that an application must go through to create an image and Kubernetes configuration. 

**Contrary to many other Kubernetes native workflow tools** that already exist in the market, **Cartographer does not “run” any of the objects themselves**. Instead, it monitors the execution of each resource and templates the following resource in the supply chain after a given resource has completed execution and updated its status.

In the **orchestration** model, which is used by most of the current CI/CD tools like Jenkins or Tekton, an **orchestrator** executes, monitor, and manage each of the steps of the path to production. The CI stage, or any others, could not function independently from the orchestrator. In the case of a path to production with a vulnerability scanning step, if a new CVE should arise, the only way to scan the code for it would be to trigger the orchestrator to initiate the scanning step or a new run through the supply chain.
![](../images/orchestrator.png)

In the **choreography** model, each step of the path to production and the tool required for that step knows nothing about the next step. It is responsible for receiving a signal that it must perform some work, completing it, and signaling that it has finished. In the same case as above, with a pipeline that has a vulnerability scanner, if there was a new CVE, the vulnerability scanner would know about it and trigger a new scan. When the scan is complete, the vulnerability scanner would send a message indicating that scanning is complete.
![](../images/choreographer.png)
Because **steps of the path to production are rarely synchronous**, for example, if a new CVE comes up, someone clicks the button on a build, and so on, choreography is a natural choice as a workflow engine. **Flexibility and the ability to swap steps of the path to production is also of extreme importance**.

##### Reusable CI/CD
By design, **supply chains can be reused by many workloads**. This allows an operator to specify the steps in the path to production a single time, and for developers to specify their applications independently but for each to use the same path to production. The intent is that developers are able to focus on providing value for their users and can reach production quickly and easily, while providing peace of mind for app operators, who are ensured that each application has passed through the steps of the path to production that they’ve defined.

![Cartographer Diagram](../images/cartographer.png)

##### Separation of Concerns
While the **supply chain is operator** facing, Cartographer also provides an **abstraction for developers** called **Workloads**. Workloads allow developers to create application specifications such as the location of their repository, environment variables and service claims.

##### Kubernetes Resource Interoperability
With Cartographer it's possible to choreograph both Kubernetes and non-Kubernetes resources within the same supply chain via **integrations to existing CI/CD pipelines** like Tekton by using the **Runnable CRD**.

##### The core of VMware Tanzu Application Platform

TAP provides a **full integration of all of its components via out of the box Supply Chains** that can be customized for customers' processes and tools.