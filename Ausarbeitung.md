# GOAT-Analyse der PDC Darts World Championship

### Eine netzwerkbasierte Untersuchung der Jahre 1994 bis 2026

*Projektarbeit im Rahmen der Lehrveranstaltung Netzwerkprojekt Wirtschaftsinformatik*

| | |
|---|---|
| **Dozent** | Prof. Dr. Peter Niemeyer |
| **Semester** | Sommersemester 2026 |
| **Verfasserin** | Mia Mainka |
| **Matrikelnummer** | 3047072 |
| **Abgabedatum** | 31.07.2026 |

---

## 1. Einleitung und Fragestellung

Die Frage nach dem besten Spieler aller Zeiten, dem sogenannten Greatest of All Time (GOAT), wird in nahezu jeder Sportart leidenschaftlich diskutiert. Im Profidarts stehen sich dabei vor allem zwei Namen gegenüber: Phil Taylor, der das Spiel über zwei Jahrzehnte dominierte und 14 PDC-Weltmeistertitel gewann, und Michael van Gerwen, der als der technisch vielleicht beste Spieler der Geschichte gilt. Solche Debatten werden üblicherweise über einfache Kennzahlen wie Titel oder Siege geführt. Diese Betrachtung greift jedoch zu kurz, denn sie ignoriert eine entscheidende Frage: Gegen wen wurden diese Siege errungen?

Ein Sieg gegen einen Weltklassespieler sollte mehr wiegen als ein Sieg gegen einen Qualifikanten, der nur einmal an einem Turnier teilgenommen hat. Genau an dieser Stelle setzt die vorliegende Arbeit an. Sie modelliert die PDC World Darts Championship, das wichtigste Turnier im Profidarts, als gerichtetes und gewichtetes Netzwerk und beantwortet die GOAT-Frage mit den Mitteln der Graphentheorie. Die zentrale Forschungsfrage lautet:

> *Wer ist der Greatest of All Time der PDC World Darts Championship, gemessen an einer netzwerkbasierten Zentralitätsanalyse aller Head-to-Head-Ergebnisse von 1994 bis 2026?*

Methodisch orientiert sich die Arbeit an Temesi et al. (2023), die eine vergleichbare Fragestellung für das Damentennis untersucht haben. Die Autoren modellieren dort die direkten Duelle von 28 Weltklassespielerinnen als unvollständige Paarvergleichsmatrix und leiten daraus ein Ranking ab. Tennis und Darts teilen eine entscheidende strukturelle Eigenschaft: Beide Turnierformate sind K.O.-Systeme, in denen nicht jeder gegen jeden spielt. Das entstehende Netzwerk ist daher unvollständig, was besondere methodische Anforderungen an das Ranking stellt und den Einsatz graphentheoretischer Verfahren motiviert.

Die Motivation für die Wahl des Darts-Turniers liegt neben dem persönlichen Interesse an der Sportart in der hervorragenden Datenlage: Die PDC World Championship wird seit 1994 jährlich ausgetragen und sämtliche Ergebnisse sind öffentlich dokumentiert. Damit lässt sich ein vollständiges Turniernetzwerk über 33 Jahre und mehrere Spielergenerationen hinweg aufbauen, was eine seltene Gelegenheit für eine generationsübergreifende Analyse darstellt.

## 2. Turnier und Datenbasis

### 2.1 Die PDC World Darts Championship

Die PDC World Darts Championship ist das prestigeträchtigste und höchstdotierte Turnier im Profidarts. Sie wird seit 1994 jährlich zum Jahreswechsel im Alexandra Palace in London ausgetragen. Das Turnier folgt einem klassischen K.O.-Format: Wer ein Match verliert, scheidet aus. Vor der Auslosung werden die stärksten Spieler anhand der PDC Order of Merit gesetzt, damit sie nicht bereits in frühen Runden aufeinandertreffen. Das Teilnehmerfeld ist über die Jahre von ursprünglich 24 auf zuletzt 96 Spieler angewachsen, wodurch auch die Anzahl der Matches pro Austragung deutlich gestiegen ist.

### 2.2 Datenerhebung

Die Matchdaten wurden mit einem eigens entwickelten Python-Skript von der öffentlich zugänglichen Datenbank dartsdatabase.co.uk erhoben. Das Skript ruft für jede Austragung der World Championship die Ergebnisseite ab und extrahiert daraus alle Matches mit Spielernamen, Ergebnis in Legs, Runde und Jahr. Um die Webseite nicht zu belasten, wurde zwischen den Anfragen eine Wartezeit eingebaut. Zusätzlich wurde für jeden Spieler die eindeutige Spieler-ID der Datenbank gespeichert, damit unterschiedliche Schreibweisen desselben Namens nicht zu doppelten Ecken im Netzwerk führen.

Der resultierende Datensatz umfasst 2110 Matches aus allen 33 Austragungen von 1994 bis 2026, an denen insgesamt 529 verschiedene Spieler beteiligt waren. Die Datenqualität wurde stichprobenartig geprüft, indem historische Ergebnisse wie das Finale von 1994 (Dennis Priestley gegen Phil Taylor 6:1) und das Finale von 2024 (Luke Humphries gegen Luke Littler 7:4) mit offiziellen Quellen abgeglichen wurden. Alle geprüften Ergebnisse erwiesen sich als korrekt.

Ursprünglich war geplant, zusätzlich die Premier League Darts einzubeziehen, um mehr direkte Duelle zwischen den Topspielern zu erhalten. Da die verfügbaren Daten dieses Turniers jedoch nur bis 2022 zurückreichen, wäre eine unausgewogene Datenbasis entstanden. Die Analyse beschränkt sich daher bewusst auf die World Championship, die als einziges Turnier die gesamte PDC-Geschichte lückenlos abdeckt.

## 3. Modellierung als Netzwerk

Das Turnier wird als gerichteter, gewichteter Graph G = (V, E) modelliert. Die Menge der Ecken V besteht aus den Spielern. Für jedes Match wird eine gerichtete Kante vom Verlierer zum Sieger eingefügt. Diese Richtungswahl folgt Temesi et al. (2023) und hat einen anschaulichen Grund: Ein Spieler, auf den viele Kanten zeigen, hat viele Duelle gewonnen. Ein hoher Eingangsgrad signalisiert also Stärke. Haben zwei Spieler mehrfach gegeneinander gespielt, wird die Kante nicht dupliziert, sondern ihr Gewicht erhöht. Das Kantengewicht gibt somit an, wie oft der Sieger gegen diesen konkreten Gegner gewonnen hat.

Vor dem Aufbau des Graphen wurde der Datensatz gefiltert: Berücksichtigt wurden nur Spieler mit mindestens zehn absolvierten Matches. Dieses Mindestkriterium folgt dem Vorgehen von Temesi et al. (2023) und verhindert, dass Einmalteilnehmer mit statistisch kaum aussagekräftigen Ergebnissen das Ranking verzerren. Nach der Filterung verbleiben 125 Spieler und 1126 Matches. Der daraus entstehende Graph umfasst 125 Ecken und 972 Kanten. Dass die Kantenzahl unter der Matchzahl liegt, erklärt sich durch die Gewichtung: Wiederholte Duelle zwischen denselben Spielern erzeugen keine neuen Kanten, sondern erhöhen bestehende Gewichte.

Da das K.O.-Format verhindert, dass jeder Spieler gegen jeden anderen antritt, ist das entstehende Netzwerk unvollständig. Von den theoretisch möglichen Duellpaarungen wurden nur 6,3 Prozent tatsächlich ausgetragen. Diese Eigenschaft entspricht exakt der unvollständigen Paarvergleichsmatrix im Tennis-Paper und ist der Grund, warum einfache Auswertungen wie Siegquoten hier zu kurz greifen: Die Aussagekraft eines Sieges hängt davon ab, gegen wen er errungen wurde, und genau diese Information steckt in der Netzwerkstruktur.

## 4. Methodik

### 4.1 Netzwerk-Kenngrößen

Zunächst wurden grundlegende Kenngrößen des Netzwerks berechnet, um seine Struktur zu charakterisieren. Geprüft wurde, ob das Netzwerk zusammenhängend ist, wie hoch seine Dichte ist und welche mittlere Weglänge sowie welchen Durchmesser es aufweist. Für die Berechnung von Weglänge und Durchmesser wurde der Graph in seine ungerichtete Form überführt, da diese Maße im gerichteten Fall einen stark zusammenhängenden Graphen voraussetzen würden, was bei einem Turniernetzwerk naturgemäß nicht gegeben ist.

### 4.2 Zentralitätsmaße

Den Kern der Analyse bilden die vier Zentralitätsmaße aus der Vorlesung, die jeweils eine andere Facette der Bedeutung eines Spielers im Netzwerk erfassen. Die Grad-Zentralität misst, gegen wie viele verschiedene Gegner ein Spieler angetreten ist, und spiegelt damit vor allem die Breite und Länge einer Karriere wider. Die Nähe-Zentralität gibt an, wie kurz die durchschnittlichen Wege eines Spielers zu allen anderen im Netzwerk sind, was bei Spielern mit langer, generationsübergreifender Präsenz zu hohen Werten führt. Die Betweenness-Zentralität misst, wie häufig ein Spieler auf dem kürzesten Weg zwischen zwei anderen liegt, und identifiziert damit Brückenspieler, die verschiedene Epochen des Sports miteinander verbinden.

Als Hauptmaß für die Beantwortung der Forschungsfrage dient die Eigenvektor-Zentralität. Ihre Grundidee passt exakt zur GOAT-Frage: Der Wert eines Spielers bemisst sich nicht an der bloßen Anzahl seiner Siege, sondern an der Stärke der besiegten Gegner. Ein Sieg gegen einen selbst zentralen Spieler trägt mehr zum eigenen Wert bei als ein Sieg gegen einen peripheren. Da die Berechnung iterativ erfolgt und sich die Werte schrittweise stabilisieren, wurde die maximale Iterationszahl auf 1000 erhöht, damit das Verfahren zuverlässig konvergiert.

### 4.3 Robustheitscheck

Um zu prüfen, ob das Ergebnis von der Wahl der Methode abhängt, wurde das Eigenvektor-Ranking mit einem PageRank-Ranking verglichen. Beide Verfahren berücksichtigen die Stärke der Gegner, unterscheiden sich aber in der Berechnung: PageRank verwendet einen Dämpfungsfaktor, der verhindert, dass einzelne Spieler mit wenigen, aber sehr prominenten Siegen unverhältnismäßig hoch bewertet werden. Die Übereinstimmung beider Rankings wurde mit der Spearman-Rangkorrelation gemessen, die nicht die absoluten Werte, sondern die Rangfolgen vergleicht. Auf eine Monte-Carlo-Simulation wurde bewusst verzichtet, da sie für die vorliegende Fragestellung keinen Mehrwert bietet: Simulationen eignen sich zur Untersuchung zufallsabhängiger Prozesse wie Auslosungen, während hier ausschließlich real stattgefundene Matches ausgewertet werden. Der Robustheitscheck über zwei unabhängige Ranking-Verfahren erfüllt denselben Zweck der Ergebnisabsicherung.

## 5. Ergebnisse

### 5.1 Netzwerkstruktur

Das Netzwerk ist zusammenhängend: Jeder der 125 Spieler ist über eine Kette von Duellen mit jedem anderen verbunden. Die Dichte beträgt 0,0627, es wurden also nur gut sechs Prozent aller möglichen Paarungen tatsächlich ausgetragen. Umso bemerkenswerter sind die Distanzmaße: Die mittlere Weglänge beträgt lediglich 2,18 und der Durchmesser 4. Zwei beliebige Spieler der 33-jährigen Turniergeschichte sind im Durchschnitt über nur etwa zwei Zwischenstationen miteinander verbunden, im Extremfall über vier. Das Netzwerk weist damit trotz seiner geringen Dichte die typischen Merkmale eines Small-World-Netzwerks auf, wie sie aus dem Milgram-Experiment bekannt sind. Inhaltlich erklärt sich dies durch Spieler mit langen Karrieren, die als Verbindungsglieder zwischen den Generationen fungieren.

### 5.2 GOAT-Ranking

Das zentrale Ergebnis der Analyse ist eindeutig: Phil Taylor ist der GOAT der PDC World Championship. Mit einem Eigenvektor-Wert von 0,526 liegt er deutlich vor Michael van Gerwen (0,404) und Gary Anderson (0,365). Auf den weiteren Plätzen folgen Raymond van Barneveld und Peter Wright. Taylor führt zudem in der reinen Sieganzahl mit 90 gewerteten Siegen klar vor van Gerwen mit 49.

Besonders aussagekräftig ist, dass Phil Taylor nicht nur nach dem Hauptmaß vorne liegt, sondern nach allen vier berechneten Zentralitätsmaßen den ersten Platz belegt. Er hat die meisten verschiedenen Gegner bespielt, ist im Netzwerk am zentralsten positioniert, verbindet als Brückenspieler die Epochen und hat die wertvollsten Siege errungen. Diese Übereinstimmung über vier konzeptionell unterschiedliche Maße hinweg ist ein starkes Indiz dafür, dass das Ergebnis die tatsächliche Struktur des Turniernetzwerks widerspiegelt und kein Artefakt einer einzelnen Methode ist.

Der Robustheitscheck bestätigt diesen Eindruck: Die Spearman-Rangkorrelation zwischen dem Eigenvektor-Ranking und dem PageRank-Ranking beträgt 0,953 bei einem p-Wert nahe null. Beide Verfahren ordnen die Spieler nahezu identisch. Das Ergebnis ist somit methodenunabhängig und deckt sich mit dem Befund von Temesi et al. (2023), die in ihrer Tennis-Analyse ebenfalls eine hohe Übereinstimmung zwischen verschiedenen Ranking-Verfahren feststellten.

## 6. Interpretation und Diskussion

Das Ergebnis mag auf den ersten Blick wenig überraschen, schließlich gilt Phil Taylor mit 14 PDC-Weltmeistertiteln auch in der öffentlichen Wahrnehmung als Jahrhundertspieler. Gerade diese Übereinstimmung mit der Expertenmeinung ist jedoch als Validierung der Methodik zu werten: Ein netzwerkbasiertes Verfahren, das ohne jedes Vorwissen über Titel oder Ranglisten auskommt und ausschließlich die Struktur der Duelle auswertet, kommt zum selben Ergebnis wie die jahrzehntelange Fachdiskussion.

Interessant ist der Blick auf den zweiten Platz. Michael van Gerwen gilt vielen als der technisch beste Spieler der Geschichte, mit den höchsten Averages, die je gespielt wurden. Dass er hinter Taylor liegt, erklärt sich vor allem durch die Karrierelänge: Taylor war 33 Jahre lang auf Weltklasseniveau aktiv und hat die jeweils stärksten Spieler dreier Generationen geschlagen, während van Gerwens Dominanzphase erst 2012 begann. Die Eigenvektor-Zentralität honoriert diese über Jahrzehnte akkumulierte Qualität der Siege. Hier zeigt sich zugleich eine konzeptionelle Eigenschaft des Ansatzes: Er misst das Lebenswerk im Turnierkontext, nicht die Spitzenleistung zu einem einzelnen Zeitpunkt.

Der Mehrwert der Netzwerkperspektive gegenüber einer einfachen Siegstatistik zeigt sich besonders bei Spielern abseits der Spitze. Rob Cross etwa liegt nach reiner Sieganzahl nur auf Rang 21, nach Eigenvektor-Zentralität jedoch auf Rang 7. Der Grund: Cross gewann 2018 sensationell den Weltmeistertitel und besiegte dabei unter anderem Phil Taylor in dessen letztem Karrierematch sowie Michael van Gerwen. Seine vergleichsweise wenigen Siege wurden gegen außergewöhnlich starke Gegner errungen, was die Eigenvektor-Zentralität korrekt erfasst, eine reine Siegzählung aber übersieht. Die Spearman-Korrelation zwischen Eigenvektor-Ranking und Sieg-Ranking von 0,81 bestätigt dieses Bild: Beide Rankings sind ähnlich, aber keineswegs identisch, und gerade in den Abweichungen liegt der Erkenntnisgewinn der Methode.

Auch die Small-World-Eigenschaft des Netzwerks verdient eine inhaltliche Einordnung. Dass Spieler aus den 1990er-Jahren und die heutige Generation um Luke Littler über nur wenige Zwischenschritte verbunden sind, liegt an Ausnahmespielern wie Taylor, van Barneveld oder Anderson, deren Karrieren mehrere Epochen überspannen. Sie fungieren als Brücken im Netzwerk, was sich in ihren hohen Betweenness-Werten niederschlägt. Das Turniernetzwerk bildet damit auch ein Stück Sportgeschichte ab: Es zeigt, wie Generationen von Spielern über direkte Duelle miteinander verwoben sind.

## 7. Limitationen

Die Ergebnisse sind vor dem Hintergrund mehrerer Einschränkungen zu interpretieren. Erstens gehen alle Siege gleichwertig in das Modell ein, unabhängig davon, ob ein Match deutlich mit 7:0 oder denkbar knapp mit 7:6 endete. Eine Gewichtung nach Leg-Differenz könnte das Ranking in Einzelfällen verschieben, wurde hier aber zugunsten der Einfachheit und Nachvollziehbarkeit des Modells nicht umgesetzt.

Zweitens beschränkt sich die Datenbasis auf die World Championship. Andere bedeutende Turniere wie die Premier League oder das World Matchplay, in denen etwa van Gerwen über Jahre dominierte, fließen nicht ein. Ein GOAT-Ranking über alle PDC-Turniere hinweg könnte die Abstände an der Spitze verändern, auch wenn die Reihenfolge der Top drei angesichts der deutlichen Werte vermutlich stabil bliebe.

Drittens hat sich die Feldstärke über die Epochen verändert. Das Turnier der 1990er-Jahre hatte ein kleineres und im Durchschnitt schwächeres Teilnehmerfeld als die heutigen Austragungen mit 96 Spielern. Taylors frühe Siege könnten dadurch tendenziell übergewichtet sein, da die Eigenvektor-Zentralität die Stärke der Gegner nur relativ zum jeweiligen Netzwerk misst. Viertens behandelt das Modell die Spielstärke als statisch: Formschwankungen innerhalb einer Karriere, Verletzungen oder das Karriereende werden nicht abgebildet. Jeder Spieler geht mit seiner gesamten Turnierhistorie als ein einziger Knoten in das Netzwerk ein.

## 8. Fazit

Die vorliegende Arbeit hat die PDC World Darts Championship von 1994 bis 2026 als gerichtetes, gewichtetes Netzwerk modelliert und die GOAT-Frage mit graphentheoretischen Methoden beantwortet. Das Ergebnis ist eindeutig: Phil Taylor ist der Greatest of All Time der PDC World Championship. Er führt das Ranking nach allen vier untersuchten Zentralitätsmaßen an, und das Ergebnis erweist sich mit einer Spearman-Korrelation von 0,953 zwischen zwei unabhängigen Verfahren als ausgesprochen robust.

Über die konkrete Antwort hinaus zeigt die Arbeit den methodischen Mehrwert der Netzwerkperspektive: Sie berücksichtigt die Stärke der Gegner und macht damit Leistungen sichtbar, die eine reine Siegstatistik übersieht, wie das Beispiel Rob Cross illustriert. Zugleich offenbart die Analyse strukturelle Eigenschaften des Turniers selbst, etwa seinen Small-World-Charakter, der die Verwobenheit der Spielergenerationen widerspiegelt. Die an Temesi et al. (2023) angelehnte Methodik hat sich dabei als gut auf den Darts-Sport übertragbar erwiesen. Für weiterführende Arbeiten bietet sich insbesondere die Ausweitung der Datenbasis auf weitere PDC-Turniere sowie eine zeitlich differenzierte Analyse einzelner Epochen an.

## Literaturverzeichnis

Csató, L. et al. (2025). How to measure the uncertainty of a tournament draw: The case of European football's Champions League. arXiv:2507.15320.

Niemeyer, P. (2026). Vorlesungsunterlagen Netzwerkprojekt Wirtschaftsinformatik. Leuphana Universität Lüneburg.

Temesi, J., Szádoczki, Z. & Bozóki, S. (2023). Incomplete pairwise comparison matrices: Ranking top women tennis players. Journal of the Operational Research Society.

dartsdatabase.co.uk (2026). PDC World Championship Results 1994 bis 2026. Abgerufen im Mai 2026.

## Anhang

Der vollständige Analysecode befindet sich im beigefügten Jupyter-Notebook (`darts_goat_notebook.ipynb`). Die zugrundeliegenden Matchdaten sind in der Datei `darts_matches_world_championship.csv` enthalten. Das zur Datenerhebung verwendete Scraping-Skript liegt der Abgabe ebenfalls bei. Die im Notebook erzeugten Abbildungen (GOAT-Ranking, Vergleich der Zentralitätsmaße, Turniernetzwerk 2024) können an den passenden Stellen in diese Ausarbeitung eingefügt werden.
