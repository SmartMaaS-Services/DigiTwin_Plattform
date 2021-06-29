# Plattform für den DigiTwin Kiel

Wir nutzen für den DigiTwin Kiel die folgende schon veröffentlichte FIWARE Kubernetes Plattform aus [Paderborn](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware "Zentrale OpenData Plattform").

Daraus haben wir nur die für das Projekt notwendigen Komponenten eingesetzt und uns an die [Anleitung](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/documentation "Dokumentation der Zentrale OpenData Plattform") gehalten:

## Liste der FIWARE Komponenten in diesem Projekt

- [KeyRock](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/keyrock "Dokumentation Keyrock") Account Management
- [Umbrella](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/umbrella "Dokumentation Umbrella") Routing
- [TenantManager](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/tenantmanager "Dokumentation TenantManager") Erweiterung um Tenants in die KeyRoch Organisationen abzulegen.
- [Orion (Version V2)](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/orion "Dokumentation Orion") Der ContextBroker für NGSI V2
- [MongoDB](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/mongo "Dokumentation MongoDB") Datenbank für die JSON Modelle und Context Daten
- [MySQLDB]()
- [Node-RED](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/node-red-on-k8s "Dokumentation Node-RED") Datenverarbeitung Flow basiert als Low Code Variante
- [Platform (APIinf)](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/platform "Dokumentation Platform") Die Notwendigen APIs der Plattform
- [Installation der Plattform und einrichten der Secrets](https://gitlab.com/zentrale-open-data-plattform-paderborn/fiware/installation/-/tree/master/platform-setup)

## Einrichten des Tenant

## Installation der DigiTwin API

## Installation der DigiTwin APP

## Datenmodelle und ihre Abhänjikeiten

Das Modell eines DigiTwin besteht aus mehreren Datenmodellen die die zusammen die Dienste einer Mobilitäts Station abbilden können.

Die Services sind dabei dynamisch und unterscheiden sich von Station zu Station

die 3 Datenmodelle MobilityRegion, MobilityStation und MobilityService sind wie im folgenden Bild aufeinander aufgebaut:

![Die Datenmodelle Mobility...](/images/MobilityModelle.png)

## Die Node-RED Flows für den DigiTwin






