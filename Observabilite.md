<h1>TP Observabilité</h1>

<h2>Module prometheus</h2>

<h3>Exercice 1</h3>

Création d'un fichier docker-compose.yml </br>

```
prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    restart: no

```
Atteindre prometheus avec l'url cible : localhost:9090 </br>
Pour vérifier l'était de santé : </br>
Status>Target : </br>
<img width="2544" height="289" alt="image" src="https://github.com/user-attachments/assets/e622dcd1-0651-44cf-91e0-3760fe4d9d59" />
Pour vérifier que Prometheus se scrap lui même, atteindre prometheus.yml (chemin affiché dans les logs du container, par défaut : /etc/prometheus/prometheus.yml) : 
<img width="1535" height="253" alt="image" src="https://github.com/user-attachments/assets/a2ac0287-0bb8-4106-b77a-619713e72a8e" />


<h3>Exercice 2</h3>
Modififer le fichier de configuration prometheus.yml

```
global:
  scrape_interval: 10s
  external_labels:
    environnment: lab

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
       
```
Recharger avec un <blockquote>curl -X POST http://localhost:9090/-/reload</blockquote>
Verifier la manipulation dans prometheus dans status>Configuration
    
<img width="1293" height="1073" alt="image" src="https://github.com/user-attachments/assets/3c26dc9d-0860-4802-b27a-344f0c91ec45" />

<h3>Exercice 3</h3>
Ajout d'un container node_exporter dans le compose
   
```
services:
  prometheus:
    image: prom/prometheus
    command:
      - --web.enable-lifecycle
      - --config.file=/etc/prometheus/prometheus.yml
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:</br>
      - "9090:9090"
    restart: no

  node_exporter:
    image: prom/node-exporter
    ports:</br>
      - "9100:9100"
    restart: no
   
```
Ajouter un nouveau job nommé 'node' dans prometheus.yml 
   
```
global:
  scrape_interval: 10s
  external_labels:
    environnment: lab
scrape_configs:</br>
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
      - targets: ['node_exporter:9100']
```
Verifier le statut sur prometheus status>Target Health
<img width="2545" height="213" alt="image" src="https://github.com/user-attachments/assets/85c93c86-80e6-425e-bb10-300cbb80711b" />

Tester node_cpu_seconds_total sur prometheus en query 
Verifier le retour : 
<img width="2559" height="1077" alt="image" src="https://github.com/user-attachments/assets/46be0346-98a5-4c19-97f3-eabe7bcbde28" />

