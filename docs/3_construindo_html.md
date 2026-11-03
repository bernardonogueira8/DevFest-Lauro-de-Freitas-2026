# Construindo uma página de documentação interativa no PowerBI

- **Confiabilidade:** **set um reflexo 100% fiel e automatizado do modelo**. A McKinsey destaca que processos manuais são inerentemente propensos a errors, tornando a automação o único caminho para que os dados, como um produto, estejam sempre prontos para uso;
- **Acessibilidade:** **funcionar como uma ferramenta de autoatendimento**. A informação precisa set compreensível para todos os públicos, alinhando-se à visão da McKinsey de uma ampla alfabetização de dados organizacional como pilar para a empresa do futuro;
- **Integração: set uma solução nativa, sem dependências externas.** Em contraste direto com os ambientes dispersos e isolados criticados pela McKinsey, uma ferramenta nativa elimina o atrito e reduz o tempo e o custo para entregar novas capacidades analíticas.

## 1 Criando INFO.TABLES, INFO.COLUMNS e INFO.MEASURES

### 1.1 Versão Manual de Referencia

```text
Catalogo_De_Dados =
VAR _Tabelas = SELECTCOLUMNS(FILTER(INFO.VIEW.TABLES(),
                                    NOT ([IsHidden])
                                    &&  [Name] <> "Catalogo_De_Dados")
                    , "LOCALIZACAO", [Name]
                    , "ELEMENTO", "Tabela"
                    , "NOME", [Name]
                    , "TIPO", "Table"
                    , "DESCRICAO", [Description]
                    , "EXPRESSAO", BLANK()
                )

VAR _Colunas = SELECTCOLUMNS(FILTER(INFO.VIEW.COLUMNS(),
                                    NOT ([IsHidden])
                                    &&  [Table] <> "Catalogo_De_Dados")
                    , "LOCALIZACAO", [Table]
                    , "ELEMENTO", "Coluna"
                    , "NOME", [Name]
                    , "TIPO", [DataType]
                    , "DESCRICAO", [Description]
                    , "EXPRESSAO", [Expression]
                )

VAR _Medidas = SELECTCOLUMNS(FILTER(INFO.VIEW.MEASURES(),
                                    LEFT([Name], 1) <> "_"
                                    &&  NOT ([IsHidden])
                                    &&  [Table] <> "Catalogo_De_Dados")
                    , "LOCALIZACAO", [DisplayFolder]
                    , "ELEMENTO", "Medida"
                    , "NOME", [Name]
                    , "TIPO", BLANK()
                    , "DESCRICAO", [Description]
                    , "EXPRESSAO", [Expression]
                )
VAR _TipodeTabela =
IF(
    Catalogo_De_Dados[ELEMENTO] = "Tabela",
    SWITCH(
        TRUE(),
        LEFT(Catalogo_De_Dados[NOME], 2) = "f_", "Tabela Fato",
        LEFT(Catalogo_De_Dados[NOME], 2) = "d_", "Tabela Dimensão",
        "Outras Tabelas"
    ),
    BLANK()
)
RETURN
UNION( _Tabelas, _Colunas, _Medidas, _TipodeTabela )
```

### 1.2 Versão Com MCP

```text
Através do MCP, você deverá tabular contendo a estrutura completa do modelo do Power BI. Este catálogo unifica Tabelas, Colunas e Medidas. Objetivo é alimentar os cards `Grade de Medidas de Documentação` e `Grade de Tabelas de Documentação`.
As colunas do catálogo que você deve analisar são:
- [LOCALIZACAO]: A qual tabela o item pertence.
- [ELEMENTO]: Se é 'Tabela', 'Coluna' ou 'Medida'.
- [NOME]: O nome do item.
- [TIPO] / [TIPO_TABELA]: O tipo de dado ou a classificação (ex: 'Tabela Fato', 'Tabela Dimensão', 'Outras Tabelas').
- [DESCRICAO]: A documentação atual do item (pode estar em branco).
- [EXPRESSAO]: O código DAX (aplicável para Medidas ou Colunas Calculadas).
- [PASTA_EXIBICAO]: Onde o item está organizado dentro da pasta de Medidas.

# [TAREFAS]
Analise os metadados fornecidos e execute estritamente as seguintes ações:
**Auditoria de Padrões e Nomenclatura (Kimball):**
   - Verifique se as tabelas seguem a convenção `F_` para Fatos e `D_` para Dimensões. Liste as que caem como "Outras Tabelas" e sugira o prefixo correto com base no contexto do nome.
   - Identifique colunas ou medidas com nomes pouco amigáveis para o usuário final (ex: camelCase, underscores excessivos, siglas não padronizadas).
**Análise de Governança e Documentação:**
   - Liste todos os elementos essenciais (todas as Fatos, Dimensões e Medidas principais) que estão com o campo [DESCRICAO] vazio (`BLANK`).
   - Para as 5 principais medidas sem descrição, gere uma sugestão de descrição de negócios baseada na leitura do código DAX em [EXPRESSAO].
**Revisão de Código DAX (Medidas):**
   - Analise o campo [EXPRESSAO] das medidas fornecidas.
   - Identifique más práticas (ex: uso excessivo de `FILTER(Tabela)` ao invés de `KEEPFILTERS`, falta de `DIVIDE` para tratamento de erros, variáveis não utilizadas).
   - Reescreva as medidas problemáticas aplicando as melhores práticas de performance (engine VertiPaq).
**Dicionário de Dados Resumido:**
   - Gere uma tabela Markdown sumarizando as métricas chave do modelo (Apenas ELEMENTO = "Medida"), organizadas por [PASTA_EXIBICAO].
# Referencia de Tabela DAX
Catalogo_De_Dados =
VAR _Tabelas = SELECTCOLUMNS(FILTER(INFO.VIEW.TABLES(),
                                    NOT ([IsHidden])
                                    &&  [Name] <> "Catalogo_De_Dados")
                    , "LOCALIZACAO", [Name]
                    , "ELEMENTO", "Tabela"
                    , "NOME", [Name]
                    , "TIPO", "Table"
                    , "DESCRICAO", [Description]
                    , "EXPRESSAO", BLANK()
                )

VAR _Colunas = SELECTCOLUMNS(FILTER(INFO.VIEW.COLUMNS(),
                                    NOT ([IsHidden])
                                    &&  [Table] <> "Catalogo_De_Dados")
                    , "LOCALIZACAO", [Table]
                    , "ELEMENTO", "Coluna"
                    , "NOME", [Name]
                    , "TIPO", [DataType]
                    , "DESCRICAO", [Description]
                    , "EXPRESSAO", [Expression]
                )

VAR _Medidas = SELECTCOLUMNS(FILTER(INFO.VIEW.MEASURES(),
                                    LEFT([Name], 1) <> "_"
                                    &&  NOT ([IsHidden])
                                    &&  [Table] <> "Catalogo_De_Dados")
                    , "LOCALIZACAO", [DisplayFolder]
                    , "ELEMENTO", "Medida"
                    , "NOME", [Name]
                    , "TIPO", BLANK()
                    , "DESCRICAO", [Description]
                    , "EXPRESSAO", [Expression]
                )
VAR _TipodeTabela =
IF(
    Catalogo_De_Dados[ELEMENTO] = "Tabela",
    SWITCH(
        TRUE(),
        LEFT(Catalogo_De_Dados[NOME], 2) = "f_", "Tabela Fato",
        LEFT(Catalogo_De_Dados[NOME], 2) = "d_", "Tabela Dimensão",
        "Outras Tabelas"
    ),
    BLANK()
)
RETURN
UNION( _Tabelas, _Colunas, _Medidas, _TipodeTabela )
```

## 2 Grade de Tabelas de Documentação

```DAX
Grade de Tabelas de Documentação =
VAR _NomeTabela = SELECTEDVALUE(Catalogo_De_Dados[LOCALIZACAO], "Selecione uma tabela")
VAR _DescricaoTabela =
    CALCULATE(
        SELECTEDVALUE(Catalogo_De_Dados[DESCRICAO], "Nenhuma descrição documentada."),
        Catalogo_De_Dados[ELEMENTO] = "Tabela"
    )
VAR _DescricaoFinal =
    IF(
        _NomeTabela = "Selecione uma tabela",
        "Aguardando seleção no menu...",
        _DescricaoTabela
    )
VAR _HtmlCabecalho =
    "<div style='font-family: Segoe UI, sans-serif; margin-bottom: 20px; padding-left:12px; padding-bottom: 16px; border-bottom: 1px solid #f3f4f6;'>
        <h2 style='margin: 0; font-size: 34px; color: #1f2937;'>Tabela: " & _NomeTabela & "</h2>
        <p style='margin: 6px 0 0 0; font-size: 21px; color: #6b7280;'>Descrição: " & _DescricaoFinal & "</p>
    </div>"

VAR _CSS = "
<style>
  .tabela-doc { width: 100%; border-collapse: collapse; font-family: 'Segoe UI', sans-serif; font-size: 18px; }
  .tabela-doc th { text-align: left; padding: 14px 10px; border-bottom: 2px solid #f3f4f6; color: #374151; font-weight: 600; }
  .tabela-doc td { padding: 12px 10px; border-bottom: 1px solid #f9fafb; color: #4b5563; vertical-align: middle; }

  .badge { padding: 6px 14px; border-radius: 14px; font-weight: 600; font-size: 18px; display: inline-block; text-align: center; min-width: 70px; }

  /* CORES DOS BADGES */
  .badge-inteiro { background-color: #f4cda5; color: #a65c2e; }
  .badge-decimal { background-color: #d1fae5; color: #065f46; }
  .badge-texto   { background-color: #dbeafe; color: #1e40af; }
  .badge-data    { background-color: #fef08a; color: #854d0e; }
  .badge-booleano{ background-color: #fbcfe8; color: #9d174d; }
  .badge-medida  { background-color: #e9d5ff; color: #6b21a8; } /* Roxo para Medidas */
  .badge-default { background-color: #e5e7eb; color: #374151; }

  .icon-circle { display: inline-flex; align-items: center; justify-content: center; width: 26px; height: 26px; border-radius: 50%; color: white; font-size: 14px; font-weight: bold; }
  .icon-cross { background-color: #d1d5db; }
  .icon-check { background-color: #10b981; }
</style>
"

VAR _Header = "
<table class='tabela-doc'>
  <thead>
    <tr>
      <th>Coluna &#9652;</th>
      <th>Descrição</th>
      <th style='text-align: center;'>Tipo</th>
      <th style='text-align: center;'>Permite Nulos</th>
      <th style='text-align: center;'>Chave Relacional</th>
    </tr>
  </thead>
  <tbody>
"

// Aqui iteramos mostrando tanto Colunas quanto Medidas se quiser integrá-las
VAR _Rows = CONCATENATEX(
    FILTER(Catalogo_De_Dados, Catalogo_De_Dados[ELEMENTO] IN {"Coluna", "Medida"}),

    VAR _NomeColuna = Catalogo_De_Dados[NOME]
    VAR _Descricao = COALESCE(Catalogo_De_Dados[DESCRICAO], "-")
    VAR _TipoDado = UPPER(Catalogo_De_Dados[TIPO])

    // Novas Variáveis lendo as colunas físicas recém-criadas no modelo
    VAR _EhChaveBD = Catalogo_De_Dados[CHAVE_RELACIONAL]
    VAR _PermiteNulosBD = Catalogo_De_Dados[PERMITE_NULOS]

    // Mapeamento das Cores
    VAR _ClasseBadge = SWITCH(
        TRUE(),
        _TipoDado = "INTEGER", "badge-inteiro",
        _TipoDado IN {"CURRENCY", "NUMBER"}, "badge-decimal",
        _TipoDado = "TEXT", "badge-texto",
        _TipoDado = "DATE", "badge-data",
        _TipoDado = "MEASURE", "badge-medida",
        "badge-default"
    )

    VAR _NomeTipo = SWITCH(
        TRUE(),
        _TipoDado = "INTEGER", "Inteiro",
        _TipoDado = "NUMBER", "Decimal",
        _TipoDado = "CURRENCY", "Moeda",
        _TipoDado = "TEXT", "Texto",
        _TipoDado = "DATE", "Data",
        _TipoDado = "MEASURE", "Medida",
        Catalogo_De_Dados[TIPO]
    )

    // ==========================================
    // NOVA LÓGICA DE ÍCONES (Direto das Colunas Físicas)
    // ==========================================
    VAR _EhChaveHtml = IF(_EhChaveBD = TRUE(),
        "<span class='icon-circle icon-check'>&#10003;</span>", // É chave (Visto Verde)
        "<span class='icon-circle icon-cross'>&#10005;</span>"   // Não é (X Cinza)
    )

    VAR _PermiteNuloHtml = IF(_PermiteNulosBD = TRUE(),
        "<span class='icon-circle icon-check'>&#10003;</span>",  // Permite nulo (Visto Verde)
        "<span class='icon-circle icon-cross'>&#10005;</span>" // Não permite (X Cinza)
    )
    // ==========================================

    RETURN
    "<tr>" &
      "<td><strong>" & _NomeColuna & "</strong></td>" &
      "<td>" & _Descricao & "</td>" &
      "<td style='text-align: center;'><span class='badge " & _ClasseBadge & "'>" & _NomeTipo & "</span></td>" &
      "<td style='text-align: center;'>" & _PermiteNuloHtml & "</td>" &
      "<td style='text-align: center;'>" & _EhChaveHtml & "</td>" &
    "</tr>",

    "", // Delimitador
    Catalogo_De_Dados[NOME], ASC
)

VAR _Footer = "</tbody></table>"

// Aumentei a fonte da mensagem de fallback de 15px para 18px
VAR _HtmlTabela = IF(
    ISBLANK(_Rows),
    "<p style='font-family: Segoe UI, sans-serif; color: #6b7280; margin-top: 24px; font-size: 18px;'>Selecione uma tabela no menu lateral para visualizar as colunas e atributos.</p>",
    _Header & _Rows & _Footer
)

// ==========================================
// 3. RETORNO FINAL CONCATENADO
// ==========================================
RETURN
_CSS & _HtmlCabecalho & _HtmlTabela
```

## 3 Card para Medidas

```dax
Grade de Medidas de Documentação = VAR _Medidas =
    FILTER(
        ALLSELECTED(Catalogo_De_Dados),
        Catalogo_De_Dados[ELEMENTO] = "Medida"
            && NOT ISBLANK(Catalogo_De_Dados[NOME])
    )
VAR _CSS =
    "<style>" &
    ".medidas-doc{width:100%;table-layout:fixed;border-collapse:collapse;font-family:'Segoe UI',sans-serif;font-size:16px}" &
    ".medidas-doc col.nome{width:20%}" &
    ".medidas-doc col.descricao{width:30%}" &
    ".medidas-doc col.formula{width:50%}" &
    ".medidas-doc th{text-align:left;padding:14px 10px;border-bottom:2px solid #f3f4f6;color:#374151;font-weight:600;overflow-wrap:anywhere}" &
    ".medidas-doc td{padding:12px 10px;border-bottom:1px solid #f9fafb;color:#4b5563;vertical-align:top;overflow-wrap:anywhere;word-break:break-word}" &
    ".medidas-doc code{display:block;white-space:pre-wrap;word-break:break-word;font-family:Consolas,'Courier New',monospace;font-size:13px;line-height:1.35;color:#374151;background:#f8fafc;border:1px solid #e5e7eb;border-radius:6px;padding:10px;max-height:260px;overflow:hidden}" &
    ".medida-titulo{font-family:'Segoe UI',sans-serif;margin:0 0 20px 0;padding:0 0 16px 12px;border-bottom:1px solid #f3f4f6;color:#1f2937;font-size:30px}" &
    ".medida-vazia{font-family:'Segoe UI',sans-serif;color:#6b7280;margin-top:24px;font-size:18px}" &
    "</style>"
VAR _Cabecalho =
    "<h2 class='medida-titulo'>Medidas de Documentação</h2>"
VAR _Header =
    "<table class='medidas-doc'><colgroup><col class='nome'><col class='descricao'><col class='formula'></colgroup><thead><tr>" &
        "<th>Nome da medida</th><th>Descrição</th><th>Fórmula DAX</th>" &
    "</tr></thead><tbody>"
VAR _Rows =
    CONCATENATEX(
        _Medidas,
        VAR _Nome = Catalogo_De_Dados[NOME]
        VAR _Descricao = COALESCE(Catalogo_De_Dados[DESCRICAO], "Nenhuma descrição documentada.")
        VAR _Formula = COALESCE(Catalogo_De_Dados[EXPRESSAO], "Fórmula não documentada.")
        RETURN
            "<tr>" &
                "<td><strong>" & _Nome & "</strong></td>" &
                "<td>" & _Descricao & "</td>" &
                "<td><code>" & _Formula & "</code></td>" &
            "</tr>",
        "",
        Catalogo_De_Dados[NOME], ASC
    )
VAR _Conteudo =
    IF(
        ISBLANK(_Rows),
        "<p class='medida-vazia'>Não há medidas visíveis documentadas no catálogo.</p>",
        _Header & _Rows & "</tbody></table>"
    )
RETURN
    _CSS & _Cabecalho & _Conteudo
```

## 4 Referencias

[Governança de Dados como Produto no Power BI \| by André Vaz \| Medium](https://medium.com/@andrevaz/governan%C3%A7a-de-dados-como-produto-no-power-bi-954d686354e2)
