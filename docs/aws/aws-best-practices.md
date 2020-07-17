# AWS Best Practices

Hier findest du eine Reihe von Empfehlungen, die du beim Umgang mit der AWS Cloud beachten solltest. Die hier genannte Empfehlungen stammen zum Teil aus unseren eigenen Erfahrungen oder wurden aus diversen Quellen von AWS übernommen.

## The Cloud is BIG, so it's time to think BIG!

Diesen Gedanken solltest du immer im Kopf haben, wenn du an die Planung der IP-Adressbereiche für dein VPC gehst. Daher empfehlen wir an dieser Stelle immer, bei einem VPC mit einem /16-CIDR-Block zu starten. Das entspricht über 65000 möglichen IP-Adressen und sollte für die meisten Szenarien ausreichend sein.

Das Gleiche gilt, wenn du an die Anlage deiner Subnetze gehst: hier machen wenige Subnetze mit großen CIDR-Blöcken mehr Sinn als viele Subnetze mit kleinen CIDR-Blöcken. Wenn du eine weitere Unterteilung deiner Subnetze erreichen willst, dann kannst du das über Security Groups steuern.

Hier ein Vorschlag für die Verteilung von CIDR-Blöcken in VPCs und Subnetzen [^1]:

![AWS VPC Design: CIDR blocks](img/aws_vpc_design_cidr.png)

* Die VPC erhält einen /16-CIDR-Block, was maximal 65534 IP-Adressen entspricht.
* Pro Availability Zone gibt es 1 öffentliches Subnetz und 2 private Subnetze.
* Das öffentliche Subnetz erhält einen /22-CIDR-Block, was maximal 1022 IP-Adressen entspricht.
* Die privaten Subnetze erhalten jeweils einen /20-CIDR-Block, was maximal 4094 IP-Adressen entspricht. 

!!! tip "Berechnen von CIDR-Blöcken"
    Im Internet gibt es mehrere Online-CIDR-Rechner, mit denen du auf verschiedende Art und Weisen CIDR-Blöcke berechnen kannst. Uns hat der CIDR-Rechner unter [http://www.subnet-calculator.com/cidr.php](http://www.subnet-calculator.com/cidr.php) gute Dienste geleistet. Dieser CIDR-Rechner hilft dir auch zu verstehen, welche /nn-Angabe wieviel mögliche IP-Adressen bedeutet.
    
## Teile die VPC mit Subnetzen in logische Schichten

Um die Administration insbesonders der Sicherheitsaspekte von VPCs zu vereinfachen, empfiehlt es sich, ein VPC erst einmal nach Erreichbarkeit in Subnetze zu unterteilen.

* __Web Layer__ oder __Internet Facing Layer__: Durch dieses öffentliche Subnetz läuft der gesamte Netzverkehr aus dem Internet in das VPC und aus dem VPC in das Internet.

* __Core Layer__: In diesem privaten Subnetz befinden sich alle Instanzen, auf denen die in der Cloud betriebenen Applikation und Dienste laufen. Der Zugriff auf dieses Subnetz ist nur über den Internet Facing Layer oder den Intranet Facing Layer möglich.

* __Intranet Facing Layer__: Durch dieses private Subnetz läuft der gesamte Netzverkehr aus dem VPC heraus  in das unternehmenseigene Netzwerk oder aus dem unternehmenseignenen Netzwerk in das VPC hinein.

![AWS Subnet Layers](img/aws_vpc_subnet_layers.png)

## Behandle deine Cloud-Infrastruktur wie Code (von Anfang an!)

Die AWS-Konsole bietet alle Möglichkeiten, sich beliebige Ressourcen in AWS bequem über die Web-UI der AWS Konsole einzurichten. Das klingt insbesondere für Anfänger sehr verlockend, birgt aber im produktiven Einsatz viele Probleme:

* Komplexere Ressourcen wie zum Beispiel ein VPC mit einem mehrschichtigen Subnetz-Layout in allen angebotenen Availability Zones einer Region sind mit der AWS-Konsole nur aufwändig zu erstellen.
* Das manuelle Anlegen von komplexeren Ressourcen ist fehleranfällig.
* Änderungen an der eigenen AWS-Infrastrukur können nicht nachvollzogen werden.
* Der aktuelle Zustand der eigenen AWS-Infrastruktur ist nirgendwo abgelegt und somit nicht zuverlässig reproduzierbar.

Daher empfehlen wir an dieser Stelle __dringend__ den Einsatz eines Infrastructure as Code-Tools. Bei AWS bieten sich hierzu zwei Tools an:

* [__CloudFormation__](https://aws.amazon.com/cloudformation/) stammt aus dem Hause AWS und stellt eine gemeinsame Sprache zur Beschreibung und Provisionierung aller Infrastruktur-Ressourcen in der AWS-Cloud dar.

* [__Terraform__](https://www.terraform.io/) von [HashiCorp](https://www.hashicorp.com/) bietet eine gemeinsame Sprache für die Beschreibung und Provisionierung von Ressourcen bei praktisch allen gängigen Cloud-Providern an. Im Gegensatz zu CloudFormation konzentriert sich Terraform nicht nur auf das _Erzeugen_ von Ressourcen sondern auch auf das _kontinuierliche Ändern_ von Ressourcen.

Wir haben uns in AT.41 für __Terraform__ als Infrastructure-as-Code-Tool entschieden:

* da es auch auf andere Cloud-Provider anwendbar ist
* da die Terraform-Sprache deutlich schlanker als das CloudFormation-Format ist 

Hier noch ein kleiner Vergleich, der euch zum Einsatz eines derartigen Tools motivieren soll:

* Für das _manuelle_ Einrichten eines VPCs über 3 Availability Zones mit 2 Subnetzen pro Availability Zone und das Einrichten eines Amazon ECS Clusters (Container-Service von AWS) benötigt man über die Konsole mindestens __1 Stunde__.
* Für das _automatische_ Einrichten der gleichen Infrastrukur mit Terraform benötigt man ca. __5 Minuten__.
 
## Starte Production-Ready

Der Aufbau von Infrastruktur-Ressourcen, die auch einem produktiven Betrieb einer Applikation oder eines Systems standhalten, ist bei einem Cloud-Provider nur unwesentlich teurer als der Aufbau einer Spielwiese. Darum solltet ihr eure Cloud-Ressourcen von Anfang an so designen und provisionieren, das sie den Ansprüchen eines Produktionsbetriebes standhalten. Das gilt insbesondere in Bezug auf die Sicherheit eurer Infrastruktur.

Zusätzlich könnt ihr bei der konsequenten Anwendung von __Infrastructure as Code__ eure produktionsreife Infrastruktur erst dann hochziehen, wenn ihr sie wirklich braucht, und sofort wieder abbauen, wenn ihr sie nicht mehr benötigt. So läßt sich der bereits geringe Kostenunterschied zwischen einer produktionsreifen Umgebung und einer "Spielwiese" praktisch gegen Null optimieren.

Einige Merkmale von Production-Readiness sind (ohne Anspruch auf Vollständigkeit):

* Ressourcen werden immer über alle Availability Zones eines VPCs verteilt: ein VPC hat immer Subnetze in allen verfügbaren Availability Zones; ein Container Cluster überspannt alle Availability Zones; jede Availability Zone hat ein eigenes NAT-Gateway.
* Jede Ressource wird redundant ausgelegt, d.h. jede Ressource existiert in jeder Availibility Zone mindestens einmal.
* Macht nur die Ressourcen öffentlich (d.h. aus dem Internet erreichbar), die unbedingt öffentlich sein müssen.
* Schützt eure privaten Ressourcen konsequent mit NAT-Gateways.
* Alle Datenbanken sind ausfallsicher, hochverfügbar und redundant. Wenn AWS die von euch benötigte Datenbank in einem hochverfügbaren Paket anbietet, dann nutzt dieses auch.
* Sämtliche gespeicherten Daten (_data on rest_) sind verschlüsselt: AWS bietet hierfür die Verschlüsselung eures Storages ohne Aufpreis an.
* Sämtliche Daten sind während der Übertragung (_data in transit_) verschlüsselt: Nutzt für die Kommunikation innerhalb eures VPCs, in euer VPC hinein und aus eurem VPC heraus konsequent TLS (HTTPS etc).
* Lasst sowohl auf Instanz-Ebene über Security Groups als auch auf Subnetz-Ebene über Network ACLs nur die Verbindungen zu, die ihr unbedingt benötigt.
  

   
[^1]:
     siehe [AWS re:Invent 2016: From One to Many: Evolving VPC Design (ARC302)](https://youtu.be/3Gv47NASmU4?t=8m5s) 