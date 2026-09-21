# Análise de Repositórios e Pipelines (CI/CD)
**O que foi analisado:** Arquivos YAML da pasta `.github/workflows`, os gatilhos (triggers) que disparam as ações e o histórico de commits focado em CI/CD.

## 1. Repositório: actions/starter-workflows
Basicamente, é o repositório oficial do GitHub com os templates prontos de Actions. Eles organizam tudo por categorias (CI, deploy, segurança, etc.) pra facilitar a vida de quem está começando.

**Como funciona o workflow de Node.js:**
*   Ele baixa o código e configura o Node (já ativando o cache do `npm` pra rodar mais rápido).
*   Usa uma matriz de testes para rodar a aplicação em três versões do Node (18, 20 e 22) ao mesmo tempo.
*   Roda os clássicos: `npm ci`, `build` e `test`.
*   **Quando roda:** Sempre que rola um `push` ou um `pull_request` (PR) na branch `main`.

**O que o histórico mostra:** O foco deles é manter as ferramentas atualizadas. Em 2024, eles subiram todas as actions principais para a versão `v4` pra evitar que os desenvolvedores usem templates defasados.

## 2. Repositório: microsoft/vscode
Como o VS Code é um projeto gigante, a abordagem deles é fugir de um "arquivão" único e usar vários workflows separados. 

**Como funciona a pipeline deles:**
*   Eles usam *jobs reutilizáveis* pra rodar testes no Linux, macOS e Windows em paralelo.
*   A pipeline checa se o build passa, instala as dependências (Node, Python, .NET) e roda testes de integração e de interface. Se der pau, ela já salva os logs/artefatos pra ajudar a debugar.
*   **Quando roda:** Em PRs para a `main` e nas branches de release. Um detalhe legal é que eles usam a regra de `concurrency`: se você fizer um novo push num PR que já estava rodando testes, ele cancela a execução antiga pra não gastar processamento à toa.

**O que o histórico mostra:** Eles melhoraram a segurança recentemente fixando as Actions pelo hash do commit (SHA) e organizaram melhor os relatórios de testes. É um ótimo exemplo de pipeline que soube crescer junto com o produto.

## 3. Repositório: nodejs/node
O Node.js leva a separação de responsabilidades para outro nível. Eles têm um monte de workflows, cada um fazendo uma coisa super específica: teste, lint, segurança (CodeQL) e release.

**Como funciona a pipeline deles:**
*   Além de compilar e testar nos três sistemas operacionais principais, eles têm automações voltadas para a comunidade: bots que colocam labels nos PRs, avisam se alguém está inativo e organizam a fila de commits.
*   **Quando roda:** Tem de tudo. Testes rolam em push e PR, coisas pesadas rodam por agendamento (CRON), e existem rotinas manuais (acionadas por `workflow_dispatch`). Até um comentário em uma issue pode disparar uma automação.

**O que o histórico mostra:** É uma pipeline focada em manter a ordem num projeto open source gigante. Eles estão sempre evoluindo as verificações de segurança e melhorando como a comunidade interage com o código.

---

## Conclusão: O que podemos aproveitar?

| Repositório | Estratégia Principal | Maior Vantagem |
| :--- | :--- | :--- |
| **actions/starter** | Templates prontos e genéricos | Muito rápido de configurar num projeto novo |
| **VS Code** | Jobs especializados e reaproveitáveis | Feedback rápido rodando testes em paralelo |
| **Node.js** | Micro-workflows focados | Organiza não só o código, mas a comunidade |

**Ideia para o nosso projeto:**
O melhor caminho é fazer um meio-termo. Podemos pegar a estrutura simples do template do GitHub e misturar com a organização do VS Code e Node: deixamos o build, lint e os testes rodando juntos nos PRs, e criamos arquivos separados pra cuidar da segurança e do deploy. Assim a pipeline não vira uma bagunça e fica fácil de dar manutenção.

---
**Fontes Consultadas:**
*   [GitHub Actions Starter Workflows](https://github.com/actions/starter-workflows)
*   [Histórico do Starter Workflows](https://github.com/actions/starter-workflows/commits/main/.github)
*   [Histórico do VS Code](https://github.com/microsoft/vscode/commits/main/.github/workflows)
*   [Histórico do Node.js](https://github.com/nodejs/node/commits/main/.github/workflows)