# GOAT-Analyse der PDC Darts World Championship

Eine netzwerkbasierte Untersuchung der Turnierjahre 1994 bis 2026

Projektarbeit im Modul **Netzwerkprojekt Wirtschaftsinformatik**, Leuphana Universität Lüneburg (Prof. Dr. Peter Niemeyer), Sommersemester 2026.

---

## Fragestellung

> Wer ist der Greatest of All Time der PDC World Darts Championship, gemessen an einer netzwerkbasierten Zentralitätsanalyse aller Head-to-Head-Ergebnisse von 1994 bis 2026?

Sportliche GOAT-Debatten werden meist über einfache Kennzahlen wie Titel oder Siegquoten geführt, dabei bleibt unberücksichtigt, *gegen wen* diese Siege errungen wurden. Ein Sieg gegen einen Weltklassespieler sollte mehr zählen als ein Sieg gegen einen Erstrunden-Qualifikanten. Dieses Projekt modelliert die PDC World Championship als gerichtetes, gewichtetes Netzwerk und beantwortet die GOAT-Frage mit Mitteln der Graphentheorie.

## Theoretischer Hintergrund und Bezug zu Temesi et al. (2023)

Die Arbeit überträgt die Methodik von **Temesi, J., Szádoczki, Z. & Bozóki, S. (2023): *Incomplete pairwise comparison matrices: Ranking top women tennis players*, Journal of the Operational Research Society** vom Tennis auf den Profidarts-Sport.

**Ausgangsproblem im Paper:** Temesi et al. wollen 28 Top-Tennisspielerinnen anhand ihrer direkten Duelle ranken. Das zentrale methodische Problem dabei: In K.O.-Turnieren spielt nicht jede Spielerin gegen jede andere. Es liegt also keine vollständige Paarvergleichsmatrix vor, sondern eine *unvollständige*: Viele Zellen der Matrix bleiben leer, weil das entsprechende Duell nie stattgefunden hat. Klassische Ranking-Verfahren, die auf vollständigen Vergleichsmatrizen aufbauen, sind darauf nicht direkt anwendbar. Die Autoren begegnen diesem Problem, indem sie die Duelle als Graph auffassen und darauf netzwerkanalytische bzw. graphentheoretische Rankingverfahren anwenden, die explizit mit unvollständigen, gerichteten Vergleichsstrukturen umgehen können.

**Übertragung auf Darts:** Die PDC World Championship weist exakt dieselbe strukturelle Eigenschaft auf wie das Tennisturnier im Paper: Sie wird im K.O.-Format ausgetragen, wodurch das Netzwerk aller möglichen Spielerpaarungen zwangsläufig unvollständig bleibt (in diesem Projekt wurden nur 6,3 % aller theoretisch möglichen Paarungen tatsächlich ausgetragen). Diese strukturelle Analogie ist die Begründung dafür, warum sich der Ansatz von Temesi et al. auf Darts übertragen lässt, obwohl es sich um eine andere Sportart handelt:

| Element im Paper (Tennis) | Umsetzung in diesem Projekt (Darts) |
|---|---|
| 28 Top-Spielerinnen, direkte Duelle | 125 Spieler (≥ 10 Matches), 972 direkte Duelle |
| Unvollständige Paarvergleichsmatrix | Unvollständiges Netzwerk (Dichte 6,3 %) |
| Gerichtete Kante als Ausdruck eines Sieges | Gerichtete Kante vom Verlierer zum Sieger |
| Mehrfachduelle als Information über Dominanz | Kantengewicht = Anzahl Siege gegen denselben Gegner |
| Netzwerkbasiertes Ranking statt reiner Siegzählung | Eigenvektor-Zentralität als Rankingmaß |
| Validierung des Rankings über mehrere Verfahren | Robustheitscheck via PageRank und Spearman-Korrelation |

**Warum Eigenvektor-Zentralität als Rankingmaß:** Der konzeptionelle Kerngedanke von Temesi et al., dass die *Qualität* der Gegner in die Bewertung einfließen muss und nicht nur die *Anzahl* der Siege, wird in diesem Projekt durch die Eigenvektor-Zentralität umgesetzt. Ihr Wert für einen Spieler hängt rekursiv von den Werten seiner besiegten Gegner ab: Ein Sieg gegen einen selbst zentralen (starken) Spieler trägt stärker zum eigenen Wert bei als ein Sieg gegen einen peripheren Spieler. Das ist die netzwerkanalytische Entsprechung dessen, was im Paper über die Auswertung der unvollständigen Vergleichsmatrix erreicht wird.

**Ergänzend zum Paper:** Über die im Paper verwendete Methodik hinaus wird in diesem Projekt zusätzlich die Robustheit des Ergebnisses geprüft, indem das Eigenvektor-Ranking mit einem PageRank-Ranking verglichen wird (Spearman-Rangkorrelation). Beide Verfahren beruhen auf demselben Grundprinzip (Stärke überträgt sich über Siege), unterscheiden sich aber in der Berechnung (PageRank nutzt einen Dämpfungsfaktor). Eine hohe Übereinstimmung zeigt, dass das Ranking nicht von der Wahl eines einzelnen Verfahrens abhängt.

**Kontext:** Das Projekt entstand als Prüfungsleistung im Modul *Netzwerkprojekt Wirtschaftsinformatik* an der Leuphana Universität Lüneburg. Aufgabenstellung war, Konzepte aus Netzwerkanalyse und Turnierbewertung eigenständig auf ein selbst gewähltes Sportturnier anzuwenden und dabei den graphentheoretischen Kern der Veranstaltung (explizite Modellierung als Graph, Anwendung von Zentralitäts-/Rankingmaßen) sichtbar zu nutzen.

## Daten

- **Quelle:** [dartsdatabase.co.uk](https://www.dartsdatabase.co.uk), erhoben per eigenem Python-Scraping-Skript
- **Umfang:** 2.110 Matches aus 33 Austragungen (1994–2026), 529 Spieler
- **Nach Filterung** (min. 10 Matches pro Spieler, analog zum Ausschluss statistisch wenig aussagekräftiger Einmalteilnehmer): 125 Spieler, 1.126 Matches → Netzwerk mit 125 Ecken und 972 Kanten
- Stichprobenartig gegen offizielle Ergebnisse validiert (u. a. Finale 1994 und 2024)

## Methodik

Das Turnier wird als gerichteter, gewichteter Graph `G = (V, E)` modelliert:

- **Ecken:** Spieler
- **Kanten:** vom Verlierer zum Sieger, mit Kantengewicht = Anzahl der Siege gegen genau diesen Gegner
- **Netzwerk-Kenngrößen:** Dichte, mittlere Weglänge, Durchmesser, Zusammenhang
- **Zentralitätsmaße:** Grad-, Nähe-, Betweenness- und Eigenvektor-Zentralität (Hauptmaß für das Ranking, siehe Begründung oben)
- **Robustheitscheck:** Vergleich des Eigenvektor-Rankings mit PageRank via Spearman-Rangkorrelation

## Ergebnisse

**Phil Taylor ist der GOAT** der PDC World Championship: Er führt das Ranking nach allen vier Zentralitätsmaßen an (Eigenvektor-Wert 0,526, vor Michael van Gerwen mit 0,404 und Gary Anderson mit 0,365).

Weitere zentrale Befunde:

- Das Netzwerk ist trotz einer Dichte von nur 6,3 % ein **Small-World-Netzwerk** (mittlere Weglänge 2,18, Durchmesser 4)
- Der Vergleich mit PageRank bestätigt die Robustheit des Rankings (Spearman-ρ = 0,953)
- Die Netzwerkperspektive macht Leistungen sichtbar, die reine Siegstatistiken übersehen: Rob Cross liegt nach Siegzahl nur auf Rang 21, nach Eigenvektor-Zentralität aber auf Rang 7

Details zu Interpretation, Diskussion und Limitationen siehe Projektbericht.

## Repository-Inhalt

| Datei | Beschreibung |
|---|---|
| [`Ausarbeitung.md`](./Ausarbeitung.md) | Vollständiger Projektbericht (direkt auf GitHub lesbar) |
| `darts_goat_notebook.ipynb` | Reproduzierbares Analyse-Notebook (Graphaufbau, Zentralitäten, Robustheitscheck, Visualisierungen) |
| `darts_matches_world_championship.csv` | Erhobener Matchdatensatz |
| Scraping-Skript | Python-Skript zur Datenerhebung von dartsdatabase.co.uk |
| `goat_ranking.png`, `zentralitaeten_vergleich.png`, `turnier_netzwerk.png` | Im Notebook erzeugte Abbildungen |

## Ausführen des Notebooks

```bash
pip install pandas networkx matplotlib scipy
jupyter notebook darts_goat_notebook.ipynb
```

Die CSV-Datei mit den Matchdaten muss im selben Verzeichnis wie das Notebook liegen.

**Verwendete Bibliotheken:** pandas, NetworkX, Matplotlib, SciPy

## Quellen

- Temesi, J., Szádoczki, Z. & Bozóki, S. (2023). *Incomplete pairwise comparison matrices: Ranking top women tennis players.* Journal of the Operational Research Society.
- dartsdatabase.co.uk (2026). *PDC World Championship Results 1994–2026.*

## Autorin

Mia Mainka · Leuphana Universität Lüneburg
