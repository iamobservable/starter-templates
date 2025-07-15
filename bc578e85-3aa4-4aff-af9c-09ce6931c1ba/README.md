# Open WebUI (OWUI) base template without GPU, nginx, or auth validator


---

## 👷 Template Overview

The [OWUI Base Template](https://github.com/iamobservable/starter-templates/tree/main/bc578e85-3aa4-4aff-af9c-09ce6931c1ba) is a quick template for setting up Open WebUI when you are not using a GPU, nginx or auth validator. More information can be found 
about configurations on the [Open WebUI Docs](https://docs.openwebui.com/) or the [main Gitub repository](https://github.com/open-webui/open-webui).

This template is used in combination with the [Open WebUI Starter project](https://github.com/iamobservable/open-webui-starter). After installing the starter project, it can be used to install this template.

**Note: This template is not connected with Open WebUI, but attempts to make sane defaults for setting up the project**


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

The OWUI template includes the following. A [Service Architecture Diagram](https://github.com/iamobservable/starter-templates/blob/main/bc578e85-3aa4-4aff-af9c-09ce6931c1ba/docs/service-architecture-diagram.md) is also available that describes how the components are connected.

- **[Docling](https://github.com/docling-project/docling-serve)**: Simplifies document processing, parsing diverse formats — including advanced PDF understanding — and providing seamless integrations with the gen AI ecosystem (created by IBM)
- **[Edge TTS](https://github.com/rany2/edge-tts)**: Python module that using Microsoft Edge's online text-to-speech service
- **[MCP Server](https://modelcontextprotocol.io/introduction)**: Open protocol that standardizes how applications provide context to LLMs
- **[Ollama](https://ollama.com/)**: Local service API serving open source large language models
- **[Open WebUI](https://openwebui.com/)**: Open WebUI is an extensible, feature-rich, and user-friendly self-hosted AI platform designed to operate entirely offline
- **[Postgresql](https://www.postgresql.org/)/[PgVector](https://github.com/pgvector/pgvector)**: (default PERSISTENCE ENGINE) A free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance (has vector addon)
- **[Redis](https://redis.io/)**: An open source-available, in-memory storage, used as a distributed, in-memory key–value database, cache and message broker, with optional durability
- **[Searxng](https://docs.searxng.org/)**: Free internet metasearch engine for open webui tool integration
- **[Sqlite](https://www.sqlite.org/index.html)**: (deprecated from project) A C-language library that implements a small, fast, self-contained, high-reliability, full-featured, SQL database engine
- **[Tika](https://tika.apache.org/)**: (default CONTENT_EXTRACTION_ENGINE) A toolkit that detects and extracts metadata and text from over a thousand different file types
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


### MCP Servers

Model Context Protocol (MCP) is a configurable set of tools, resources, prompts, samplings, and roots. They provide 
a structured way to expose local functionality to the LLM. Examples are providing access to the local file system, 
searching the internet, interacting with git or github, and much more.

#### Initial configuration

<img width="745" alt="image" src="https://github.com/user-attachments/assets/ae93c255-9473-440c-a313-fd267cb7296c" />

Configurations for MCP services can be found in the [conf/mcp/config.json.template](https://github.com/iamobservable/starter-templates/blob/main/bc578e85-3aa4-4aff-af9c-09ce6931c1ba/conf/mcp/config.json.template#)) file. Links below in the table describe the initially configuration.

***Note - the time tool is configured using uvx instead of directly with the python binary, as the repository describes*** 

A few tool examples are listed below.

| Tool                                                                               | Description                                                               | Configuration |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------- |
| [time](https://github.com/modelcontextprotocol/servers/tree/main/src/time)         | Provides current time values for [configured timezone](https://github.com/iamobservable/starter-templates/blob/main/conf/mcp/config.example#L5)                      | [config/mcp/config.json.template](https://github.com/iamobservable/starter-templates/blob/6e58e3e23c6661f591844aeed4bce73c15d35505/bc578e85-3aa4-4aff-af9c-09ce6931c1ba/conf/mcp/config.json.template#L3) |
| [postgres](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres) | Provides sql querying for the configured database (defaults to openwebui) | [config/mcp/config.json.template](https://github.com/iamobservable/starter-templates/blob/6e58e3e23c6661f591844aeed4bce73c15d35505/bc578e85-3aa4-4aff-af9c-09ce6931c1ba/conf/mcp/config.json.template#L7) |

#### MCP Server Discovery

The following three MCP Server sources are a great place to look for tools to add.

- [Model Context Protocol servers](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#model-context-protocol-servers)
- [Awesome MCP Servers](https://mcpservers.org/)
- [Smithery](https://smithery.ai/)

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

1. Edit and uncomment [env/watchtower.env.template](https://github.com/iamobservable/starter-templates/blob/6e58e3e23c6661f591844aeed4bce73c15d35505/bc578e85-3aa4-4aff-af9c-09ce6931c1ba/env/watchtower.env.template#L2) with a discord link. [More information](https://containrrr.dev/shoutrrr/v0.8/services/discord/) is provided on how to create a discord link (token@webhookid).
2. Restart the watchtower container

```bash
docker compose down watchtower && docker compose up watchtower -d
```

### Migrating from Sqlite to Postgresql

**Please note, the starter project does not use Sqlite for storage. Postgresql has been configured by default.**

For installations where the environment was already setup to use Sqlite, please refer to 
[Taylor Wilsdon](https://github.com/taylorwilsdon)'s github repository 
[open-webui-postgres-migration](https://github.com/taylorwilsdon/open-webui-postgres-migration). 
In it, he provides a migration tool for converting between the two databases.


---

## 💪 Contribution

I am deeply grateful for any contributions to the Observable World project! If you’d like to contribute, 
simply fork this repository and submit a [pull request](https://github.com/iamobservable/starter-templates/pulls) with any improvements, additions, or fixes you’d 
like to see. I will review and consider any suggestions — thank you for being part of this journey!

---


## License

This project is licensed under the [Apache 2 License](https://github.com/iamobservable/starter-templates?tab=Apache-2.0-1-ov-file#readme). Find more in the [LICENSE document](https://raw.githubusercontent.com/iamobservable/starter-templates/refs/heads/main/LICENSE).

