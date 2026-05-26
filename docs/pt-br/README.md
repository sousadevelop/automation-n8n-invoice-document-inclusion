# Automação n8n para inclusão de documentos de fatura

## Visão geral

Este repositório documenta um workflow n8n exportado em `workflow/workflow.sanitized.json`. A automação processa anexos recebidos por Gmail, analisa documentos de fatura com Google Gemini, faz upload dos arquivos no Google Drive e registra dados estruturados em uma planilha Google Sheets.

O arquivo do workflow foi mantido intacto nesta organização. A documentação descreve a automação existente sem alterar lógica, nós, conexões, credenciais ou placeholders.

## Objetivo do workflow

Automatizar a triagem e o registro de faturas recebidas por e-mail, reduzindo trabalho manual em quatro etapas:

1. Detectar e-mails recebidos com anexos.
2. Extrair informações estruturadas do documento anexado.
3. Salvar o arquivo no Google Drive.
4. Registrar metadados e dados extraídos no Google Sheets.

## Problema resolvido

Processos manuais de inclusão de faturas costumam exigir abrir e-mails, baixar anexos, interpretar documentos, renomear arquivos, salvar em pastas e preencher planilhas. O workflow centraliza esse fluxo no n8n e padroniza a saída em colunas de planilha.

## Arquitetura da automação

Fluxo lógico:

1. `Gmail Trigger` monitora a caixa Gmail e baixa anexos.
2. `If` verifica se existe `attachment_0`.
3. `Split Out` separa os binários de anexos.
4. `Loop Over Items` processa anexos em lote.
5. `Analyze document` envia o anexo ao Google Gemini para extração em JSON.
6. `Edit Fields` normaliza o objeto retornado.
7. `If1` avalia se o documento é `corporativo`.
8. `Merge` e `Merge1` combinam dados estruturados com binários.
9. `Upload file` e `Upload file1` enviam o arquivo ao Google Drive.
10. `Append row in sheet` grava os dados no Google Sheets.

Diagrama Mermaid: [assets/diagrams/workflow-architecture.mmd](../../assets/diagrams/workflow-architecture.mmd).

## Nós n8n utilizados

| Nome no workflow | Tipo |
| --- | --- |
| `Gmail Trigger` | `n8n-nodes-base.gmailTrigger` |
| `If` | `n8n-nodes-base.if` |
| `Split Out` | `n8n-nodes-base.splitOut` |
| `Loop Over Items` | `n8n-nodes-base.splitInBatches` |
| `Analyze document` | `@n8n/n8n-nodes-langchain.googleGemini` |
| `No Operation, do nothing` | `n8n-nodes-base.noOp` |
| `Edit Fields` | `n8n-nodes-base.set` |
| `If1` | `n8n-nodes-base.if` |
| `Merge` | `n8n-nodes-base.merge` |
| `Upload file` | `n8n-nodes-base.googleDrive` |
| `Merge1` | `n8n-nodes-base.merge` |
| `Upload file1` | `n8n-nodes-base.googleDrive` |
| `Append row in sheet` | `n8n-nodes-base.googleSheets` |

## Integrações necessárias

- n8n com suporte aos nós oficiais usados no workflow.
- Gmail OAuth2 para leitura de e-mails e download de anexos.
- Google Gemini ou Google PaLM API para análise de documento.
- Google Drive OAuth2 para upload de arquivos.
- Google Sheets OAuth2 para inserir linhas em planilha.

## Variáveis e configurações esperadas

Use `templates/env.example` como referência de nomes e placeholders. Não adicione valores reais ao repositório.

Configurações esperadas:

- Conta Gmail autorizada no n8n.
- Credencial de Gemini/PaLM configurada no n8n.
- Credencial Google Drive configurada no n8n.
- Credencial Google Sheets configurada no n8n.
- ID ou URL da planilha Google Sheets definida no nó `Append row in sheet`.
- Nome ou aba da planilha com as colunas esperadas.
- Pasta de destino no Google Drive para documentos corporativos e pessoais, conforme a configuração final do workflow.

## Colunas registradas no Google Sheets

O nó `Append row in sheet` mapeia os campos abaixo:

- `Data da Fatura`
- `Fornecedor`
- `Valor`
- `Moeda`
- `Impostos`
- `Número da Fatura`
- `Categoria`
- `Corporativo ou Pessoal`
- `Método de Pagamento`
- `URL do Arquivo`
- `E-mail de Origem`

## Passo a passo de importação no n8n

1. Abra o n8n.
2. Acesse a área de workflows.
3. Escolha a opção de importar workflow.
4. Selecione o arquivo `workflow/workflow.sanitized.json`.
5. Revise os nós importados antes de ativar.
6. Reassocie as credenciais locais em cada nó que exigir autenticação.
7. Atualize referências de planilha, aba e pastas do Drive com valores do seu ambiente.
8. Execute testes com anexos fictícios antes de usar em e-mails reais.
9. Ative o workflow somente após validar permissões, dados de saída e logs.

## Passo a passo de configuração das credenciais

1. Crie uma credencial Gmail OAuth2 no n8n com permissões adequadas para ler mensagens e anexos.
2. Crie uma credencial Google Gemini/PaLM API para o nó `Analyze document`.
3. Crie uma credencial Google Drive OAuth2 para os nós de upload.
4. Crie uma credencial Google Sheets OAuth2 para o nó de append.
5. Substitua apenas no ambiente n8n os placeholders de credenciais do workflow importado.
6. Não edite o JSON sanitizado para inserir secrets.
7. Teste cada credencial isoladamente no n8n antes de ativar a automação.

## Exemplo de entrada

Consulte [examples/sample-input.md](../../examples/sample-input.md).

## Exemplo de saída

Consulte [examples/sample-output.md](../../examples/sample-output.md).

## Limitações

- O workflow depende da qualidade do documento anexado e da resposta do modelo de IA.
- O prompt exige JSON puro; respostas inválidas podem quebrar etapas posteriores.
- O fluxo atual verifica explicitamente `attachment_0` antes de separar anexos.
- A classificação `corporativo` direciona ramificações do fluxo; categorias diferentes precisam estar coerentes com a configuração dos nós.
- URLs e IDs de planilhas/pastas devem ser revisados após importação.
- O workflow sanitizado contém referências que parecem específicas de ambiente; consulte `SECURITY_NOTES.md`.

## Próximos passos

- Remover ou substituir referências específicas de ambiente no workflow sanitizado, mantendo placeholders seguros.
- Adicionar screenshots atualizados da importação e configuração.
- Validar o workflow com documentos fictícios representativos.
- Documentar critérios de aceite para JSON retornado pelo Gemini.
- Adicionar variações de exemplo para fatura corporativa e pessoal.
- Avaliar tratamento de erros para falhas de IA, Drive ou Sheets.

## Segurança

- Nunca versionar credenciais reais.
- Não inserir tokens, IDs privados, webhooks ou secrets em documentação.
- Não remover placeholders de segurança.
- Não compartilhar anexos reais, e-mails reais ou dados financeiros sensíveis.
- Revisar `SECURITY.md` e `SECURITY_NOTES.md` antes de usar em produção.
