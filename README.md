# Starter Templates

A pre-configured server environment bundled with Docker Compose and configuration files, designed to simplify setup. They authenticate the owner, validate best practices, and enforce security, ensuring a reliable, secure, and standardized foundation for your local or cloud managed server.

As the platform evolves, more templates will be added for other tools and applications, expanding the range of use cases and ensuring a standardized, secure, and best-practice-based environment for any server setup.


---

## 👷 Project Overview

This repository is meant to be used in combination with the [Open WebUI starter](https://github.com/iamobservable/open-webui-starter). While the project initial was created to create an Open WebUI environment quickly, additional templates are available to build other environments.


---

## Table of Contents
1. [Connect](#-connect-with-the-observable-world-community)
2. [Subscriptions & Donations](#%EF%B8%8F-subscriptions--donations)
3. [Current Templates](#current-templates)
4. [Learn About Templates](#locker-yaml-definition)
5. [Dependencies](#dependencies)
6. [Contribution](#-contribution)
7. [Star History](#-star-history)
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

## Current Templates

- [Open Webui Starter Template](https://github.com/iamobservable/starter-templates/tree/main/4b35c72a-6775-41cb-a717-26276f7ae56e)
- [Open Webui Starter Template - No GPU](https://github.com/iamobservable/starter-templates/tree/main/bccab6b1-091c-498a-abed-13c3b0b705da)
- [Open Webui Starter Template - No GPU, Nginx, or Auth Validator](https://github.com/iamobservable/starter-templates/tree/main/bc578e85-3aa4-4aff-af9c-09ce6931c1ba)
- [N8N Basic Template](https://github.com/iamobservable/starter-templates/tree/main/51524bbb-e387-48a2-91b1-6b8be1d09d3f)


---

## Locker Yaml Definition

The locker.yaml defines configuration templates for Docker-based application stacks. It's a declarative specification that allows users to create customizable, multi-service environments with user inputs, dynamic variable assignment, and automated setup commands.

### File Definition with Key Descriptions

**Core Structure:**
- `id`: Unique UUID identifier for the template
- `title`: Human-readable template name  
- `description`: Template purpose explanation
- `url`: Repository URL for the template
- `files`: Array of template files to process (e.g., `compose.yaml.template`)
- `inputs`: User-configurable variables and dynamic value generators
- `assignments`: Mappings from inputs to configuration files with formatting
- `commands`: Executable shell commands for post-creation setup and management

### What are Inputs

Inputs define variables that customize the deployment and come in two types:

**Human Inputs** - User-prompted values with required fields:
- `key`: Variable name (required)
- `title`: Question shown to user (required)  
- `default`: Default value (required)

```yaml
- key: NGINX_HOST
  title: "What ip or domain name should we use"
  default: localhost
```

**Dynamic Inputs** - Automatically generated values with required fields:
- `key`: Variable name (required)
- `type`: Generation method (required)

```yaml
- key: SECRETKEY
  type: dynamic:RandStr 16
```

**Dynamic types include:**
- `dynamic:RandStr N` - Random N-character string where N is any positive integer (e.g., `dynamic:RandStr 16`, `dynamic:RandStr 32`)
- `dynamic:Timezone` - System timezone

### What are Assignments  

Assignments are used to set values in configuration files by mapping input variables to file content. They generate dynamically formatted values by substituting input variables into format templates using placeholders that conform to the Unix printf function specification.

```yaml
assignments:
  - service: auth            # Service identifier
    format: |               # Multi-line content template
      GIN_MODE="release"
      JWT_SECRET="%s"       # printf-style placeholders for variable substitution
    inputs:
      - SECRETKEY           # Variables substituted in order
    path: "env/auth.env"    # Output file path
```

Assignments can be used with inputs to generate various file types: environment files, Docker Compose YAML, JSON configs, and Nginx templates. The format string supports complex multi-line content where variables are substituted using standard printf format specifiers (`%s`, `%d`, `%f`, etc.) in the order they appear in the inputs array.

### What are Commands

Commands define executable shell commands that can be executed after the project has been created. These are post-creation setup and management operations that automate common tasks like starting services, downloading models, or configuring components.

```yaml
commands:
  - name: "Download embedding model"  # Human-readable description
    command: "docker compose exec ollama ollama pull %s"  # Shell command with placeholders
    inputs:
      - EMBEDDING_MODEL     # Variables for substitution (optional)
```

Commands support variable substitution using printf-style placeholders and the inputs array. They are typically used for:
- **Service management**: `docker compose up service -d`
- **Model/asset downloads**: `docker compose exec ollama ollama pull %s` 
- **Configuration updates**: `wget` commands for remote configs
- **Status reporting**: `echo "View in browser at http://%s:%s/"`
- **Multi-step setup sequences**: Ordered list of commands for complete environment initialization

The specification supports complex multi-service stacks like the Open WebUI example with 12+ services including authentication, databases, LLM serving, and reverse proxies.


---

## Dependencies

- **[Starter](https://github.com/iamobservable/open-webui-starter)**: Tool for quickly creating docker compose environments based on predefine templates


---

## 💪 Contribution

I am deeply grateful for any contributions to the Observable World project! If you’d like to contribute, 
simply fork this repository and submit a [pull request](https://github.com/iamobservable/starter-templates/pulls) with any improvements, additions, or fixes you’d like to see. I will review and consider any suggestions — thank you for being part of this journey!


---

## ✨ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=iamobservable/starter-templates&type=Date)](https://www.star-history.com/#iamobservable/starter-templates&Date)


---

## License

This project is licensed under the [Apache 2 License](https://github.com/iamobservable/starter-templates?tab=Apache-2.0-1-ov-file#readme). Find more in the [LICENSE document](https://github.com/iamobservable/starter-templates/blob/main/LICENSE).

