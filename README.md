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
        admin -> sistema.webapp "Administra o sistema"

        operador -> sistema.webapp "Publica vagas e aprova candidatos"

        profissional -> sistema.webapp "Consulta vagas e realiza candidaturas"

        sistema.webapp -> sistema.api "Consome APIs REST"

        sistema.api -> sistema.db "Lê e grava dados"

        sistema.api.authComponent -> sistema.db "Gerencia autenticação"

        sistema.api.usuariosComponent -> sistema.db "Gerencia usuários"

        sistema.api.vagasComponent -> sistema.db "Gerencia vagas"

        sistema.api.candidaturasComponent -> sistema.db "Gerencia candidaturas"

        sistema.api.notificacoesComponent -> sistema.db "Consulta destinatários e registra notificações"

        sistema.api.adminComponent -> sistema.db "Consulta auditorias"

        sistema.api.vagasComponent -> sistema.api.notificacoesComponent "Dispara evento de vaga publicada"

        sistema.api.candidaturasComponent -> sistema.api.vagasComponent "Atualiza status da vaga"

    }

    views {

        systemContext sistema "DiagramaContexto" {
            include *
            autolayout lr
        }

        container sistema "DiagramaContainer" {
            include *
            autolayout lr
        }

        component sistema.api "DiagramaComponentes" {
            include *
            autolayout lr
        }

        styles {

            element "Element" {
                color #ffffff
                stroke #0b4884
                background #1168bd
                strokeWidth 2
                shape roundedbox
            }

            element "Person" {
                background #08427b
                shape person
            }

            element "Database" {
                shape cylinder
                background #438dd5
            }

            element "WebApp" {
                background #0f6ab4
            }

            element "Boundary" {
                strokeWidth 3
                stroke #0b4884
            }

            relationship "Relationship" {
                thickness 2
                color #707070
            }

        }
    }

    configuration {
        scope softwaresystem
    }
}
```
