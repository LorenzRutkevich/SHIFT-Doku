---
title:  "Erstellung des Datensatzes"
summary: "Für das Training der Modelle von SHIFT wurde ein eigener Datensatz erstellt, der viele Gewebestrukturen präzise abdeckt."
#externalUrl: ""
#showSummary: true
date: 2024-11-06
draft: false
---

### Bestandteile

Der für das Training der interaktiven Methode von SHIFT erstellte Datensatz ist eine Zusammenstellung öffentlich zugänglicher Datensätze aus Veröffentlichungen und Challenges. Alle enthaltenen Datensätze stammen aus ethisch vertretbaren Quellen und zeichnen sich durch hohe Qualität und Diversität aus. Zu den Bestandteilen dieses Datensatzes zählen beispielsweise [MoNuSeg](https://ieeexplore.ieee.org/document/8880654) und der [PanNuke](https://arxiv.org/pdf/2003.10778v7) Datensatz, wobei der Großteil des Materials aus menschlichen Proben unterschiedlichster Organe besteht. Überlappende Bilder aus verschiedenen Datensätzen wurden entfernt, um Redundanzen zu vermeiden.

![Bild und korrespondierende Maske im MoNuseg-Datensatz](https://raw.githubusercontent.com/LorenzRutkevich/SHIFT-Doku/refs/heads/images/images/MoNuSeg.png)  

### Vorbereitung

Für die automatische Generierung von Trainings-Batches für die interaktive Methode sind vorzugsweise Instanzmasken erforderlich. Diese ermöglichen es, jeden Zellkern als separates Objekt auf einer individuellen Teilmaske darzustellen. Da solche Instanzmasken nicht in allen Datensätzen vorhanden sind, können semantische Masken genutzt werden, bei denen sich die Zellkerne möglicherweise nicht unmittelbar voneinander trennen lassen. Durch Subtraktion vorhandener Randmasken lassen sich die Zellkerne in diesen semantischen Masken voneinander separieren, was die Erstellung von Instanzmasken ermöglicht.

Die Teilgröße jedes Bildes beträgt 128x128 Pixel. Um den Datensatz weiter zu erweitern, wurden größere Bilder in Teilbilder dieser Größe unterteilt. So entstand ein Datensatz mit insgesamt 56.130 Trainingsbildern und zugehörigen Masken sowie 9.905 Testbildern mit entsprechenden Masken.

![Train/Test-Aufteilung](https://raw.githubusercontent.com/LorenzRutkevich/SHIFT-Doku/refs/heads/images/images/data_chart.png)  
