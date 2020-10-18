---
layout: post
title:  "Ansible Run Analysis"
author: Ajinkya
description: Analysing Ansible Runs with ARA tool.
categories: [ ansible, ara ]
image: assets/images/2017-09-14-Ansible-Run-Analysis/ara-with-icon-old.png
---
Ansible can be used for a lot of things and it’s grown pretty popular for managing servers and their configuration. Today, Ansible is heavily used to deploy or test through Continuous Integration (CI).

In the world of automated continuous integration, it’s not uncommon to have hundreds, if not thousands of jobs running every day for testing, building, compiling, deploying and so on.


## Problem

Well, you cannot call it as a "Problem!",  but Ansible runs generate quite a large amount of console data. And yes, it continues to grow with each flag (Remember -v & -vvv?) that you add to the run!

Keeping up with such a large amount of Ansible runs and their outcome, not just in the context of CI, is challenging.

Due to this mess, one tends to feel the need of something, that will present this verbose output in a way which is easily readable, graphical, tabular & more representative of the job status and debug information.

> That something is ARA.


## Ansible Run Analysis (ARA) Tool

ARA records Ansible playbook runs and makes the recorded data available and intuitive for users and systems. ARA organizes recorded playbook data in a way to make it intuitive for you to search and find what you’re interested in as fast and as easily as possible.

![ara1]({{ site.baseurl }}/assets/images/2017-09-14-Ansible-Run-Analysis/ara1.png)

It provides summaries of task results per host or per playbook.

![ara2]({{ site.baseurl }}/assets/images/2017-09-14-Ansible-Run-Analysis/ara2.png)

It allows you to filter task results by playbook, play, host, task or by the status of the task.

![ara3]({{ site.baseurl }}/assets/images/2017-09-14-Ansible-Run-Analysis/ara3.png)

With ARA, you’re able to easily drill down from the summary view for the results you’re interested in, whether it’s a particular host or a specific task.

![ara4]({{ site.baseurl }}/assets/images/2017-09-14-Ansible-Run-Analysis/ara4.png)

Beyond browsing a single ansible-playbook run, ARA supports recording and viewing multiple runs in the same database.

![ara5]({{ site.baseurl }}/assets/images/2017-09-14-Ansible-Run-Analysis/ara5.png)


## Installation

There are 2 ways in which you can install ARA in your system.

+ Using Ansible Role hosted on my [GitHub Account](https://github.com/AjinkyaBapat/Ansible-Run-Analyser){:target="_blank"}
    
    Clone the repo & do:

    ```yaml
    ansible-playbook Playbook.yml
    ```

    If Playbook run is successful, you will get:    

    ```yaml
    TASK [ara : Display ara UI URL] ************************
    ok: [localhost] => {}
    "msg": "Access playbook records at http://YOUR_IP:9191"
    ``` 

    Note: It picks the IP address from ansible_default_ipv4 fact gathered by Ansible. If there is no such fact gathered, replace it with your IP in `main.yml` file present in roles/ara/tasks/ folder.

+ ARA is an open source project available on [Github](https://github.com/dmsimard/ara){:target="_blank"} under the Apache v2 license. Installation instructions are present under Quickstart chapter.

The [Documentation](http://ara.readthedocs.io/en/latest/){:target="_blank"} and [frequently asked questions](http://ara.readthedocs.io/en/latest/faq.html){:target="_blank"} are available on readthedocs.io.


## Conclusion

Ever since I came across this tool, it's been a useful resource for me to get more out of Ansible run logs and outputs. I would highly recommend it for all Ansible Ninjas! out there.

Feel free to share this with others and do let me know your experience of using ARA.