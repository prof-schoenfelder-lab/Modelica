# Objektorientierte Programmierung

Im Folgenden geht es vordergründig um die **objektorientierte Programmierung** (OOP), die einen zentralen Punkt für die **Skalierbarkeit** und **Wiederverwendbarkeit** der Modelica Modelle darstellt. Dieser Begriff beschreibt eine allgemeine Herangehensweise an die Struktur des Programms.

Das Grundkonzept der OOP besteht aus sogenannten **Klassen**, die eine Blaupause festlegen. Klassen sind **Attribute** und **Methoden** zugeordnet. Diese Klassen können instanziiert werden, um **Objekte** von dieser Klasse zu erzeugen. 
```{card}
:class-header: bg-light
:text-align: center
:width: 50%
:margin: auto
Name der Klasse
^^^
Attribute der Klasse
+++
Methoden der Klasse
```

Verdeutlichen wir die eingeführte Terminologie an einem Beispiel. In einem Zeichenprogramm soll ein Dreieck dargestellt werden, deshalb wird die Klasse Dreieck definiert. Sie besitzt 5 Attribute und 2 Methoden. Um ein Dreieck zu zeichnen, also ein Objekt dieser Klasse zu instanziieren, wird diese Blaupause genutzt. Das gezeichnete Dreieck wird `D1` genannt. Es können beliebig viele Objekte der Klasse Dreieck instanziiert werden (siehe Objekt `D2`).



```````{tab-set}
``````{tab-item} Bespiel: Klasse
`````{grid}
```{grid-item-card}
:class-header: bg-light
:text-align: center
:columns: 6
Dreieck
^^^
P1

P2

P3

A

U
+++
calcA()

calcU()
```
````{grid-item}
```{list-table}
* - A
  - Flächeninhalt
* - P
  - Punkt
* - U
  - Umfang
```
````
`````
``````
``````{tab-item} Beispiel: Objekte
`````{grid}
```{grid-item-card}
:class-header: bg-light
:text-align: center
:columns: 2
D1
^^^
P1=[0, 0]

P2=[4, 0]

P3=[0, 3]

A=0

U=0
+++
calcA()

calcU()
```
```{grid-item-card}
:class-header: bg-light
:text-align: center
:columns: 2
D2
^^^
P1=[2, 3]

P2=[1, 4]

P3=[5, 3]

A=0

U=0
+++
calcA()

calcU()
```
````{grid-item}
```{figure} ../data/img/D1.png
:width: 350px
```
````
`````
``````
``````{tab-item} Beispiel: Methoden
`````{grid}
````{grid-item}
:columns: 6
```python
D1.calcA()
D2.calcA()
D1.calcU()
D2.calcU()
```
````
```{grid-item-card}
:class-header: bg-light
:text-align: center
:columns: 3
D1
^^^
P1=[0, 0]

P2=[4, 0]

P3=[0, 3]

A=6

U=12
+++
calcA()

calcU()
```
```{grid-item-card}
:class-header: bg-light
:text-align: center
:columns: 3
D2
^^^
P1=[2, 3]

P2=[1, 4]

P3=[5, 3]

A=1.5

U=8.54
+++
calcA()

calcU()
```
`````
``````
```````
Auffällig ist, dass die Attribute `A` und `U` den Wert `0` zugewiesen haben. Eigentlich haben die Objekte `D1` und `D2` aber einen Flächeninhalt und einen Umfang. Methoden sind in der Lage Attribute zu verändern. In diesem Fall müssen erst die Methoden `calcA()` und `calcB()` ausgeführt werden, um den Flächeninhalt bzw. Umfang zu berechnen.

**Aufgabe**


Neben der Klasse `Dreieck` können weiter Klassen wie z.B. `Viereck` oder `Kreis` definiert werden. 
````{grid}
```{grid-item-card}
:class-header: bg-light
:text-align: center
Dreieck
^^^
P1

P2

P3

A

U
+++
calcA()

calcU()
```
```{grid-item-card}
:class-header: bg-light
:text-align: center
Viereck
^^^
P1

P2

P3

P4

A

U
+++
calcA()

calcU()
```
```{grid-item-card}
:class-header: bg-light
:text-align: center
Kreis
^^^
P1

R

A

U
+++
calcA()

calcU()
```
````
Um Arbeit zu sparen oder Strukturen vorzugeben können Gemeinsamkeiten von Klassen **vererbt** werden. Alle definierten Klassen besitzen die Attribute `P1`, `A`, `U` und die Methoden `calcA()` und `calcU()`. Wir können eine übergeordnete Klasse `Fläche` einführen, die diese Attribute und Methoden besitzt, um redundante Definitionen zu sparen.
```{card}
:class-header: bg-light
:text-align: center
:width: 50%
:margin: 0 3 auto auto 
Fläche
^^^
P1

A

U
+++
calcA()

calcU()
```
````{grid}
```{grid-item-card}
:class-header: bg-light
:text-align: center
Dreieck
^^^

P2

P3

```
```{grid-item-card}
:class-header: bg-light
:text-align: center
Viereck
^^^
P2

P3

P4

```
```{grid-item-card}
:class-header: bg-light
:text-align: center
Kreis
^^^
R
````
Da die Klassen `Dreieck`, `Viereck` und `Kreis` alle Attribute und Methoden von der Klasse `Fläche` erben, entsprechen sie der Definition oberhalb. Ein instanziiertes Objekt, z.B. das Dreieck `D1` kann also auf `D1.calcA()` zugreifen.

Es ist nicht immer sinnvoll auf Attribute oder Methoden von Objekten zugreifen zu können. Inwiefern bzw. von wo zugegriffen werden kann, wird von **Zugriffsbeschränkungen** festgelegt. Allgemein gibt es drei Arten von Beschränkungen:
```{list-table}
* - **public**
  - auf Attribute und Methoden darf von jeder Klasse aus zugegriffen werden
* - **protected**
  - auf Attribute und Methoden darf von der Klasse selbst und von ihren Kindern (Klassen die geerbt haben) zugegriffen werden
* - privat (gibt es in Modelica nicht)
  - auf Attribute und Methoden darf nur die Klasse selbst zugreifen
```

```{admonition} Hinweis
Wenn nichts genauer angegeben wird, sind z.B. Variablen in Modelica standardmäßig **public** 
```

**Aufgabe:** Welche Klassen würden Sie für das Modell [Abkühlung eines Raumes](einfuehrung:grundlagen:abkuehlung) definieren, wenn Sie objektorientiert arbeiten sollen?

<!-- `````{admonition} Lösung:
:class: dropdown
```{list-table}
:header-rows: 1
* - naheliegend
  - abstrakter
* - Raum
  - thermische Masse
* - Wand
  - Konduktion
* - Außenluft
  - Feste Temperatur

````` -->

`````{admonition} Lösung:
:class: dropdown
`````{grid}
````{grid-item-card}
naheliegend:
1. Raum
2. Wand
3. Außenluft
````
````{grid-item-card}
abstrakter:
1. thermische Masse
2. Konduktion
3. Feste Temperatur
````
`````