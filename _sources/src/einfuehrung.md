# Einführung

**Was ist Modelica?**

Modelica ist eine Programmiersprache.
Im Gegensatz zu Sprachen wie Python, C++ oder Java, sogenannte **imperative**
Sprachen bei denen der Anwender den Ablauf in einer Reihenfolge definiert,
handelt es sich bei Modelica um eine **deklarative** Sprache, 
die ein Problem beschreibt, aber nicht wie es zu lösen ist.

Genutzt wird Modelica, um mathematische Probleme zu lösen.
Diese Probleme können aus den unterschiedlichsten Bereichen wie z.B. 
der Mechanik, der Thermodynamik, der Elektrotechnik und vielen weiteren stammen.


**Simulationsumgebungen**

Da ein geschriebenes Modelica Programm lediglich einer Problembeschreibung 
entspricht, wird eine Simulationsumgebung benötigt, die in der Lage ist 
das Problem zu lösen. Diese Simulationsumgebungen bestehen in der Regel aus 
einer grafischen Benutzeroberfläche, numerischen Lösungsalgorithmen und
Auswertungsfunktionen.

```{margin} 
Weitere Simulationsumgebungen sind auf der Webseite der [Modelica Association](https://modelica.org/tools.html) gelistet.
```

Gängige Simulationsumgebungen:
* [Dymola](https://www.3ds.com/de/produkte-und-services/catia/produkte/dymola/)
* [SimulationX](https://www.esi-group.com/products/system-simulation)
* [OpenModelica](https://openmodelica.org/)
  
```{admonition} Ergänzung
Die Simulationsumgebungen Dymola und SimulationX sind kommerzielle Softwareprodukte.
Bei OpenModelica handelt es sich um ein *open source* Projekt, das frei verfügbar ist. 
```

```{admonition} Hinweis
Die Modelica Beispiele in diesem Buch sind für die Simulationsumgebung SimulationX geschrieben.
```