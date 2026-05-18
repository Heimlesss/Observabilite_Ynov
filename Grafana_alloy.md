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
