# AWS First Steps

Hier werden die ersten Schritte aufgezählt, die auszuführen sind, um mit den von AWS angebotenen Cloud-Diensten arbeiten zu können.

!!! info "Weiterführende Information direkt verlinkt"
    Das User- und Rechte-Management bei AWS ist ein komplexes Thema, dessen vollständige Behandlung den Rahmen dieser Seite komplett sprengen würde. Die meisten der hier nur kurz erklärten Begriff sind daher direkt mit der ausführlichen Dokumentation bei AWS verlinkt.

## User- und Rechte-Management bei AWS

Die zentrale Verwaltung von Usern und Rechten innerhalb eines AWS Benutzerkontos erfolgt über den __AWS Identity and Access Management Service (IAM)__.

AWS unterscheidet zwei verschieden Arten von Benutzern:

* __Account Root User__: Master-User, über den alle Ressourcen für ein Projekt oder ein Team in dessen Benutzerkonto verwaltet werden können.

* __IAM User__: User innerhalb eines AWS Accounts, der einen menschlichen User aus einem (Projekt-)Team oder einen technischen User für einen Service repräsentiert.

### AWS Identity and Access Management (IAM)

Die Verwaltung von Usern und Rechten eines AWS Benutzerkontos, sowie die Verwaltung von konto- oder user-spezifischen Artefakten wie Passwörtern, SSH-Schlüsseln etc erfolgt im AWS Identity and Access Management (IAM).

Die Aufzählung der Funktionen von IAM würde den Rahmen dieser Seite sprengen. Daher sein an dieser Stelle nur auf die hervorragende [Dokumentation](https://aws.amazon.com/documentation/iam/) von AWS verwiesen.

### Account Root User

Über den Root-User können den alle Ressourcen für ein Projekt oder ein Team verwaltet werden können. Der Master-User wird normalerweise nur zur initialen Einrichtung eines AWS Benutzerkontos verwendet. Danach wird ein regulärer IAM User als Admin für das AWS Benutzerkonto eingerichtet und für die weitere Verwaltung des Benutzerkontos nur noch dieser IAM User verwendet.

Ein Root User verfügt immer über das Recht, sich an der AWS Konsole anzumelden. Die Anmeldung erfolgt dabei über den Root-User-Namen (meist eine Email-Adresse) und ein Passwort.

!!! warning "Sorgsamer Umgang mit dem Passwort des Root Users" 
    Das es sich bei dem Root User um einen sehr wichtigen User handelt, sollte mit dessen Passwort auch sehr sorgsam umgegangen werden. Für die Verwaltung des Passworts des Root Users empfiehlt sich ein Passwort-Management-Tool wie KeePass, über das sichere Passwörter generiert werden können. Das Passwort sollte nur mit sehr wenigen Kollegen geteilt und immer sicher verwahrt werden.

!!! tip "Zwei-Faktor-Authentisierung aktivieren"    
     Für den Root User sollte immer die Zwei-Faktor-Authentisierung für die Anmeldung aktiviert werden.
    
Der Root User hat automatisch vollen Zugriff auf alle Ressourcen innerhalb seines AWS Benutzerkontos.

### [IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html)
  
Innerhalb eines AWS Benutzerkontos können über das AWS Identity and Access Management (IAM) weitere IAM User eingerichtet werden, die dann einen menschlichen User aus einem (Projekt-)Team oder einen technischen User für einen Service repräsentieren. Hierbei kann auch unterschieden werden, ob der IAM User sich zum Beispiel an der AWS-Konsole anwenden darf und somit AWS-Ressourcen über die AWS-Konsole verwaltet oder nur über die AWS Kommandozeile (AWS CLI) auf AWS-Ressource zugreifen kann.

IAM User _mit_ Zugriff auf die AWS Konsole erhalten eine User-ID und ein Passwort. Die User-ID und das Passwort können frei bestimmt werden. Zusätzlich ist auch die Option möglich, dass sich der IAM User bei der ersten Anmeldung an die AWS Konsole ein neues Passwort vergeben muss.

IAM User _ohne_ Zugriff auf die AWS Konsole erhalten einen Access Key und einen Secret Key. Access Key und Secret Key werden dabei bei der Anlage des IAM Users automatisch von AWS IAM erzeugt und sind danach nicht mehr änderbar.

!!! tip "Root User durch IAM Admin User ersetzen"
    Der Root User sollte so schnell wie möglich durch einen IAM User mit Admin-Rechten auf alle Ressourcen ersetzt werden.
    
An einem IAM User hängen die folgenden beweglichen Teile:

* __Gruppen__: Ein Gruppe repräsentiert eine Menge von Usern, die alle eine bestimmte Menge von Rollen haben sollen. Jedem IAM User sollte mindestens eine Gruppe zugeordnet werden.
* __Rollen__: Ein Rolle definiert eine Menge von Policies. Jede Rolle muss mindestens eine Policy enthalten.
* __Policies__: Eine Policy definiert eine Menge von Permissions. Jede Policy muss mindestens eine Permission enthalten.
* __Permission__: Eine Permission definiert ein bestimmtes Zugriffsrecht (zum Beispiel: _Lesen_) auf eine bestimmte Ressource oder eine Gruppe von Ressourcen (zum Beispiel: _S3 Bucket_) eines bestimmte AWS Dienstes (zum Beispiel: _S3_).

### [IAM Gruppen](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html)

Eine __IAM Gruppe__ definiert eine Sammlung von IAM Usern mit den gleichen Zugriffsrechten:

* Ein _User_ gehört einer oder mehrerer _Gruppen_ an.
* Eine _Gruppe_ enthält eine oder mehrere _Rollen_.

Gruppen können das Rechtemanagement erheblich erleichtern, wenn man sich an ein paar Regeln hält:

* Rollen sollten immer an Gruppen und nie direkt an User gehängt werden.
* Gleiche Zugriffrechte sollten immer in einer Gruppe gebündelt werden. 
 
### [IAM Rollen](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)

Eine __IAM Rolle__ bestimmt, was eine Identität in AWS tun darf und was sie nicht tun darf. Eine Identität kann neben einem _User_ auch eine Ressource wie eine Applikation oder eine virtuelle Maschine (EC2) sein.

Eine Rolle besteht aus mindestens einer _Policy_.  

### [IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)

Eine __IAM Policy__ definiert die Zugriffsrechte (_Permissions_) einer Identität oder Ressource. AWS wertet diese Policies immer dann aus, wenn zum Beispiel ein User an einen AWS Dienst eine Anfrage stellt. _Permissions_ in der Policy bestimmen dann, ob die Anfrage zugelassen oder abgewiesen wird.

Eine Policy wird durch JSON-Dokument repräsentiert. Hier ein Beispiel für die Policy __AdministratorAccess__, die einem Admin User Vollzugriff auf alles gewährt:

``` json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

Die Verwaltung von Policies wird glücklicherweise durch die sogenannten _Managed Policies_ erleichtert. Bei den Managed Policies handelt es sich um eine große Sammlung von vordefinierten Policies, die AWS für seine Dienste bereitstellt. So hat zum Beispiel ein Admin User in seiner Rolle die Managed Policy __AdministratorAccess__, die vollen Zugriff auf alle AWS Dienste und Ressourcen gewährt.

!!! tip "Korrekte Bindung von Policies an User"
    Policies sollte immer nur über Rollen und Gruppen an User vergeben werden: Eine Policy gehört zu einer _Rolle_. Die Rolle gehört zu einer _Gruppe_. Der _User_ gehört zu dieser Gruppe.

## Einrichten eines AWS Benutzerkontos mit Root User

Für den Zugriff auf AWS Dienst benötigt man ein AWS Benutzerkonto. Ein derartiges AWS Benutzerkonto lässt sich leicht über die [Homepage von AWS](https://aws.amazon.com) einrichten: 

* Im Browser https://aws.amazon.com öffnen.
* Rechts oben auf den gelben Button __Kostenloses Konto erstellen__ klicken.
* Du brauchst die Kreditkartennummer der [Firmenkreditkarte](/msg/accounting.md) zur Abrechnung der Kosten.
* Über einen kurzen Telefonanruf wird verifiziert, dass es dich wirklich gibt.
* Als Kontotyp immer Firmenkonto (Corporate Account) angeben.

!!! tip "Ein AWS Benutzerkonto pro Projekt"
    Pro Projekt ist es völlig ausreichend, ein AWS Benutzerkonto einzurichten. Alle weiteren Projektmitglieder werden dann IAM User.
    
!!! info "AWS Benutzerkonten an AT melden"
    Um die Abrechnung von Kosten aus verschiedenen AWS Benutzerkonten zu vereinfachen, haben wir bei AT eine AWS Organisation eingerichtet. An die Branche Automotive geht dann eine konsolidierte Rechnung, in der alle Kosten nach dem verursachenden Benutzerkonto aufgeschlüsselt sind. Daher die dringende Bitte an euch, eure Benutzerkonten immer an AT zu melden!
    
## Einrichten von IAM Usern

Nach erfolgreicher erster Anmeldung durch den Root User sollte dieser sofort über die Konsole weitere IAM User für sich und seine KollegInnen einrichten.

Vor der Einrichtung weitere IAM User empfiehlt es sich, einmal durch die [Best Practices beim Umgang mit IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) zu stöbern.

Die Einrichtung eines IAM User verläuft also nach folgendem Prozess:

1. Zugriffsrechte für alle Teammitglieder zumindest grob planen.
1. Gleiche Zugriffsrechte über Zugriffsprofile gruppieren.
1. Für jedes Zugriffsprofil eine IAM Gruppe anlegen.
1. IAM User über die AWS Konsole anlegen und die passende IAM Gruppe zuordnen.

!!! tip "Best Practices"
    Wir von AT empfehlen die folgenden Regeln beim Umgang mit IAM Usern:
    
    * Die Anzahl der Admin User mit Vollzugriff auf alle Dienste sollte möglichst gering (1 - 2 User) gehalten werden.
    * Bei Anlegen der IAM User immer die Option wählen, dass der neu angelegte User bei seiner Erstanmeldung ein neues Passwort vergeben muss. So kennt auch der Admin nicht die Passwörter seines Teams.
    * Programmatischer Zugriff auf AWS sollte immer über technische User mit Access Key und Secret Key erfolgen, nie über Konsolen-User.
    * Bei allen Usern sollte die Passwort Policy gewählt werden, die Passwörter nach einem bestimmten Intervall ungültig werden lässt.
    * Bei IAM-Usern, die Teammitglieder repräsentieren sollen, sollte immer die msg-Windows-Kennung als User-ID verwendet werden.

## Konsolidierte Abrechnung über gemeinsame AWS Organisation

Normalerweise würde für jedes bei AWS eingerichtete Benutzerkonto eine eigene Rechnung gestellt werden, obwohl jedes AWS Benutzerkonto für die Branche Automotive immer die gleiche Firmenkreditkarte verwendet. Um die Abrechnung der Kosten von mehreren AWS Benutzerkonten zu vereinfachen, haben wir über [AWS Organizations](https://aws.amazon.com/organizations/) bei AWS eine Organisation für die Branche Automotive eingerichtet, unter der alle AWS Benutzerkonten der Branche Automotive verwaltet werden.

