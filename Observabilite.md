<h1>TP Observabilité</h1>

<h2>Module prometheus</h2>

<h3>Exercice 1</h3>

Création d'un fichier docker-compose.yml
<blockquote>  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    restart: no</blockquote>
  

