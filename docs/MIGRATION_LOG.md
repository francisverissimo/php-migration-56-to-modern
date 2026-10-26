# Diário de Migração (MIGRATION_LOG)

Registro obrigatório e cronológico de **cada passo** da migração. Nenhum passo é
considerado concluído sem uma entrada aqui. Entradas mais recentes no topo.

Regra de ouro: toda mudança precisa estar amarrada a uma **incompatibilidade concreta**
da versão alvo do PHP. Se não há incompatibilidade, não há mudança.

---

## Modelo de entrada (copiar para cada passo)

### [AAAA-MM-DD HH:MM] <título curto do passo>

- **Versão origem → destino:** ex. PHP 7.4 → 8.0
- **Escopo:** arquivos / módulos tocados
- **Motivo (incompatibilidade):** qual mudança do PHP forçou a alteração (ex.: `curly brace`
  para acesso a string removido no 8.0; `each()` removido no 8.0; etc.)
- **O que mudou:** descrição objetiva, sem "melhorias"
- **Comandos executados:**
  ```bash
  # ex:
  # vendor/bin/rector process app --dry-run
  # vendor/bin/rector process app
  # php vendor/bin/phpunit
  ```
- **Resultado dos testes de caracterização:** PASSOU / FALHOU (+ detalhes)
- **Decisões levadas ao usuário:** (se houve alguma mudança de comportamento inevitável)
- **Commit:** `<hash>` (preencher após revisão)

---

## Estado atual

- **Versão alvo final pretendida:** _(a definir — ex. PHP 8.3)_
- **Versão atual do código:** PHP 5.6
- **Suíte de caracterização:** _(não criada ainda)_
- **Cobertura aproximada do comportamento crítico:** _(a medir)_

---

## Histórico

<!-- Novas entradas acima desta linha, em ordem decrescente de data -->

### [AAAA-MM-DD] Setup inicial do repositório e do agente

- **Escopo:** criação do repositório, do agente Kiro `php-migration` e da documentação base.
- **O que mudou:** estrutura inicial. Nenhum código de aplicação tocado ainda.
- **Próximo passo:** trazer o código legado para o repositório e levantar a suíte de
  testes de caracterização sobre o comportamento atual (ainda em PHP 5.6).
