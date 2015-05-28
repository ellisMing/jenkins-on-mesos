jenkins-on-mesos
================

Jenkins standalone instance package with the Mesos plugin pre-installed. Besides jenkins slave **auto scaling**, marathon is supporting **high availability** with plugin **scm-sync-configuration**.

###How to use it quickly

1. Wget the *start* script template ``start-jenkins.app.sh.template`` and ``marathon.json``

  ```bash
  $wget https://raw.githubusercontent.com/Dataman-Cloud/jenkins-on-mesos/master/start-jenkins.app.sh.template
  $wget https://raw.githubusercontent.com/Dataman-Cloud/jenkins-on-mesos/master/marathon.json
  ```

2. Create a git repo on your private repo server or github to persist jenkins' data

  We have to create a git repo named **jenkins-on-mesos**, such as **git@gitlab.dataman.io:core/jenkins-on-mesos.git**, to persist jenkins master data, which is needed by plugin [SCM Sync configuration plugin](https://wiki.jenkins-ci.org/display/JENKINS/SCM+Sync+configuration+plugin). BTW, make sure there is one user on mesos-slave can pull/push the repo. 

3. Edit to get your *start* script

  ```bash
  $cp start-jenkins.app.sh.template start-jenkins.app.sh
  $vim start-jenkins.app.sh
  ```

  we need to edit 3 var in the file ``start-jenkins.app.sh``: 

  1. **SCM_SYNC_GIT**: The above git repo addr, here is my example ``git@gitlab.dataman.io:core/jenkins-on-mesos.git``
  2. **APP_USER**: will deploy jenkins on marathon as user APP_USER, **who must have been granted to pull/push repo    SCM_SYNC_GIT** since jenkins will sync the configure with git repo **SCM_SYNC_GIT**.
  3. **MARATHON_PORTAL**: Marathon PORTAL, for example: http://marathon.dataman.io:8080/v2/apps
  
4. Run the *start* script to deploy a new jenkins master on marathon

  ```bash
  bash start-jenkins.app.sh
  ```

5. Register jenkins master as mesos framework

  We need visit jenkins-master configure page to edit section **Mesos Cloud**, set the **Mesos Master [hostname:port]** as your mesos entry. [more config detail](http://www.ebaytechblog.com/2014/05/12/delivering-ebays-ci-solution-with-apache-mesos-part-ii/)

###Features

1. auto scaling jenkins slave nodes
2. deployed over marathon for high availability
3. pre-installed plugin scm-sync-configuration for data persistence
