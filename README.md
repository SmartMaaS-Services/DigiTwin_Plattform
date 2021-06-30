<h2 align="center">
  <a href="https://smart-maas.eu/en/"><img src="https://github.com/SmartMaaS-Services/Transaction-Context-Manager/blob/main/docs/images/Header.jpeg" alt="Smart MaaS" width="500"></a>
  <br>
      SMART MOBILITY SERVICE PLATFORM
  <br>
  <a href="https://smart-maas.eu/en/"><img src="https://github.com/SmartMaaS-Services/Transaction-Context-Manager/blob/main/docs/images/Logos-Smart-MaaS.png" alt="Smart MaaS" width="250"></a>
  <br>
</h2>

<p align="center">
  <a href="mailto:info@smart-maas.eu">Contact</a> •
  <a href="https://github.com/SmartMaaS-Services/Transaction-Context-Manager/issues">Issues</a> •
  <a href="https://smart-maas.eu/en/">Project Page</a>
</p>


***

<h1 align="center">
  <a>
    Plattform für den DigiTwin Kiel
  </a>
</h1>

***


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



## Installation der DigiTwin API

[Installation DigiTwin API](https://github.com/SmartMaaS-Services/DigiTwin_API)

## Installation der DigiTwin APP

[Installation DigiTwin APP](https://github.com/SmartMaaS-Services/DigiTwin_APP)

## Anmelden an die Plattform

Um mit der Plattform interagieren zu können ist ein Account notwendig den man sich über https://accounts.hauptdomain.tld/sign_up/ einrichten muss.
Wir benötigen 2 System Accounts für den Betrieb des DigiTwin.

- *node-red* für die Datenverarbeitung, dem laden der Daten in die Plattform und dem berechnen der Ergebnisse
- *apidt* als System User für die DigiTwin API damit die REST API Daten aus der Plattform lesen kann.

![Anmeldung](/images/SignUp.png)

Der Administrator berechtigt danach die Accounts im TenantManager für den DigiTwin Tenant.

## Einrichten des Tenant

Die FIWARE Plattform benötigt mindestens einen Tenant.
Unter https://apis.hauptdomain.tld meldet sich der Plattform Administrator an.

![LogIn](/images/LogIn.png)

Und wechselt auf den TAB Teanant.

![TenantManager](/images/TenantManager.png)

Dort wird der neue Tenant erstellt, dier mit dem Namen *DigiTwin*

![Tenant DigiTwin](/images/TenantDigiTwin.png)

Damit wird ein neuer Tenant erstellt und der User node-red zugeordnet der für Node-RED die Erreichbarkeit des Tenants ermöglicht.
Den Token der FIWARE Plattform holt sich Node-RED nun direkt aus dem Flow und verwaltet seinen Token auch über seine Gültigkeit.
Sollte der Token nach der voreingestellten zeit auslaufen kann sich Node-RED diesen sebststängig erneuern.
Somit dürfen nun die Flows aus Node-RED Daten aus dem Teanant DigiTwin lesen und auch frische Daten in diesen Teanant schreiben.

## Datenmodelle und ihre Abhängigkeiten

Das Modell eines DigiTwin besteht aus mehreren Datenmodellen die die zusammen die Dienste einer Mobilitäts Station abbilden können.

Die Services sind dabei dynamisch und unterscheiden sich von Station zu Station

die 3 Datenmodelle MobilityRegion, MobilityStation und MobilityService sind wie im folgenden Bild aufeinander aufgebaut:

![Die Datenmodelle Mobility...](/images/MobilityModelle.png)

Die Datenmodelle Mobility\* sind frei unter [SMARTCITIES/mobility](https://github.com/smart-data-models/incubated/tree/master/SMARTCITIES/mobility) errreichbar.
Die Datenmodelle der MobilityServices sind entsprechend der örtlichen Gegebenheiten aus dem Fundus von [smartdatamodels.org](https://smartdatamodels.org/) zu beziehen.

Als Beispiel ist in dem folgenden Schaubild Ein Parkplatzmodell angebildet:

![Parken](/images/parking.png)

Dieses Modell nutzt die zusätzliche Finktionalität des Bosch Parksensores TPS110 die Bodentemperatur aufzunehmen.
Damit ist der Modellstruktur zusätzlich noch ein WeatherObserved Datenmodell zugeordnet.

Die hier gezeigte Struktur beliefert nun die MobilityServices OffStreetParking und WeatherObserved welches als Info Dienst an der MobilityStation verfügbar ist.

## Die Node-RED Flows für den DigiTwin

Dem Node-RED POD müssen die folgenden Environment Variablen übergeben werden:

|Name|Beschreibung|
|--------|--------|
|ACCOUNTBROKER_PRODUCTION|URL zum ACCOUNT Server|
|ACCOUNTBROKER_STAGING|URL zum ACCOUNT Server|
|CLIENTID_PRODUCTION|OAuth2 ClientID|
|CLIENTID_STAGING|OAuth2 ClientID|
|CLIENTSECRET_STAGING|OAuth2 Client Secret|
|CLIENTSECRET_PRODUCTION|OAuth2 Client Secret|
|CONTEXTBROKER_PRODUCTION|URL zum ContextBroker|
|CONTEXTBROKER_STAGING|URL zum ContextBroker|
|FIWARE_SERVICE_PRODUCTION| Fiware_Service|
|FIWARE_SERVICE_STAGING| Fiware_Service|
|FIWARE_SERVICEPATH_PRODUCTION| Fiware_Service_Path|
|FIWARE_SERVICEPATH_STAGING|Fiware_Service_Path|
|NODE_RED_CREDENTIAL_SECRET_PRODUCTION|
|NODE_RED_CREDENTIAL_SECRET_STAGING|
|NODE_RED_FLOW_USER_PRODUCTION|Der Node Red User|
|NODE_RED_FLOW_USER_STAGING|Der Node Red User|
|NODE_RED_FLOW_USER_SECRET_PRODUCTION|Das Secret des Users|
|NODE_RED_FLOW_USER_SECRET_STAGING|Das Secret des Users|
|FLOW_TOKEN|Das Flow Token|



Damit NODE-RED die Daten für den DigiTwin bereitstellen kann werden verschiedene Flows benötigt:

- Automatisches Update der Token -> /NODE-RED/FlowTokenVerwaltung.json
- Endpunkte DigiTwin -> /NODE-RED/FlowEndpunkteDigiTwin.json 
- Empfange Notifications -> /NODE-RED/FlowEmpfangeNotifications.json
- Parking Device verarbeiten -> /NODE-RED/FlowParkingDevice.json
- ParkingSpot verarbeiten -> /NODE-RED/FlowParkingspotVerarbeitung.json
- ParkingSide verarbeiten -> /NODE-RED/FlowParkingSideVerarbeiten.json
- BodenTemperatur ParkingDevice -> /NODE-RED/FlowBodenTemperaturParkingDevice.json
- insert GTFS Stops -> /NODE-RED/FlowinsertGTFSStops.json
- Umweltbundesamt -> /NODE-RED/FlowUmweltbundesamt.json
- TimeTable erstellen -> /NODE-RED/FlowTimeTableErstellen.json

### Automatisches Update der Token

![Token](/images/AutomatischesUpdateToken.png)

### Endpunkte DigiTwin

![Token](/images/EndpunkteDigiTwin.png)

### Empfange Notifications

![Notifications](/images/EmpfangeNotifications.png)

### Parking Device verarbeiten

![Parking Device](/images/ParkingDevice.png)

### ParkingSpot verarbeiten

![ParkingSpot](/images/ParkingSpotVerarbeiten.png)

### ParkingSide verarbeiten

![ParkingSide](/images/AParkingSideVerarbeiten.png)

### BodenTemperatur ParkingDevice

![BodenTemperatur](/images/BodenTemperaturParkingDevice.png)

### insert GTFS Stops

![GTFS](/images/insertGTFSStops.png)

### Umweltbundesamt

![Umweltbundesamt](/images/Umweltbundesamt.png)

### TimeTable erstellen

![TimeTable](/images/TimeTableErstellen.png)

