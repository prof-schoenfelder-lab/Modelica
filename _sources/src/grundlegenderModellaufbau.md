# Grundlegender Modellaufbau

Ein Modelica Modell lässt sich im einfachsten Fall auf das Deklarieren von Variablen
und einer Beschreibung von deren Zusammenhängen reduzieren. Wie hier am Beispiel $f(time)=time$ demonstriert wird.
```````{tab-set}
````{tab-item} Modelica Modell
```modelica
model Gerade
    extends SimModel;
    Real f;
    equation
        f = time;
end Gerade;
```
````
````{tab-item} Modelica Ergebnis
```{figure} ../data/img/02_Gerade.png
:width: 300px
```
````
```````

Das erste Signalwort `model` beschreibt den Beginn der Modellbeschreibung, die bis zum Signalwort `end` fortgesetzt wird. Nach den beiden Signalwörtern wird der Name des Modells `Gerade` angegeben. Um zu verdeutlichen, dass die Modellbeschreibung innerhalb dieser Umgebung definiert wird, sind die betreffenden Zeilen eingerückt.

```{margin} 
Neben dem Modelica eigenen Variablentyp (sogenannten Built-In Types) `Real` stehen die Typen `Integer`, `Boolean` und `String` zur Verfügung.
```

Nachdem die Modellumgebung definiert wurde, wird die erste Variable `f` mit dem **Variablentypen** `Real` definiert. Das Semikolon am Ende der Zeile signalisiert das Ende der ersten Definition.
:::{admonition} Fehlermeldung, falls `f` nicht definiert wird
:class: tip
Fehler in Gerade: Unbekannte Variable f referenziert.
:::
Das nächste Signalwort `equation` öffnet eine neue Umgebung innerhalb des Modells. Im Gegensatz zur Modellumgebung wird diese nicht geschlossen. Daraus resultiert, dass alle folgenden Zeilen Gleichungen sind bis eine neue Umgebung geöffnet oder das Modell geschlossen wird. Innerhalb der Umgebung `equation` wird der Zusammenhang zwischen `f` und der Zeit `time` definiert. Auffällig ist, dass `time` nicht deklariert wurde. Die Variable `time` stellt diesbezüglich eine Ausnahme dar. Sie ist in jedem beliebigen Modell enthalten, ohne explizit deklariert werden zu müssen und repräsentiert die Simulationszeit.
:::{admonition} Achtung
:class: caution
Die grundsätzlich zur verfügungstehende Variable `time` kann, durch eigene Deklaration, überschieben werden und verliert dardurch den Bezug zur Simulationszeit.
:::
Um das Modell simulieren zu können muss die Voraussetzung, dass die Anzahl der Unbekannten (hier `f`, also eine Unbekannte) mit der Anzahl der Gleichungen übereinstimmt.

Modelle können zum besseren Verständnis mit beschreibenden *Strings* ergänzt werden. Diese sind Teil des Modells und unterscheiden sich deshalb von Kommentaren, die an beliebiger Stelle eingefügt werden können und unberücksichtigt bleiben.
:::modelica
model Gerade "mögliche Modellbeschreibung"
    /* Kommentar, der unberücksichtigt bleibt */
    Real f "zeitabhängige Funktion";
    equation
        f = time "Zusammenhang zwischen f und der Simulationszeit";
end Gerade;
:::

## Parameter 

Neben veränderlichen Variablen können auch konstante Variablen eingeführt werden. Das bereits eingeführte Beispielmodell `Gerade` wird im folgenden um eine Steigung `m` zu dem Modell `GeradeMitSteigung` erweitert ($f(time)=m\cdot time$).

```````{tab-set}
````{tab-item} Modelica Modell
```modelica
model GeradeMitSteigung
    extends SimModel;
    Real f;
    parameter Real m=0.5;
    equation
        f = m*time;
end GeradeMitSteigung;
```
````
````{tab-item} Modelica Ergebnis
```{figure} ../data/img/02_GeradeMitSteigung.png
:width: 300px
```
````
```````


Die Besonderheit der Variable `m`, die auf das Signalwort `parameter` folgt, ist, dass sie bereits vor der Simulation bekannt sein muss (*a priori*) und ihren Wert während der Simulation nicht verändert. Parametern werden entweder Zahlenwerte oder Berechnungsvorschriften aus anderen Parametern zugewiesen.
:::{admonition} Achtung
:class: caution
Wird einem Parameter kein Wert zugewiesen, nimmt SimulationX den Wert 0, unter der Warnung: **Der Parameter m hat keinen Wert, 0 wird angenommen**, an.
:::

## Aufgabe: Gerade
Schreiben Sie ein Modell `Gerade`, dass neben der Steigung auch einen Parameter für die Anfangsbedingung $f(time=0\,\text{s})=b$ enthält. Weisen Sie $b$ einen Zahlenwert von $-1$ zu.

`````{admonition} Lösung:
:class: dropdown
`````{grid}
````{grid-item}
```modelica
model Gerade
    extends SimModel;
    Real f;
    parameter Real m=0.5;
    parameter Real b=-1;
    equation
        f = m*time+b;
end Gerade;
```
````
````{grid-item}
```{figure} ../data/img/02_GeradeAufgabe.png
:width: 500px
```
````
`````
(einfuehrung:grundlagen:anfangsbedingungen)=
## Anfangsbedingungen
Sobald die Modellbeschreibungen Ableitungen nach der Zeit `der()` enthalten, können die Anfangsbedingungen nicht mehr, wie in der vorangegangenen Aufgabe, in die Gleichung aufgenommen werden, ohne die Integration selbst durchzuführen. Zur Veranschaulichung wird das Beispiel der gleichmäßig beschleunigten Bewegung mit der Erdbeschleunigung modelliert.

```````{tab-set}
``````{tab-item} Physikalischer Zusammenhang
`````{grid}
````{grid-item}
```{math}
h(t) &= \int v(t)\,dt = -\frac{1}{2}gt^2+v_0 t+h_0 \\
v(t) &= \int a(t)\,dt = -gt+v_0\\
a(t) &= -g    
```
````
````{grid-item}
```{list-table}
* - $a$
  - Beschleunigung
* - $g$
  - Erdbeschleunigung
* - $h$
  - Höhe
* - $h_0$
  - Anfangshöhe
* - $t$
  - Zeit
* - $v$
  - Geschwindigkeit
* - $v_0$
  - Anfangsgeschwindigkeit
```
````
`````
``````
````{tab-item} Modelica Modell
```modelica
model FreierFall
    extends SimModel;
    Real h "Höhe";
    Real v "Geschwindigkeit";
    parameter Real h0=5   "Anfangshöhe";
    parameter Real v0=0   "Anfangsgeschwindigkeit";
    parameter Real g=9.81 "Erdbeschleunigung";
    initial equation
        h = h0;
        v = v0;
    equation
        der(h) = v;
        der(v) = -g;
end FreierFall;
```
````
````{tab-item} Modelica Ergebnis
```{figure} ../data/img/Bewegung.png
:width: 350px
```
````
```````
Der physikalische Zusammenhang führt, durch die Integration, zu zwei Konstanten $h_0$ und $v_0$. Da das Integral eigentlich nicht vom Anwender, sondern von dem Programm gelöst werden soll, gibt es die Möglichkeit Anfangsbedingungen in der Umgebung `initial equation` zu definieren. Diese Bedingungen gelten für den ersten Zeitpunkt, der nicht unbedingt $t=0\,\text{s}$ entsprechen muss.

## Einheiten
Neben der Betrachtung von rein mathematisch Problemen sind Variablen in der Realität mit Einheiten behaftet. Das zuvor eingeführte Modell der gleichmäßig beschleunigten Bewegung wird um die entsprechenden Einheiten erweitert.
```modelica
model FreierFall
    extends SimModel;
    Real h(unit="m")   "Höhe";
    Real v(unit="m/s") "Geschwindigkeit";
    parameter Real h0(unit="m")  =5    "Anfangshöhe";
    parameter Real v0(unit="m/s")=0    "Anfangsgeschwindigkeit";
    parameter Real g(unit="m/s2")=9.81 "Erdbeschleunigung";
    initial equation
        h = h0;
        v = v0;
    equation
        der(h) = v;
        der(v) = -g;
end FreierFall;
```
Die angegebenen Einheiten können von Simulationsumgebungen wie OpenModelica überprüft werden. In SimulationX steht diese Option noch nicht zur Verfügung.

## Anmerkungen
Anmerkungen oder `annotations` sind zusätzliche Informationen, die nicht das eigentliche Modell beschreiben, sondern z.B. Angaben zu Simulationseinstellungen enthalten. SimulationX ergänzt das Modell `Gerade` automatisch, mit einer Anmerkung am Modellende, damit das Modell simuliert werden kann.
```modelica
model Gerade
	extends SimModel;
	Real f;
	equation
		f = time;
	annotation(f(flags=2));
end Gerade;
```
Hier werden die Variablen spezifiziert, deren Ergebnisse gespeichert werden sollen und Angaben zu dem Solver gemacht. Wenn die Anmerkung am Ende des Modells steht, bezieht sie sich auf das gesamte Modell. Alternativ können z.B. auch Anmerkungen zu Variablen gemacht werden. 

Die zweite Ergänzung des Modells mit dem Signalwort `extends` wird zu einem späteren Zeitpunkt erläutert.

(einfuehrung:grundlagen:abkuehlung)=
## Aufgabe: Abkühlung eines Raumes

```{margin} 
```{list-table}
* - $c_p$
  - spezifische Wärmekapazität (isobar)
* - $k$
  - Wärmeleitfähigkeit
* - $\rho$
  - Dichte
* - $T_R$
  - Temperatur des Raums (homogen)
* - $T_A$
  - Temperatur der Außenluft (constant)
* - $t$
  - Zeit
```
Modellieren Sie einen Innenraum, der nur eine thermisch aktive Fläche besitzt, die ihn von der Außenluft trennt (siehe  {numref}`AufgabeAbkuehlungRaum`). Alle anderen Flächen werden adiabat angenommen. Recherchieren Sie die formalen Zusammenhänge für die Wärmeleitung (Konduktion), sowie die Grundgleichung der Wärmelehre und implementieren Sie diese im Modell. Das Ergebnis soll aus dem zeitabhängigen Verlauf der homogenen Innenraumtemperatur bestehen.


<!-- ```modelica
model Konduktion
	extends SimModel(tStop=72000);
	Modelica.Thermal.HeatTransfer.Components.HeatCapacitor mass1(
		C=78000,
		T(
			start=394.15,
			fixed=true)) annotation(Placement(transformation(extent={{-100,20},{-40,80}})));
	Modelica.Thermal.HeatTransfer.Components.ThermalConductor conduction(G=9.1) annotation(Placement(transformation(extent={{-30,-20},{30,40}})));
	Modelica.Thermal.HeatTransfer.Sources.FixedTemperature fixedTemperature1(T=273.15) annotation(Placement(transformation(extent={{115,-15},{65,35}})));
	equation
		connect(mass1.port,conduction.port_a) annotation(Line(
			points={{-70,20},{-70,15},{-70,10},{-35,10},{-30,10}},
			color={191,0,0}));
		connect(fixedTemperature1.port,conduction.port_b) annotation(Line(
			points={{65,10},{60,10},{35,10},{30,10}},
			color={191,0,0},
			thickness=0.0625));
	annotation(
		uses(Modelica(version="4.0.0")),
		__esi_protocols(
			transient={
				Protocol(var=time),
				Protocol(var=mass1.T),
				Protocol(var=conduction.Q_flow)}),
		__esi_solverOptions(
			solver="CVODE",
			typename="ExternalCVODEOptionData"));
end Konduktion;
``` -->



```````{tab-set}
````{tab-item} Aufgabe
```{figure} ../data/img/AbkuehlungRaum.png
:width: 550px
:name: AufgabeAbkuehlungRaum
Beschreibung der Geometrie- sowie der Materialparameter und gegebener Temperaturen.
```
````
````{tab-item} Lösung: Modelica Modell
```modelica
model Raum "Abkuehlung eines unbeheizten Raums"
	extends SimModel(tStop=86400);
	
	parameter Real A   (unit="m2")      =13     "Wandfläche";
	parameter Real c_p (unit="J/(kg.K)")=1000   "spezifische Waermekapazität (isobar)";
	parameter Real k   (unit="W/(m.K)") =0.28   "Waermeleitfaehigkeit";
	parameter Real rho (unit="kg/m3")   =1.2    "Dichte";
	parameter Real L   (unit="m")       =0.4    "Wanddicke";
	parameter Real T_R0(unit="K")       =294.15 "Anfangstemperatur des Innenraums";
	parameter Real T_A (unit="K")       =273.15 "Aussentemperatur";
	parameter Real V   (unit="m3")      =65     "Volumen";
	
	Real T_R(unit="K") "Innenraumtemperatur";
	
	initial equation
		T_R=T_R0;
		
	equation
		c_p*V*rho*der(T_R)=k*A/L*(T_A-T_R);
end Raum;
```
````
````{tab-item} Lösung: interaktive Grafik
<!DOCTYPE HTML>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta content="text/html; charset=utf-8" http-equiv="Content-Type">
    <link rel="stylesheet" type="text/css" href="https://jsxgraph.uni-bayreuth.de/distrib/jsxgraph.css" />
    <script src="https://cdn.jsdelivr.net/npm/jsxgraph/distrib/jsxgraphcore.js" type="text/javascript" charset="UTF-8"></script>
  </head>
  <body>

  <div id="jxgbox" class="jxgbox" style="width:743px; height:400px;"></div>

  <script>
    var board = JXG.JSXGraph.initBoard('jxgbox', {
      boundingbox: [-1, 25, 25, -2],
      axis:true,
      showCopyright:false,
      showNavigation:false,
      defaultAxes:{
        x: {
          name: 't [h]',
          withLabel: true,
          label: {
            position: 'rt',
            offset: [-15, 20]
          }
        },
        y: {
          withLabel: true,
          name: 'T_R [°C]',
          label: {
            position: 'rt',
            offset: [10, -10]
          }
        }
      }
    });
    var V = board.create('slider', [[15, 24], [20, 24], [20, 65, 100]], {name:'V', snapWidth: 5, postLabel: ' m^3'});
    var A = board.create('slider', [[15, 22.5], [20, 22.5], [1, 13, 25]], {name:'A', snapWidth: 1, postLabel: ' m^2'});
    var L = board.create('slider', [[15, 21], [20, 21], [0.05, 0.4, 1]], {name:'L', snapWidth: 0.05, postLabel: ' m'});

    var cp = board.create('slider', [[15, 19], [20, 19], [100, 1000, 1900]], {name:'c_p', snapWidth: 100, postLabel: ' J.kg^{-1}.K^{-1}'});
    var k = board.create('slider', [[15, 17.5], [20, 17.5], [0.05, 0.28, 0.5]], {name:'k', snapWidth: 0.01, postLabel: ' W.m^{-1}.K^{-1}'});
    var rho = board.create('slider', [[15, 16], [20, 16], [0.05, 1.2, 3]], {name:'rho', snapWidth: 0.05, postLabel: ' kg.m^3'});

    var TA = board.create('slider', [[15, 14], [20, 14], [-5, 0, 15]], {name:'T_A', snapWidth: 0.5, postLabel: ' °C'});
    var TR0 = board.create('slider', [[15, 12.5], [20, 12.5], [15, 21, 25]], {name:'T_{R0}', snapWidth: 0.5, postLabel: ' °C'});
    var graph = board.create('functiongraph', [function(x){return -(TA.Value()-TR0.Value())*Math.exp(-k.Value()*A.Value()/(cp.Value()*V.Value()*rho.Value()*L.Value())*(x*3600))+TA.Value();}, -0, 24],   {name:'T_R(t)', withLabel:false, strokeColor:'red'});

    var reset = board.create('button',[17, 10,'Reset', function(){
      V.setValue(65),
      A.setValue(13),
      L.setValue(0.4),
      cp.setValue(1000),
      k.setValue(0.28),
      rho.setValue(1.2),
      TA.setValue(0)
      TR0.setValue(21)
    }]);

    var glider = board.create('glider', [5, 10, graph], {name:'T_R'});
    var x = board.create('point', [() => glider.X(), 0], {size:1, name:'', visible:false});
    var y = board.create('point', [0, () => glider.Y()], {size:1, name:'', visible:false});
    xseg = board.create('segment', [glider, x], {dash:2, strokeColor:'#000000'});
    yseg = board.create('segment', [glider, y], {dash:2, strokeColor:'#000000'});
  </script>
  </body>
</html>
````
```````
