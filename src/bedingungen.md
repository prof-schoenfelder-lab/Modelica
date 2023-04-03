# Bedingungen

Die bisherigen Modelica Modelle wurde durch einheitlich definierte Funktionen beschrieben. Gegebenenfalls lassen sich reale Zusammenhänge nicht immer durch solche Funktionen abbilden. Die Randbedingung unseres Modells könnte sich z.B. aufgrund von einem Ereignis verändern. Im Einführungsbeispiel ([Aufgabenstellung](einfuehrung:beispiel:aufgabe)) bei dem ein Körper durch Konvektion abkühlt, kommt es zu einer plötzlichen Temperaturänderung der Umgebungsluft. Bisher haben wir die Änderung der Randbedingung vermieden, indem die Simulation erst startet, wenn der Körper in der kühleren Umgebung ist. Ereignisse ermöglichen allerdings die Modellierung solcher Änderungen.

Der formale Zusammenhang der plötzlichen Änderung der Umgebungstemperatur ändert zu stückweise definierten Funktionen:
::::{grid}
:::{grid-item}
:columns: 6
```{math}
T_U = 290\,\text{K}
```
:::
:::{grid-item}
:columns: 6
```{math}
T_U(t) = 
\begin{cases}
    360\,\text{K} & \text{if }t<2\,\text{s} \\
    290\,\text{K} & \text{if }t\geq2\,\text{s} \\
\end{cases}
```
:::
:::{grid-item}
:columns: 6
```{figure} ../data/img/RandbedingungKonstant.png
:width: 500px
:name: RBKonstant
Konstante Randbedingung
```
:::
:::{grid-item}
:columns: 6
```{figure} ../data/img/RandbedingungStueckweise.png
:width: 500px
:name: RBStueck
Stückweise Randbedingung
```
:::
::::

Die neue Definition der Randbedingung hat Konsequenzen für das Modelica Modell.
1. Die Variable $T_U$ verliert das Signalwort `parameter` und die Zuweisung eines Zahlenwerts, da sie als zweite zeitabhängige Unbekannte betrachtet wird.
2. Entsprechend der Anzahl an Unbekannten muss eine zweite Gleichung für $T_U$ definiert werden. Hier müssen die Fälle $t<2\,\text{s}$ und $t\geq2\,\text{s}$ berücksichtigt werden.

## If-Anweisung
Die Fallunterscheidung wird mit **If**-Anweisung wie folgt umgesetzt:

```````{tab-set}
````{tab-item} Modelica Modell
```modelica
model Abkuehlung
  extends SimModel(tStop=6);
  parameter Real T_K0(unit="K")       =360 "Anfangstemperatur";
  parameter Real h   (unit="W/(m2.K)")=0.7 "Wärmeübergangskoeffizient";
  parameter Real A   (unit="m2")      =1.0 "Oberfläche";
  parameter Real m   (unit="kg")      =0.3 "Masse";
  parameter Real c   (unit="J/(kg.K)")=1.2 "spezifische Wärmekapazität";
  Real T_K(unit="K") "Temperatur";
  Real T_U(unit="K") "Temperatur der Umgebungsluft";
  initial equation
    T_K = T_K0;
  equation
    c*m*der(T_K) = h*A*(T_U-T_K);
    if (time<2)then
      T_U = 360;
    else
      T_U = 290;
    end if;
end Abkuehlung;
```
````
````{tab-item} Modelica Ergebnis
```{figure} ../data/img/AbkuehlungStueckweise.png
:width: 350px
```
````
```````
```{admonition} Erinnerung
Modelica interpoliert standardmäßig linear zwischen den einzelnen Zeitschritten.
```

## When-Anweisung
Neben der **If**-Anweisung gibt es die **When**-Anweisung, deren Inhalt (im Gegensatz zu `if`) nur ausgeführt wird, wenn ihre Bedingung erfüllt wird. Die Umsetzung der gleichen Randbedingung mit `when` sieht wie folgt aus:
```````{tab-set}
````{tab-item} Modelica Modell
```modelica
model Abkuehlung
  extends SimModel(tStop=6);
  parameter Real T_K0(unit="K")       =360 "Anfangstemperatur";
  parameter Real h   (unit="W/(m2.K)")=0.7 "Wärmeübergangskoeffizient";
  parameter Real A   (unit="m2")      =1.0 "Oberfläche";
  parameter Real m   (unit="kg")      =0.3 "Masse";
  parameter Real c   (unit="J/(kg.K)")=1.2 "spezifische Wärmekapazität";
  Real T_K(unit="K") "Temperatur";
  Real T_U(unit="K") "Temperatur der Umgebungsluft";
  initial equation
    T_K = T_K0;
    T_U = 360;
  equation
    c*m*der(T_K) = h*A*(T_U-T_K);
    when time>=2 then
      T_U = 290;
    end when;
end Abkuehlung;
```
````
````{tab-item} Modelica Ergebnis
```{figure} ../data/img/AbkuehlungStueckweise.png
:width: 350px
```
````
```````

## Beispiel: Springender Ball
Bedingungen müssen nicht auf der Variable `time` basieren. Die Einführung von [Anfangsbedingungen](einfuehrung:grundlagen:anfangsbedingungen) wurde am Beispiel des freien Falls veranschaulicht. Basierend auf diesem Modell, stellen wir uns vor, dass ein Ball fällt und wieder nach oben springt, wenn er auf den Boden trifft. Beim Aufprall kommt es zu einem [realen Stoß](https://de.wikipedia.org/wiki/Sto%C3%9F_(Physik)). $80\,$% der mechanischen Energie bleiben erhalten und $20\,$% werden in innere Energie umgewandelt (Restitutionskoeffizient $k=0.8$).
`````{margin}
```{card}
:link: https://build.openmodelica.org/Documentation/ModelicaReference.Operators.%27reinit()%27.html
:class-header: bg-light
:text-align: center
`reinit()`
^^^
Der Operator ermöglicht die Initialisierung einer Variable ähnlich der Umgebung `initial equation`.
```
```{card}
:link: https://build.openmodelica.org/Documentation/ModelicaReference.Operators.%27pre()%27.html
:class-header: bg-light
:text-align: center
`pre()`
^^^
Der Operator stellt das letzte berechnete Ergebis der Variable zur Verfügung.
```
`````

```````{tab-set}
````{tab-item} Modelica Modell
```modelica
model SpringenderBall
  extends SimModel(tStop=3);
  Real h(unit="m") "Höhe";
  Real v(unit="m/s") "Geschwindigkeit";
  parameter Real h0(unit="m")  =1   "Anfangshöhe";
  parameter Real v0(unit="m/s")=0   "Anfangsgeschwindigkeit";
  parameter Real k             =0.8 "Restitutionskoeffizient";
  initial equation
    h = h0;
    v = v0;
  equation
    v = der(h);
    der(v) = -9.81;
    when h<0 then
      reinit(v, -k*pre(v));
    end when;
end SpringenderBall;
```
````
````{tab-item} Modelica Ergebnis
```{figure} ../data/img/SpringenderBall.png
:width: 450px
```
````
```````