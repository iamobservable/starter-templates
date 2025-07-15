# N8N Basic Template


---

## 👷 Template Overview

The [N8N Basic Template](https://github.com/iamobservable/starter-templates/tree/main/51524bbb-e387-48a2-91b1-6b8be1d09d3f) is a quick start for setting up N8N. More information can be found 
about self-hosting N8N in the [N8N Docker Compose Docs](https://docs.n8n.io/hosting/installation/server-setups/docker-compose/).

**Note: This template is not connected with N8N, but attempts to make setup of N8N a one step command**


---

## Table of Contents
1. [Connect](#-connect-with-the-observable-world-community)
2. [Subscriptions & Donations](#%EF%B8%8F-subscriptions--donations)
3. [Tooling and Applications](#tooling-and-applications)
4. [Additional Setup](#additional-setup)
5. [Contribution](#-contribution)
6. [License](#license)

---


## 📢 Connect with the Observable World Community

Welcome! Join the [Observable World Discord](https://discord.gg/xD89WPmgut) to connect with like-minded 
others and get real-time support. If you encounter any challenges, I'm here to help however I can!

---


## ❤️ Subscriptions & Donations

Thank you for finding this useful! Your support means the world to me. If you’d like to [help me 
continue sharing code freely](https://github.com/sponsors/iamobservable), any subscripton or donation—no matter 
how small—would go a long way. Together, we can keep this community thriving!

---


## Tooling and Applications

The N8N Basic Template includes the following. A [Service Architecture Diagram](https://github.com/iamobservable/starter-templates/blob/main/51524bbb-e387-48a2-91b1-6b8be1d09d3f/docs/service-architecture-diagram.md) is also available that describes how the components are connected.

- **[N8N](https://docs.n8n.io/)**: Leading visual editor for fast iteration where you see results instantly. Build what you need with native nodes or custom code – perfect for both everyday automations and complex AI agent workflows
- **[Ollama](https://ollama.com/)**: Local service API serving open source large language models
- **[Postgresql](https://www.postgresql.org/)/[PgVector](https://github.com/pgvector/pgvector)**: (default PERSISTENCE ENGINE) A free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance (has vector addon)
- **[Watchtower](https://github.com/containrrr/watchtower)**: Automated Docker container for updating container images automatically

---


## Additional Setup

### Download more Ollama models

The install.sh script automatically downloaded the below two models:

1. nomic-embed-text:latest (used for RAG embeddings)
2. qwen3:0.6b (small model for testing purposes)

Additional LLMs can be downloaded for Ollama using the following commands. Qwen3:4b is listed below, but feel free to use any model that works best. [More on Ollama models](https://ollama.com/search)

```sh
docker compose exec ollama bash

ollama pull qwen3:4b
```


### Watchtower and Notifications

A Watchtower container provides a convenient way to check in on all container
versions to see if updates have been released. Once updates are found, Watchtower 
will pull the latest container image(s), stop the currently running container and 
start a new container based on the new image. **And it is all automatic, look no hands!**

After completing its process, 
Watchtower can send notifications. More can be found on notifications via 
the [Watchtower website](https://containrrr.dev/watchtower/notifications/).

For the sake of simplicity, this document will cover the instructions for setting 
up notifications via Discord. The Watchtower [arguments section](https://containrrr.dev/watchtower/arguments/) describes 
additional settings available for the watchtower setup.

1. Edit and uncomment [env/watchtower.env.template](https://github.com/iamobservable/starter-templates/blob/main/51524bbb-e387-48a2-91b1-6b8be1d09d3f/env/watchtower.env.template#L2) with a discord link. [More information](https://containrrr.dev/shoutrrr/v0.8/services/discord/) is provided on how to create a discord link (token@webhookid).
2. Restart the watchtower container

```bash
docker compose down watchtower && docker compose up watchtower -d
```

---


## 💪 Contribution

I am deeply grateful for any contributions to the Observable World project! If you’d like to contribute, 
simply fork this repository and submit a [pull request](https://github.com/iamobservable/starter-templates/pulls) with any improvements, additions, or fixes you’d 
like to see. I will review and consider any suggestions — thank you for being part of this journey!

---


## License

This project is licensed under the [Apache 2 License](https://github.com/iamobservable/starter-templates?tab=Apache-2.0-1-ov-file#readme). Find more in the [LICENSE document](https://raw.githubusercontent.com/iamobservable/starter-templates/refs/heads/main/LICENSE).

