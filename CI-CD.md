# Resumo sobre

## Conceitos de CI/CD

- Integração Contínua (CI): Processo de integrar novas alterações ao código base de forma automatizada. Envolve a execução de builds, linting (padronização) e testes (unitários, integração e E2E) sempre que o código é mesclado (merged) para garantir a qualidade do software.
- Entrega Contínua (Continuous Delivery): Etapa em que o código, após passar pelos testes e integração, encontra-se pronto para ser lançado aos usuários a qualquer momento. Pode exigir uma intervenção humana ou aprovação final para ser colocado em produção.
- Deploy Contínuo (Continuous Deploy): Automatização completa do fluxo de envio. A partir do momento em que o código é integrado à branch principal, ele é enviado automaticamente para o servidor de produção, sem necessidade de interação humana.
- Diferença entre Deploy e Release: O deploy refere-se à instalação do código no ambiente (servidor ou cloud). Já a release é a disponibilização funcional daquela atualização para o público final, o que pode ser gerenciado por ferramentas como feature flags ou rollouts graduais.

## Práticas de Implementação

- Automação de Pipelines: O uso de workflows em ferramentas como o GitHub Actions é fundamental para padronizar processos, eliminar erros manuais e garantir que cada etapa (testes e envio) ocorra de forma consistente.
- Gestão de Segurança: Informações críticas, como senhas de servidores (VPS) ou credenciais de serviços em nuvem, não devem ser incluídas no código fonte. Devem ser armazenadas nas configurações de Secrets do repositório e acessadas apenas durante a execução da pipeline.
- Fluxo de Ambientes: É prática comum utilizar branches intermediárias (como dev ou staging) antes da main. Isso permite testar as funcionalidades em um ambiente que replica a produção, muitas vezes utilizando bancos de dados anonimizados para validar a aplicação com segurança antes do lançamento final.
