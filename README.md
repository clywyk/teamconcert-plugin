[.conf-macro .output-inline]# #

[.aui-icon .aui-icon-small .aui-iconfont-info .confluence-information-macro-icon]##

Older versions of this plugin may not be safe to use. Please review the
following warnings before using an older version:

* https://jenkins.io/security/advisory/2019-12-17/#SECURITY-1605%20(1)[CSRF
vulnerability and missing permission checks allows capturing
credentials]
* https://jenkins.io/security/advisory/2019-12-17/#SECURITY-1605%20(2)[Users
with Overall/Read access can enumerate credential IDs]

[.conf-macro .output-inline]#Integrates Jenkins with
https://jazz.net/products/rational-team-concert/[Rational Team Concert]
https://jazz.net/products/rational-team-concert/features/scm[source
control] and
https://jazz.net/products/rational-team-concert/features/build[build]
using the richer features of the build toolkit instead of the command
line.#

With the build toolkit this plugin adds traceability links from a
Jenkins build to an RTC build result, workspace and snapshot.  It also
publishes links to work items, change sets and file contents captured in
the snapshot.  It leverages the current RTC features and workflows that
users are already familiar with such as, emails, toaster popups,
reporting, dashboards, etc.

[[TeamConcertPlugin-Documentation]]
== Documentation

[[TeamConcertPlugin-RationalTeamConcertHelpTopics]]
=== Rational Team Concert Help Topics

. https://jazz.net/help-dev/clm/topic/com.ibm.team.build.doc/topics/t_build_overview.html[Rational
Team Concert Build Overview]
. https://jazz.net/help-dev/clm/topic/com.ibm.jazz.install.doc/topics/t_install_build_toolkit.html[Installing
the Build System Toolkit]
. https://jazz.net/help-dev/clm/topic/com.ibm.team.build.doc/topics/tcreatepasstxt.html[Creating
encrypted password files]
. https://jazz.net/help-dev/clm/index.jsp?re=1&topic=/com.ibm.team.build.doc/topics/c_hudson_overview.html&scope=null[Hudson/Jenkins
build engine type]
. https://jazz.net/help-dev/clm/index.jsp?re=1&topic=/com.ibm.team.build.doc/topics/tcreateworkspace.html&scope=null[Dedicated
build workspaces]

[[TeamConcertPlugin-Requirements]]
=== Requirements

[[TeamConcertPlugin-Jenkins]]
==== Jenkins

* Team Concert Plugin v 1.2.0.5 and above requires Jenkins 1.625.1 and
above. Fix for JENKINS-26100 requires Jenkins 2.60 and above,
workflow-job 2.12 and above
* Team Concert Plugin v 1.1.9.3 till v 1.2.0.4 requires Jenkins 1.580.1
and above. 
* Team Concert Plugin v 1.1.2 and later depends on
the https://wiki.jenkins-ci.org/display/JENKINS/Credentials+Plugin[Jenkins
Credentials plugin version] 1.10 or later. +
 +

[[TeamConcertPlugin-RTC]]
==== RTC

* This plugin requires
https://jazz.net/products/rational-team-concert/features/build#build-toolkit[Rational
Team Concert Build Toolkit] version 4.0.7 or newer. Older versions of
the plugin supports build toolkit versions starting from 3.0.1.5. See
the
https://jazz.net/help-dev/clm/topic/com.ibm.jazz.install.doc/topics/t_install_build_toolkit.html[Installing
the Build System Toolkit] help topic to learn how to install the build
toolkit.
* For all the supported build configurations - Build Definition,
Repository Workspace, Stream and Snapshot - *a valid build toolkit
should be present on both the master and slave machines* and the Jenkins
jobs should be configured to use this toolkit.
* Some features depend on specific Rational Team Concert build toolkit
 or server versions. See below.
** Stream configuration works only from build toolkit v. 5.0 or higher.
** Post Build Deliver for Build Definition configuration introduced in
Team Concert Plugin v. 1.2.0.3 depends on Rational Team Concert server
version 6.0.4 or higher.
** Support for Load Rules in build definition has some requirements on
the version of RTC client used to create the build definition. See *Load
Rules Support* section for more details.
** If you will be fetching workspaces that contain symbolic links, there
is some additional symbolic link setup required. See *Symbolic Link
Support* section for more details.
** Version details of build toolkit can be obtained in the build log
only if you are using build toolkit version 5.0.2 and above.

[[TeamConcertPlugin-JenkinsConfiguration]]
=== Jenkins Configuration

. Navigate to the Jenkins Global Tool configure page (Jenkins > Manage
Jenkins >  Global Tool Configuration) and find the "RTC Build toolkit"
section.  This section is used to define one or more build toolkits
available to the plugin.If you are using Jenkins 1.x, this will be under
(Jenkins -> Manage Jenkins -> Configure System)
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_1.png?version=2&modificationDate=1461323530000&api=v2[image,width=794,height=59]#
. Click the "RTC Build toolkit installations..." button and add a new
build toolkit.
* See the
https://jazz.net/help-dev/clm/topic/com.ibm.jazz.install.doc/topics/t_install_build_toolkit.html[Installing
the Build System Toolkit] help topic to learn how to install the build
toolkit.
* There can be multiple RTC build toolkits associated with one jenkins
instance.
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_2.png?version=2&modificationDate=1461325443000&api=v2[image,width=794,height=182]#
. Click the "Apply" button to apply the changes.
. Navigate to the Jenkins Global Configuration page (Manage Jenkins ->
Configure System).
. Find the "Rational Team Concert (RTC)" section. This section is used
to define global connection settings that will be the defaults for any
jobs created with the plugin. If connection settings will be set on each
job, then skip this section. 
* Select a build toolkit
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_3.png?version=3&modificationDate=1461325407000&api=v2[image,width=789,height=246]#
. Credentials are managed by the
https://wiki.jenkins-ci.org/display/JENKINS/Credentials+Plugin[Credentials
plugin]. The Team Concert plugin supports username and password type
credentials. Credentials can be defined within a domain or a folder (if
you are using the folder's plugin).
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_4.png?version=1&modificationDate=1461325230000&api=v2[image,width=794,height=329]#
. Choose the credentials to use when logging into RTC for polling and
building.
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_5.png?version=1&modificationDate=1461325883000&api=v2[image,width=794,height=235]#
* If you are using the 1.0.12 (or earlier) version of the Team Concert
plugin, instead of credentials, you will need to supply a userId and
password or password file.
. Click the "Test connection" button to verify the repository connection
details.
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_6.png?version=1&modificationDate=1461326078000&api=v2[image,width=796,height=42]#
. Click the "Save" button to save the settings and return to the Jenkins
main page.

[[TeamConcertPlugin-JobConfiguration]]
=== Job Configuration

. Create a new free-style software project and find the "Source Code
Management" section.
. Select "Rational Team Concert (RTC)".
. If global connection settings were not configured above or do not
apply to this job, then check the "Override global RTC repository
connection" check box and enter the connection settings here.
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_1.png?version=2&modificationDate=1461327544000&api=v2[image,width=794,height=364]#
. Click the "Test connection" button to verify the repository connection
details.
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_build_tookit_1200_6.png?version=1&modificationDate=1461326078000&api=v2[image,width=796,height=41]#
. Prior to 1.2.0.0 a job can be configured with RTC SCM using either a
build definition or a build workspace. In 1.2.0.0 there is support to
configure RTC SCM with a build stream or build snapshot also.
. To benefit most from the integration between this plugin and RTC
Build, select "Build Definition" from the Build Configuration dropdown
and enter a build definition ID. See the
https://jazz.net/help-dev/clm/index.jsp?re=1&topic=/com.ibm.team.build.doc/topics/c_hudson_overview.html&scope=null[Hudson/Jenkins
build engine type] help topic to learn how to create a Jenkins build
definition. Follow these steps to setup a Jenkins Build Definition and
Jenkins Job to avoid a catch-22 situation.  A Jenkins job requires a
Hudson/Jenkins build definition and a Hudson/Jenkins build definition
requires a Jenkins job.  RTC actually won't let you save the build
definition without a job selected. However, Jenkins will let you save a
job without a build definition.  So it is important to configure your
build definition and job this way.
.. In Jenkins, create the job first using RTC for source control, but
with no build definition. Leave the _Build Definition_ text box blank.
Save the Jenkins Job.
.. In RTC, create a Jenkins build engine that connects to the Jenkins
server.
See https://jazz.net/help-dev/clm/topic/com.ibm.team.build.doc/topics/t_hudson_build_eng.html[Creating
a build engine]
.. In RTC, create a build definition that uses the build engine created
in step b and select the job created in step a.
See *https://jazz.net/help-dev/clm/topic/com.ibm.team.build.doc/topics/t_hudson_build_def.html[Creating
a build definition]*
.. Lastly, in Jenkins, open the Jenkins job and set the _Build
Definition_ field with the id of the build definition created in step c.
* [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_2.png?version=1&modificationDate=1461328211000&api=v2[image,width=794,height=119]#
* Notice the "Build Configuration" dropdown which replaces the radio
buttons for build definition and build workspace.
* Click the "Validate" button to verify the RTC build definition exists.
. To load the jenkins build workspace using a RTC repository workspace,
select "Build Workspace" from the Build Configuration dropdown. See the
https://jazz.net/help-dev/clm/index.jsp?re=1&topic=/com.ibm.team.build.doc/topics/tcreateworkspace.html&scope=null[Dedicated
build workspaces] help topic to learn how to create a build workspace.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_4.png?version=1&modificationDate=1461329162000&api=v2[image,width=794,height=125]#
.. Click the "Validate" button to verify the RTC build workspace exists.
.. To add a "Related Artifact" link to a Jenkins build in all the
included work items, select the option "Add Jenkins build link to
accepted work items" option.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/JenkinsWorkspaceAddLink.png?version=1&modificationDate=1556704491000&api=v2[image,height=237]#
. To load the jenkins build workspace using a snapshot, select "Build
Snapshot" from the Build Configuration dropdown. This configuration is
mainly intended to be used in builds that capture the current state of
the RTC SCM workspace/stream in a snapshot and start downstream builds
that would populate the jenkins build workspace from the snapshot
created and passed from the upstream builds.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_5.png?version=1&modificationDate=1461329608000&api=v2[image,width=794,height=173]#
.. To start a downstream snapshot build Parameterized Trigger plugin is
required.
... Consider a parent job that is configured to load from a RTC
repository workspace. When the build runs, Team Concert Jenkins plugin
creates a snapshot on the build workspace. The snapshot uuid is
available as the build environment property team_scm_snapshotUUID.
... Add a post build action to trigger parametrized build on other
projects.
... [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_6.png?version=2&modificationDate=1461330328000&api=v2[image,width=794,height=293]#
.. Configure a downstream snapshot build
... Create a new job and with a string parameter named
"rtcBuildSnapshot"
... [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_7.png?version=1&modificationDate=1461330685000&api=v2[image,width=793,height=458]#
... Configure Rational Team Concert under Source Control options to
build from a snapshot.
... [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_8.png?version=1&modificationDate=1461330886000&api=v2[image,width=794,height=236]#
.. Now when an upstream build is started and once it is done it will
trigger the downstream build with the UUID of the snapshot created on
the workspace.
.. Note that the change log is not generated and polling is not
supported for load from snapshot as this as an immutable configuration.
. To load the jenkins build workspace using a stream, select "Build
Stream" from the Build Configuration dropdown.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_9.png?version=1&modificationDate=1461331347000&api=v2[image,width=794,height=141]#
.. Click the "Validate" button to verify the build stream exists.
.. This configuration supports building from the current state of the
specified stream.
.. Subsequent builds capture the changes made to the stream since the
previous build.
.. In this configuration change log can be chosen to be generated by
comparing the current build with the previous successful build. By
default this option is unchecked.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_10.png?version=1&modificationDate=1461331478000&api=v2[image,width=794,height=316]#
.. For this configuration the RTC user configured globally or for this
job needs to have permission to attach snapshots to a stream
. In 1.2.0.0 some of the load and accept options that were previously
configurable only in RTC build definitions, can be specified in the
jenkins job configuration. The accept and load options are available for
build configurations other than load using a build definition.
.. The directory on the build machine under which the repository files
will be loaded can be specified.
.. Contents of the load directory can be deleted before reloading
.. Load Policy field, added in 1.2.0.4, can be used to configure the
components to load. You can either specify the components to load or
choose to use a remote load rule file or dynamic load rules, to
determine which components to load.
... Specify which components to load +
.... When specifying components to load you can choose to create folders
for components, in which case the load directory would have folders for
components at the top level and each of these folders will have the
files/folders for that component.
.... You can also choose to exclude some components.
... [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/Specify-Components-To-Load.png?version=1&modificationDate=1512377776000&api=v2[image,height=250]#
... Load components by using a load rule file
... [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/Load-Using-Load-Rule-File.png?version=1&modificationDate=1512376770000&api=v2[image,height=250]#
... Load using dynamic load rules
... [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/Load-Using-Dynamic-Load-Rules.png?version=1&modificationDate=1512377991000&api=v2[image,height=250]#
.. For more details on load rules support and how to configure dynamic
load rules, see the Load Rules Support section.
.. When loading the jenkins build workspace from a RTC repository
workspace, there is an option to configure whether to accept latest
changes before loading. By default, this option is selected.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_job_1200_11.png?version=1&modificationDate=1461332037000&api=v2[image,width=794,height=315]#
.. To add a "Related Artifact" link to a Jenkins build in all the
included work items, select the option "Add Jenkins build link to
accepted work items" option.
.. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/JenkinsBuildStreamAddLinkOption.png?version=1&modificationDate=1556704560000&api=v2[image,height=145]#
. Find the "Build Triggers" section.
. Check the "Poll SCM" check box to poll for incoming changes to the
build workspace.
. Enter a schedule.  Click the help button beside the "Schedule" field
to get help with the syntax.
. Click the "Save" button to save the settings and return to the job
page.

[[TeamConcertPlugin-ConfiguringJenkinsjobforPostBuildDeliver(BuildDefinitionconfigurationonly)]]
=== Configuring Jenkins job for Post Build Deliver (Build Definition configuration only)

. In 1.2.0.3, Post Build Deliver is supported for Build Definition
configuration. The RTC server version should be 6.0.4 or higher.
. Configure the RTC Build Definition with Post Build Deliver
configuration.
. In the Jenkins Freestyle job configuration, add the "RTC Post Build
Deliver" post build action. Select "Fail on Error", if you want  the
build to fail if post build deliver fails.
. Optional : If a Pipeline job is being used, then add the following
snippet before the end of the script to perform post build deliver as
the last step of the build.
. {blank}
+
*PB deliver snippet*

[source,syntaxhighlighter-pre]
----
step([$class: 'RTCPostBuildDeliverPublisher', failOnError: true])
----

[[TeamConcertPlugin-UsingPipelineasSCM]]
=== Using Pipeline as SCM

Team Concert Plugin supports Pipeline as SCM but doesn't support
lightweight checkout. Captured below are some ways to use this pipeline
feature with RTC SCM.

[[TeamConcertPlugin-Whenyouwishtocheckoutthesamecontentthatwasinitiallyloadedinthemasterunderpipeline@scriptfolderbyPipelinescriptfromSCM:]]
==== When you wish to checkout the same content that was initially loaded in the master under pipeline@script folder by Pipeline script from SCM:

Note: In this case, you should set skipDefaultCheckout(true). Otherwise
a checkout will happen everytime an agent directive is used. The
following snippet can be used to skip default checkout

....
options {skipDefaultCheckout()}
....

[[TeamConcertPlugin-Scenario1-Noloadrulesinthesecondcheckout]]
===== Scenario 1 - No load rules in the second checkout

{empty}1. Checkout only the JenkinsFile using minimal load rules. That
is, configure  load rules  in the pipeline job's SCM configuration to
load just the JenkinsFile and nothing else. This can be done for build
definition, repository workspace, snapshot and stream configuration.
This keeps the loading time very minimal in the master.

{empty}2. Inside the JenkinsFile, if you want to checkout on a slave or
master again (not under pipeline@script folder), load the snapshot. For
build definition and personal builds triggered from RTC, checkout the
build definition once again.

....
For Build Definition

node ("slavexyz") {
if ("${env.personalBuild}" == "true") { // This is true when a personal build is started from RTC.
          echo "Checking out the build definition in node"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildDefinition: '<build definition used in Pipeline script from SCM>', customizedSnapshotName: '', value: 'buildDefinition'], overrideGlobal: false timeout: 480'])
      } else { // Otherwise we checkout the snapshot created by the Pipeline Script from SCM's checkout.
          echo "Checking out a snapshot in node"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot']], timeout: 480])
      }
   }
}

For Stream:
node ("slavexyz") {
          echo "Checking out a snapshot in node from stream configuration"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot']], timeout: 480])
} 

For Repository workspace:
node ("slavexyz") {
          echo "Checking out a snapshot in node from snapshot configuration"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot']], timeout: 480])
} 

For snapshot:
node ("slavexyz") {
          echo "Checking out a snapshot in node from snapshot configuration"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot']], timeout: 480])
}
....

 +

[[TeamConcertPlugin-Scenario2-Loadrulesinthesecondcheckout]]
===== Scenario 2 - Load rules in the second checkout

{empty}1. Checkout the JenkinsFile using minimal load rules. That is,
configure  load rules  in the pipeline job's SCM configuration to load
just the JenkinsFile and nothing else. This can be done for build
definition, repository workspace, snapshot and stream configuration.
This keeps the loading time very minimal in the master.

{empty}2. Inside the JenkinsFile, if you want to checkout on a slave or
master again (in a different path), configure different load rules that
will load the content required for the build. Note that you can
parameterize the load rules using Jenkins Job property instead of
directly providing the value.

_Note: For Build definition, personal builds are not supported when load
rules or components to include/exclude are used in Jazz SCM
configuration in the build defintion. Therefore, the following sample
will error out when it sees a personal build_

_Note: You can use load rules or components to exclude. In the sample
below, I am assuming load rules. You can substitute loadPolicy with
useComponenLoadConfig and provide components to exclude._

 +

....
For Build Definition
node ("slavexyz") {
if ("${env.personalBuild}" == "true") { // This is true when a personal build is started from RTC.
         error "Personal builds not supported when using load rules or components to include/exclude"
      } else { // Otherwise we checkout the snapshot created by the Pipeline Script from SCM's checkout.
          echo "Checking out a snapshot in node"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot' loadPolicy: 'useLoadRules', pathToLoadRuleFile: 'Comp1/loadrules/build.loadRule'], timeout: 480])
      }
   }
}

Stream:
node ("slavexyz") {
          echo "Checking out a snapshot in node from stream configuration"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot', loadPolicy: 'useLoadRules', pathToLoadRuleFile: 'Comp1/loadrules/build.loadRule'], timeout: 480])
}

For Repository workspace:
node ("slavexyz") {
          echo "Checking out a snapshot in node from repository workspace configuration"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot', loadPolicy: 'useLoadRules', pathToLoadRuleFile: 'Comp1/loadrules/build.loadRule'], timeout: 480])
} 

For snapshot:
node ("slavexyz") {
          echo "Checking out a snapshot in node from snapshot configuration"
          checkout([$class: 'RTCScm', avoidUsingToolkit: false, buildType: [buildSnapshot: "${env.team_scm_snapshotUUID}", buildSnapshotContext: [snapshotOwnerType: 'none'], currentSnapshotOwnerType: 'none', loadDirectory: '.', value: 'buildSnapshot', loadPolicy: 'useLoadRules', pathToLoadRuleFile: 'Comp1/loadrules/build.loadRule'], timeout: 480])
}
....

[[TeamConcertPlugin-Whenyouwanttoloadnewcontentineveryagent(includingthemaster)aftertheinitialPipelinescriptfromSCMcheckout]]
==== When you want to load new content in every agent (including the master) after the initial Pipeline script from SCM checkout

You can add new checkout steps with different options (like loadrules,
or load directory) as required by directly referencing the build
definition, stream or build workspace in the JenkinsFile, instead of
loading from the last snapshot. If you have a build definition
configuration and the build is triggered from RTC, then multiple
checkouts on the same build definition will reuse the build result
instead of creating new ones. This is different from how it works when
the build is triggered from Jenkins where each checkout step with the
same build definition will still create new build results.

[[TeamConcertPlugin-Considerationswhenusingdeclarativepipeline]]
==== *Considerations when using declarative pipeline*

If you are using declarative pipeline, then every agent directive will
cause a checkout to happen in that agent using the same configuration
asPipeline Script from SCM. When using a build definition, stream or
workspace configuration, this will cause an accept to happen in each of
those cases, leading to different content being loaded in each agent. In
the case of build definition, an additional build result will be created
if the build is triggered from Jenkins. This may or may not be what you
want. If you want to prevent the extra checkout for every agent
directive, add the options directive below the agent directive with
skipDefaultCheckout(true) +
 +

....
options {skipDefaultCheckout(true)}
....

[[TeamConcertPlugin-GeneratingPipelineSnippetforTeamConcertPluginfromSnippetGenerator]]
=== Generating Pipeline Snippet for Team Concert Plugin from Snippet Generator

For pipeline jobs, you can generate the snippet for Team Concert
Plugin's RTCScm using the Pipeline snippet generator.

____
Note : Even if you do not want to override global configuration for Team
Concert Plugin, the snippet generator will create a RTCScm snippet with
values for *serverUri*, *credentialsId*. If you copy this snippet into
your pipeline script, it could create maintenance issues when you intend
to change the global server URI, credentials and build tool kit. If you
intend to use the global settings for RTCScm configuration, then remove
the following attributes in the snippet and then copy it into your
pipeline script.
____

* {blank}
+
....
 serverURI
....
* {blank}
+
....
 credentialsId
....
* {blank}
+
....
 timeout
....
* {blank}
+
....
 buildTool
....
* {blank}
+
....
 overrideGlobal
....

[[TeamConcertPlugin-Master/SlaveConfiguration]]
=== Master/Slave Configuration

 +

Master and slave configurations are supported by this plugin.  See the
Jenkins documentation on
https://wiki.jenkins-ci.org/display/JENKINS/Distributed+builds[distributed
builds] for more information.  The RTC build toolkit home path is
required for the master to be able to test connections and build
artifacts.

Note: If a password file is being used to authenticate with the RTC
server for a particular job, it is unnecessary to copy that file to each
of the slaves.  The master extracts the password from the password file
and passes it to each slave required.

. Navigate to the Jenkins /computer/? page (Jenkins > Manage Jenkins >
Manage Nodes) and click the "New Node" link.
. Enter a name and create a "Node name", select the "Dumb Slave" radio
button and click the "OK" button.
. In the node configuration page, find the "Node Properties" section and
check the "Tool Locations" check box.
. From the list of tool locations, select the build toolkit you want to
define for the node, and set the value in the "Home" field.
..  
[.confluence-embedded-file-wrapper]#image:https://wiki.jenkins.io/download/attachments/66847632/node_configure_build_toolkit.png?version=2&modificationDate=1363368239000&api=v2[image]#

Build toolkits can also be installed automatically on slaves.  And
labels can be used to match build toolkits to slaves.  However, a home
path is still required so the master can test connections and build
artifacts. 
[.confluence-embedded-file-wrapper]#image:https://wiki.jenkins.io/download/attachments/66847632/toolkit_installations.png?version=6&modificationDate=1401898566000&api=v2[image]#

 +

[[TeamConcertPlugin-RTCLog]]
=== RTC Log

You can capture logs from the Team Concert plugin to debug any problems
that you may encounter.

[[TeamConcertPlugin-ConfiguringJavaLogging]]
==== Configuring Java Logging

 +

. Navigate to the Jenkins /log page (Jenkins > Manage Jenkins > System
Log) and click the "Add new log recorder" button.
. Name it something like "RTC Log" and click the "Add" button to add a
logger.
. Enter a logger of "com.ibm.team.build" and set the log level to
"FINER".
. [.confluence-embedded-file-wrapper]#image:https://wiki.jenkins.io/download/attachments/66847632/jenkins_configure_rtc_log.png?version=3&modificationDate=1391180511000&api=v2[image]#
. Click the "Save" button.
. Return to this log if a problem is ever experienced using this
plugin.  The log will help to identify the problem.
. Logging on Slaves
.. On the Slave while messages are logged at level FINER, the logs never
come back.

[[TeamConcertPlugin-Logginginthebuildconsolelog]]
==== Logging in the build console log

. There is support for a debug flag which will result in the debug
output going into a build's console log
. The environment variable "com.ibm.team.build.debug" with the value
"true" will activate the debug logging on a slave.
. To configure on a single Slave
.. Jenkins > Manage Jenkins > Manage nodes
.. Hover over the link of the node to configure. Choose Configure from
the popup context menu
.. In the Node properties section, select and check the Environment
variables checkbox
.. Click the Add button beside the List of key value pairs.
.. Supply "com.ibm.team.build.debug" as the name and "true" as the value
.. Click the Save button.
. Alternately to configure on the Master and all Slaves
.. Jenkins > Manage Jenkins > Configure System
.. In the Global Properties section, select and check the Environment
variables checkbox
.. Click the Add button beside the List of key value pairs.
.. Supply "com.ibm.team.build.debug" as the name and "true" as the value
.. Click the Save button.
. Alternatively, you can add com.ibm.team.build.debug as a parameter to
the Job and set its value to true.

The debug flag currently only logs information relating to the class
loader setup. The rest of the logic should not be affected by running on
a Master or a Slave so if you need those logs, consider running on the
Master to get the detailed logs.

[[TeamConcertPlugin-Loggingtheversionofbuildtoolkit]]
==== Logging the version of build toolkit

If you have turned on the variable "com.ibm.team.build.debug", either
through the environment variables or as a Job parameter, then the
version of build toolkit used in the master and slave for that build
will appear in the build log.

You should see messages such as the following in the build log.

[source,console-output]
----
Version of build toolkit "<buildtoolkit-name>" on master is "6.0.4".
Version of build toolkit "<buildtoolkit-name>" on "<slave-name>" is "6.0.4".
----

[[TeamConcertPlugin-CollectingMetronomeLogsfordifferentbuildconfigurations]]
==== Collecting Metronome Logs for different build configurations

Build Definition

. Add the following build property to the build definition.
.. Name - team.build.reportStatistics 
.. Value - true
. Open the build definition editor in RTC Eclipse or RTC Web UI, click
Properties tab and add the property.
. From the Jenkins console, run a build.
. Open the build result associated with the Jenkins build.
. Click the Logs tab.
. You should see two files statistics-<timestamp>.log and
statisticsData-<timestamp>.log

Repository Wokspace, Stream and snapshot

. Add the following String Job property to the Jenkins job. +
.. Name - team.build.reportStatistics 
.. Value - true
. From the Jenkins console, run a build.
. In the machine that hosts the Jenkins master, go to <jenkins config
dir>/jobs/<jobname>/builds/<build number>/teamconcert/diagnostics
. You should see two files statistics-<timestamp>.log and
statisticsData-<timestamp>.log

[[TeamConcertPlugin-RTCrelatedEnvironmentVariablesavailabletotheBuild]]
=== RTC related Environment Variables available to the Build

The following environment variables are available to the build after
Rational Team Concert source control step is completed.

[cols=",",options="header",]
|===
|property |description
|team_scm_changesAccepted |The number of changes accepted or discared
during the build.

|team_scm_snapshotUUID |UUID of the snapshot created after accepting
changes. Not set if no snapshot was created.

|team_scm_workspaceUUID |The UUID of the Repository workspace used in
the build. Only set if the build is using a build definition.

|buildResultUUID |UUID of the build result. Only set if the build is
using a build definition

|RTCBuildResultUUID |UUID of the build result. Only set if the build is
using a build definition

|requestUUID |UUID of the build request. Only set if the build is using
a build definition.

|buildDefinitionId |UUID of the build definition being used by the
build. Only set if the build is using a build definition.

|repositoryAddress |Address of the RTC repository.

|buildEngineId |Name of the build engine associated with the build
request/result (if there is a build result). An RTC build engine is not
actually running, but some ant tasks need the engine id.

|buildEngineHostName |Host name of the Jenkins master or slave that the
build is running on.

|buildRequesterUserId |User id of the RTC user that requested the build
be started. Only set if the build is using a build definition

|personalBuild |True if the build is a personal build (requested from
RTC), otherwise, not set

|rtcTempRepoWorkspaceName |The name of the temporary Repository
Workspace created during a build using Stream configuration

|rtcTempRepoWorkspaceUUID |The UUID of the temporary Repository
Workspace created during a build using Stream configuration
|===

Apart from these built-in properties, when using Build definition
configuration, all the build properties set in the build definition and
potentially modified when requesting the build will be available as
environment variables in the Jenkins build  after the Team Concert
plugin runs.

 +

[[TeamConcertPlugin-AccessingRTCBuildpropertiesinaFreestylejob]]
=== Accessing RTC Build properties in a Freestyle job

In a freestyle job, after Team Concert Plugin completes, you can access
any built-in property ( for all configurations) or user defined build
property (only for Build definition configuration) using the following
syntax: +
*Windows*

%<propertyname>%

Unix

$propertyname

[[TeamConcertPlugin-AccessingRTCBuildpropertiesinaPipelinejobbeforecheckoutstepruns(onlyforBuilddefinitionconfiguration)]]
=== Accessing RTC Build properties in a Pipeline job before checkout step runs (only for Build definition configuration)

When using build definition configuration, you can access build
properties set in the RTC build result in the pipeline build even before
the checkout step runs. These could be built-in properties set in the
build result or user defined RTC build properties. In both cases, you
have to create a String parameter in the Jenkins job with the same name
as the RTC build property (built-in or user defined). The actual value
will be set by the RTC build result that starts the Jenkins build. You
can supply different values to the user defined RTC build properties
when requesting the RTC build.

The following built-in properties are available to the Jenkins build
even before the checkout step runs.

[cols=",",options="header",]
|===
|property |description
|buildResultUUID |UUID of the build result. Only set if the build is
using a build definition

|requestUUID |UUID of the build request. Only set if the build is using
a build definition.

|buildDefinitionId |UUID of the build definition being used by the
build. Only set if the build is using a build definition.

|repositoryAddress |Address of the RTC repository.

|buildEngineId |Name of the build engine associated with the build
request/result (if there is a build result). An RTC build engine is not
actually running, but some ant tasks need the engine id.

|buildEngineHostName |Host name of the Jenkins master or slave that the
build is running on.

|buildRequesterUserId |User id of the RTC user that requested the build
be started. Only set if the build is using a build definition

|personalBuild |True if the build is a personal build (requested from
RTC), otherwise, not set
|===

For instance, consider the scenario where you want to know if the RTC
build result that started this pipeline build is a personal build or
not.

. First create a Job parameter "personalBuild" type is String in the
Jenkins pipeline job and  set the default value to false.

[[TeamConcertPlugin-]]
=== [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/2-CreateNewStringParameterForPersonalBuild.png?version=1&modificationDate=1559037996000&api=v2[image,height=250]#

{empty}2. Request a personal build in the RTC build definition
associated with the Jenkins job.

{empty}3. In your pipeline script, you can check whether the RTC build
is a personal build or not as follows

....
if ("${env.personalBuild}" == "true") {
   // Do something } else {   // Do something else}
}
// or

if ("${personalBuild}" == "true") {
   // Do something } else {   // Do something else}
}
....

{empty}4. To access the buildRequesterUserId property in your script,
define a new String parameter called "buildRequesterUserId" to the
Jenkins job and set the default value to an empty string.

{empty}5. Back in your pipeline script, you can access the property as

....
"${env.buildRequesterUserId}"
// or
"${buildRequesterUserId}"
....

____
Note : This is different from accessing personalBuild property of env
object after the checkout step runs. In that case, the personalBuild
property was repopulated by the checkout step and can be accessed only
through the $\{env} variable.  Here, the property is set by the RTC when
starting the Jenkins build.

 +
____

[[TeamConcertPlugin-AccessingRTCEnvironmentVariablesinaPipelineJobaftercheckoutstepruns]]
=== Accessing RTC Environment Variables in a Pipeline Job after checkout step runs

_checkout_ step now returns a map that is populated by Team Concert
plugin. For instance, you can do something like the following to get the
required values into scmvars variable and access them using the syntax
"$\{scmvars.<rtc environment variable>}". For a list of built-in
properties exported to the environment, see
https://wiki.jenkins.io/display/JENKINS/Team+Concert+Plugin[this
section]

*checkoutstep*

[source,syntaxhighlighter-pre]
----
def scmvars = checkout([$class: 'RTCScm'...])
----

Available in plugin version 1.2.0.5 and above and Jenkins 2.60 and above
with workflow-cps 2.40 and above. With the fix from
https://issues.jenkins-ci.org/browse/JENKINS-26100[Issue 26100],

[[TeamConcertPlugin-Note1:]]
==== Note 1:

Even if you are not using the latest version of Team Concert Plugin,
with workflow-cps 2.40, the env object is repopulated every time the
checkout step runs. See
https://issues.jenkins-ci.org/browse/JENKINS-42499[JENKINS-42499] and
this Jenkins developers
https://groups.google.com/forum/#!msg/jenkinsci-dev/FM_Nx_K_v9g/4BzWXd3cAgAJ[forum
post].

Therefore, the issue reported in
https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=370979[Defect
370979 - Environment variables for snapshot, build result UUID are null
if env object is accessed before running teamconcert checkout step, in a
pipeline script]  and the issue reported in this jazz.net
https://jazz.net/forum/questions/236515/team_scm_snapshotuuid-environment-variable-overwritten-when-loading-jenkins-pipeline-library[forum
post] are not seen anymore. As an example, after every checkout, you can
save the snapshot UUID value into a separate variable as follows

[source,syntaxhighlighter-pre]
----
echo "${env.BUILD_NUMBER}"

 node {
   checkout([$class: 'RTCScm'...])
   // At this point, env contains RTC related environment variables from the first checkout
   def snapshotUUID1 = "${env.team_scm_snapshotUUID}"
   echo "${snapshotUUID1}"

   checkout([$class: 'RTCScm' ....])
   // At this point, env contains RTC related environment variables from the second checkout. The environment variables contributed by the first checkout are overwritten.
   def snapshotUUID2 = "${env.team_scm_snapshotUUID}"
   echo "${snapshotUUID2}"
 }
----

[[TeamConcertPlugin-Note2:]]
==== Note 2:

If you are using workflow-cps < 2.40, follow the workaround mentioned
below.

In a pipeline job the environment variables published by the Team
Concert Jenkins plugin is null if the env object is accessed once before
the RTC SCM checkout step. For instance, the following script would
return the UUID of the snapshot published by the Team Concert plugin.

[source,syntaxhighlighter-pre]
----
node('master') {
    // run teamconcert scm step
    echo "${env.team_scm_snapshotUUID}"
 }
----

But in the script given below the env object is accessed once before
running the checkout step and hence accessing the snapshot UUID from the
env object returns null

[source,syntaxhighlighter-pre]
----
echo "${env.BUILD_NUMBER}"
node('master') {
    // run teamconcert scm step
    echo "${env.team_scm_snapshotUUID}"
 }
----

Though the Team Concert plugin publishes the environment variables when
checkout is invoked, in pipeline scripts the env object once constructed
is not refreshed with any of the environment variables, published later.

If you run into issues accessing the environment variables published by
the Team Concert plugin, the suggested work around is to access the
RTCBuildResultAction object that is added to the build by the Team
Concert plugin. The following code returns the build properties stored
in RTCBuildResultAction object. This can be used in a pipeline script to
obtain snapshot UUID.

[source,syntaxhighlighter-pre]
----
def action = currentBuild.build().getAction(com.ibm.team.build.internal.hjplugin.RTCBuildResultAction.class)
def buildProps = action.getBuildProperties()
println(buildProps['team_scm_snapshotUUID'])
----

Please note that if you invoke RTC SCM multiple times, then there will
be that many RTCBuildResultActions in the build. Therefore,
currentBuild.build().getActions(com.ibm.team.build.internal.hjplugin.RTCBuildResultAction.class)
should be used. The action added by the last invocation of RTC SCM
should be available at the end of the list. For instance, if there are
two RTCScm checkouts, the second RTCBuildResultAction can be accessed as
follows.

 +

[source,syntaxhighlighter-pre]
----
def actions = currentBuild.build().getActions(com.ibm.team.build.internal.hjplugin.RTCBuildResultAction.class)
def buildProps = actions.get(1).getBuildProperties()
println(buildProps['team_scm_snapshotUUID'])
----

[[TeamConcertPlugin-WrappingthecodeinaGlobalSharedLibrary]]
==== Wrapping the code in a Global Shared Library

The above code cannot be directly used in a pipeline script. You can
wrap this code inside a method and add it to a Global Shared Library.
You can then call the method from your pipeline script.

If you are already using a Global Shared Library in your environment,
add the following code in a file called rtcutils.groovy and place the
file under the *vars* directory,

[source,syntaxhighlighter-pre]
----
 def getSnapshotUUID(actionNum) { // The n'th RTCBuildResultAction.
    def actions = currentBuild.build().getActions(com.ibm.team.build.internal.hjplugin.RTCBuildResultAction.class)
    if (actions != null && actions.size() > 0 && actionNum > 0 && actionNum <= actions.size()) {
        def buildProps = actions.get(actionNum-1).getBuildProperties()
        return (buildProps['team_scm_snapshotUUID'])
    } 
    return null
}
----

Then, in your pipeline script, you can write the following to get the
snapshotUUID of the checkout step.

[source,syntaxhighlighter-pre]
----
@Library('your-shared-library')_

node {
   checkout([$class: 'RTCScm'...])

   def snapshotUUID = rtcutils.getSnapshotUUID(2) // pass 2 if the shared library is fetched from RTC, otherwise pass 1
   echo "${snapshotUUID}"
}
----

 +

If you don't have Global Shared Library in your environment, consult
https://jenkins.io/doc/book/pipeline/shared-libraries/[https://jenkins.io/doc/book/pipeline/shared-libraries]
on how to create and access a shared library in your pipeline script.
Note that if you use RTC for hosting the Global Shared Library, then
there will be a RTCBuildResultAction added to the build at the point
where the library is brought into the pipeline script.

[[TeamConcertPlugin-SymbolicLinksupport]]
=== Symbolic Link support

RTC support for symbolic links requires one or two additional libraries
(.dll/.so files).

. RTC file system natives
. Eclipse file system natives

The reason is Java 6 and earlier doesn't have support for
creating/looking at properties of symbolic links. Java 7 has symbolic
link support that works on linux, but on Windows there are some
limitations when creating links (if the target has not yet been created
the type is defaulted to file which is not good if its a directory). If
you are running Linux and can use Java 7 you only need the Eclipse
natives. Otherwise, you will need both the RTC and Eclipse natives.

In the Build engine directory (<your RTC build install
directory>\buildengine\eclipse\plugins), look for (or equivalent jars
for your platform/release).

. `+com.ibm.team.filesystem.client_3.1.600.v20130415_0257.jar+` (RTC
file system natives)
. `+org.eclipse.core.filesystem.win32.x86_1.1.201.R36x_v20100727-0745.jar+`
(Eclipse file system natives)

From the com.ibm.team.filesystem.client jar you want to extract
`+winfsnatives.dll+` (`+libfsnatives.so+` on linux). Take all the
.dll/.so files from the org.eclipse.core.filesystem jar. Place them
directly in a directory (eg. c:\natives\winfsnatives.dll).

When you start Jenkins, we need to tell java about the directory so that
it can load the libraries from it. To this, you can add the directory to
the search path. +
Change the `+PATH+` variable on Windows or the `+LD_LIBRARY_PATH+`
variable on linux prior to starting Jenkins. Alternatively, you can also
specify it when starting Java through the `+-Djava.library.path+`
setting. +
eg.
`+java -Djava.library.path="c:\natives;%Path%" -jar jenkins-1.509.1.war+`

If you are running on Windows, you need to be sure that you have
permission to create symbolic links. The
https://jazz.net/library/article/970/[Symbolic links article] in the
jazz.net library describes how.

Note: If you are running your jenkins builds on slaves and the symbolic
links fail to load, then the native libraries should be included in the
JVM library path of slaves too.

[[TeamConcertPlugin-LoadRulesSupport]]
=== Load Rules Support

. When a jenkins build is configured with an RTC build definition, the
component load rules specified in the RTC build definition, if any, will
be applied when loading the jenkins build
workspace. https://www.ibm.com/support/knowledgecenter/SSCP65_6.0.3/com.ibm.team.build.doc/topics/r_scm_build_loadrules.html[Component
load rules in builds] describes how to specify load rules in a build
definition.
. When a jenkins build is configured with an RTC repository workspace,
stream, or snapshot load rules can be specified by setting the load
policy field to "Load components by using a load rule file".
. [.confluence-embedded-file-wrapper .confluence-embedded-manual-size]#image:https://wiki.jenkins.io/download/attachments/66847632/Load-Using-Load-Rule-File.png?version=1&modificationDate=1512376770000&api=v2[image,height=250]#
. To configure load policy in a pipeline build, set the "loadPolicy"
field to one of - "useComponentLoadConfig", "useLoadRules", or
"useDynamicLoadRules".
.. When loadPolicy is set to useComponentLoadConfig, you can either
choose to load all components or exclude some components by setting the
value for "componentLoadConfig" to either "loadAllComponents" or
"excludeSomeComponents".
. The load policy field for RTC build definition can be set only using
the 6.0.5 RTC client.
. Component load rules can also be specified through dynamic load rules
extension. For more details refer
https://jazz.net/wiki/bin/view/Main/DynamicLoadRulesJenkinsPlugin[DynamicLoadRulesJenkinsPlugin].
Dynamic load rules feature is supported across all build configurations
- build definition, repository workspace, stream, and snapshot.
. In build definition configuration, when load rules are configured in
the build definition and dynamic load rules are also provided, dynamic
load rules take precedence over the component load rules.
. Note that the till 1.2.0.4 the behavior of load rules in Jenkins
builds, when using the component load rules specified in RTC build
definition or the load rules generated by the dynamic load rules
extension, is different from how eclipse client enforces the load rules.
Say, you have a load rules file that loads some but not all of the
components in a workspace. This load rules file when used to load a
workspace in the eclipse client, will result in loading of only those
components specified in the load rules file. When the same load rules
file is configured in an RTC build definition, all components from the
workspace, including those not specified in the load rules file, are
loaded; those components for which load rules are specified are loaded
according to the specified load rules, all the other components are
loaded as is. [.ph .cmd]#The [.ph .uicontrol]#Components to exclude#
option, in the RTC build definition can be used to restrict which
components are loaded during the build - for more details refer
https://www.ibm.com/support/knowledgecenter/SSCP65_6.0.3/com.ibm.team.build.doc/topics/tcreatebuilddefinition.html[Creating
RTC build definitions].#
. [.ph .cmd]##From 1.2.0.4 the behavior of load rules in Jenkins builds
is at par with RTC SCM. So, only those components for which load rules
are specified will be loaded, according to those rules; all the other
components for which load rules are not specified will not be loaded. To
maintain backward compatibility in Jenkins builds configured with an RTC
build definition, old load rules behavior will be enforced unless the
load policy field in the build definition is set to use load
rules. ##[.ph .cmd]## ##[.ph .cmd]# +
#

[[TeamConcertPlugin-Knownlimitations:]]
== Known limitations:

. In the version 1.2.0.0, polling is not supported for stream and
snapshot build configurations, when "avoid using toolkit on master
(experimental)" is checked.
. In the version 1.2.0.0 temporary workspaces are created to support
loading from a stream and snapshot. Teamconcert plugin deletes the
temporary workspaces when the completes. These temporary workspaces
could be left behind in case of network issue during the build. The
temporary workspaces can be located by searching for workspaces that
starts with the prefix "HJP_".
. In the version 1.1.9.5, validating the connections when "avoid using
toolkit on master (experimental)" is checked is broken. This issue seems
to be do with maven dependencies. The issue is tracked in the work item
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/366894[Error
shown when validating a connection with avoid using toolkit on master
option checked]
. You may need to recycle Jenkins and slaves when updating the Team
Concert plugin to a new version, or when automatically installing a new
build toolkit.
. Following are knows issues with Workflow support
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362360[Deleting
a workflow build does not delete the corresponding RTC build result]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/365198[365198:
[Workflow plugin] Using the groovy script generated by snippet generator
for TeamConcert step in a workflow job throws NPE in RTCScm]. For a
workaround change the generated script
from teamconcert([value:"buildDefinition", buildDefinition:"<>")]
to teamconcert buildType: [value:"buildDefinition",
buildDefinition:"<>"]. For more information on this issue refer to
https://issues.jenkins-ci.org/browse/JENKINS-29711[JENKINS-29711]
. [.ph .cmd]##Using com.ibm.team.build.debug to know the RTC build
toolkit version in a slave for a particular job doesn't work in the
first build processed by the slave. Subsequent build of the job on the
same slave will output the build tooolkit version in use. See
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/461155[461155:
Logging version of build toolkit on the slave doesn't work in the first
build processed by the slave after a slave restart.]##[.ph .cmd]## +
##

[[TeamConcertPlugin-KnownLimitations(withfixesinnewerreleasesofRTC):]]
== Known Limitations (with fixes in newer releases of RTC) :

. Issue with RTC 6.0 build tool kit and load rules. Due to a breaking
change in the RTC 6.0, load rules will not work when using RTC 6.0 build
tool kit. **_Fix is available in 6.0 Ifix07 build toolkit
(_**https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362564[work
item 362564)]*_._* Refer to the work item
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/361926[Load
rules is broken with Jenkins plugin and RTC 6.0 build tool kit (361926)]
for more details. If you are using load rules then its recommended to
use the RTC 5.0.2 build tool kit and not RTC 6.0 build tool kit. Note
that this recommendation if only or the version of the RTC build tool
kit and and not for the RTC server. The RTC server can either be 5.0.2
or 6.0, since RTC supports n-1 compatibility (i.e an older client can
connect to a later server) a 5.0.2 version of the build tool kit will
work with RTC 6.0 server.
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/363342[Each build request initiated from RTC creates a buildResultUUID parameter in the Jenkins workflow job].
.. *This issue is fixed in RTC v6.0.1 or higher and in 6.0 ifix04, 5.0.2
ifix12.*
.. For a workaround follow the steps listed below
... In the workflow job configuration page, delete all but one
buildResultUUID parameters.
... Add the following under the <flow-definition> tag in the workflow
job's config.xml +
  <actions> +
    <hudson.model.ParametersDefinitionProperty> +
      <parameterDefinitions> +
        <hudson.model.StringParameterDefinition> +
          <name>buildResultUUID</name> +
          <description>The UUID of the build result in RTC. It is
supplied by builds initiated through RTC. For builds initiated through
Hudson/Jenkins, no value should be supplied.</description> +
          <defaultValue></defaultValue> +
        </hudson.model.StringParameterDefinition> +
      </parameterDefinitions> +
    </hudson.model.ParametersDefinitionProperty> +
  </actions>
... Click Manage Jenkins-> Reload Configuration from Disk. 

[[TeamConcertPlugin-Tutorial]]
== Tutorial

. jazz.net wiki
topic: https://jazz.net/wiki/bin/view/Main/JazzScmWithJenkinsPlugin[Integrating
with Jazz SCM and Builds from Hudson and Jenkins using the Team Concert
Plugin]
. YouTube video: http://www.youtube.com/watch?v=e8XUE5MDtsU[Team Concert
Plugin for Hudson/Jenkins]

[[TeamConcertPlugin-BestPractices]]
== Best Practices

Refer to the best practices document
https://jazz.net/wiki/bin/view/Main/JenkinsBestPractices[here].

[[TeamConcertPlugin-References]]
== References

. Using the Team Concert plugin in Pipeline jobs -
https://jazz.net/wiki/bin/view/Main/DynamicLoadRulesJenkinsPlugin[https://jazz.net/wiki/bin/view/Main/JenkinsWorkflowPluginSupport]
. Using dynamic load rules in Team Concert plugin -
https://jazz.net/wiki/bin/view/Main/DynamicLoadRulesJenkinsPlugin

[[TeamConcertPlugin-Releases]]
== Releases

[[TeamConcertPlugin-1.3.0May1,2019]]
=== 1.3.0 May 1, 2019

____
NOTE: The default behavior of creating "Related artifact" link to a
Jenkins build in all the accepteed work items when using Repository
Workspace or Stream job configuration (introduced by work item 388795)
has changed.

In 1.2.0.5, links will be created in all the accepted work items. In
1.3.0, links will NOT be created in all the accepted work items. There
is a new option "Add Jenkins build link to accepted work items" in the
Job configuration to create these links and is unchecked by default. You
must select the option in the Job configuration to create related
artifact links to a Jenkins build in all the accepted work items. See
work item 461859 for more details.
____

* You can collect metronome information for all build configurations.
See Collecting Metronome Logs section for more details.
** See Work Item 438208: Enhance Team Concert Plugin to collect
metronome log like JBE
* In this release, we have changed the behavior of creating "Related
artifact" links to Jenkins builds in all the accepted work items
originally introduced by work item 388795. You must choose the option
"Add Jenkins build link to accepted work items" in the Jenkins job
configuration to create "Related artifact" links to Jenkins builds in
all the accepted work items.
** See Work Item 461859: Make the "creation of Jenkins build links to
work items in accepted change sets" an opt - in for the users in
Repository Workspace and Stream configuration
* We have fixed an incompatibility with Pipeline jobs wherein messages
from Team Concert Plugin were not printed in the build log.
** See Work Item 478877: Pipeline builds do not output messages from
RTCScm

https://github.com/jenkinsci/teamconcert-plugin/commit/660127fb63b0e411db113278738a7[GitHub
commit - 166456d2a65]

[[TeamConcertPlugin-1.2.0.5June15,2018]]
=== 1.2.0.5 June 15, 2018

____
Important information : The minimum required version of Jenkins is now
1.625.1. After upgrade, it is recommended to check that the Team Concert
plugin (RTCScm) configuration is intact in a few jobs.
____

* In repository workspace and stream build configuration, plugin now
creates links to the Jenkins build in the work items attached to the
change sets
** See WorkItem 388795: In Team Concert Jenkins Plugin, when using build
workspace/stream configuration, create backlinks in included work item
(s) to the Jenkins build
* You can view the version of build toolkit used in master and slave in
the build log by adding com.ibm.team.build.debug = true to the
environment or as a job parameter.
** See WorkItem 449539: [Jenkins] Log the version of build toolkit in
the build log
* You can access the environment variables exported by RTCScm in a
checkout step by assigning it to a groovy variable. +
** WorkItem 446242: Adopt changes to SCM from
[.jira-issue .conf-macro .output-block]#
https://issues.jenkins-ci.org/browse/JENKINS-26100[[.aui-icon .aui-icon-wait .issue-placeholder]##
##JENKINS-26100] - [.summary]#Getting issue details...#
[.aui-lozenge .aui-lozenge-subtle .aui-lozenge-default .issue-placeholder]#STATUS#
#
* Other fixes
** WorkItem 398804: Upgrade parent pom version to 2.x
** WorkItem 448725: Jenkins Build Error: An invalid XML character
(Unicode: 0x10) was found
** WorkItem 458158: Move to Java 7 - upgrade minimum required Jenkins
version to 1.625.1

[[TeamConcertPlugin-1.2.0.4December04,2017]]
=== 1.2.0.4 December 04, 2017

. Support for load rules in Jenkins jobs configured with an RTC
repository workspace, stream, or, snapshot.
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/402834[402834:
[CCM] Support for load rules in the Jenkins Integration Plugin]
. Per checkout dynamic load rules configuration.
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/403461[403461:
Provide an interface in the Jenkins job configuration to check for
dynamic load rules during a run]
. Fix for
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/403254[403254:
Dynamic load rules should have precedence over load rules from Build
Definition]
. getComponentLoadRules method in dynamic load rules extension is
deprecated. Instead dynamic load rules have to be returned by the newly
added getPathToLoadRuleFile method. For more information, see
https://jazz.net/wiki/bin/view/Main/DynamicLoadRulesJenkinsPlugin[DynamicLoadRulesJenkinsPlugin].
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/367019[367019:
[Jenkins-Plugin] Export Build parameter via API]
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/410454[410454:
team_scm_workspaceUUID should be available as an environment variable
for Repository workspace based builds.]

[[TeamConcertPlugin-1.2.0.3Jun16,2017]]
=== 1.2.0.3 Jun 16, 2017

. In Build Definition configuration, Post Build Deliver is supported
when using Rational Team Concert server 6.0.4 or higher.  You can edit
the Build Definition in RTC to include Post Build Deliver configuration.
The configuration information will be used by the plugin to perform post
build deliver.
.. https://jazz.net/jazz/web/projects/Rational%20Team%20Concert%20%28SAFe%29#action=com.ibm.team.workitem.viewWorkItem&id=401131[Improve
the Team Concert Plugin for Jenkins to support post-build deliver for
build definition configuration]

[[TeamConcertPlugin-1.2.0.2Dec6,2016]]
=== 1.2.0.2 Dec 6, 2016

. Support for customising the name of the snapshot created during the
build. You can use Jenkins job parameters and/or environment variables
in the snapshot name. During the build, the parameters will be resolved
to their values to construct the snapshot name.
.. https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=368222[368222:
Support customization of the name of the generated snapshot]
. In Stream configuration, allow check-in and deliver changes using SCM
CLI during the build. The temporary Repository Workspace created for
loading content is now deleted at the end of the build, thus permitting
check-in and deliver operations. The name and UUID of the temporary
Repository Workspace created during the build is available as
'rtcTempRepoWorkspaceName' and 'rtcTempRepoWorkspaceUUID'
.. https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=397202[397202:
Ability to check-in and deliver changes in Stream configuration based
Jenkins build]
. Fixes for the following issues
.. https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=398434[398434:
RepositoryConnection.accept() is taking unusually long time for
workspace and build definition configuration]
.. https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=401392[401392:
Environment variables are missing when loading from Snapshot]
.. https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=405661[405661:
Include workaround for "SQL Duplicate Value exception" when loading from
a snapshot into Team Concert Plugin]

[[TeamConcertPlugin-1.2.0.1Aug16,2016]]
=== 1.2.0.1 Aug 16, 2016

. A String parameter can be provided in the text field for Build
Definition, Repository Workspace or Stream as '$\{paramater_name}'.
https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=324449[Enhancement
324449]https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=324449[-
Jenkins Team concert plugin can support parameters for stream,workspace
and build definition fields]
. A Snapshot can be scoped to a Repository Workspace or Stream.
https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=392790[Task
392790
-]https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=392790[For
build snapshot configuration, provide options to specify the project
area/team area and the owner workspace/stream]
. A Stream can be scoped to a Project Area/Team Area.
https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=391633[Task
391633]https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=391633[-
In the build stream configuration, use the project area/team area value,
if configured, to resolve the stream specified by name]
. Support for configuration level validation instead of validating
individual fields in the Rational Team Concert section.
. Temporary Repository Workspace created for Snapshot and Stream
configuration have a comment of the form "Created by Team Concert Plugin
for job ### in Jenkins server ####". 
https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=388924[Task
388924 - Add a comment to the temporary workspace so that it becomes
easier to identify it as a build workspace]
. Link to the Build Definition, Repository Workspace, Stream used in the
build now appears in the build page.
https://jazz.net/jazz/web/projects/Rational%20Team%20Concert#action=com.ibm.team.workitem.viewWorkItem&id=396340[Task
396340 - Add links to the current configuration used in a build of a
Jenkins job]

[[TeamConcertPlugin-1.2.0.0April22,2016]]
=== 1.2.0.0 April 22, 2016

. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/376827[Enhancement
376827: Support Load Directory and Delete before loading in Jenkins Job]
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/382347[Enhancement
382347: Support RTC BuildDefinition's Accept Options in Jenkins job]
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/366909[Enhancement
366909: Support for loading from a snapshot]
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/375548[Enhancement
375548: Support for loading from stream]
. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/376098[Enhancement
376098: Provide dropdown combo box support for various build
configurations]
. Fixes for the following issues
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/346653[346653:
Jenkins plugin repeatedly resets the "Quiet period"]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/380220[380220:
Rework the Jenkins Plugin messages to display the error trace]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/388284[388284:
Loading a jenkins build workspace with a RTC build definition
configuration fails in Jenkins 1.655]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/383194[383194:
Insufficient error handling or error logging for dynamic load rule
generation] - with this fix, implementations of dynamic load rules can
propagate any exceptions to the teamconcert jenkins plugin.
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/387320[387320:
Validating workspace/connection during job configuration fails if the
job is created under a folder with global credentials scoped to the
folder]

[[TeamConcertPlugin-1.1.9.9January25,2016]]
=== 1.1.9.9 January 25, 2016

. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/338976[Enhancement
338976 Provide a mechanism to generate and input the Load Rules file in
the Jenkins Team Concert plugin]. Dynamic Load Rules feature allows
users to provide load rules for components during the build. For more
information, see
https://jazz.net/wiki/bin/view/Main/DynamicLoadRulesJenkinsPlugin[DynamicLoadRulesJenkinsPlugin]
. Fixes for the following issues
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/377090[377090:
Team Concert plugin for Jenkins triggers builds even there are no real
changes]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/379521[379521:
RTC Jenkins plugin leaving .jazzlock file in the workspace]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/380589[380589:
[Jenkins Integration] Build Toolkit on Slave not found (1.1.9.8)]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/380708[380708:
During delta computation for determining if a build has to be fired,
ignore outgoing changes in the build workspace]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/381693[381693:
When]
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/381693[starting
a Jenkins job from RTC and if the Jazz source control Load directory is
specified as . and delete before loading is checked , build fails]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/381794[381794:
Fix for .jazzlock on abandoning the build (from work item 379521)
doesn't work as expected]

[[TeamConcertPlugin-1.1.9.8December21,2015]]
=== 1.1.9.8 December 21, 2015

[.aui-icon .aui-icon-small .aui-iconfont-info .confluence-information-macro-icon]#
#

Fix for work item 379521- RTC Jenkins plugin leaving .jazzlock file in
the workspace, is not available in 1.1.9.8. The issue has been fixed
1.1.9.9

. Fixes for the following issues
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/375855[Jenkins
Jobs config.xml file broken when upgrading the plugin from 1.1.9.4 to
1.1.9.7]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/375688[Jenkins
Plugin v1.1.9.7 doesn't save the credentials of the global RTC
configuration (Manage Jenkins)]

[[TeamConcertPlugin-1.1.9.6and1.1.9.7October26,2015]]
=== 1.1.9.6 and 1.1.9.7 October 26, 2015

. Fixes for the following issues
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/367265[RTCScmStep
uses serverUri but RTCScm uses serverURI]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/365859[RTC
Jenkins integration for "Recent Changes" does not work properly when a
changeset is related to two workitems]
. Note that 1.1.9.6 and 1.1.9.7 releases are the same, 1.1.9.7 is a
respin of the 1.1.9.6 release to fix the error in the release 1.1.9.6.1

[[TeamConcertPlugin-1.1.9.6.1October26,2015]]
=== 1.1.9.6.1 October 26, 2015

. Invalid plugin release, do not use

[[TeamConcertPlugin-1.1.9.5September21,2015]]
=== 1.1.9.5 September 21, 2015

. Fixed multiple issues with supporting WorkFlow projects
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/363339[Add
visual support for the snippet generator when using TeamConcert step]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362197[Add
setters for optional parameters in the teamconcert step]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/363499[Personal
build for a build definition connected to a workflow job shows up in the
Changes section of the job]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362199[In
the changes section of a workflow job and build, work item numbers,
change sets are not displayed as links]
.. [Expose RTC build information to the environment so that it can be
used in the workflow script |
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/363665
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/363337[Using
snippet generator in the workflow definiton section for Rational Team
Concert plugin generates incorrect groovy script]
.. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362122[Improve
logging in RTC Jenkins plugin - additional logging statements]

[[TeamConcertPlugin-1.1.9.4August04,2015]]
=== 1.1.9.4 August 04, 2015

. Fixed issue with load rules and RTC 5.x build tool kit
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/364161[Remote
load rules not working using Jenkins Team Concert Plugin 1.1.9.3
(364161)]

[[TeamConcertPlugin-1.1.9.3July26,2015]]
=== 1.1.9.3 July 26, 2015

. Implement Quite period support for FreeStyle project
types. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362725[work
item 362725]
. Initial implementation for Workflow
jobs. https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/362121[362121:
RTC Jenkins plugin - workflow support]. Refer to
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/363992[Usage
guide and documentation] for more details.
. Note that this is a initial implementation with some limitations and
issues, refer to the the limitation section for know issues and
workarounds.

[[TeamConcertPlugin-1.1.9.2June11,2015]]
=== 1.1.9.2 June 11, 2015

. Translation update and release for RTC
6.0https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/360197[work
item 360197]

[[TeamConcertPlugin-1.1.9.1March26,2015]]
=== 1.1.9.1 March 26, 2015

. Provide a Group ID in the Team Concert
plugin https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/336266[work
item 336266]
. RTC build plugin for Jenkins repeatedly resets the "Quiet
period" https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/350379[work
item 350379]

[[TeamConcertPlugin-1.1.9October9,2014]]
=== 1.1.9 October 9, 2014

There is a migration impact for this release. See the Migrations section
below.

. When a Jenkins build is deleted, the corresponding RTC build result(s)
(if there are any) are deleted from RTC. The RTC build result will not
be deleted it it is flagged as deletion is not allowed.
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/330249[work
item 330249]
. Improve support for "Multiple SCMs" plugin. You can now specify
multiple RTC SCM configurations referencing different servers (when
builds are started from Jenkins).
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/300164[work
item 300164]
. Support so RTC's Build Definition editor can warn the user if the
Jenkins job doesn't point to that definition
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/276139[work
item 276139]
. Thread's context class loader not reset properly + work around for
unexpected failure to load LogFactory class
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/322272[work
item 322272]

[[TeamConcertPlugin-1.1.8July10,2014]]
=== 1.1.8 July 10, 2014

. Main Jenkins configuration page was not showing the chosen Build
toolkit
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/320832[work
item 320832]
. Add warning to console log when build workspace has components not
visible to the build user
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/203294[work
item 203294]
. When the SCM provider is the "Multiple SCMs" plugin, the detailed
changes for a build does not list the change details
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/323307[work
item 323307]

[[TeamConcertPlugin-1.1.2June11,2014]]
=== 1.1.2 June 11, 2014

. Support for Jenkins credentials has been added which introduced a
dependency on the
https://wiki.jenkins-ci.org/display/JENKINS/Credentials+Plugin[Credentials
plugin]. If a job is already configured to use user ID and password (or
password file) , it will continue to run but these fields are read only.
Any changes will require credentials going forward.  Using the
Credentials plugin offers more flexibility and solves some issues.
.. Multiple credentials can be defined and used in multiple jobs
.. Can use a global user ID and password (or password file) when the RTC
URL is overridden
https://issues.jenkins-ci.org/browse/JENKINS-21537[issue 21537]
.. Improves Security
https://issues.jenkins-ci.org/browse/JENKINS-21038[issue
21038]https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/295009[work
item 295009]
. Work related to starting a build with an RTC build result has been
moved from the Master to the Slave (assuming the job is running on the
Slave).
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/306172[Work
item 306172]
. When builds are started within RTC, the server will manage the
lifecycle of the build result by periodically polling Jenkins to
determine if the build is completed. With RTC 5.0, build definitions
support the boolean property
`+com.ibm.rational.connector.hudson.queueOnly+`. When used in
conjunction with this release of the plugin, the plugin will terminate
the RTC build result when the build completes (just as it does when the
build is started in Jenkins). If a lot of builds are started from within
RTC, this will be more efficient. Requires RTC version 5.0 or later.
https://jazz.net/jazz/resource/itemName/com.ibm.team.workitem.WorkItem/308749[Work
item 308749]
. New option to use rest service calls to communicate with the RTC
Server when performing configuration, polling and build result
management (as opposed to the build toolkit). This means if all the jobs
have this configured and run on slaves, the toolkit classes will not be
loaded on the master. Requires RTC version 5.0 or later.

[[TeamConcertPlugin-1.0.12(andearlier)October23,2013]]
=== 1.0.12 (and earlier) October 23, 2013

. This plugin version does not have any Jenkins specific dependencies.
. To authenticate against an Team Concert server a user id and password
is required. The password can be supplied directly or it can be placed
in a
https://jazz.net/help-dev/clm/topic/com.ibm.team.build.doc/topics/tcreatepasstxt.html[password
file].
. The RTC build toolkit is used perform build related tasks within
Jenkins Master and Slave processes (as opposed to using a command line
client). The RTC related tasks include validating the configuration,
polling and working with the RTC build result as well as performing the
Accept and Checkout phases of the build.
. Support for a simple build workspace
.. Changes are accepted into the build workspace from the stream(s)
referenced by the flow target(s)
.. Snapshot of the workspace is created for a build
.. Change log is created
.. Build workspace is loaded
. Integrated support for build definitions
.. Traceability links from a Jenkins build to an RTC build result,
workspace and snapshot. 
.. Publishes links to work items, change sets and file contents captured
in the snapshot. 
.. Build workspace is identified by the Build definition
.. Changes are accepted into the build workspace from the stream(s)
referenced by the flow target(s)
.. Snapshot of the workspace is created for a build
.. Additional SCM configuration options available in the build
definition
.. RTC Build result is created for a deeper integration with the work
items included in the build
.. Builds (including personal builds) can be started from RTC
.. Environment variables defined in the RTC build definition are
available in the build environment
. RTC build environment variables are available in the build environment
+
[cols=",",]
|===
|property |description

|team_scm_changesAccepted |How many changes were accepted. Not set if
there were no changes.

|team_scm_snapshotUUID |UUID of the snapshot created after accepting
changes. Not set if no snapshot was created.

|RTCBuildResultUUID |UUID of the build result. Only set if the build is
using a build definition

|requestUUID |UUID of the build request. Only set if the build is using
a build definition.

|buildDefinitionId |UUID of the build definition being used by the
build. Only set if the build is using a build definition.

|repositoryAddress |Address of the RTC repository.

|buildEngineId |Name of the build engine associated with the build
request/result (if there is a build result). An RTC build engine is not
actually running, but some ant tasks need the engine id.

|buildEngineHostName |Host name of the Jenkins master or slave that the
build is running on.

|buildRequesterUserId |User id of the RTC user that requested the build
be started. Only set if the build is using a build definition

|personalBuild |True if the build is a personal build (requested from
RTC), otherwise, not set
|===

[[TeamConcertPlugin-Migrations]]
== Migrations

[[TeamConcertPlugin-1.1.9]]
=== 1.1.9

. The environment variable buildResultUUID is a parameter that is
supplied to the Jenkins job when the build started from RTC. It was
sometimes also being updated (contributed by this plugin) even if the
build was started in Jenkins. In order to better support building
multiple projects with the Multiple SCM plugin, the environment variable
will not be updated by this plugin. The build result UUID is still
available from the RTCBuildResultUUID regardless of where the build was
started from.

[[TeamConcertPlugin-1.1.2]]
=== 1.1.2

. Jenkins
https://wiki.jenkins-ci.org/display/JENKINS/Credentials+Plugin[Credentials
Plugin] is now used for storing the user ID and password. For an
existing global configuration and jobs, the user ID and password (or
password file) fields will be read-only. If a job is using a password
file and needs to change a password, the password file contents can be
replaced. Otherwise, to update the password the job will need to start
using credentials. If this not acceptable, the plugin can work in the
old mode by setting the system/environment property:
com.ibm.team.build.credential.edit=true.

[[TeamConcertPlugin-1.0.10]]
=== 1.0.10

. On Linux, a build definition with a load directory starting with "/"
(i.e. "/any/folder") used to be interpreted as a relative path, but is
now correctly interpreted as an absolute path.  So, any build definition
relying on the previous behavior need only prefix the load directory
with a "." (i.e. "./any/folder").

