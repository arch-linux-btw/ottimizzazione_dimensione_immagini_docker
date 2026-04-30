# Docker Image Optimization

Progetto scolastico per il corso ITS Systems Security.

## Obiettivo
Ottimizzare un'immagine Docker da ~1GB a meno di 100MB usando
multi-stage build e best practices.

## Struttura
├── app/                    # Applicazione Flask
├── Dockerfile.naive        # Immagine non ottimizzata (~1GB)
├── Dockerfile.optimized    # Immagine ottimizzata (<100MB)
├── .dockerignore           # Esclusioni build
└── optimization-report.md  # Confronto size e tecniche usate

## Tecniche usate
- Multi-stage build
- Immagine base Alpine Linux
- Layer caching ottimizzato
- Utente non-root

## Gruppo
- Ale — App + Dockerfile.naive
- Paolo — Dockerfile.optimized + .dockerignore
- Filippo — Analisi size + documentazione
