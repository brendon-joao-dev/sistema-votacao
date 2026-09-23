```mermaid
stateDiagram-v2
    A: Não Iniciada
    B: Em Andamento
    C: Encerrada

    [*] --> A
    A --> B: Começar votação
    B --> C: Encerrar votação

    A: do / Configurar votação

    B: do / Receber votos

    C: entry / Contabilizar e revelar resultado
```