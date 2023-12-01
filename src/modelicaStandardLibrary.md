# Modelica Standard Library
Die Modelica Standard Library (MSL) strukturiert sich wie folgt:
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Electrical.Analog
^^^
```{figure} ../data/img/Lib-Electrical.png
```

````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Electrical.Digital
^^^
```{figure} ../data/img/Lib-Digital.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Electrical.
Machines
^^^
```{figure} ../data/img/Lib-Machines.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Magnetic.
FluxTubes
^^^
```{figure} ../data/img/Lib-FluxTubes.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Mechanics.
Translational
^^^
```{figure} ../data/img/Lib-Translational.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Mechanics.
Rotational
^^^
```{figure} ../data/img/Lib-Rotational.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Mechanics.
MultiBody
^^^
```{figure} ../data/img/Lib-MultiBody1.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Fluid
^^^
```{figure} ../data/img/Lib-Fluid.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Media
^^^
```{figure} ../data/img/Lib-Media.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Thermal.
FluidHeatFlow
^^^
```{figure} ../data/img/Lib-Thermal.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Thermal.
HeatTransfer
^^^
```{figure} ../data/img/Lib-HeatTransfer.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Blocks
^^^
```{figure} ../data/img/Lib-Blocks1.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.StateGraph
^^^
```{figure} ../data/img/Lib-StateGraph.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Math
^^^
```{figure} ../data/img/Lib-Math.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Utilities
^^^
```{figure} ../data/img/Lib-Utilities.png
:width: 100 px
```
````
`````

Wie können wir mit den Klassen, die bereits in der Bibliothek deklariert wurden, arbeiten bzw. Systeme modellieren?

## Rückblick: Abkühlung eines Raumes

Die [Aufgabe: Abkühlung eines Raumes](einfuehrung:grundlagen:abkuehlung) aus der Einführung wird anschließend mit dem objektorientierten Ansatz gelöst.

```````{tab-set}
````{tab-item} Erinnerung: Aufgabe
```{figure} ../data/img/AbkuehlungRaum.png
:width: 550px
```
````
````{tab-item} Erinnerung: Modelica Modell
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
````{tab-item} Lösung: OOP
```Modelica
model RaumOOP
    extends SimModel(tStop=86400);
    Modelica.Thermal.HeatTransfer.Components.HeatCapacitor    thermischeMasse(
		C=78000,
		T(start=294.15));
    Modelica.Thermal.HeatTransfer.Components.ThermalConductor konduktion(G=9.1);
    Modelica.Thermal.HeatTransfer.Sources.FixedTemperature    festeTemperatur(T=273.15);
    equation
        connect(thermischeMasse.port, konduktion.port_a);
        connect(konduktion.port_b, festeTemperatur.port);
end RaumOOP;
```
````
```````

Neben der rein textbasierten Modellierung, bietet SimulationX eine **graphische Benutzeroberfläche**, die genutzt werden kann, um Objekte zu instanziieren,
Verbindungen zu erstellen und Parameter zu ändern. Die Abkühlung des Raumes wird im Folgenden graphisch modelliert. Vergleicht man den entstehenden Quellcode der graphischen Modellierung mit der vorherigen objektorientierten Lösung, fallen die zusätzlichen `annotations` auf. Diese beschreiben die Position der graphischen Darstellung der Objekte.

```````{tab-set}
````{tab-item} Lösung: Graphisch
```{figure} ../data/img/AbkuehlungRaumGraphisch.png
:width: 550px
```
````
````{tab-item} Quellcode: Graphisch
```modelica
model RaumGraphisch
	extends SimModel(tStop=86400);
	Modelica.Thermal.HeatTransfer.Components.HeatCapacitor thermischeMasse(
		C=78000,
		T(start=294.15)) annotation(Placement(transformation(extent={{-120,65},{-100,85}})));
	Modelica.Thermal.HeatTransfer.Components.ThermalConductor konduktion(G=9.1) annotation(Placement(transformation(extent={{-85,45},{-65,65}})));
	Modelica.Thermal.HeatTransfer.Sources.FixedTemperature festeTemperatur(T=273.15) annotation(Placement(transformation(extent={{-15,45},{-35,65}})));
	equation
		connect(konduktion.port_a,thermischeMasse.port) annotation(Line(
			points={{-85,55},{-90,55},{-110,55},{-110,60},{-110,65}},
			color={191,0,0},
			thickness=0.0625));
		connect(festeTemperatur.port,konduktion.port_b) annotation(Line(
			points={{-35,55},{-40,55},{-60,55},{-65,55}},
			color={191,0,0},
			thickness=0.0625));
end RaumGraphisch;
```
````
```````
```{admonition} Hinweis
Um die Anfangstemperatur der thermischen Masse zu definieren, muss in die Textansicht gewechselt werden. Hier kann die Anfangstemperatur durch `T(start=294.15)` ergänzt werden. Die graphische Benutzeroberfläche von SimulationX ermöglicht lediglich die Änderung von Parametern, von Objekten, die direkt initialisiert werden.
```

## Aufgaben zu Wärmetransport innerhalb der MSL

1. Finden Sie die Zusammenhänge zwischen unserer bisherigen Modellierung des Raums und den neuen Parametern `thermischeMasse.C` und `konduktion.G`. Nutzen Sie dafür die Klassendefinitionen der Modelica Standard Library.

2. Welche formale Bedeutung hat das Verbinden von den Anschlüssen der Objekte? Schauen Sie dafür in die Deklaration des Connectors.

3. Erweitern Sie das bisherige graphische Modell um die thermische Masse der Wand. Ändern Sie dafür den bisherigen Wandaufbau zu einer Kombination aus Mauerwerk und Dämmung. Wie interpretieren Sie die resultierenden Wärmeströme und Temperaturen?
``````{grid}
````{grid-item}
```{figure} ../data/img/Wandaufbau.png
:width: 300px
```
````
````{grid-item}
```{list-table}
:header-rows: 1

* - Symbol
  - Einheit
  - Mauerwerk
  - Dämmung
* - $c$
  - J/(kg.K)
  - 836 
  - 920
* - $\rho$
  - kg/m<sup>3</sup>
  - 1400
  - 200
* - $k$
  - W/(m.K)
  - 0,058
  - 0,035
```
````
``````
4. Vergrößern Sie ihr Wandmodell um die Transmissionsverluste eines 3 m<sup>2</sup> großen Fensters ($G =\,$ 2,4 W/K) in der Wand. Wie verändern sich die Wärmeströme?

5. Welche Leistung bräuchten Sie ungefähr, um die Anfangstemperatur zu halten?

``````{admonition} Lösung:
:class: dropdown

`````{tab-set}
````{tab-item} Graphisch
```{figure} ../data/img/Wandmodell_MSL.png
:width: 7000px
```
````
````{tab-item} Quellcode
```modelica
model Transmission
	extends SimModel(tStop=1209600);
	Modelica.Thermal.HeatTransfer.Components.HeatCapacitor Raumluft(C=78000) annotation(Placement(transformation(extent={{-135,65},{-115,85}})));
	Modelica.Thermal.HeatTransfer.Components.ThermalConductor Mauerwerk(G=3.77) annotation(Placement(transformation(extent={{-85,50},{-65,70}})));
	Modelica.Thermal.HeatTransfer.Components.ThermalConductor Daemmung(G=2.725) annotation(Placement(transformation(extent={{-45,50},{-25,70}})));
	Modelica.Thermal.HeatTransfer.Sources.FixedTemperature Aussentemperatur(T=273.15) annotation(Placement(transformation(extent={{25,50},{5,70}})));
	Modelica.Thermal.HeatTransfer.Components.HeatCapacitor MauerwerkUndDaemmung(C=3521440) annotation(Placement(transformation(extent={{-80,20},{-60,40}})));
	Modelica.Thermal.HeatTransfer.Components.ThermalConductor Fenster(G=2.4) annotation(Placement(transformation(extent={{-65,80},{-45,100}})));
	initial equation
		Raumluft.T = 294.15;
		MauerwerkUndDaemmung.T = 294.15;
	equation
		connect(Aussentemperatur.port,Daemmung.port_b) annotation(Line(
			points={{5,60},{0,60},{-20,60},{-25,60}},
			color={191,0,0},
			thickness=0.0625));
		connect(Daemmung.port_a,Mauerwerk.port_b) annotation(Line(
			points={{-45,60},{-50,60},{-60,60},{-65,60}},
			color={191,0,0},
			thickness=0.0625));
		connect(Raumluft.port,Mauerwerk.port_a) annotation(Line(
			points={{-125,65},{-125,60},{-90,60},{-85,60}},
			color={191,0,0},
			thickness=0.0625));
		connect(MauerwerkUndDaemmung.port,Mauerwerk.port_b) annotation(Line(
			points={{-70,20},{-70,15},{-55,15},{-55,60},{-65,60}},
			color={191,0,0},
			thickness=0.0625));
		connect(Fenster.port_b,Aussentemperatur.port) annotation(Line(
			points={{-45,90},{-40,90},{-15,90},{-15,60},{5,60}},
			color={191,0,0},
			thickness=0.0625));
		connect(Fenster.port_a,Raumluft.port) annotation(Line(
			points={{-65,90},{-95,90},{-95,60},{-125,60},{-125,65}},
			color={191,0,0},
			thickness=0.0625));
end Transmission;
```
````
`````
``````

## Signalquellen innerhalb der MSL

Um veränderliche Randbedingungen definieren zu können - in unserem bisherigen Modell z.B. eine veränderliche Außentemperatur - gibt es die Klasse [PrescribedTemperature](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Thermal_HeatTransfer_Sources.html#Modelica.Thermal.HeatTransfer.Sources.PrescribedTemperature). Ein Objekt dieser Klasse kann einen, vor der Simulation bekannten, Temperaturverlauf abbilden. Neben der variablen Temperatur gibt es auch die Möglichkeit einen veränderlichen Wärmestrom mit der Klasse [PrescribedHeatFlow](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Thermal_HeatTransfer_Sources.html#Modelica.Thermal.HeatTransfer.Sources.PrescribedHeatFlow) abzubilden.
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Variable Temperatur
^^^
```{figure} ../data/img/VariableTemperaturRandbedingung.png
:width: 200px
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Variabler Wärmestrom
^^^
```{figure} ../data/img/VariablerWärmestromRB.png
:width: 250px
```
````
`````
In der graphischen Darstellung der Objekte fällt auf, dass neben dem bereits genutzten [thermischen Anschluss](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Thermal_HeatTransfer_Interfaces.html#Modelica.Thermal.HeatTransfer.Interfaces.HeatPort_b) - hier als `port` bezeichnet (weißes Quadrat mit rotem Rand) - ein weiterer Anschluss `T` bzw. `Q_flow` (gefülltes blaues Dreieck) abgebildet ist. Der neue blaue Anschluss wird in der Bibliothek als [RealInput](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Blocks_Interfaces.html#Modelica.Blocks.Interfaces.RealInput) bezeichnet. Zwischen den beiden Anschlüssen gibt es zwei wesentliche Unterschiede. Zum einen die gleichgesetzten Variablen, im Fall des thermischen Anschlusses Temperatur und Wärmestrom und im Fall des Real Input ein Zahlenwert der reellen Zahlen. Zum anderen differenziert der neue Anschluss zwischen Input und Output. Da es sich bei der neuen Anschluss lediglich um einen Zahlenwert handelt, versucht die Bibliothek durch die Benennung des Anschlusses zu präzisieren was dieser Zahlenwert repräsentiert. Hier also eine Temperatur bzw. ein Wärmestrom.

```{admonition} Hinweis
Die Simulationsumgebung verhindert, dass Sie unterschiedliche Anschlüsse oder Input mit Input bzw. Output mit Output verbinden.
```

Die eigentlichen **Signalquellen**, die Sie anschließend benötigen, um eine variable Temperatur oder einen variablen Wärmestrom zu modellieren, finden Sie im Package [Sources](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Blocks_Sources.html#Modelica.Blocks.Sources). Hier sind verschiedene Signalquellen, die farblich in die Built-In Variablentypen `Real` (blau), `Integer` (gelb) und `Boolean` (lila) unterteilt sind, hinterlegt. Anschließend sind einige beispielhafte Signalquellen in Kombination mit der variablen Temperatur dargestellt. Analog funktionieren diese Signalquellen auch für den variablen Wärmestrom.

```````{tab-set}
````{tab-item} RealExpression
```{figure} ../data/img/Signalquelle_Expression.png
:width: 300px
```
````
````{tab-item} Constant
```{figure} ../data/img/Signalquelle_Constant.png
:width: 300px
```
````
````{tab-item} Step
```{figure} ../data/img/Signalquelle_Jump.png
:width: 300px
```
````
````{tab-item} Sine
```{figure} ../data/img/Signalquelle_Sin.png
:width: 300px
```
````
````{tab-item} CombiTimeTable
```{figure} ../data/img/Signalquelle_Table.png
:width: 300px
```
````
```````

```{admonition} Hinweis
Sie können den Signalquellen Einheiten zuweisen, indem Sie in den Attributen des Ergebnisparameters eine `quantity` angeben.
```