# Docker Image Optimization

Ottimizzazione immagini Docker usando 
multi-stage build e Alpine Linux.

## Obiettivo

Dimostrare come ridurre drasticamente la dimensione di un'immagine Docker
da 752MB a 53.6MB (-92.9%) applicando best practices reali.

## Risultati

| Versione | Base Image | Size | 
|---|---|---|
| Naive | ubuntu:22.04 | 752MB |
| Ottimizzata | python:3.12-alpine | 53.6MB |

**Riduzione totale: 92.9%** (requisito minimo: 70%)

## Struttura

```
docker-image-optimization/
├── app/
│   ├── app.py            # Applicazione Flask
│   └── requirements.txt  # Dipendenze (flask==3.1.0)
├── Dockerfile.naive       # Versione non ottimizzata (752MB)
├── Dockerfile.optimized   # Versione ottimizzata (53.6MB)
├── .dockerignore          # Esclusioni build
└── optimization-report.md # Analisi e confronto
```

## Come eseguire

**Versione naive:**
```bash
docker build -f Dockerfile.naive -t app:naive .
docker run -d -p 5000:5000 app:naive
```

**Versione ottimizzata:**
```bash
docker build -f Dockerfile.optimized -t app:optimized .
docker run -d -p 5000:5000 app:optimized
```

Apri il browser su `http://localhost:5000`

## Tecniche di ottimizzazione

- **Multi-stage build** — separa lo stage di build da quello finale
- **Alpine Linux** — immagine base minimale vs ubuntu
- **Layer caching** — COPY requirements.txt prima del codice
- **Utente non-root** — USER nobody per sicurezza
- **.dockerignore** — esclude file inutili dalla build
