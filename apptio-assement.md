### RESOURCES:

Ansible host list passed at runtime, bypassing inventory file, to make it easier for user to control deployment:
https://stackoverflow.com/questions/37652464/how-to-run-ansible-without-hosts-file/37652886

Ansible example of a when changed registration:
https://raymii.org/s/tutorials/Ansible_-_Only-do-something-if-another-action-changed.html

Standard Ansible documentation for file fetch, debug, handlers, and role syntax/options:
https://docs.ansible.com/

Following for testing ansible in travis:
https://www.jeffgeerling.com/blog/testing-ansible-roles-travis-ci-github

Dillinger.io for markdown editing:
https://dillinger.io/

### DESIGN CHOICES:

I decided to use ansible to deploy the tarball in this case. Ansible provides declaritive language, and as such provides built-in verification and testing frameworks. This also presents easy enhancement in the future to integrate into a CICD pipline or larger scale production deployment orchestration. Also due to the user being a novice developer, I presumed they did not have root on all systems to perform tasks, and only had a resonable set of local tools.

### ALTERNATIVES:

- BASH:  I chose not to use bash, due to indempotence and requiring more work to include testing/validation.  It could be used as a bootstrap/wrapper if other significant work outside of ansible would need to be done, but in this case of single command deployment of a webserver, ansible with a user supplied host at runtime was easiest.

- Python/Ruby:  Similarly to bash, there was just no need in this case to use any robust scripting capabilities.

- Docker: Creating a Dockerfile and allowing a user to do a docker run would have worked, however since this was meant to be a simple install, I didn't want to assume that docker was available or installable by the end user.

### DEPLOYMENT: 
Single role that can be checked out via git, or downloaded via browser.  Only requirement is ansible being installed on the workstation.

### MONITORING/REPORTING/ALERTS: 
In a more production like pipeline/environment, you would want to take playbook output and store in a central log archive for review (splunk/elk/etc..).  For compliance reasons you could use this to report on any change activity.  You could also use this to overlay deploy activity on top of metric/monitoring dashboards to more easily corellate potential fluxuations in monitoring with deployment events.  You could also add slack output support that would send notifications to any stakeholders of these activities. Further on in production it would make more sense to create startup scripts and make this more of a service as well as opposed to installing locally in a users space. Supporting other destination OS like OSX could be easily added to the playbook if necessary.

### CONFIGURATION:  
Only configuration needed is a target host, comma separated list of hosts, or an actual ansible inventory.  The web application itself could in a production like environment, either use the built-in ansible templating combined with a variety of facts to create a configuration file on the fly.  Or it could a centralized configuration management system such as etcd, or consul.

### TESTING: 
Ansible has significant built-in testing inherent to the internals. This allows for less unit testing necessity to be written by the end user.  Actual final use case verification such as working web requests, can be specified in the playbook, or farmed out to QA servers to run their suite of automated integration tests.  See https://docs.ansible.com/ansible/latest/reference_appendices/test_strategies.html for Ansibles opinionated view on testing. Using travis-ci integration, I am testing both fresh install, and running twice to see if there is any issue with indempotence.

