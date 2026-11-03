---
name: brand-identity-extractor
description: >-
  Extrai elementos de identidade visual (paleta de cores, tipografia, logotipo, assets visuais e tom de voz) a partir da URL de um site fornecida pelo usuário. Gera um manual de marca estruturado (brandbook) e cria automaticamente a skill correspondente de forma global no ambiente Antigravity (~/.gemini/config/skills/<marca>/SKILL.md) nomeada apenas com o nome da marca (ex: apple, tesla), permitindo consistência estética permanente em todos os workspaces. Ative esta skill sempre que o usuário fornecer uma URL para extrair identidade visual, analisar paleta/estilo de uma marca ou criar diretrizes visuais.
---

# Brand Identity Extractor

Esta skill orienta o agente a analisar a URL de um site fornecido pelo usuário, extrair seus elementos centrais de identidade de marca (cores, tipografia, logotipos, componentes e tom de voz), apresentar um manual de diretrizes visuais estruturado e **criar automaticamente a nova skill de marca de forma global**, usando exclusivamente o nome da marca como identificador.

---

## Regras Fundamentais de Operação

1. **Criação Automática Global:** Logo após extrair as informações da marca, o agente **deve criar automaticamente** a respectiva skill no diretório global de configurações:
   - Caminho global: `C:\Users\Bernardo.silva\.gemini\config\skills\<nome-da-marca>\SKILL.md` (ou `~/.gemini/config/skills/<nome-da-marca>/SKILL.md`).
   - Caminho local no workspace (espelho): `skills/<nome-da-marca>/SKILL.md`.
2. **Nomenclatura Estrita:**
   - O identificador `name` no YAML frontmatter e o nome da pasta devem ser **apenas o nome da marca**, em minúsculas (ex: `apple`, `tesla`, `google`, `nubank`), **nunca** prefixado com `brand-`.

---

## Fluxo de Execução Passo a Passo

### 1. Coleta de Dados do Site
1. **Inspeção de Conteúdo e Estilos:**
   - Utilize a ferramenta `read_url_content` para inspecionar o HTML inicial, tags `<head>`, folhas de estilo CSS linkadas (`<link rel="stylesheet">`) e variáveis declaradas em `:root`.
   - Caso a requisição seja bloqueada por proteções (ex: 403, Cloudflare, Akamai) ou trate-se de uma SPA rica em JavaScript, utilize o `browser_subagent` para navegar e inspecionar os estilos computados.
2. **Extração de Cores:**
   - Procure por CSS Custom Properties (ex: `--primary`, `--color-primary`, `--tds-*`, `:root`).
   - Identifique e documente em HEX e RGB: Cor Primária/Brand, Secundária, Acento/Interativo, Fundos (Light e Dark), Textos (Principal e Secundário).
3. **Extração de Tipografia:**
   - Verifique declarações de fontes (`@font-face`, Google Fonts, Adobe Typekit, `font-family`).
   - Mapeie a hierarquia: Títulos (Headings), Corpo (Body/Text), Monospace (Código/Dados) e suas fontes de fallback (`sans-serif`, `system-ui`).
4. **Logotipos e Assets Visuais:**
   - Localize o logotipo (SVG inline, imagem no header/nav, OpenGraph logo).
   - Estilo de botões e cartões: Raio de borda (`border-radius`), sombras (`box-shadow`), efeitos translúcidos (`backdrop-filter`).
5. **Tom de Voz:**
   - Analise os slogans, chamadas de ação (CTAs) e proposta de valor para classificar o estilo de escrita da marca.

---

### 2. Estruturação do Manual da Marca (Brandbook Simplificado)

Organize os dados em formato Markdown padronizado:

```markdown
# Diretrizes de Marca: [Nome da Marca]

## 1. Visão Geral
- **Nome da Marca:** [Nome Oficial]
- **URL Analisada:** [URL]
- **Segmento / Proposta:** [Resumo curto]

## 2. Paleta de Cores
| Amostra | Papel | Código HEX | RGB | Aplicação & Uso |
| :---: | :--- | :--- | :--- | :--- |
| ![#HEX](https://via.placeholder.com/18/HEX/HEX.png) | **Primária** | `#XXXXXX` | `rgb(...)` | ... |

## 3. Tipografia
- **Títulos (Headings):** `[Fonte]`, fallback: `[sans-serif]` (Pesos: ...)
- **Corpo de Texto (Body):** `[Fonte]`, fallback: `[system-ui]` (Pesos: ...)
- **Monospace:** `[Fonte]`

## 4. Assets e Estilo Visual
- **Logotipo Principal:** [URL do Logo ou SVG]
- **Favicon:** [URL do Favicon]
- **Componentes:** Bordas, sombras, botões primários e secundários.

## 5. Tom de Voz e Copywriting
- **Estilo:** [ex: Minimalista, direto, aspiracional]
- **CTAs Padronizados:** [ex: "Comprar", "Saiba mais", "Order Now"]
```

---

### 3. Criação Automática da Skill Global

O agente **cria imediatamente** o arquivo da skill no caminho global:
`C:\Users\Bernardo.silva\.gemini\config\skills\<nome-da-marca>\SKILL.md` (e no workspace `skills/<nome-da-marca>/SKILL.md`).

#### Template da Skill Criada:

```markdown
---
name: [nome-da-marca]
description: >-
  Diretrizes de design, cores, tipografia, componentes e tom de voz da marca [Nome da Marca].
  Ative automaticamente sempre que for criar, estilizar ou refatorar interfaces, componentes visuais,
  páginas web, dashboards ou textos para a [Nome da Marca].
---

# Diretrizes Visuais: [Nome da Marca]

Esta skill define as regras estéticas e de comunicação visual da marca **[Nome da Marca]**.

## 1. Paleta de Cores
- **Primária:** `#...`
- **Secundária:** `#...`
- **Acento / Interativo:** `#...`
- **Background Claro:** `#...`
- **Background Escuro:** `#...`
- **Texto Principal:** `#...`
- **Texto Secundário:** `#...`

## 2. Tipografia
- **Headings:** `[Fontes]`
- **Body:** `[Fontes]`
- **Monospace:** `[Fontes]`

## 3. Componentes & Estilo
- **Botões:** `border-radius: ...`, cores primária/secundária.
- **Superfícies & Cards:** `border-radius: ...`, sombras e efeitos.

## 4. Tom de Voz
- **Estilo de escrita:** `[Diretrizes de comunicação]`
```

---

### 4. Apresentação ao Usuário

1. Exiba o Brandbook completo no chat.
2. Informe o caminho onde a nova skill foi criada globalmente (`C:\Users\Bernardo.silva\.gemini\config\skills\<nome-da-marca>\SKILL.md`) com link clicável.
3. Disponibilize-se para gerar componentes de UI ou telas completas utilizando a nova skill.
