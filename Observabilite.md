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
  

