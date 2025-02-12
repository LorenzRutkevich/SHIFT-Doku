---
title: "Die Interaktive Segmentation im Überblick"
summary: "Mittels wenigen Klicks zu einer vollen und präzisen Segmentierung von Zellkernen und Zellstrukturen – und darüber hinaus: universelle Ansätze für ungelenkte Segmentierungsaufgaben."
#externalUrl: ""
#showSummary: true
date: 2024-11-06
draft: false
---

## Die Methode hinter der interaktiven Segmentation

Die Basis hinter dem Ansatz stellt [Koohbanani et al.](https://arxiv.org/pdf/2005.14511) dar. Hierfür werden sogenannte **Inclusion Maps** und **Exclusion Maps** als Leitlinien genutzt, um präzise und effiziente Segmentierungen zu erzeugen – und das mit minimalem Benutzereingriff.  

![Überblick der Methode](https://raw.githubusercontent.com/LorenzRutkevich/SHIFT-Doku/refs/heads/images/images/interactive-maps.png)  

---

### Inclusion Map

Die Inclusion Map wird erstellt, indem ein Benutzer innerhalb eines Zellkerns klickt. Dieser Klick erzeugt eine Karte, bei der der geklickte Punkt auf "1" gesetzt wird, während alle anderen Pixel den Wert "0" erhalten. Diese Karte hilft dem Modell, den genauen Bereich des Zellkerns zu erkennen, auf den sich der Benutzer konzentrieren möchte. Ist beispielsweise nur ein Kern von Interesse, genügt die Inclusion Map als präziser Hinweis.

---

### Exclusion Map

In Szenarien, in denen mehrere Zellkerne dicht beieinander liegen, wird zusätzlich eine Exclusion Map erstellt. Diese Karte markiert alle anderen Zellkerne im Bildausschnitt – auch hier werden die entsprechenden Pixel auf "1" gesetzt, während alle übrigen Pixel den Wert "0" behalten. So kann das Modell zwischen dicht gepackten Kernen unterscheiden und sicherstellen, dass sich die Segmentierung ausschließlich auf den gewünschten Zellkern konzentriert.

---

### Während des Trainings

Im Trainingsprozess werden die Inclusion- und Exclusion Maps variabel eingesetzt. Für die Inclusion Map wird der Klickpunkt zufällig innerhalb des Kerns gewählt – dabei wird ein Mindestabstand zu den Randpixeln eingehalten, um die Robustheit des Modells zu erhöhen. Die Exclusion Map wird auf Basis der Zentren der übrigen Kerne erstellt. Diese Vorgehensweise lehrt das Modell, auch dann eine präzise Segmentierung vorzunehmen, wenn die Eingaben nicht exakt im Kernzentrum liegen.

---

### Ergebnisse der gelenkten (interaktiven) Segmentierung

Die im Rahmen dieses Projekts entwickelten Architekturen übertreffen derzeit alle vergleichbaren Modelle in der interaktiven Segmentierung. Der zugrunde liegende Datensatz wurde speziell für dieses Projekt kuratiert und umfasst 56.130 Trainingsbilder mit zugehörigen Masken sowie 9.905 Testbilder. Die Modelle wurden auf Patches der Größe 128×128 trainiert, was dem Standardansatz dieser Methode entspricht.

Im Folgenden ein Vergleich einiger Modelle aus der Literatur sowie der eigens entwickelten Ansätze:

| Model                         | Dice    | Jaccard | F1     | Parameters    |
|-------------------------------|---------|---------|--------|---------------|
| Nuclick                       | 84,11   | 85,42   | 74,57  | 66.800.321    |
| UNet                          | 84,61   | 85,61   | 74,85  | 12.583.425    |
| <span style="color:#00f5bc">NucDep</span>  | 85,39   | 86,65   | 76,46  | 69.944.961    |
| UDTransNet                    | 86,46   | 87,88   | 78,41  | 33.799.617    |
| <span style="color:#00f5bc">SHIFT-S</span> | 86,9    | 88,2    | 78,92  | 12.845.537    |
| <span style="color:#00f5bc">SHIFT</span>   | **88,0**| **89,56**| **81,0**| 28.817.665    |

Die Ergebnisse zeigen, dass der Einsatz von Attention-Mechanismen, Depthwise-Separable-Convolutions und vortrainierten Schichten in der Architektur – unter dem Namen **SHIFT** (bisher ESRUT-Net) – zu einer signifikanten Verbesserung der Segmentierungsleistung führt. Im Vergleich zum UDTransNet erzielt SHIFT einen um 1,5 Punkte höheren Dice-Score und einen um 2,6 Punkte besseren F1-Score, bei gleichzeitig effizienterer Parameterverwendung.

---

## Ungelenkte Segmentierung

Neben der interaktiven (gelenkten) Segmentierung zeigt die Architektur ihre universelle Anwendbarkeit auch in ungelenkten Szenarien, bei denen keine zusätzlichen Benutzereingaben vorliegen. Ein besonders anschauliches Beispiel stellt die Segmentierung von Drüsen im Dickdarm dar – eine medizinisch relevante Aufgabe zur Beurteilung des Zustands bei Dickdarmkrebs. Für diese Aufgabe wurde der CRAG-Datensatz genutzt, der ursprünglich zur Evaluierung von Segmentierungsmodellen wie MILD-Net eingeführt wurde.

SHIFT erzielt in dieser ungelenkten Segmentierung herausragende Ergebnisse:
- **Dice-Score:** 93,4
- **F1-Score:** 92,7

Im Vergleich dazu erreichen andere Modelle folgende Werte:

| Modell      | Dice-Score | F1    |
|-------------|------------|-------|
| **SHIFT**   | **93,4**   | **92,7** |
| PatchCL     | 89,2       | 88,1  |
| Mild-Net    | 88,3       | 86,9  |
| G-CNN (C12) | 83,4       | 81,8  |

Diese Ergebnisse unterstreichen, dass SHIFT – durch die Kombination aus der Erfassung lokaler und globaler Merkmale – nicht nur in der interaktiven, sondern auch in der ungelenkten Segmentierung hervorragende Leistungen erbringt. Die Fähigkeit, präzise und zusammenhängende Regionen zu erfassen, macht SHIFT zu einer optimalen Lösung für verschiedenste medizinische Segmentierungsaufgaben.

---

## Zusammenfassung

Der vorgestellte Ansatz zeigt eindrucksvoll, wie bereits mit minimalen Benutzereingaben durch Inclusion- und Exclusion Maps präzise Segmentierungen erreicht werden können. Gleichzeitig beweist die Architektur unter dem Namen **SHIFT**, dass sie auch in ungelenkten Anwendungen – wie der Drüsensegmentierung – ihre Überlegenheit demonstriert. Die systematische Integration moderner CNN-Module und Attention-Mechanismen führt zu einer signifikanten Verbesserung der Segmentierungsqualität, was insbesondere in ressourcenarmen Szenarien von großem Vorteil ist.
