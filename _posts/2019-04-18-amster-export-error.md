---
id: 39
title: "Amster export error"
date: 2019-04-18T16:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=41
permalink: /2019/04/18/amster-export-error/
categories:
  - amster
  - kubernetes
  
---

When migrating my ForgeRock Access Management 6.5 within Kubernetes I was getting an export error

```bash
---------------------------------------------------------------------
   EXPORT ERRORS
---------------------------------------------------------------------
Failed to export /tmp/amsterexport/realms/root/Title  : Illegal character in path at index 11: json/users/{user}/devices/2fa/webauthn?_queryFilter=true
```

Here is how I fixed it.

<!-- more -->

## More error details

```bash
amster fram-0.fram.forgerock.svc.cluster.local:8080> export-config --path /tmp/amsterexport --realms / --globalEntities 'DefaultCtsDataStoreProperties' --failOnError true
ERROR java.lang.IllegalArgumentException:
Illegal character in path at index 11: json/users/{user}/devices/2fa/webauthn?_queryFilter=true
        at org.forgerock.amster.org.forgerock.openam.sdk.http.HttpSessionImpl.createRequest (HttpSessionImpl.java:244)
        at org.forgerock.amster.org.forgerock.openam.sdk.http.HttpSessionImpl.createRequest (HttpSessionImpl.java:251)
        at org.forgerock.amster.org.forgerock.openam.sdk.http.HttpSessionImpl.request (HttpSessionImpl.java:202)
        at org.forgerock.amster.org.forgerock.openam.sdk.crest.CrestResourceProviderAsync.queryCollectionWithFilter (CrestResourceProviderAsync.java:412)
        at org.forgerock.amster.org.forgerock.openam.sdk.crest.HttpCrestResourceProvider.queryCollectionWithFilter (HttpCrestResourceProvider.java:357)
        at org.forgerock.amster.org.forgerock.openam.sdk.operations.CrestOperations.queryWithFilter (CrestOperations.java:490)
        at org.forgerock.amster.org.forgerock.openam.sdk.operations.CrestOperations.queryWithFilter (CrestOperations.java:475)
        at org.forgerock.openam.amster.loadster.exporter.EntityWriter.writeCollection (EntityWriter.groovy:85)
        at org.forgerock.openam.amster.loadster.exporter.GenericExporter.exportEntity (GenericExporter.groovy:29)
        at org.forgerock.openam.amster.loadster.exporter.EntityExporter.exportEntity (EntityExporter.groovy:108)
        at org.forgerock.openam.amster.loadster.exporter.EntityExporter.exportRealmEntities (EntityExporter.groovy:101)
        at org.forgerock.openam.amster.loadster.exporter.EntityExporter.exportEntities (EntityExporter.groovy:78)
        at org.forgerock.openam.amster.commands.ExportCommand.execute (ExportCommand.groovy:62)
        at org.forgerock.openam.amster.Main$_addCommandLineWrapping_closure2.doCall (Main.groovy:92)
        at java_lang_Runnable$run.call (Unknown Source)
        at org.forgerock.openam.amster.Main.main (Main.groovy:62)
```

Looking at [Amster export failure](https://bugster.forgerock.org/jira/browse/OPENAM-14049) it says was addressed in an update, so of I went looking to see if I could download it via backstage.

## The Fix

1. I downloaded the latest ForgeRock Access Manager 6.5.1 Evaluation and Amster 6.5.1 files.
1. I placed them within my Docker build directory.
1. I updated my `Dockerfile` to use those new files.

   ```bash
   COPY AM-eval-6.5.1.war /var/tmp/am.war
   COPY Amster-6.5.1.zip /var/tmp/amster.zip
   ```

1. I issued the command to build the new version.

   ```bash
   docker build . -t forgerock/accessmanager:6.5.1.eval
   ```

1. I updated my kubernetes file to use the new version

   ```bash
   containers:
     - name: fram
       image: forgerock/accessmanager:6.5.1.eval
   ```

1. Confirm the current version deployed

   ```bash
   kubectl describe pods fram-0 -n forgerock

   Containers:
    fram:
        Container ID:   docker://1bf75df90ce14d81880f2c45b9feb2dc3f5749558fe26443063d347f52061831
        Image:          forgerock/accessmanager:6.5.0.eval
   ```

1. Apply the new kubernetes file

   ```bash
   kubectl apply -f AccessManager.yaml

   statefulset.apps/fram configured
   service/fram-lb unchanged
   service/fram unchanged
   ```

1. Confirm the new version deployed

   ```bash
   kubectl describe pods fram-0 -n forgerock

   Containers:
    fram:
        Container ID:   docker://123228d82963343e0015008feb2a77a3560f84b3d16bfe726cb28e6646096f1c
        Image:          forgerock/accessmanager:6.5.1.eval
   ```

1. Open ForgeRock Access Manager via a web browser and confirm that it has detected 6.5.1 and perform the upgrade

   **Note:**  Yes I know this should be automated and it will be ;-)

1. Find that there is an error within the Chrome Browser that stops the upgrade via the browser as I am not SSL.

   ![Image failure](/images/2019-04-18-08-28-16.png)

   ```bash
   Uncaught TypeError: Cannot set property 'innerHTML' of null
    at YAHOO.widget.Panel.<anonymous> (options.htm:167)
    at YAHOO.util.CustomEvent.fire (yahoo-dom-event.js:120)
    at YAHOO.widget.Panel.render (container-min.js:59)
    at YAHOO.widget.Panel.render (container-min.js:181)
    at renderUpgradePanel (options.htm:172)
    at Object.handleSuccess [as success] (AjaxUtils.js:120)
    at Object.handleTransactionResponse (connection-min.js:51)
    at connection-min.js:45
   ```

1. Perform the upgrade via the command line.

   ```bash
   java -jar openam-upgrade-tool-14.1.2.3.jar --file sampleupgrade

   Upgrade Failed. Please check the amUpgrade debug file for errors
   ```

   checking the `amUpgrade` log I see

   ```bash
   amUpgrade:04/17/2019 10:20:56:679 PM GMT: Thread[http-nio-8080-exec-2,5,main]: TransactionId[1a90ef2c-363a-45ba-8d0b-2765008252b4-29]
   ERROR: Unable to read directory schema, the schema won't be upgraded
   No Results Returned: The entry ou=am-config does not include a subschemaSubentry attribute
   ```

## Conclusion

Looks like I have an issue with my ForgeRock Directory Service Proxy that I need to resolve before I can complete this task. Also will need to enable SSL within my environment, which was planned to do anyway, just need to bring it forward in my tasks of deployment.