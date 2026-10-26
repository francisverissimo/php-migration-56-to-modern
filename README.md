# Migração PHP 5.6 → Moderno (Compatibilização)

Repositório de trabalho para migrar uma aplicação legada de **PHP 5.6** para uma versão
moderna do PHP, com um objetivo estrito:

> **Compatibilizar, não modernizar.** O sistema deve funcionar **exatamente como antes**,
> apenas em uma versão atualizada do PHP. Nada de refatoração, mudança de regra de negócio,
> "melhorias" ou adoção de novos padrões por iniciativa própria.

## Princípios

1. **Comportamento preservado acima de tudo** — inclusive bugs existentes. Se o legado
   tem um comportamento estranho, ele é mantido.
2. **Migração incremental** — uma versão menor por vez: `5.6 → 7.0 → 7.4 → 8.0 → 8.1 → 8.2 → 8.3`.
   Nunca pular direto para a versão final.
3. **Testes de caracterização (golden master)** — antes de mexer em qualquer coisa,
   capturamos o comportamento atual em testes. Depois de cada passo, rodamos a suíte.
   Verde = comportamento preservado. Esta é a rede de segurança do projeto.
4. **Transformações mecânicas e auditáveis** — usar [Rector](https://getrector.com/) com os
   rulesets de **upgrade de versão** do PHP (não os de modernização estética), sempre em
   `--dry-run` primeiro, revisando o diff antes de aplicar.
5. **Documentação obrigatória** — todo passo é registrado em [`docs/MIGRATION_LOG.md`](docs/MIGRATION_LOG.md)
   antes de ser considerado concluído.

## Por que testes de caracterização, e não TDD clássico?

TDD clássico define comportamento *novo/desejado*. Aqui não queremos comportamento novo —
queremos travar o comportamento *atual* e provar que ele não mudou após a migração.
Testes de caracterização fazem exatamente isso.

## O agente Kiro

A configuração do agente está em [`.kiro/agents/php-migration.json`](.kiro/agents/php-migration.json).
Ele embute as regras acima como comportamento permanente e uma política de confiança que
deixa o Kiro trabalhar sem interrupções nas tarefas repetitivas (ler, escrever no projeto,
rodar testes/Rector em dry-run/composer), mas **para e pede confirmação** em ações sensíveis
(arquivos de segredo, comandos destrutivos, aplicar Rector de verdade, commit/push).

Para usar:

```bash
cd php-migration-56-to-modern
# coloque o código legado dentro deste repositório (ex: ./app) e então:
kiro-cli chat --agent php-migration
```

## Estrutura

```
.
├── .kiro/agents/php-migration.json   # Configuração do agente Kiro
├── docs/
│   └── MIGRATION_LOG.md              # Diário obrigatório da migração
├── .gitignore
└── README.md
```

## Nota sobre a skill netresearch/php-modernization-skill

Avaliada e **descartada** para este projeto: o foco dela é *modernização* (DTOs, enums,
`readonly`, PHPStan nível 10, PSR/PER-CS), que é o oposto do objetivo aqui. A única ideia
aproveitada em espírito é o uso de Rector para transformações de upgrade de versão.
