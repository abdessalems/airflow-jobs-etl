<div align="center">

# 🌬️ Jobs ETL — Airflow Pipeline

**Scaffold for an Airflow pipeline that scrapes Python job postings, cleans them, and persists them to Postgres.**

![Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=flat-square&logo=apacheairflow&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![Postgres](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Status](https://img.shields.io/badge/status-scaffold-F59E0B?style=flat-square)

</div>

---

> ## ⚠️ Status: scaffold — not implemented
>
> The infrastructure is defined (Docker Compose, dependencies, environment, project layout), but **the Python modules are empty**. The DAG, scraper, cleaner, repositories, tests and CI workflow exist as placeholder files with no code in them. Nothing runs yet.
>
> This README documents the intended design and what remains to be built. It is a starting point, not a working pipeline.

---

## Intended design

An ETL orchestrated by **Apache Airflow**, running on a **CeleryExecutor** with **Postgres** as the metadata and warehouse store and **Redis** as the broker:

```
scrape → clean → persist → export
```

1. **Scrape** — pull Python job postings from public job APIs
2. **Transform** — normalise and de-duplicate the postings
3. **Load** — write to Postgres
4. **Export** — CSV / report artefacts, optionally to S3, with Redis caching the raw payload

---

## Layout

| Path | Purpose | State |
|---|---|---|
| `docker-compose.yml` | Airflow, Postgres, Redis services | ✅ defined |
| `pyproject.toml` | Dependencies — requests, BeautifulSoup, pandas, SQLAlchemy, psycopg, redis, boto3, pydantic | ✅ defined |
| `.env.example` | Airflow, Postgres, Redis, S3 configuration | ✅ defined |
| `airflow/dags/jobs_etl_dag.py` | The DAG | ⬜ empty |
| `src/jobs_etl/scrape/scraper.py` | Job-posting scraper | ⬜ empty |
| `src/jobs_etl/transform/cleaner.py` | Normalisation / de-duplication | ⬜ empty |
| `src/jobs_etl/storage/postgres_repo.py` | Postgres repository | ⬜ empty |
| `src/jobs_etl/storage/redis_cache.py` | Redis cache | ⬜ empty |
| `src/jobs_etl/storage/s3_repo.py` | S3 export | ⬜ empty |
| `src/jobs_etl/config.py`, `domain.py` | Config and domain models | ⬜ empty |
| `tests/test_cleaner.py` | Unit tests | ⬜ empty |
| `.github/workflows/ci.yml` | CI pipeline | ⬜ empty |

> The source lives on the **`dev`** branch; `main` holds only this README.

---

## Running the infrastructure

The Docker stack does come up, even though the DAG is empty:

```bash
git clone -b dev https://github.com/abdessalems/airflow-jobs-etl.git
cd airflow-jobs-etl
cp .env.example .env
docker compose up -d
```

Airflow's UI is then at **http://localhost:8080**.

---

## Roadmap

- [ ] Domain models (`pydantic`) and config loading
- [ ] Scraper against a public jobs API
- [ ] Cleaner — normalise fields, drop duplicates
- [ ] Postgres repository (SQLAlchemy + psycopg)
- [ ] Redis cache for raw payloads
- [ ] S3 export of CSV / Markdown reports
- [ ] Wire the four tasks into `jobs_etl_dag.py`
- [ ] Unit tests for the cleaner
- [ ] CI workflow (ruff + pytest)
