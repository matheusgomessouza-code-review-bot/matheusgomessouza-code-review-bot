# matheusgomessouza-code-review-bot

Conta de automação para code review. Opera em nome de [@matheusgomessouza](https://github.com/matheusgomessouza).

## O que faz

- Lê o diff de um pull request e publica um parecer técnico.
- Aprova o pull request quando não há bloqueio.
- Comenta, responde threads e anexa evidências.

## O que não faz

- Não dá push, não commita, não faz merge.
- Não aprova pull request em que deu o último push.
- Não tem credencial de escrita em código.

A separação é proposital. Quem corrige é a conta humana. Quem revisa e aprova é esta conta.

## Como funciona

```text
gatilho      poller local (60 s)  →  pull request com review solicitado
execução     opencode run         →  skill review-code
modelo       FreeLLMAPI :3001     →  endpoint OpenAI-compatible com fallback
publicação   GitHub API           →  parecer + APPROVED
```

Nada roda em nuvem. Não há webhook, endpoint público nem app instalado. O poller consulta a API do GitHub e executa na máquina do dono.

## Arquitetura

| Camada | Componente | Papel |
|---|---|---|
| Gatilho | poller local | Detecta pull request aguardando review |
| Execução | opencode | Roda a skill de review no worktree |
| Modelo | FreeLLMAPI | Endpoint OpenAI-compatible com rotação entre modelos |
| Identidade | esta conta | Publica o parecer e aprova |

## Regras

- Um parecer por commit. Novo push exige novo review.
- Sem aprovação sem parecer. O `APPROVED` é consequência do review, não atalho.
- Sem escrita em código. A conta não tem token de push.

## Status

Protótipo. A aprovação já foi validada em pull request real, com ruleset exigindo `required_approving_review_count: 1` e `require_last_push_approval: true`. O poller e a integração com a FreeLLMAPI estão em construção.

---

Dúvidas ou incidentes: abra uma issue neste repositório.
