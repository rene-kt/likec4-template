# LikeC4 Template

Live mode: https://rene-kt.github.io/likec4-template/#/

![c4](/assets/c4.png)

Repositório template de [LikeC4](https://likec4.dev/) que agrupa skills para agentes de IA e estrutura de pasta escaláveis para múltiplos serviços e estruturas.
## Como rodar localmente?

Você precisa de **Node.js 22.22.3 ou superior** e npm. Na raiz do repositório:

```bash
npm install
npm run dev
```

O comando `dev` inicia a interface local do LikeC4 e mostra a URL no terminal. Para conferir os arquivos e gerar o site estático:

```bash
npm run validate
npm run build
```

## Como o projeto está organizado?

```text
architecture/
├── bounded-contexts.yaml                 # índice legível dos domínios e responsabilidades
├── models/
│   ├── _spec.c4                          # tipos de elementos e relações
│   ├── _bounded-context-colors.c4        # cores dos contextos
│   ├── actors.c4                         # pessoas que interagem com o sistema
│   ├── bounded-context-*.c4              # contextos, subcontextos e componentes
│   └── relationships.c4                  # ligações estáticas entre componentes
└── contexts/
    ├── orders/                          # visões da jornada de pedidos
    │   ├── level-1/                     # contexto do sistema
    │   ├── level-2/                     # aplicações e dados
    │   ├── level-3/                     # componentes e fluxo de compra
    │   └── level-4/                     # classes ilustrativas
    ├── payments/                        # contexto pai de pagamentos
    │   ├── purchase/                    # subcontexto de compra
    │   │   ├── level-1/
    │   │   ├── level-2/
    │   │   └── level-3/
    │   └── processor/                   # subcontexto de processamento
    │       ├── level-1/
    │       ├── level-2/
    │       └── level-3/
    └── account/                         # visões de contas
        ├── level-1/
        ├── level-2/
        └── level-3/
```

## Referências

- [Introdução à DSL do LikeC4](https://likec4.dev/dsl/intro/) — estrutura dos arquivos `.c4`.
- [Tutorial do LikeC4](https://likec4.dev/tutorial/) — primeiro modelo e primeiras views.
- [Modelos](https://likec4.dev/dsl/model/) e [views](https://likec4.dev/dsl/views/) — elementos, relações e recortes.
- [Dynamic views](https://likec4.dev/dsl/views/dynamic/) — jornadas e interações em sequência.
- [CLI do LikeC4](https://likec4.dev/tooling/cli/) — comandos para validar, gerar e visualizar.
- [C4 Model](https://c4model.com/) — a ideia por trás dos níveis de diagramas.
