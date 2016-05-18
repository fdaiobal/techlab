# Lab 11: Applikationstemplates

In diesem Lab zeigen wir auf, wie Templates ganze Infrastrukturen beschreiben und entsprechend mit einem Befehl instanziert werden können.

## Templates

Wie Sie in den vorangegangenen Labs gesehen haben können einfach über die Eingabe unterschiedlicher Befehle Applikationen, Datenbank, Services und deren Konfiguration erstellt und deployed werden.

Dies ist fehleranfällig und etwas mühsamer zu automatisieren.

OpenShift biete dafür das Konzept von Templates in welchen man eine Liste von Resourcen beschrieben kann, die über Parameter parametrisiert werden können. Sie sind also quasi ein Rezept für eine ganze Infrastruktur (bspw. 3 ApplikationsContainer, eine Datenbank mit Persistent Storage)

**Note:** der Clusteradmin kann globale Templates erstellen, welche allen Usern zur Verfügung stehen.

Alle vorhandenen Template anzeigen
```
$ oc get template -n openshift
```

Über die Web Console kann dies via "Add to Project" gemacht werden, dort können Templates dann auch direkt instanziert werden.

Die Templates beschreiben in einem json File, das im Git Repo neben ihrem Code liegen, über eine URL aufgerufen werden oder lokal im Filesystem abgelegt sein kann.


## Aufgabe: LAB11.1: Template instanzieren.

Die einzelnen Schritte die wir in den vorherigen Labs manuell vorgenommen haben, können nun mittels Template in einem "Rutsch" durchgeführt werden. 

```
$ oc new-project [USER]-template
```

Template erstellen

```
$ oc create -f https://raw.githubusercontent.com/appuio/example-spring-boot-helloworld/master/example-spring-boot-template.json
```

Template instanzieren (Ersetzen Sie `[project]` mit `[USER]-template`)

```
$ oc new-app example-spring-boot -pMYSQL_DATASOURCE=jdbc:mysql://mysql.[project].svc.cluster.local/appuio

--> Deploying template example-spring-boot for "example-spring-boot"
     With parameters:
      APPLICATION_DOMAIN=
      DATABASE_SERVICE_NAME=postgresql
      MYSQL_DATABASE_NAME=appuio
      MYSQL_USER=appuio
      MYSQL_PASSWORD=appuio
      MYSQL_DATASOURCE=jdbc:mysql://mysql.techlab-template.svc.cluster.local/appuio
      MYSQL_DRIVER=com.mysql.jdbc.Driver
--> Creating resources ...
    ImageStream "example-spring-boot" created
    DeploymentConfig "example-spring-boot" created
    DeploymentConfig "mysql" created
    Route "example-spring-boot" created
    Service "example-spring-boot" created
    Service "mysql" created
--> Success
    Run 'oc status' to view your app.

```

OpenShift startet danach automatisch einen Build und deployed die Container danach wie spezifiziert.


Als Abschluss dieses Labs können Sie sich noch das Template anschauen
```
https://github.com/appuio/example-spring-boot-helloworld/blob/master/example-spring-boot-template.json
```


**Note:** Bestehende Resourcen können als Template exportiert werden, verwenden Sie dafür den `oc export [ResourceType] --as-myapptemplate`
Bspw. 

```
oc export bc,is,dc,route,service --as-template=example-spring-boot -o json > example-spring-boot-template.json
```

---

**Ende Lab 11**

[<< zurück zur Übersicht] (../README.md)



