---
title: "Ablationsstudie der SHIFT-Architektur"
summary: "Eine detaillierte Analyse, wie verschiedene Konfigurationskomponenten der SHIFT-Architektur die Segmentierungsleistung beeinflussen."
date: 2025-02-10
draft: false
---

## Überblick

In diesem Beitrag stellen wir die Ergebnisse einer umfassenden Ablationsstudie vor, mit der wir die Effektivität der einzelnen Komponenten innerhalb der **SHIFT-Architektur** untersucht haben. Ziel war es, den Einfluss von Attention-Mechanismen, spezialisierten CNN-Modulen (wie Depthwise-Separable-Convolutions und MB-Conv) sowie vortrainierten Gewichten auf die Metriken *Dice-Score* und *F1-Score* sowie auf die Modellkomplexität zu evaluieren.

Dabei wurden sechs Varianten des Netzwerks entwickelt und systematisch modifiziert, um zu analysieren, welche Konfigurationen die beste Performance liefern – insbesondere im Kontext der interaktiven Segmentierung.

## Ablationsstudie im Detail

Die Ergebnisse der getesteten Modelle fassen wir in folgender Tabelle zusammen:

| Dice-Score | F1    | Parameter   | F/b (MB) | Encoder                                         | Bridge           | Decoder                                                      |
|------------|-------|-------------|----------|-------------------------------------------------|------------------|--------------------------------------------------------------|
| 88,0       | 81,0  | 28.817.665  | 497,88   | 2x SepConv ✔, Residual ✔, Attention ✔           | Pretrained ✔     | 2x SepConv ✔, Residual ✔, Attention ✔                         |
| 87,8       | 80,4  | 31.836.865  | 984,88   | 2x SepConv ✔, Residual ✔, Attention ✔           | Pretrained ✔     | 2x SepConv ✔, Residual ✔, Attention ✔                         |
| 87,6       | 80,2  | 37.751.681  | 331,25   | 1x SepConv ✔, Residual ✘, Attention ✘           | Pretrained ✔     | 1x SepConv ✘, Residual ✘, Attention ✘                         |
| 87,6       | 80,0  | 35.012.957  | 1037,12  | 1x SepConv ✔, Residual ✔, Attention ✔           | Pretrained ✔     | 1x SepConv ✔, Residual ✔, Attention ✔, MBConv ✔                |
| 87,3       | 79,6  | 46.663.261  | 801,75   | 2x SepConv ✘, Residual ✔, Attention ✔           | Pretrained ✔     | 2x SepConv ✘, Residual ✔, Attention ✔, MBConv ✔                |
| 86,9       | 78,9  | 12.845.537  | 380,38   | 1x SepConv ✔, Residual ✘, Attention ✘           | Pretrained ✘     | 1x SepConv ✘, Residual ✘, Attention ✘                         |

Die Tabelle zeigt, dass die Integration von Attention-Mechanismen – insbesondere im Encoder und Decoder – zu den höchsten *Dice-* und *F1-Scores* führt. Im Folgenden gehen wir näher auf die wichtigsten untersuchten Komponenten ein.

---

### Attention-Mechanismen

Zwei unterschiedliche Attention-Mechanismen wurden evaluiert: **Efficient-Attention** und **Quick-Attention**. Während Efficient-Attention globale Zusammenhänge im Bild berücksichtigt und so präzisere Ergebnisse liefert, besticht Quick-Attention durch einen deutlich geringeren Rechenaufwand.

Beim Quick-Attention-Mechanismus wird zunächst auf die Eingabe-Feature-Map eine spezielle Faltung angewendet, die dafür sorgt, dass wichtige Merkmale hervorgehoben werden. Anschließend wird das Ergebnis einer Aktivierung unterzogen, die die Werte normalisiert. Schließlich wird dieses aktivierte Ergebnis zur ursprünglichen Feature-Map hinzugefügt. Vereinfacht gesagt, wird die Feature-Map in zwei Teile geteilt: Einer wird verarbeitet, und das Ergebnis wird dann mit dem unberührten Teil kombiniert, um die finale Attention-Karte zu erhalten.

Diese Vorgehensweise ist ressourcenschonender als alternative Methoden, wobei der Fokus bei Efficient-Attention auf der Erfassung globaler Abhängigkeiten liegt, was zu einer noch höheren Präzision führt.

---

### CNN-Module

Ein weiterer Schwerpunkt der Studie lag auf dem Einsatz spezialisierter CNN-Module:

- **Depthwise-Separable-Convolutions (DSConv):**  
  Diese Module ermöglichen es, den Rechenaufwand erheblich zu reduzieren, ohne dabei signifikante Einbußen bei der Genauigkeit zu riskieren. Zum Vergleich wurden auch herkömmliche Convolutionen getestet, die zwar präzise Ergebnisse lieferten, jedoch weniger effizient arbeiteten.

- **MB-Conv Module:**  
  Dieses Modul kombiniert mehrere Faltungsschritte, um die Anzahl der Parameter zu senken und gleichzeitig die Tiefe des Modells zu erhöhen. Trotz des zusätzlichen Rechenaufwands führte der Einsatz von MB-Conv nicht zu einer deutlichen Verbesserung der Ergebnisse, sondern erhöhte hauptsächlich die Modellkomplexität.

---

### Vortrainierte Gewichte

Ein wesentlicher Faktor für die Leistungssteigerung war der Einsatz vortrainierter ResNet-Schichten in der sogenannten *Bridge*-Komponente der Architektur. Diese vortrainierten Gewichte sorgen für eine robuste Initialisierung und verbessern die Fähigkeit des Netzwerks, relevante Merkmale aus den Eingabedaten zu extrahieren – ein besonderer Vorteil, wenn nur begrenzte Trainingsdaten zur Verfügung stehen. Allerdings führt diese Vorgehensweise zu einem Anstieg der Modellparameter und des Speicherbedarfs.

---

## Fazit

Die Ablationsstudie zeigt deutlich, dass die gezielte Integration von Attention-Mechanismen innerhalb der SHIFT-Architektur zu einer signifikanten Leistungssteigerung führt – insbesondere im Hinblick auf die Metriken Dice-Score und F1-Score. Obwohl zusätzliche Module wie MB-Conv die Tiefe des Modells erweitern, sollten sie aufgrund des erhöhten Ressourcenverbrauchs mit Bedacht eingesetzt werden. Ebenso verbessert der Einsatz vortrainierter Gewichte die Merkmalextraktion, erfordert jedoch auch mehr Speicher.

Diese Erkenntnisse bieten wertvolle Hinweise für die Optimierung zukünftiger Architekturen im Bereich der interaktiven Segmentierung und zeigen, wie durch systematische Variationen einzelner Komponenten die Modellleistung weiter verbessert werden kann.
