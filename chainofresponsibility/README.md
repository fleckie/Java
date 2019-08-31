# ChainOfResponsibility
**Programmieren 2 - Chain of Responsibility pattern (Martin Frühwirth, Joseph Murgescu, Martin Muzik)**

**Funktion**

Mehrere Objekte werden hintereinander geschaltet (miteinander verkettet), um gemeinsam eine eingehende Anfrage bearbeiten zu können. 
Diese Anfrage wird an der Kette entlang geleitet, bis eines der Objekte die Anfrage beantworten kann. 
Der Klient, von dem die Anfrage ausgeht, hat dabei keine Kenntnis darüber, von welchem Objekt die Anfrage beantwortet werden wird.
(Quelle: wikipedia.de)

Im Prinzip ist das Chain-of-Responsibility Pattern die objektorientierte Version einer if...else... else... else etc. Verzweigung.

**Anwendung**

Wir wollen das Pattern anhand eines simplen Rechners veranschaulichen. Zunächst erstellen wir jeweils 1 Objekt für die 4 Grundrechnungsarten;
die von uns gewählte Reihenfolge der Abarbeitung ist Addition (1), Subtraktion (2), Multiplikation (3) und Division (4).
Weiters wird festgelegt, welches Objekt das Nachfolgeglied in unserer Zuständigkeitskette ist. Dies tun wir für den Fall, dass das aktuelle
Objekt die vom Nutzer gewünschte Operation nicht durchführen kann. Im Falle des letzten Kettenglieds (aktuell Division, aber das Programm
kann nach Belieben erweitert werden) wird bei Nichtzuständigkeit die Meldung ausgegeben, dass nur die zuvor definierten
Rechnungsarten/Operationen zulässig sind.

Unsere einzelnen Kettenglieder sollen die vom Nutzer gewünschte Berechnung ausführen (sofern möglich) sowie Ihren Nachfolger angeben (sofern
vorhanden bzw. falls Sie die Anforderung nicht selbst bearbeiten können). Daher definieren wir ein Interface mit dem Namen „Chain“ mit 2
entsprechenden Methoden:

setNextChain(Chain nextChain): erhält als Parameter das nächste Objekt in der Kette und speichert dieses als Referenz im Objekt, das die
Methode aufruft.
calculate(Numbers request): erhält als Parameter ein Objekt der Klasse Numbers, dieses besteht aus 2 Zahlen und einer Operation (auf diesen
Zahlen).

Das praktische am Interface „Chain“ und den Klassen, die dieses implementieren, ist, dass z.B. der „setNextChain“-Methode ein Objekt der
Klasse „Chain“ übergeben werden kann.
So spielt es keine Rolle, welche Operation die verschiedenen Objekte ausführen bzw. welcher Klasse Sie angehören – sie besitzen alle den Typ
„Chain“ und können so als Parameter übergeben werden.
Damit wird die Kette modular aufgebaut und lässt sich leicht erweitern; die zweite Methode „calculate“ kann ebenso leicht recycelt werden – 
hierbei muss lediglich der Name der Operation und deren mathematische Definition angepasst werden.

Aufgerufen wird unser Programm über die erste definierte Rechenoperation  (Chain chainCalc1) – für den Anwender ist nicht erkennbar, ob die
Anfrage direkt bearbeitet werden konnte oder weitergereicht werden musste.

**Vorteile**

Ein wesentlicher Vorteil ist, dass das Entwicklerteam die Reihenfolge, in der eine Anfrage bearbeitet werden soll, festlegen kann. 

Single-Responsibility-Prinzip
Der Ersteller der Anfrage (Klient) ist vom zuständigen Bearbeiter getrennt. Die Kettenmitglieder kennen selbst nur ihren direkten Nachfolger.
Die Koppelung innerhalb des Design Patterns bleibt gering und die Elemente müssen nicht das Gesamtbild kennen.

Open/Closed-Prinzip
Neue Elemente können leicht in die Kette eingefügt werden, ohne dass dabei der gesamte Code anzupassen ist oder Bugs eingeführt werden.

**Nachteile**

Es gibt auf der anderen Seite keine Garantie, dass die Anfrage tatsächlich bearbeitet wird. 
Wenn das letzte Glied der Kette eine Anfrage erhält, für die es ebenfalls nicht zuständig ist, wird die Anfrage nach obigem Muster verworfen.
Dies muss durch eine entsprechende Fallbehandlung abgefangen werden.
Es muss sichergestellt werden, dass jeder Bearbeiter in der Kette nur einmal vorkommt, sonst entstehen Kreise und das Programm bleibt in einer
Endlosschleife hängen.
(Quelle: wikipedia.de)

Der Programmierer muss weiters darauf achten, dass immer auch das nächste Kettenglied im aktuellen Kettenglied angegeben wird, andernfalls
wird die Anfrage unter Umständen nie abgearbeitet. Auch sollte vermieden werden, dass ein Kettenglied besonders langsam abgearbeitet wird
und somit unter Umständen den gesamten Programmablauf verlangsamt oder gar blockiert.
