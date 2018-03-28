Benutzer eines Unix-Systems können das Programm ssh verwenden, um sich über eine Shell im Pool einzuloggen. Man kann dann von zu Hause auf der Konsole arbeiten, als säße man vor einem Rechner im Pool.
  

### Zum Verbinden folgendes Komando verwenden:
```
ssh st123456@ssh.physcip.uni-stuttgart.de
```

Dabei ist st123456 durch den eigenen Benutzernamen zu ersetzen. Du wirst automatisch auf einen verfügbaren Rechner umgeleitet.
  
Falls du in einem Netzwerk bist, das ausgehende Verbindungen auf Port 22 sperrt, kannst du alternativ Port 443 nutzen, z.B. per
```
ssh -p 443 st123456@ssh.physcip.uni-stuttgart.de
```
  
Benutzer eines Windows Systems können z.B. [Putty](https://www.putty.org/) verwenden.
  

### Zum Dateizugriff kann das SCP- oder das SFTP-Protokoll benutzt werden.
Um die Datei test.txt von deinem Desktop im Pool auf deinen Rechner zu Hause zu kopieren, verwende
```
scp st123456@ssh.physcip.uni-stuttgart.de:~/Desktop/test.txt test.txt
```

um umgekehrt die Datei test.txt von deinem Rechner zu Hause auf deinen Desktop im Pool zu kopieren, verwende
```
scp test.txt phy12345@ssh.physcip.uni-stuttgart.de:~/Desktop/test.txt
```

Alternativ kann SFTP benutzt werden, dafür gibt es mit [WinSCP](http://winscp.net) für Windows, [Cyberduck](http://cyberduck.ch) für Mac und [FileZilla](http://filezilla-project.org) für fast alle Betriebssysteme grafische Clients. Das zu wählende Protokoll ist SFTP, die Serveradresse ist `ssh.physcip.uni-stuttgart.de`.
  

### Hinweis
Wenn du mehrfach hintereinander dein Passwort oder deinen Benutzernamen falsch eingibst, wirst du für 15 Minuten gesperrt (das äußert sich dann in einer Fehlermeldung wie `ssh: connect to host ssh.physcip.uni-stuttgart.de port 22: Connection refused` oder darin, dass die Verbindung sofort wieder getrennt wird).
  

### Public-Key-Authentifizierung
Da wir im Netzwerk aus Sicherheitsgründen passwortgeschützte Dateifreigaben benutzen, kann SSH-Public-Key-Authentifizierung (sprich: ohne Passwort) nur sehr eingeschränkt genutzt werden: Die Benutzung von SFTP oder interaktiven Shell-Sitzungen ist damit nicht möglich.
Es ist jedoch möglich, sogenanntes SSH-Forwarding mit Public-Key-Authentifizierung zu machen. Beim OpenSSH-Client sind dies die Optionen -L, -W und -D. Um dem Server zu sagen, dass man dies wünscht, muss man beim Verbindungsaufbau dem Benutzernamen den Präfix key- voranstellen, z.B. key-st123456. Um eine Warnmeldung zu unterdrücken, sei noch die Option -N empfohlen.
  
So kann z.B. eine nur aus dem Uninetz zugängliche Webseite auf den lokalen Rechner holen:
```
ssh key-st123456@ssh.physcip.uni-stuttgart.de -N -L 8080:www.verwaltung.uni-stuttgart.de:80
```
Damit ist die Verwaltungs-Webseite unter [http://localhost:8080](http://localhost:8080) zugänglich.
  
Man kann auf diesem Weg auch sogenanntes SSH-Bouncing machen. Dazu legt man eine Datei ~/.ssh/config an, in die man folgendes einträgt:
  
```
Host *.icp.uni-stuttgart.de
User mmustermann
ProxyCommand ssh -W %h:%p key-st123456@ssh.physcip.uni-stuttgart.de
```
  
Dies ist hilfreich, um andere Rechner per SSH zu erreichen, die zwar aus dem Uninetz erreichbar sind, nicht aber aus dem Internet. Hier ist als Beispiel der CIP-Pool des Instituts für Computerphysik als Zieladresse angegeben. Damit kann man einfach `ssh cip12.icp.uni-stuttgart.de` von zuhause machen, obwohl der Zielrechner nicht aus dem Internet erreichbar ist.
Beachte: Einige Anleitungen für SSH-Bouncing, die man im Internet findet, verwenden eine ProxyCommand-Zeile wie

```
ProxyCommand ssh key-st123456@ssh.physcip.uni-stuttgart.de nc %h %p
```

Dieser verwendet netcat und macht genau das gleiche wie die Zeile oben, wird aber bei uns **nicht** für Public-Key-Authentifizierung unterstützt.
