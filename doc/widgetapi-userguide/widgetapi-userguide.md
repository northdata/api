![image alt text](image_0.png)

# North Data Widget API 

Zur Einbindung der Visualisierungen (interaktiven Grafiken) von North Data. 

## Inhaltsverzeichnis

- [Übersicht](#ubersicht)
- [Einbindung von Widgets](#einbindung-von-widgets)
  - [Konfiguration](#konfiguration)
  - [Liste Widget-übergreifender Optionen](#liste-widget-ubergreifender-optionen)
  - [Firmen-Identifikation](#firmen-identifikation)
  - [Autorisierung](#autorisierung)
  - [Abmessungen](#abmessungen)
  - [Navigation / Click Handler](#navigation-click-handler)
  - [Layout von Mehrfach-Grafiken / Item Handler](#layout-von-mehrfach-grafiken-item-handler)
- [Widget-Referenz](#widget-referenz)
  - [Widget-Typ *"history"*](#widget-typ-history)
  - [Widget-Typ *"graph"*](#widget-typ-graph)
  - [Widget-Typ *"pubTable"*](#widget-typ-pubtable)
  - [Widget-Typ "*barChart"*](#widget-typ-barchart)
  - [Widget-Typ "*financials"*](#widget-typ-financials)
  - [Widget-Typ "*sheets"*](#widget-typ-sheets)
  - [Widget-Typ "*drillDown"*](#widget-typ-drilldown)

## Übersicht

Folgende Widgets sind aktuell verfübar:

<table>
  <thead>
  <tr>
    <th>type</th>
    <th>Beschreibung</th>
    <th>Bild</th>
    <th>Beispiel-Seite</th>
  </tr>
  </thead>
  <tr>
    <td>history</td>
    <td>Firmenhistorie (Timeline)</td>
    <td><img alt="" src="image_2.png" style="width: 215px;"></td>
    <td><a href="https://www.northdata.de/widgetTestHistory.html">https://www.northdata.de/widgetTestHistory.html</a> </td>
  </tr>
  <tr>
    <td>graph</td>
    <td>Netzwerk einer Firma oder Person`</td>
    <td><img alt="" src="image_3.png" style="width: 215px"></td>
    <td><a href="https://www.northdata.de/widgetTestGraph.html">https://www.northdata.de/widgetTestGraph.html</a> </td>
  </tr>
  <tr>
    <td>pubTable</td>
    <td>Publikationen einer Firma oder Person</td>
    <td><img alt="" src="image_4.png" style="width: 215px"></td>
    <td><a href="https://www.northdata.de/widgetTestPubTable.html">https://www.northdata.de/widgetTestPubTable.html</a>< </td>
  </tr>
  <tr>
    <td>barChart</td>
    <td>Balkendiagramm für finanzielle Kennzahlen</td>
    <td><img alt="" src="wBarChart.png" style="width: 215px"></td>
    <td><a href="https://www.northdata.de/widgetTestBarChart.html">https://www.northdata.de/widgetTestBarChart.html</a></td>
  </tr>
  <tr>
    <td>sheet</td>
    <td>Tabellarische Darstellung der Bilanz oder GuV</td>
    <td><img alt="" src="image_6.png" style="width: 215px"></td>
    <td><a href="https://www.northdata.de/widgetTestSheet.html">https://www.northdata.de/widgetTestSheet.html</a></td>
  </tr>
  <tr>
    <td>financials</td>
    <td>Tabelle mit finanziellen Kennzahlen</td>
    <td><img alt="" src="image_7.png" style="width: 215px"></td>
    <td><a href="https://www.northdata.de/widgetTestFinancials.html">https://www.northdata.de/widgetTestFinancials.html</a> </td>
  </tr>
  <tr>
    <td>drillDown</td>
    <td>Darstellung der Größe der wichtigsten Jahresabschlussposten als unterteiltes Rechteck</td>
    <td><img alt="" src="image_8.png" style="width: 215px"></td>
    <td><a href="https://www.northdata.de/widgetTestDrillDown.html">https://www.northdata.de/widgetTestDrillDown.html</a> </td>
  </tr>
</table>

### Künftige Widgets

Folgende Widgets werden in Kürze verfügbar sein:

| Beschreibung | Status |
| --- | --- |
| Firmen-Stammdaten | Abgeschlossen, Dokumentation fehlt |
| Personen-Stammdaten | Abgeschlossen, Dokumentation fehlt |
| Firmensuche | Abgeschlossen, Dokumentation fehlt |
| Powersuche | In Entwicklung |
| Interaktive Karte | In Entwicklung |

## Einbindung von Widgets

Zur Einbindung werden benötigt: 1. externe Scripts, 2. externes Stylesheet, 3. Initialisierung/Konfigurationscode:

1. Externe Scripts:

```html 
<script src="[https://d3js.org/d3.v3.min.js](https://d3js.org/d3.v3.min.js)" charset="utf-8"></script>
<script src="[https://www.northdata.de/js/viz.min.js](https://www.biz-q.com/js/viz.min.js)"></script>
```

D3 ist eine verbreitete Open-Source-Visualisierungsbibliothek ([https://d3js.org/](https://d3js.org/)). Wenn D3 bereits eingebunden wurde, kann der erste Script-Tag entfallen.

2. Externes Stylesheet:

<link href="https://www.northdata.de/viz.css?color1=#ff0000&color2=#00ff00&color3=#0000ff" rel="stylesheet">


Über die drei Parameter können Farben in der Netzwerkdarstellung ausgewählt werden:  Farbe des primären Knoten (color1), Farbe der anderen Knoten (color2), Farbe der anderen Knoten bei Mouse-Hover (color3)

3. Initialisierung und Konfiguration per Javascript

```html
<figure id="graph" data-layout="graph" data-name="Weiler Werkzeugmaschinen GmbH" data-address="Emskirchen" data-min-height="300"></figure>
<script>
   new NorthData.Widget( document.getElementById("graph"), { /*options*/ }   )
</script>
```

Für jede Visualisierung muss ein Widget-Objekt angelegt werden, mit einem HTML-Element als "Root". 

### Konfiguration

Die Optionen können entweder als zweiter Parameter in Javascript übergeben werden, oder als Data-Attributes am HTML-Element. 

Beispiel Javascript: 

```html
<script>
   new NorthData.Widget( document.getElementById("myFigure"), { 
    layout: "history",
    apiKey: "abcd-wxyz",
    name: "Weiler Werkzeugmaschinen GmbH",
    address: "Emskirchen",
    minHeight: 300,
    success: function() { console.info("ok") },
    error: function(error) { console.error(error) }
  })
</script>
```
Beispiel HTML-Element:

```html
<figure id="myFigure" data-layout="graph" data-name="Weiler Werkzeugmaschinen GmbH" data-address="Emskirchen" data-min-height="300" data-api-key="abcd-wxyz"></figure>
```

**Wichtig**: HTML-Data-Attributes werden mit Bindestrichen formatiert, nicht als Camel-Case. Also `minHeight` in Javascript entspricht `data-min-height` in HTML.

Javascript-Optionen und HTML-Data-Attributen können auch frei kombiniert werden. (Die Javascript-Optionen überschreiben die Attribute.) 

Bei Javascript-Optionen vom Typ Zahl oder String können auch Funktionen verwendet werden, die passende Werte zurückliefern.

### Liste Widget-übergreifender Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>apiKey</td>
    <td>String</td>
    <td>API Key (siehe Autorisierung)</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung:
graph: Netzwerk der Person bzw. der Firma
history: Historie (Timeline)
barChart: Balkendiagramm
sheet: tabellarische Darstellung der Bilanz/GuV
drillDown: Darstellung der Größe der wichtigsten Jahresabschlussposten als unterteiltes Rechteck
pubTable: Tabelle der neuesten Publikationen
dendrogram: Horizontale Baumdarstellung (auf northdata.de verwendet zur Darstellung der Publikationsgliederung)</td>
  </tr>
  <tr>
    <td>domain</td>
    <td>String</td>
    <td>Typ des dargestellten Objekts (optional)
<br/>company: Firma (default)
<br/>person: Person
<br/>hrb: Handelsregisterbekanntmachung</td>
  </tr>
  <tr>
    <td>name</td>
    <td>String</td>
    <td>Name des Objekts, z.B. Firmenname </td>
  </tr>
  <tr>
    <td>address</td>
    <td>String</td>
    <td>optional, aber verbessert die Wahrscheinlichkeit eines eindeutigen Suchergebnis
Bsp: 
Strasse Nr. 123, 12345 Stadt
oder auch nur 
Hamburg</td>
  </tr>
  <tr>
    <td>registerCity</td>
    <td>String</td>
    <td>Sitz des Amtsgericht, optional, aber verbessert die Wahrscheinlichkeit eines eindeutigen Suchergebnis</td>
  </tr>
  <tr>
    <td>registerId</td>
    <td>String</td>
    <td>Handelsregister in der Form 
HRA 12345</td>
  </tr>
  <tr>
    <td>minHeight</td>
    <td>Zahl</td>
    <td>minimale Höhe der Darstellung in Pixel</td>
  </tr>
  <tr>
    <td>maxHeight</td>
    <td>Zahl</td>
    <td>maximale Höhe in Pixel</td>
  </tr>
  <tr>
    <td>ratio</td>
    <td>Zahl</td>
    <td>fixes Verhältnis von Höhe zu Breite</td>
  </tr>
  <tr>
    <td>success</td>
    <td>Funktion</td>
    <td>wird aufgerufen, wenn das Widget erfolgreich erstellt wurde</td>
  </tr>
  <tr>
    <td>error</td>
    <td>Funktion</td>
    <td>wird im Fehler-Fall aufgerufen. Erhält als Argument eine Fehlermeldung oder ein Error-Objekt</td>
  </tr>
</table>


### Firmen-Identifikation

Die Firma wird über die Konfigurationsoptionen `name`, `address`, `registerCity` und `registerName` identifiziert.

1. Es muss mindestens die Kombination Name/Ort oder Amtsgericht/HR angegeben werden – idealerweise beide.

2. Verwenden Sie als Adresse einfach nur den Ort. Dies ist absolut ausreichend zur Identifikation: kleine Fehler der Adresse (Hausnummern!) können dazu führen, dass eine Firma nicht identifiziert wird.

3. Die Identifikation der Firma ist ansonsten weitgehend tolerant gegenüber verschiedenen Schreibweisen der Firma, früheren Namen/Orten, unterschiedlichen Schreibweisen des Orts, alten Formen des HR-Zeichen, etc.

### Autorisierung

Die Autorisierung erfolgt per API-Key (wie beim Daten-API), oder über die Freischaltung der Domain, wo die Widgets eingebunden werden. Um einen API-Key zu erhalten bzw. eine Domain freischalten zu lassen, wenden Sie sich bitte an North Data.

Der API-Key sollte nur beim Testen verwendet werden. Beim Testen ist der API Key meistens erforderlich, da die beim Testen verwendete Domain nicht freigeschaltet ist. 

Im Produktionsbetrieb sollte der API-Key nicht verwendet werden, da er im HTML-Source-Code sichtbar ist.

### Abmessungen

Widgets nutzen immer die vorgegebene Breite des HTML-Elements. Die Höhe passen sie aber zur optimalen Darstellung dynamisch an. 

### Navigation / Click Handler

Interaktivität und User Engagement der Netzwerk- und Historiendarstellung werden maßgeblich verbessert, wenn die Knoten anklickbar sind und weitere Seiten verlinken. Dazu können Click Event Handler über die Optionen `companyClick`, `personClick` und `publicationClick` definiert werden:

```javascript
function handleCompanyClick(node) {
  var hr = node.registerCity + " " + node.registerId  
  var url = "/companies.html?hr=" + encodeURIComponent(hr)
  window.location.assign(url)
}

new NorthData.Widget( myFigure, { 
  companyClick: handleCompanyClick,
  // personClick: …,
  // publicationClick: ...
})
```

Die  Optionen `companyClick`, `personClick` und `publicationClick` sollten nur gesetzt werden, wenn der Click tatsächlich behandlelt wird! (Grund: korrekte Darstellung des Mauszeigers.)

Die Attribute des der Funktion übergebenen Knoten (im Beispiel: `node`) sind in der folgenden Tabelle zusammengestellt.

<table>
  <tr>
    <td>Attribut</td>
    <td>Erläuterung</td>
  </tr>
  <tr>
    <td>name</td>
    <td>Name (z.B. Name der Firma oder der Person)</td>
  </tr>
  <tr>
    <td>text</td>
    <td>Beschriftung des Knotens (ggf. gekürzter Name)</td>
  </tr>
  <tr>
    <td>address</td>
    <td>Adresse (Firmen, Personen - falls bekannt)</td>
  </tr>
  <tr>
    <td>registerName</td>
    <td>Sitz des Amtsgericht (nur Firmen)</td>
  </tr>
  <tr>
    <td>registerId</td>
    <td>Handelsregister in der Form 
HRA 12345 (nur Firmen)</td>
  </tr>
  <tr>
    <td>firstName</td>
    <td>bei Personen</td>
  </tr>
  <tr>
    <td>lastName</td>
    <td>bei Personen</td>
  </tr>
  <tr>
    <td>birthDate</td>
    <td>bei Personen (falls bekannt)</td>
  </tr>
  <tr>
    <td>id</td>
    <td>interne North Data ID</td>
  </tr>
  <tr>
    <td>url</td>
    <td>bei Publikationen (in der Regel keine öffentliche URL, da die Publikationen nicht öffentlich sind. Kann aber als eindeutiger Schlüssel für eine Publikationen verwendet werden)</td>
  </tr>
  <tr>
    <td>hash</td>
    <td>bei Publikationen (Hashwert über den Publikationstext. Kann als Schlüssel verwendet werden)</td>
  </tr>
</table>

### Layout von Mehrfach-Grafiken / Item Handler

Verschiedene Widgets können mehrere Grafiken zu einer Firma erzeugen: Balkendiagramme (Widget-Type *barChart*) für Umsatz, Gewinn, usw., die tabellarische Darstellung der Bilanz neben der Bilanz auch eine Gewinn- und Verlustrechnung. 

![image alt text](image_1.png)

Es gibt in diesem Fall viele Möglichkeiten der Darstellung: nebeneinander, untereinander, oder - häufig die eleganteste Lösung - in Tabs. Die Darstellung kann angepasst werden über einen Item-Handler:

```javascript
function wrapChart(title, element) {
  // wrap title and element in figure and legend tags
  var figure = document.createElement("figure")
  var legend = document.createElement("legend")
  legend.textContent = title
  figure.appendChild(legend)
  figure.appendChild(element)
  var chart = document.createElement("div")
  chart.className = "chart"
  chart.appendChild(legend)
  chart.appendChild(figure)
  return chart
}

new NorthData.Widget( myFigure, { 
  type: "barChart",
  handleItem: wrapChart
})
```

Dem Item-Handler werden die Überschrift der Grafik (z.B. "Gewinn") und das jeweilige Grafikelement übergeben. Der Rückgabewert ist das "neue Element", was dann in das Dokument eingefügt wird, oder null, falls nichts eingefügt wird (d.h., das komplette DOM-Handling von der Funktion übernommen wird). Beispiel siehe hier: [https://www.northdata.de/widgetTestBarChart.html](https://www.northdata.de/widgetTestPubTable.html) 

Ist kein Item-Handler definiert, wird nur die Überschrift in einen `<h2>`-Tag gesetzt.

## Widget-Referenz

Im folgenden werden die verschiedenen Widgets im Detail vorgestellt, und die nur für sie jeweils verfügbaren Konfigurations-Optionen aufgelistet.

### Widget-Typ *"history"*

Stellt die wichtigsten Ereignisse zu einer Firma als Timeline dar.
Beispielseite: [https://www.northdata.de/widgetTestHistory.html](https://www.northdata.de/widgetTestHistory.html)

![image alt text](image_2.png)

##### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: history</td>
  </tr>
  <tr>
    <td>rootColor</td>
    <td>Farbe</td>
    <td>Farbe, auf deren Basis eine passende Farbpalette für die Färbung der Ereignisse erstellt wird.</td>
  </tr>
  <tr>
    <td>maxEvents</td>
    <td>Zahl</td>
    <td>Maximale Anzahl der angezeigten Ereignisse. Default: 25</td>
  </tr>
  <tr>
    <td>companyClick</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Navigation / Click Handler</td>
  </tr>
  <tr>
    <td>personClick</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Navigation / Click Handler</td>
  </tr>
  <tr>
    <td>publicationClick</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Navigation / Click Handler</td>
  </tr>
</table>


### Widget-Typ *"graph"*

Stellt das Netzwerk einer Firma oder Person, d.h. die Verflechtungen mit Firmen (Beherrschungen, Komplementäre, etc.) und Personen (Geschäftsführer, Vorstände, etc.) als Graph dar.

Beispielseite: [https://www.northdata.de/widgetTestGraph.html](https://www.northdata.de/widgetTestGraph.html) 

![image alt text](image_3.png)

##### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: graph</td>
  </tr>
  <tr>
    <td>maxNodes</td>
    <td>Zahl</td>
    <td>bei Layouts "graph", "dendrogram": Maximale Anzahl der angezeigten Knoten. Default: 12</td>
  </tr>
  <tr>
    <td>companyClick</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Navigation / Click Handler</td>
  </tr>
  <tr>
    <td>personClick</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Navigation / Click Handler</td>
  </tr>
</table>

### Widget-Typ *"pubTable"*

Interaktive Liste von Publikationen (Handelsregister, Insolvenzregister) zur jeweiligen Firma. 

Beispielseite: [https://www.northdata.de/widgetTestPubTable.html](https://www.northdata.de/widgetTestPubTable.html) 

Die einzelnen Publikationen können durch Klick auf der Überschrift ein- und ausgeklappt werden. Semantische Konzepte wie Firmen oder Personen sind im HTML-Text markiert, und können per CSS in der Darstellung verändert werden.

![image alt text](image_4.png)

#### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: pubTable</td>
  </tr>
  <tr>
    <td>maxRows</td>
    <td>Zahl</td>
    <td>Maximale Anzahl der angezeigten Elemente. Default: 25</td>
  </tr>
</table>

### Widget-Typ "*barChart"*

Stellt finanzielle Kennzahlen als Balkendiagramm dar.

Beispielseite: [https://www.northdata.de/widgetTestBarChart.html](https://www.northdata.de/widgetTestPubTable.html) 

![image alt text](image_5.png)

#### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: barChart</td>
  </tr>
  <tr>
    <td>items</td>
    <td>Array of String</td>
    <td>Gewünschte Kennzahlen: Liste der IDs.  Default: ["earnings", "revenue", "tax", "employees"]. 
zu den möglichen IDs siehe Finanzielle Kennzahlen</td>
  </tr>
  <tr>
    <td>handleItem</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Layout von Mehrfach-Grafiken / Item Handler</td>
  </tr>
  <tr>
    <td>colors</td>
    <td>Array von Farben</td>
    <td>3 Farbangaben, Nr. 1 für absolute Werte (z.B. Umsatz), Nr. 2 für positive (z.B. Gewinn), Nr. 3 für negative (z.B. Verlust)</td>
  </tr>
  <tr>
    <td>maxYears</td>
    <td>Zahl</td>
    <td>Maximale Anzahl der angezeigten Jahre. Default: 8 </td>
  </tr>
  <tr>
    <td>chronology</td>
    <td>String</td>
    <td>Zeitliche Reihenfolge: Wert "normal" für aufsteigend,  Wert "reverse" für absteigend. Default: "normal"</td>
  </tr>
</table>

### Widget-Typ "*financials"*

Stellt finanzielle Kennzahlen (soweit verfügbar) als Tabelle dar.

Beispiel: [https://www.northdata.de/widgetTestFinancials.html](https://www.northdata.de/widgetTestFinancials.html) 

Das Tabellen-"Basis-Styling" ist nur rudimentär, so dass Sie es problemlos nach Ihren Bedürfnissen anpassen können. Möchten Sie das nicht tun, kann ein erweitertes Styling über folgende CSS-Datei eingebunden werden:

[https://www.northdata.de/css/styling.css](https://www.northdata.de/css/styling.css) 

![image alt text](image_6.png)

#### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: financials</td>
  </tr>
  <tr>
    <td>maxYears</td>
    <td>Zahl</td>
    <td>Maximale Anzahl der angezeigten Jahre. Default: 2 </td>
  </tr>
  <tr>
    <td>chronology</td>
    <td>String</td>
    <td>Zeitliche Reihenfolge: Wert "normal" für aufsteigend,  Wert "reverse" für absteigend. Default: "normal"</td>
  </tr>
  <tr>
    <td>popup</td>
    <td>Funktion</td>
    <td>Beim Click auf die Quellenangabe werden Auszüge der Originalpublikation in einem Popup-Dialog gezeigt. Durch Angabe einer Funktion function(title, html) können modale Dialog der eigenen UI-Library verwendet werden. </td>
  </tr>
</table>

### Widget-Typ "*sheets"*

Stellt Bilanz und Gewinn-/Verlustrechnung (soweit verfügbar) als interaktive Tabelle dar. Summen-Zeilen können per Klick ein- und aufgeklappt werden.

Beispielseite: [https://www.northdata.de/widgetTestSheet.html](https://www.northdata.de/widgetTestSheet.html) 

Das Tabellen-"Styling" ist nur rudimentär, so dass sie es problemlos nach Ihren Bedürfnissen anpassen können. Möchten Sie das nicht tun, kann ein erweitertes Styling kann über folgende CSS-Datei eingebunden werden:

[https://www.northdata.de/css/styling.css](https://www.northdata.de/css/styling.css) 

![image alt text](image_7.png)

#### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: financials</td>
  </tr>
  <tr>
    <td>maxYears</td>
    <td>Zahl</td>
    <td>Maximale Anzahl der angezeigten Jahre. Default: 2 </td>
  </tr>
  <tr>
    <td>chronology</td>
    <td>String</td>
    <td>Zeitliche Reihenfolge: Wert "normal" für aufsteigend,  Wert "reverse" für absteigend. Default: "normal"</td>
  </tr>
  <tr>
    <td>items</td>
    <td>Array of String</td>
    <td>Gewünschte Tabellen als Liste von Strings.
Default: ["balance", "income"]. Mögliche Angaben: 
balance - Bilanz
income - Gewinn- und Verlustrechnung</td>
  </tr>
  <tr>
    <td>handleItem</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Layout von Mehrfach-Grafiken / Item Handler</td>
  </tr>
  <tr>
    <td>popup</td>
    <td>Funktion</td>
    <td>Beim Click auf die Quellenangabe werden Auszüge der Originalpublikation in einem Popup-Dialog gezeigt. Durch Angabe einer Funktion function(title, html) können modale Dialog der eigenen UI-Library verwendet werden. </td>
  </tr>
</table>

### Widget-Typ "*drillDown"*

Stellt die größten Positionen in den Ausgaben, Aktiva und Passiva des neuesten verfügbaren Jahresabschluss als untergliedertes Rechteck dar. Die Fläche der jeweiligen Boxen entspricht der Größe der Position, so dass die Größenverhältnisse auf den ersten Blick ersichtlich sind. In der Bilanz zusammengehörige Position erhalten "ähnliche" Farben.

Beispielseite: [https://www.northdata.de/widgetTestDrillDown.html](https://www.northdata.de/widgetTestDrillDown.html) 

![image alt text](image_8.png)

#### Besondere Konfigurations-Optionen

<table>
  <tr>
    <td>Name</td>
    <td>Typ</td>
    <td>Erklärung</td>
  </tr>
  <tr>
    <td>type</td>
    <td>String</td>
    <td>Auswahl der grafischen Darstellung: drillDown</td>
  </tr>
  <tr>
    <td>rootColor</td>
    <td>Farbe</td>
    <td>Farbe, auf deren Basis eine passende Farbpalette erstellt wird.</td>
  </tr>
  <tr>
    <td>items</td>
    <td>Array of String</td>
    <td>Gewünschte Grafiken als Liste von Strings.
Default: ["cost", "assets", "liabilities"]. 
Mögliche Angaben: 
cost - Ausgaben
assets - Aktiva
liabilities - Passiva</td>
  </tr>
  <tr>
    <td>handleItem</td>
    <td>Funktion</td>
    <td>siehe Abschnitt Layout von Mehrfach-Grafiken / Item Handler</td>
  </tr>
</table>


