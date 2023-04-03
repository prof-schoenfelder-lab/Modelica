# Klassen in Modelica

Die Programmiersprache Modelica basiert auf dem objektorientierten Ansatz. **Alles** ist letztendliche eine Klasse. Modelica bietet verschiedene Möglichkeiten Klassen zu definieren. Die **allgemeinste** Umgebung für Klassendefinitionen nutzt das Signalwort `class`.

```modelica
class Gerade
    extends SimModel;
    Real f;
    equation
        f = time;
end Gerade;
```

Neben dieser allgemeinen Umgebung gibts es **spezialisierte** Klassen, in denen bestimmte Definitionen nicht möglich sind.
```{margin} 
Die hier aufgezählten spezialisierten Klassen sind nicht vollständig. Weitere Informationen sind in den [Modelica Specifications](https://modelica.org/documents.html) zu finden.
```
```{list-table}
* - **Signalwort**
  - **Anwendungsfall**
  - **Einschränkung**
* - model
  - Systemmodellierung
  - keine Einschränkungen
* - record
  - Datenbank für Modelle
  - Signalwörter die nicht verfügbar sind: `equation`, `algorithm`, `initial equation`, `initial algorithm`, `protected`, `input`, `output`, `inner`, `outer`, `stream`
* - type
  - Definition eigener Variablentypen
  - Besteht aus vordefinierten Typen (z.B. `Integer`, `Real`) oder erbt von ihnen
* - connector
  - Verbindungen zwischen Modellen
  - Signalwörter die nicht verfügbar sind: `equation`, `algorithm`, `initial equation`, `initial algorithm`, `protected`
* - package
  - Speichern und Strukturieren von Klassendeklarationen
  - Kann nur Klassendeklarationen enthalten, keine "losen" Variablen
```

```````{tab-set}
``````{tab-item} model
```Modelica
model Gerade
    extends SimModel;
    Real f;
    equation
        f = time;
end Gerade;
```
``````
``````{tab-item} record
```Modelica
record Aussenwand
    parameter Real rho(unit="kg/m3") =1400 "Dichte der Wand";
    parameter Real c  (unit="J/kg.K")=850  "spezifische Wärmekapazität";
    parameter Real d  (unit="m")     =0.3  "Wanddicke";
    parameter Real U  (unit="W/m2.K")=0.3  "Wärmedurchgangskoeffizient";
end Aussenwand;
```
``````
``````{tab-item} type
```Modelica
type Temperatur=Real (unit="K", min=0);
```
``````
``````{tab-item} connector
```Modelica
connector HeatPort_a
    Real T(unit="K", min=0);
    flow Real Q_flow(unit="W");
end HeatPort_a;
```
``````
``````{tab-item} package
```text
.
└── Components/
    ├── BodyRadiation.mo
    ├── Convection.mo
    ├── ConvectiveResistor.mo
    ├── GeneralHeatFlowToTemperatureAdaptor.mo
    ├── GeneralTemperatureToHeatFlowAdaptor.mo
    ├── HeatCapacitor.mo
    ├── package.mo
    ├── package.order
    ├── ThermalCollector.mo
    ├── ThermalCollectorMatrix.mo
    ├── ThermalConductor.mo
    └── ThermalResistor.mo
```
``````
```````

Es gibt ein öffentliches standard `package` (Modelica Standard Library), dass grundlegende Modelle aus verschiedenen Bereichen (Elektrotechnik, Magnetismus, Mechanik, Thermodynamik, ...) zur Verfügung stellt.
