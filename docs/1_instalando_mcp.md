## 1 Introdução

> **Nota:** O repositório official sugere o uso do GitHub Copilot para uma configuração automatizada e mais fácil. No entanto, este artigo focará exclusivamente no **método de instalação manual**. Para especificações mais técnicas, a leitura da documentação official no repositório é altamente recomendada.

O primeiro desafio foi obter a versão mais recente da extensão. A Microsoft não oferece um botão simples de download para versões específicas do MCP no marketplace fora do VS Code, então precisei forjar meu próprio caminho. A lógica é simples: você monta uma URL específica combinando a versão desejada e a arquitetura do seu sistema operacional. Para encontrar a versão mais recente do VSIX, precisa acessar [Marketplace do Visual Studio](https://marketplace.visualstudio.com/items?itemName=analysis-services.powerbi-modeling-mcp).
![[04 Resources/attachments/54d5c714a739a66a229d8f71a2c923ad_MD5.jpg]]
No memento em que este artigo foi escrito, a versão mais recente é a **0.5.15**. O download manual é feito através de um link parametrizado, estruturado da seguinte forma:
`https://marketplace.visualstudio.com/_apis/public/gallery/publishers/analysis-services/vsextensions/powerbi-modeling-mcp/[version]/vspackage?targetPlatform=[platform]`
**Plataformas suportadas:** `win32-x64`, `win-arm64`, `linux-arm64`, `linux-x64` e `osx-arm64`.
**Exemplo prático de download (v0.5.15 no Windows 64-bits):** [Baixar versão 0.5.15 (win32-x64)](https://marketplace.visualstudio.com/_apis/public/gallery/publishers/analysis-services/vsextensions/powerbi-modeling-mcp/0.5.15/vspackage?targetPlatform=win32-x64&utm_source=gemini)

## 2 Instalar a extensão no Antigravity IDE e configurando o servidor MCP
![Como instalar a extensão no Antigravity IDE e configurando o servidor MCP - YouTube](https://youtu.be/iGXkIaKhO7Y)
Com o pacote em mãos, o próximo passo era introduzi-lo ao Antigravity IDE. O processo foi rápido e silencioso.
1. Abra o Antigravity IDE e acesse a aba **Extensions**.
2. Clique no menu **Views and More Actions…** (geralmente representado por três pontos).
3. Selecione a opção **Install from VSIX…**.
4. Localize e selecione o arquivo que você acabou de baixar.
5. Após a instalação, precisamos apontar a configuração do agente para o executável do Power BI Modeling MCP.
6. Abra o chat do **Agent**.
7. Vá em **Additional Options**.
8. Entre em **MCP Services** e acesse **Manage MCP Servers**.
9. Clique na opção **View Raw Config**.
10. Cole o código JSON abaixo:

	```json
	{
	  "mcpServers": {
	    "powerbi-authoring-local": {
	      "command": "C:\\Users\\[SEU_USUARIO]\\.antigravity-ide\\extensions\\analysis-services.powerbi-modeling-mcp-0.5.15\\server\\powerbi-modeling-mcp.exe",
	      "args": [
	        "--start",
	        "--skip-confirmation"
	      ]
	    }
	  }
	}
	```

	**Dica de atenção:** Lembre-se de substituir `[SEU_USUARIO]` pelo nome do seu usuário do computador (como `Bernardo.silva`). O caminho abaixo já foi atualizado para refletir a pasta da versão `0.5.15`.
11. Salve e feche o arquivo `mcp_config.json`.
12. Retorne à tela **Manage MCPs** e clique em **Refresh**.

Então em poucos segundos, a IDE ficou equipada com o vocabulário e as ferramentas necessárias para entender a estrutura de um projeto do Power BI.

## 3 Testando e manipulando o Power BI via Chat
O verdadeiro memento da verdade acontece no chat. Com uma pasta de trabalho aberta, testei os limites do meu novo co-piloto. A beleza do MCP é a flexibilidade; você pode conversar com diferentes tipos de origens de dados usando apenas linguagem natural.
Sendo assim, abra o chat e encaminhe o commando:
**Para o Power BI Desktop:**

```
Conecte ao arquivo xxxxx.pbix no power bi desktop
```

**Para o modelo semântico no Workspace do Fabric:**

```
Conecte-se ao modelo semântico "[Nome do Modelo Semântico]" no workspace do Fabric "[Nome do Workspace]"
```

**Para arquivos de projeto do Power BI (PBIP):**

```
Abra o modelo semântico a partir da pasta PBIP "[Caminho para a pasta de definição/TMDL no PBIP]"
```

Assim que o agente confirmou a conexão, o paradigma mudou. Pedi para criar medidas DAX, alterar nomes de colunas e documentar tabelas inteiras, e vi o modelo set transformado remotamente pelas "mãos" da IA. A era do copia-e-cola ficou para trás; a modelagem semântica agora é uma conversa.