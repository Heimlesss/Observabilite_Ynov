Exercice 1 : 

Suivre les étapes https://grafana.com/docs/alloy/latest/tutorials/send-logs-to-loki/

<img width="2209" height="614" alt="image" src="https://github.com/user-attachments/assets/2cbcd334-a2be-49c8-a611-41a76146adff" />


<img width="2546" height="1254" alt="Capture d&#39;écran 2026-05-18 100318" src="https://github.com/user-attachments/assets/8de48231-0fa7-4036-b57d-14d7fa765e96" />


Exercice 2 : 

Modification du config.alloy

```
local.file_match "local_files" {
  path_targets = [{"__path__" = "/var/log/*.log"}]
  sync_period = "5s"
}
loki.source.file "log_scrape" {
  targets    = local.file_match.local_files.targets
  forward_to = [loki.process.filter_logs.receiver]
  tail_from_end = true
}
loki.process "filter_logs" {
  stage.label_drop {
    values = ["filename"]
  }
  stage.drop {
    source              = ""
    expression          = ".*Connection closed by authenticating user root"
    drop_counter_reason = "noisy"
  }
    stage.static_labels {
    values = {
      environment = "development",
    }
  }
  stage.regex {
  expression = "(?i)(?P<loglevel>INFO|WARN|WARNING|ERROR|DEBUG|CRITICAL|NOTICE|FATAL)"
}
    stage.labels {
  values = {
    loglevel = null,
  }
}
  
  forward_to = [loki.write.grafana_loki.receiver]
}
loki.write "grafana_loki" {
  endpoint {
    url = "http://localhost:3100/loki/api/v1/push"

    // basic_auth {
    //  username = "admin"
    //  password = "admin"
    // }
  }
}
```     

Les labels loglevel et environment apparaissent bien sur Grafana
<img width="2540" height="1261" alt="image" src="https://github.com/user-attachments/assets/bc55991c-b199-4043-8732-abf3a4d7eaf9" />

Exercice 3 : 

Préparer le répertoire et la config Alloy
```
sudo mkdir -p /var/log/apps
sudo chmod 777 /var/log/apps
``` 
Modifier la config Alloy pour ajouter le nouveau répertoire :
```
alloylocal.file_match "local_files" {
  path_targets = [
    {"__path__" = "/var/log/*.log"},
    {"__path__" = "/var/log/apps/*.log"},
  ]
  sync_period = "5s"
}
```
Ne pas oublier de copier la conf et reload avec : 
```
sudo cp config.alloy /etc/alloy/config.alloy
sudo systemctl reload alloy
curl -X POST http://localhost:12345/-/reload
```
<img width="2066" height="1088" alt="image" src="https://github.com/user-attachments/assets/48d6222a-4c89-495a-b53d-b87a877df82d" />

Verification des logs dans Grafana

<img width="2547" height="1264" alt="image" src="https://github.com/user-attachments/assets/136d6151-1100-450a-9aa3-5cc6c8033bac" />
