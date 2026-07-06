# DevCLI — Developer Automation Toolchain

**Repository:** [View Source Code](https://github.com/brian-codington/devcli)

**Status:** Production Ready | Open Source

**PyPI:** `pip install devcli`

**Tech Stack:** Python, Click, Docker, Jinja2, Rich

---

## Project Overview

A command-line toolchain that automates repetitive developer workflows — from scaffolding new services to running deployments and managing environment configs. Built internally at NovaSoft and later open sourced.

**The Problem:** The engineering team was spending 2-3 hours per week on repetitive tasks: spinning up new microservices, syncing environment variables, running staged deployments, and generating boilerplate. Every developer did it slightly differently.

**The Solution:** A unified CLI that:
- Scaffolds new services from opinionated templates in under 30 seconds
- Syncs environment configs across dev/staging/prod via AWS SSM
- Automates Docker builds and pushes with one command
- Generates API documentation from OpenAPI specs
- Reduced repetitive dev work by **80%**

---

## Key Features

- **Service Scaffolding** — Generate full project structure from templates
- **Env Management** — Sync `.env` files with AWS SSM Parameter Store
- **Docker Automation** — Build, tag, and push images with smart caching
- **Doc Generation** — Auto-generate API docs from OpenAPI/Swagger specs
- **Plugin System** — Extend with custom commands via a plugin API

---

## Commands

```bash
# Scaffold a new microservice
devcli new service my-service --template fastapi

# Sync environment variables from AWS SSM
devcli env pull --env staging --output .env.staging

# Build and push Docker image
devcli docker push --tag v1.2.3 --registry ecr

# Generate API docs
devcli docs generate --spec openapi.yaml --output ./docs

# Run a staged deployment
devcli deploy --env staging --service my-service --tag v1.2.3
```

---

## Code Sample

### Service Scaffolding Command

```python
# devcli/commands/new.py
import click
from pathlib import Path
from jinja2 import Environment, FileSystemLoader
from rich.console import Console
from rich.progress import track

console = Console()

TEMPLATES = ["fastapi", "express", "django", "nextjs"]

@click.command()
@click.argument("name")
@click.option("--template", "-t", type=click.Choice(TEMPLATES), default="fastapi",
              help="Service template to use")
@click.option("--output", "-o", default=".", help="Output directory")
def new_service(name, template, output):
    """Scaffold a new microservice from a template."""

    output_path = Path(output) / name

    if output_path.exists():
        console.print(f"[red]Error:[/red] Directory '{name}' already exists.")
        raise click.Abort()

    console.print(f"\n[bold]Creating [cyan]{name}[/cyan] from [cyan]{template}[/cyan] template...[/bold]\n")

    env = Environment(loader=FileSystemLoader(f"templates/{template}"))
    template_files = env.list_templates()

    for template_file in track(template_files, description="Generating files..."):
        tmpl = env.get_template(template_file)
        content = tmpl.render(service_name=name, service_name_pascal=name.title().replace("-", ""))

        output_file = output_path / template_file
        output_file.parent.mkdir(parents=True, exist_ok=True)
        output_file.write_text(content)

    console.print(f"\n[green]✓[/green] Created [bold]{name}[/bold] in [bold]{output_path}[/bold]")
    console.print(f"\n[dim]Next steps:[/dim]")
    console.print(f"  cd {name}")
    console.print(f"  devcli env pull --env dev")
    console.print(f"  docker-compose up\n")
```

---

## Impact Metrics

| Metric | Result |
|--------|--------|
| Time Saved per Developer/Week | 2–3 hours |
| New Service Setup Time | 30 seconds (was 45+ minutes) |
| PyPI Downloads | 8,000+ |
| Open Source Contributors | 14 |

---

[← Back to Main Portfolio](../README.md)
