# FalconThreatHunting

lsass.exe’nin unsigned process tarafından çalıştırılması ve erişimlerin tespit edilmesi için

`"#event_simpleName" = ProcessRollup2
| ParentBaseFileName = "lsass.exe"`

Sisteme sızan saldırganlar kullanıcıların dizinlerinde Microsoft’a ait legal uygulamaları kullanarak kendi amaçları doğrultusundaki script’leri çalıştırmak adına wscript veya cscript legal prosesleri kullanmaktadır. 
wscript veya cscript kullanılmasının tespiti için

`| FileName : ("wscript" OR "cscript") OR ParentBaseFileName : ("wscript" OR "cscript")`

Mshta.exe'nın shell başlatmasının tespiti

`| FileName : "mshta.exe" OR ParentBaseFileName : "mshta.exe"`

Şüpheli Path’lerde Çalışan Dosyaların Tespiti

`| FilePath : ("Windows\\Temp\\" OR "Users\\Public\\" OR "Windows\\Debug\\" OR "Users\\Default\\" OR "$Recycle.Bin\\" OR "Windows\\Media\\" OR "Windows\\Repair\\" OR "PerfLogs\\")`

Oluşturulan Servislerin Tespiti

`| CommandLine : ("sc create" OR "New-Service" OR "sc start" OR "start-service" OR "set-service")
| FileName : ("sc.exe" OR "powershell.exe")`

Oluşturulan Zamanlanmış (Scheduled) Görevlerin Tespiti

`| CommandLine : (("schtasks /create" OR "schtasks /delete" OR "schtasks /run" OR "schtasks /change" OR "schtasks /end") OR ("powershell" AND "New-ScheduledTask"))
| FileName : ("schtasks.exe")`

Webshell Aktivitelerinin Tespiti

`| ParentBaseFileName : ("w3wp.exe" OR "tomcat" OR "httpd.exe" OR "nginx.exe")
| FileName : ("cmd.exe" OR "powershell.exe" OR "net.exe" OR "whoami.exe" OR "hostname.exe" OR "ipconfig.exe" OR "systeminfo.exe" OR "*.php" OR "*.asp" OR "*.jsp")`
