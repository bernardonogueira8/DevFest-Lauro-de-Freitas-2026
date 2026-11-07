## **Estrutura do Projeto**

- `requirements/`
    - Guarda os requisitos de negócio e contexto do projeto.
    - O arquivo principal é `requirements/projeto.md`.
    - Ele explica que o projeto usa um dataset de vendas sintéticas, com foco em limpeza, EDA, análise de clientes, KPIs e painel de BI.
- `data/`
    - Guarda os dados de entrada.
    - Aqui tem:
        - `data/Sales_transactions_2022_2025.csv`: transações de vendas.
        - `data/archive.zip`: backup/arquivo comprimido (NÃO PRECISA SER LIDO)
- `docs/`
    - Guarda documentação técnica/instrucional.
    - NÃO PRECISA SER LIDO.
- `skills/`
    - É o coração do “automação de governança BI”.
    - Arquivos:
        - `skills/README.md`
        - `skills/bi-naming-convention-optimizer/SKILL.md`
        - `skills/bi-model-documenter/SKILL.md`
        - `skills/bi-relationship-advisor/SKILL.md`
    - Em tese, essas skills ajudam a IA a:
        - padronizar nomenclatura
        - documentar medidas/tabelas
        - auditar relacionamento do modelo
        - melhorar o design e a governança do Power BI
- `assets/`
    - Guarda elementos visuais e materiais complementares.
    - NÃO PRECISA SER LIDO
- `test/`
    - Essa pasta, segundo as regras do `AGENTS.md`, deveria receber “visualizações previas em HTML para cada tela”.
    - É onde ficariam mockups/HTML de telas do dashboard, protótipos, demonstrações visuais e testes de interface.
- `readme.md`
    - É o documento central de apresentação do projeto.
    - Ele descreve o evento DevFest, a proposta de trabalho e o dataset escolhido.
- `AGENTS.md`
    - É um arquivo de regras de operação para o agente/IA.

## **Regra de Contexto**

Realize a leitura da pasta `requirements`, para compreensão do projeto, regras de negocio, para atuar como arquiteto de software. Sempre forneça um planejamento de execução, crie uma visualização previsa em html para cada tela e coloque na pasta `test`, utilize sempre HTML5 cru.

## **Regra de Registro de Missões (History Tracking)**

Sempre que concluir uma missão ou tarefa solicitada, você deve documentar a execução obrigatoriamente seguindo este padrão:

1. **Local de Salvamento:** Acesse ou crie a raiz do diretório `history/`.
2. **Subpasta:** Crie uma subpasta com a data da execução no formato `YYYY-MM-DD` (ex: `history/2026-10-22/`).
3. **Nome do Arquivo:** Crie um arquivo `.md` nomeado com a função principal executada em kebab-case (ex: `criar-processo-agente.md`).
4. **Título do Arquivo:** Use o padrão _Conventional Commits_ no título principal (H1). Exemplo: `# feat: criação da regra de histórico para agents`.
5. **Estrutura do Conteúdo:** O arquivo deve ser preenchido exatamente com a seguinte estrutura estruturada:

- **Pergunta/Solicitação:** [A pergunta exata enviada pelo usuário]
- **Planejamento:** [O raciocínio e o passo a passo que você estabeleceu antes de iniciar a execução]
- **Resumo da Execução:** [A resposta final consolidada e o que foi efetivamente construído ou alterado]

Siga este bloco de forma estrita em cada final de interação.
