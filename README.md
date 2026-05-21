# OSDO Pre-commit Hooks

Configuraciones Docker-based para pre-commit hooks en múltiples lenguajes y frameworks.
Todas las herramientas se ejecutan en contenedores Docker para garantizar reproducibilidad.

## Instalación rápida

### Requisito previo

```bash
pip install pre-commit
# o
brew install pre-commit
```

### Configuración básica (todos los lenguajes)

```bash
# En la raíz de tu proyecto
osdo init --with-precommit   # con el CLI OSDO v2

# O manualmente:
cp .pre-commit-config.yaml tu-proyecto/
pre-commit install
```

## Configuraciones por lenguaje

### PHP

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: phpstan        # Análisis estático nivel 8
      - id: php-cs-fixer   # PSR-12 code style
      - id: composer-validate  # Validación de composer.json
      - id: php-security-checker  # Dependencias con vulnerabilidades
```

### Node.js / TypeScript

```yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: eslint-security  # ESLint con plugin de seguridad
      - id: nodejsscan        # Análisis de seguridad Node.js
      - id: npm-audit         # Auditoría de dependencias npm
```

### Python

```yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: bandit    # OWASP Top 10 para Python
      - id: ruff      # Linting rápido
      - id: black     # Formateo de código
      - id: safety    # Vulnerabilidades en dependencias
```

### Go

```yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: gosec          # Análisis de seguridad Go
      - id: golangci-lint  # Suite de linters Go
      - id: govulncheck    # Vulnerabilidades en módulos Go
```

### Docker

```yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: hadolint  # Dockerfile best practices
      - id: trivy-dockerfile  # CVE scan del Dockerfile
```

### IaC (Terraform, Kubernetes)

```yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: checkov        # Terraform, K8s, Helm
      - id: tfsec          # Terraform security
      - id: kube-score     # Kubernetes manifests
```

## Hooks universales (todos los proyectos)

Estos hooks se recomiendan para cualquier proyecto:

```yaml
repos:
  # Detección de secretos — SIEMPRE incluir
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: gitleaks         # Secretos en el código
      - id: detect-secrets   # Detección de patrones de secretos

  # Standard pre-commit hooks
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: check-added-large-files
      - id: no-commit-to-branch
        args: ['--branch', 'main', '--branch', 'master']
```

## Configuración completa para proyectos Laravel (PHP + Node.js)

```yaml
repos:
  - repo: https://github.com/opensecdevops/pre-commit
    rev: v2.0.0
    hooks:
      - id: phpstan
      - id: php-cs-fixer
      - id: php-security-checker
      - id: eslint-security
      - id: npm-audit

  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: gitleaks
      - id: check-yaml
      - id: end-of-file-fixer
      - id: no-commit-to-branch
        args: ['--branch', 'main']
```

## Variables de entorno para hooks Docker

Los hooks Docker respetan estas variables:

| Variable | Descripción | Default |
|----------|-------------|---------|
| `OSDO_SKIP_DOCKER` | Saltar hooks que requieren Docker | `false` |
| `PHPSTAN_LEVEL` | Nivel de PHPStan (0-9) | `8` |
| `ESLINT_MAX_WARNINGS` | Máximo de warnings ESLint | `0` |

## Integración en el flujo OSDO

Los pre-commit hooks de OSDO son la primera línea de defensa:

```
Developer commits → pre-commit hooks (local) → PR → osdo-actions (CI/CD) → Deploy
      ↑                    ↑                         ↑
  Más rápido           seconds/mins              minutes
  Sin costo CI         Inmediato                Pipeline completo
```

## Troubleshooting

```bash
# Ver qué hooks están configurados
pre-commit run --all-files --verbose

# Saltarse un hook puntualmente (NO recomendado)
git commit --no-verify -m "mensaje"

# Actualizar a la última versión de los hooks
pre-commit autoupdate

# Limpiar caché de hooks
pre-commit clean
```
