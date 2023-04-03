# Beispiel: Abkühlung

(einfuehrung:beispiel:aufgabe)=
## Aufgabenstellung
Das folgende Beispiel kommt aus dem Bereich der Thermodynamik. Das Abkühlungsverhalten eines Festkörpers $(K)$
soll untersucht werden. Seine Temperatur ($T_K$) und die seiner Umgebungsluft ($T_U$), sind in
{numref}`fig_einfuehrungsbeispiel` dargestellt.
```{figure} ../data/img/01_einfuehrungsbeispiel.png
:width: 600px
:name: fig_einfuehrungsbeispiel
Start- und Randbedingungen für die Temperaturen des Festkörpers und seiner Umgebung.
```
## Formale Zusammenhänge
```{margin} 
```{list-table}
* - $A$
  - Oberfläche
* - $c$
  - spezifische Wärmekapazität
* - $h$
  - Wärmeübergangskoeffizient
* - $m$
  - Masse
* - $Q$
  - Wärmemenge
* - $\dot{Q}$
  - Wärmestrom
* - $T_K$
  - Temperatur des Festkörpers in [K]
* - $T_U$
  - Temperatur der Umgebungsluft in [K]
* - $t$
  - Zeit
```
Das Abkühlungsverhalten eines Körpers kann mit dem [Newtonschen Abkühlungsgesetz](https://www.spektrum.de/lexikon/physik/newtonsches-abkuehlungsgesetz/10298) 
beschrieben werden
```{math}
:label: cooling
\dot{Q}(t) = h\cdot A\cdot (T_U-T_K(t))\quad.
```
Durch die Grundgleichung der Wärmelehre 
```{math}
:label: grundGleichungWärme
\Delta Q(t) &= c\cdot m\cdot (T_K(t=t_1)-T_K(t=t_0)) \\
= \Delta Q(t) &= c\cdot m\cdot \Delta T_K(t) \quad ,
```
die auf sehr kleine Zeitschritte bezogen wird
```{math}
:label: grundGleichungWärmeDiff
 \frac{\delta Q(t)}{\delta t} &= c\cdot m\cdot \frac{dT_K(t)}{dt} \\
= \dot{Q}(t) &= c\cdot m\cdot \frac{dT_K(t)}{dt}
```
kann der Zusammenhang zwischen der Temperatur des Körpers, seinen Materialkonstanten und
seiner Abkühlung durch das Gleichsetzen der Gleichungen {eq}`grundGleichungWärmeDiff` und {eq}`cooling` mit 
```{math}
:label: equation
c\cdot m\cdot \frac{dT_K(t)}{dt} = h\cdot A\cdot (T_U-T_K(t))
```
beschrieben werden.

## Modellbeschreibung in Modelica
```{margin} 
Das Modelica Modell Abkuehlung orientiert sich an Tillers [Modelica by Example](https://mbe.modelica.university/behavior/equations/physical/).
```

```modelica
model Abkuehlung
	extends SimModel(tStop=4);

	parameter Real T_U (unit="K")       =290 "Temperatur der Umgebungsluft";
	parameter Real T_K0(unit="K")       =360 "Anfangstemperatur";
	parameter Real h   (unit="W/(m2.K)")=0.7 "Wärmeübergangskoeffizient";
	parameter Real A   (unit="m2")      =1.0 "Oberfläche";
	parameter Real m   (unit="kg")      =0.3 "Masse";
	parameter Real c   (unit="J/(kg.K)")=1.2 "spezifische Wärmekapazität";

	Real T_K(unit="K") "Temperatur";
	
	initial equation
		T_K = T_K0 "Anfangsbedingung";
	
	equation
		c*m*der(T_K) = h*A*(T_U-T_K) "Fromaler Zusammenhang";
end Abkuehlung;
```

## Numerische Lösung des Modells in SimulationX
Die Lösung des Modells findet in der Simulationsumgebung SimulationX statt. Dafür wird das Programm
gestartet und die Textansicht des standardmäßig erscheinenden *Model1* geöffnet. Der bereits vorhandene
Code wird mit dem Modell Abkuehlung überschrieben. Anschließend kann die Simulation gestartet und 
das Ergebnis über ein Ergebnisfenster dargestellt werden.

```{admonition} Numerische Lösung:
:class: dropdown
```{figure} ../data/img/01_Beispiel_Modelica.png
:width: 500px
:name: ergebnis_modelica
Temperatur des Körpers in Abhängigkeit der Zeit als Simulationsergebnis von SimulationX.
```

## Analytische Lösung
```{math}
c\,m\,\frac{dT_K(t)}{dt} &= h\,A\,(T_U-T_K(t)) \\
\frac{c\,m}{T_U-T_K}\,dT_K &= h\, A\, dt \\
c\,m\int\frac{1}{T_U-T_K}\,dT_K &= \int h\,A\, dt \\
-c\,m\,\ln{(T_U-T_K)}+c_1 &= h\,A\,t+c_2 \\
\ln{(T_U-T_K)}+c_1 &= -\frac{h\, A}{c\,m}t+c_2 \\
(T_U-T_K)\,e^{c_1} &= e^{-\frac{h\,A}{c\,m}t}\,e^{c_2} \\
T_U-T_K &= c\, e^{-\frac{h\,A}{c\,m}t} \\
```
```{math}
:label: analytische_loesung
T_K(t) = -c\, e^{-\frac{h\,A}{c\,m}t}+T_U 
```
Durch das Einsetzen der Anfangsbedingung $T_K(t=0\,\text{s})=360\,\text{K}$ in Gleichung {eq}`analytische_loesung` 
kann die Konstante $c$ zu
```{math}
360\,\text{K} &= -c\, e^{-\frac{h\,A}{c\,m}0\,\text{s}}+T_U \\
c &= T_U-360\,\text{K}
```
ermittelt werden. Der Temperaturverlauf des Festkörpers in Abhängigkeit der Zeit wird mit folgender Funktion beschrieben:
```{math}
:label: analytische_loesung_c
T_K(t) = (360-T_U)\, e^{-\frac{h\,A}{c\,m}t}+T_U \quad .
```
```{admonition} Vergleich von analytischer und numerischer Lösung:
:class: dropdown
```{figure} ../data/img/01_Beispiel_Modelica_vgl.png
:name: ergebnis_modelica_vgl
:width: 500px
Temperatur des Körpers in Abhängigkeit der Zeit als Ergebnis der analytischen Funktion {eq}`analytische_loesung_c`
im Vergleich zu den Simulationsergebnissen von SimulationX.
```

## Simulationseinstellungen

Der Vergleich von numerischer und analytischer Lösung scheint sich abgesehen von dem begrenzten Lösungsintervall
nicht zu unterscheiden. Durch die folgende Modifikation der Simulationseinstellungen werden die Besonderheiten 
der numerischen Lösung betont und Unterschiede offensichtlich. Die erste Modifikation vergrößert die Ausgabeschrittweite
von $0.001\,\text{s}$ auf $1\,\text{s}$. Die zweite Modifikation erhöht die minimale Rechenschrittweite
($10^{-8}\,\text{s}$ &#8594; $1\,\text{s}$), die maximale Rechenschrittweite ($0.04\,\text{s}$ &#8594; $2\,\text{s}$) sowie 
die absolute und relative Toleranz ($10^{-5}\,\text{s}$ &#8594; $1\,\text{s}$).

```{list-table}
* - ```{admonition} Modifikation 1:
	:class: dropdown
	```modelica
		extends SimModel(
			tStop=4,
			dtProtMin=1);
	```
  - ```{admonition} Modifikation 2:
	:class: dropdown
	```modelica
		extends SimModel(
			tStop=4,
			absTol=1,
			relTol=1,
			dtMin=1,
			dtMax=2);
	```
```
Wie in {numref}`ergebnis_modelica_vgl_Mod` abgebildet, haben die Simulationseinstellungen
einen großen Einfluss auf die Ergebnisse. Da die erste Modifikation lediglich die Ausgabeschrittweite,
nicht aber die eigentlichen Rechenschritte vergrößert, entsprechen die Werte, die ausgegeben werden 
den ursprünglichen Ergebnissen. Die Bereiche zwischen den diskreten Ergebnissen werden von den 
Auswertungsfunktionen in SimulationX standardmäßig linear interpoliert, sind aber eigentlich nicht 
Teil des Simulationsergebnisses (siehe {numref}`ergebnis_modelica_vgl_point_Mod`).

::::{grid} 2
:::{grid-item}
```{figure} ../data/img/01_Beispiel_Modelica_vgl_Mod.png
:width: 500px
:name: ergebnis_modelica_vgl_Mod
Auswirkung unterschiedlicher Simulationseinstellungen auf das numerische Ergebnis.
```
:::
:::{grid-item}
```{figure} ../data/img/01_Beispiel_Modelica_vgl_point_Mod.png
:width: 500px
:name: ergebnis_modelica_vgl_point_Mod
Auswirkung unterschiedlicher Simulationseinstellungen auf das numerische Ergebnis ohne lineare Interpolation.
```
:::
::::

Die zweite Modifikation verändert die Simulationseinstellungen wesentlich grundlegender, sodass starke Abweichungen des Ergebnisses im Vergleich zur analytischen Lösung zu erkennen sind.