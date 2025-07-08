# OWUI base template

The [OWUI Base Template](https://github.com/iamobservable/starter-templates/tree/main/4b35c72a-6775-41cb-a717-26276f7ae56e) is a quick template for setting up Open WebUI. More information can be found 
about configurations on the [Open WebUI Docs](https://docs.openwebui.com/) or the [main Gitub repository](https://github.com/open-webui/open-webui).


---

## 👷 Template Overview

This template is used in combination with the [Open WebUI Starter project](https://github.com/iamobservable/open-webui-starter). After installing the starter project, it can be used to install this template.


---

## Table of Contents
1. [Connect](#-connect-with-the-observable-world-community)
2. [Donations](#service-examples)
3. [Tooling and Applications](#tooling-and-applications)
4. [JWT Auth Validator Purpose](#jwt-auth-validator-purpose)
5. [Contribution](#contribution)
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


## 💪 Contribution

I am deeply grateful for any contributions to the Observable World project! If you’d like to contribute, 
simply fork this repository and submit a [pull request](https://github.com/iamobservable/open-webui-starter/pulls) with any improvements, additions, or fixes you’d 
like to see. I will review and consider any suggestions — thank you for being part of this journey!

---


## License

This project is licensed under the [MIT License](https://en.wikipedia.org/wiki/MIT_License). Find more in the [LICENSE document](https://raw.githubusercontent.com/iamobservable/open-webui-starter/refs/heads/main/LICENSE).

