# 📊 Power BI & Analytics - MCP Skills Pack

Este repositório contém um pacote de skills (habilidades) para assistentes de IA, focado em acelerar o desenvolvimento, a governança e o design de projetos de Business Intelligence. 

As skills foram desenvolvidas com integração nativa ao **Model Context Protocol (MCP) para Power BI**, permitindo que a IA leia o modelo de dados em tempo real, audite relacionamentos, gere documentações e sugira scripts (C#/DAX/SQL/Python) de forma autônoma.

## 📦 Skills Inclusas

O pacote é composto por 8 skills especializadas:

### 🛠️ Engenharia e Governança de BI
*   **`bi-naming-convention-optimizer`**: Analisa o modelo e gera scripts em C# (Tabular Editor) para renomear em massa tabelas (D_ e F_) e colunas fora do padrão.
*   **`bi-model-documenter`**: Lê as medidas DAX do modelo e gera descrições de negócio e pastas de exibição (Display Folders) automaticamente.
*   **`bi-relationship-advisor`**: Audita o Star Schema em busca de ambiguidades ou relacionamentos ineficientes (N:N, filtros Both) e sugere melhorias para aprovação.

### 🎨 UX/UI e Data Storytelling
*   **`bi-dynamic-html-titles`**: Rastreia filtros visíveis (segmentadores) nas páginas e gera medidas DAX avançadas para renderizar títulos HTML dinâmicos.
*   **`bi-html-wireframe-creator`**: Lê as métricas principais do modelo e gera um wireframe (layout) em HTML/CSS para validar ideias de design rápido do dashboard.

## 🚀 Como Instalar

Certifique-se de ter o `npx` instalado em sua máquina. Para baixar e instalar todas as skills deste pack, rode o comando abaixo no seu terminal:

```bash
npx skills add https://skills.sh/p/l0BugDdwMbjMA2j1
```
