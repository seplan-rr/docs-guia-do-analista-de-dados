# Sites Dinâmicos

> Esta página ainda está sendo construída. Se quiser contribuir com o guia, [acesse o nosso repositório](https://github.com/seplan-rr/guia-do-analista-de-dados).

## Ferramentas

**Preferidas**

- [Scrapy](https://www.scrapy.org/)
- [Playwright](https://playwright.dev/)
- [Crawlee](https://crawlee.dev/js/docs/deployment/apify-platform)

**Outras**

- [Selenium](https://www.selenium.dev/)

## Exemplo 1: Bot para Coleta Automática de Novos Diários Oficiais do Estado de Roraima (Falso Site Dinâmico)

Uma situação recorrente é acreditar que um site é dinâmico, mesmo que ele forneça outros meios de se encontrar o mesmo dado de forma estática. Este exemplo mostra uma tarefa que, à primeira vista, pode parecer um trabalho de extração de site dinâmico (um dos piores casos), mas que, na verdade, pode ser resolvido por consultas à uma API pública (um dos melhores casos).

**Dependências**

- [uv](https://docs.astral.sh/uv/getting-started/installation/)

**Passo a Passo**

**1. Inicie um novo ambiente Python**

```sh
mkdir bot-doe-rr
cd bot-doe-rr
uv init
```

**2. Instale o [Scrapy](https://www.scrapy.org/)**

```sh
uv add scrapy
uv lock
```

**3. Use seu editor preferido para escrever o código abaixo** 

Arquivo: main.py

```sh
import scrapy

LINK_DOE_RR_RAIZ = 

def main():
  


if __name__ == "__main__":
  main()
```

| a |
| - |
| b |
