# diagramas-dsl

```dsl
workspace "Portal de Vagas Online" "Arquitetura do sistema de gerenciamento de vagas temporárias e prestação de serviços diários" {

    !identifiers hierarchical

    model {

        admin = person "Administrador" "Responsável pela administração geral do sistema"

        operador = person "Responsável Operacional" "Usuário responsável pela criação de vagas e aprovação de candidatos"

        profissional = person "Profissional" "Prestador de serviço que se candidata às vagas"

        sistema = softwareSystem "Portal de Vagas Online" "Sistema para gerenciamento de vagas temporárias e candidaturas" {

            webapp = container "Web Application" "Interface web para usuários e administradores" "Angular" {
                tags "WebApp"
            }

            api = container "API Backend" "Responsável pelas regras de negócio e processamento do sistema" "Java + Spring Boot" {

                authComponent = component "Módulo de Autenticação" "Gerencia login, JWT e controle de acesso"

                usuariosComponent = component "Módulo de Usuários" "Gerencia usuários, perfis e permissões"

                vagasComponent = component "Módulo de Vagas" "Gerencia criação e encerramento de vagas"

                candidaturasComponent = component "Módulo de Candidaturas" "Gerencia candidaturas e aprovação de profissionais"

                notificacoesComponent = component "Módulo de Notificações" "Responsável pelo processamento assíncrono de notificações e envio de e-mails"

                adminComponent = component "Módulo Administrativo" "Auditoria e operações administrativas"

            }

            db = container "PostgreSQL Database" "Armazena dados do sistema" "PostgreSQL" {
                tags "Database"
            }

        }

    }

}
```
