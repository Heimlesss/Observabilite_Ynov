<h1>TP Observabilité</h1>

<h2>Module prometheus</h2>

<h3>Exercice 1</h3>

Création d'un fichier docker-compose.yml </br>
<blockquote>  prometheus:</br>
    image: prom/prometheus</br>
    volumes:</br>
      - ./prometheus.yml:/etc/prometheus/prometheus.yml</br>
    ports:</br>
      - "9090:9090"</br>
    restart: no</br></blockquote>
  
Pour vérifier l'était de santé : </br>
Status>Target : </br>
<img width="2544" height="289" alt="image" src="https://github.com/user-attachments/assets/e622dcd1-0651-44cf-91e0-3760fe4d9d59" />

