# Open WebUI (OWUI) base template


---

## 👷 Template Overview

The [OWUI Base Template](https://github.com/iamobservable/starter-templates/tree/main/bccab6b1-091c-498a-abed-13c3b0b705da) is a quick template for setting up Open WebUI when you are not using a GPU. More information can be found 
about configurations on the [Open WebUI Docs](https://docs.openwebui.com/) or the [main Gitub repository](https://github.com/open-webui/open-webui).

This template is used in combination with the [Open WebUI Starter project](https://github.com/iamobservable/open-webui-starter). After installing the starter project, it can be used to install this template.

**Note: This template is not connected with Open WebUI, but attempts to make sane defaults for setting up the project**


---

## Table of Contents
1. [Connect](#-connect-with-the-observable-world-community)
2. [Donations](#service-examples)
3. [Tooling and Applications](#tooling-and-applications)
4. [JWT Auth Validator Purpose](#jwt-auth-validator-purpose)
5. [Additional Setup](#additional-setup)
6. [Service Examples](#service-examples)
7. [Contribution](#contribution)
8. [License](#license)

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

The OWUI template includes the following. A [Service Architecture Diagram](https://github.com/iamobservable/starter-templates/blob/main/4b35c72a-6775-41cb-a717-26276f7ae56e/docs/service-architecture-diagram.md) is also available that describes how the components are connected.

- **[JWT Auth Validator](https://github.com/iamobservable/jwt-auth-validator)**: Provides a service for the Nginx proxy to validate the OWUI token signature for restricting access
- **[Docling](https://github.com/docling-project/docling-serve)**: Simplifies document processing, parsing diverse formats — including advanced PDF understanding — and providing seamless integrations with the gen AI ecosystem (created by IBM)
- **[Edge TTS](https://github.com/rany2/edge-tts)**: Python module that using Microsoft Edge's online text-to-speech service
- **[MCP Server](https://modelcontextprotocol.io/introduction)**: Open protocol that standardizes how applications provide context to LLMs
- **[Nginx](https://nginx.org/)**: Web server, reverse proxy, load balancer, mail proxy, and HTTP cache
- **[Ollama](https://ollama.com/)**: Local service API serving open source large language models
- **[Open WebUI](https://openwebui.com/)**: Open WebUI is an extensible, feature-rich, and user-friendly self-hosted AI platform designed to operate entirely offline
- **[Postgresql](https://www.postgresql.org/)/[PgVector](https://github.com/pgvector/pgvector)**: (default PERSISTENCE ENGINE) A free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance (has vector addon)
- **[Redis](https://redis.io/)**: An open source-available, in-memory storage, used as a distributed, in-memory key–value database, cache and message broker, with optional durability
- **[Searxng](https://docs.searxng.org/)**: Free internet metasearch engine for open webui tool integration
- **[Sqlite](https://www.sqlite.org/index.html)**: (deprecated from project) A C-language library that implements a small, fast, self-contained, high-reliability, full-featured, SQL database engine
- **[Tika](https://tika.apache.org/)**: (default CONTENT_EXTRACTION_ENGINE) A toolkit that detects and extracts metadata and text from over a thousand different file types
- **[Watchtower](https://github.com/containrrr/watchtower)**: Automated Docker container for updating container images automatically

---


## JWT Auth Validator Purpose

As this project was being developed, I had a need to restrict access to all web requests to the environment. The initial goal was to restrict the /docs link of a given dev version of OWUI -- it comes out of the box with unrestricted access. I also was using Redis and Searxng and wanted to use the same authentication as OWUI, so as not to require an additional "login" when accessing these sites. So I started researching how OWUI is managing authentication. During the research, I found OWUI creates a [JWT (JSON Web Token)](https://en.wikipedia.org/wiki/JSON_Web_Token) during the authentication process. This JWT (JSON Web Token) allows OWUI to verify a user has authenticated by validating the signature created in the JWT. The JWT is stored in the browser as a cookie and passed to any subsequent requests on the same Host + Port (e.g. localhost:4000). Given this pattern, I decided to let the nginx proxy become a gatekeeper of sorts. I setup an nginx configuration to capture the request header for the JWT and pass it to an internal service for verifying the signature. That service is the JWT Auth Validator. Its sole purpose is to tell the nginx proxy if a signature was signed by the appropriate authority -- in this case OWUI -- and return True or False to the nginx proxy. The nginx proxy then, based on a True response, allows the initial request to continue through to the expected service container. If the response is False, nginx will redirect to a /auth route for login.

More can be found about the configuration at [the JWT Auth Validator documentation](https://github.com/iamobservable/jwt-auth-validator?tab=readme-ov-file#nginx-proxy-example).

*For its use in this project, I created an [image for the JWT Auth Validator](https://github.com/iamobservable/jwt-auth-validator/pkgs/container/jwt-auth-validator). This allows a prebuilt docker image that is pulled during the setup process. This eliminates the need to build during the docker compose up step.*

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

Configurations for MCP services can be found in the [conf/mcp/config.json.template](https://github.com/iamobservable/starter-templates/blob/main/4b35c72a-6775-41cb-a717-26276f7ae56e/conf/mcp/config.json.template#)) file. Links below in the table describe the initially configuration.

***Note - the time tool is configured using uvx instead of directly with the python binary, as the repository describes*** 

A few tool examples are listed below.

| Tool                                                                               | Description                                                               | Configuration |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------- |
| [time](https://github.com/modelcontextprotocol/servers/tree/main/src/time)         | Provides current time values for [configured timezone](https://github.com/iamobservable/open-webui-starter/blob/main/conf/mcp/config.example#L5)                      | [config/mcp/config.json.template](https://github.com/iamobservable/starter-templates/blob/6e58e3e23c6661f591844aeed4bce73c15d35505/4b35c72a-6775-41cb-a717-26276f7ae56e/conf/mcp/config.json.template#L3) |
| [postgres](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres) | Provides sql querying for the configured database (defaults to openwebui) | [config/mcp/config.json.template](https://github.com/iamobservable/starter-templates/blob/6e58e3e23c6661f591844aeed4bce73c15d35505/4b35c72a-6775-41cb-a717-26276f7ae56e/conf/mcp/config.json.template#L7) |

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

1. Edit and uncomment [env/watchtower.env.template](https://github.com/iamobservable/starter-templates/blob/6e58e3e23c6661f591844aeed4bce73c15d35505/4b35c72a-6775-41cb-a717-26276f7ae56e/env/watchtower.env.template#L2) with a discord link. [More information](https://containrrr.dev/shoutrrr/v0.8/services/discord/) is provided on how to create a discord link (token@webhookid).
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

## Service Examples

This section is to show how services within docker compose infra can be used 
directly or programmatically without the Open WebUI interface. The examples 
have been created as *.sh scripts that can be executed via the command line.


### Docling

**PDF document to markdown**

Generates a JSON document with the markdown text included. Changes to the config.json document, located in the same directory, can change how Docling responds. More information on how to configure Docling can be found in the [Advanced usage section](https://github.com/docling-project/docling-serve/blob/main/docs/usage.md) of the [Docling Serve documentation](https://github.com/docling-project/docling-serve/blob/main/docs/README.md).

```sh
curl -X POST "http://localhost:3000/docling/v1alpha/convert/source" \
    -H "Cookie: token=<add-jwt-token>" \
    -H "accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ \
      "options": { \
        "do_picture_description": false, \
        "image_export_mode": "embedded", \
        "images_scale": 2.0, \
        "include_images": false, \
        "return_as_file": false, \
        "to_formats": ["md"] \
      }, \
      "http_sources": [{ "url": "https://arxiv.org/pdf/2408.09869" }] \
    }'
```


### Edgetts

EdgeTTS is a service integration that uses Microsoft's online text-to-speech 
service. Keep in mind, if this is NOT completely local, as it requires a
connection to Microsoft's service.

*More information about [available voice samples](https://tts.travisvn.com/) 
the [EdgeTTS codebase and configuration](https://github.com/travisvn/openai-edge-tts)*.

**Speech in Spanish**

Generate Spanish speech from a speaker with a Spanish accent.

```sh
curl -X POST "http://localhost:3000/edgetts/v1/audio/speech" \
    -H "Cookie: token=<add-jwt-token>" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer your_api_key_here" \
    -d '{ \
      "input": "Hola! Mi nombre es Alonso", \
      "response_format": "mp3", \
      "speed": 1, \
      "stream": true, \
      "voice": "es-US-AlonsoNeural", \
      "model": "tts-1-hd" \
    }' > alonso-es-hola.mp3
```

**Speech in English**

Generates English speech from a speaker with an English accent.

```sh
curl -X POST "http://localhost:3000/edgetts/v1/audio/speech" \
    -H "Cookie: token=<add-jwt-token>" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer your_api_key_here" \
    -d '{ \
      "input": "Hi, my name is Wayland. This is an audio example.", \
      "response_format": "mp3", \
      "speed": 1, \
      "stream": true, \
      "voice": "en-US-AndrewMultilingualNeural", \
      "model": "tts-1-hd" \
    }' > wayland-intro.mp3
```


### Tika

**Information about the PDF document**

Generates meta data from a provided url. More information can be found via the [Metadata Resource documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=148639291#TikaServer-MetadataResource)

```sh
curl https://arxiv.org/pdf/2408.09869v5 > 2408.09869v5.pdf
curl http://localhost:3000/tika/meta \
    -H "Cookie: token=<add-jwt-token>" \
    -H "Accept: application/json" -T 2408.09869v5.pdf 
```

**PDF document (url) to HTML**

Generates HTML from a provided url. More information can be found via the [Tika Resource Documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=148639291#TikaServer-GettheTextofaDocument)

```sh
curl https://arxiv.org/pdf/2408.09869v5 > 2408.09869v5.pdf
curl http://localhost:3000/tika/tika \
    -H "Cookie: token=<add-jwt-token>" \
    -H "Accept: text/html" -T 2408.09869v5.pdf 
```

**PDF document (url) to plain text**

Generates plain text from a provided url. More information can be found via the [Tika Resource Documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=148639291#TikaServer-GettheTextofaDocument)

```sh
curl https://arxiv.org/pdf/2408.09869v5 > 2408.09869v5.pdf
curl http://localhost:3000/tika/tika \
    -H "Cookie: token=<add-jwt-token>" \
    -H "Accept: text/plain" -T 2408.09869v5.pdf 
```

---

## 💪 Contribution

I am deeply grateful for any contributions to the Observable World project! If you’d like to contribute, 
simply fork this repository and submit a [pull request](https://github.com/iamobservable/starter-templates/pulls) with any improvements, additions, or fixes you’d 
like to see. I will review and consider any suggestions — thank you for being part of this journey!

---


## License

This project is licensed under the [Apache 2 License](https://github.com/iamobservable/starter-templates?tab=Apache-2.0-1-ov-file#readme). Find more in the [LICENSE document](https://raw.githubusercontent.com/iamobservable/starter-templates/refs/heads/main/LICENSE).

