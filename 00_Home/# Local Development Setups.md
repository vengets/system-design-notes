

# Local Development Setups

> Centralized reference for locally running tools, containers, AI workspaces, and development environments.

---

|Tool|Purpose|Local Path|Ports|Access|Status|
|---|---|---|---|---|---|
|**StaffMentor-OS**|AI-powered career mentor, learning tracker, interview prep, job tracking, and engineering growth platform|`~/code/projects/staffmentor-os`|`3000`, `8080`|Frontend: `http://localhost:3000`Backend: `http://localhost:8080`|🟢 Active|
|**JupyterForAI**|AI experimentation workspace with OpenAI, Claude, LangChain, LangGraph, JupyterLab, and notebooks running inside Docker|`~/code/containers/jupyterForAi`|`8888`|`http://localhost:8888`|🟢 Active|

---

# Docker Commands

|Action|Command|
|---|---|
|Start container|`docker compose up -d`|
|Rebuild container|`docker compose up --build`|
|Stop container|`docker compose down`|
|View logs|`docker logs <container-name>`|
|Enter container shell|`docker exec -it <container-name> bash`|
|Running containers|`docker ps`|

---

# AI Workspace Stack (JupyterForAI)

|Category|Tools|
|---|---|
|LLM Providers|OpenAI, Claude|
|AI Frameworks|LangChain, LangGraph|
|Notebook Environment|JupyterLab|
|Vector/Embeddings|ChromaDB|
|Backend APIs|FastAPI|
|Utilities|pandas, matplotlib, requests|

---

# Future Additions

|Tool|Purpose|Ports|Notes|
|---|---|---|---|
|Ollama|Local LLM hosting|TBD|Planned|
|Grafana|Observability dashboards|TBD|Planned|
|Prometheus|Metrics collection|TBD|Planned|
|Kafka|Event-driven workflows|TBD|Planned|
|PostgreSQL|Persistent memory/storage|TBD|Planned|

---

# Notes

- Prefer Dockerized local setups for reproducibility and isolation.
    
- Keep ports documented to avoid collisions.
    
- Use `.env` files for secrets and API keys.
    
- Maintain one compose file per independent workspace/tool when possible.
    
- Update this page whenever adding new local services or containers.