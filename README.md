# LikeC4 Template

Este repositório é um template que demonstra o uso escalável do [LikeC4](https://likec4.dev/): modelos e relações canônicas ficam em `architecture/models/`, enquanto visões e casos de uso ficam em `architecture/contexts/`.

A arquitetura de exemplo é uma loja fictícia. Ela tem catálogo, pedidos, checkout, contas, pagamentos e comunicações. É pequena de propósito: há peças suficientes para mostrar como o modelo e os diagramas se conectam, sem exigir que você entenda uma plataforma inteira antes de começar.

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

## O que é o LikeC4 aqui?

O [C4 Model](https://c4model.com/) propõe olhar para um sistema em diferentes níveis de detalhe. O LikeC4 permite descrever os elementos e suas relações em arquivos `.c4` e gerar **views** desse mesmo modelo. Uma view pode mostrar o panorama dos sistemas; outra pode acompanhar uma jornada passo a passo. O LikeC4 deixa você escolher os níveis e recortes que fazem sentido para o seu caso.

Neste template, `architecture/models/` guarda as definições canônicas compartilhadas: cliente, sistemas, serviços, componentes, classes ilustrativas, bancos, fila e relações. `architecture/contexts/` reúne as visões e os casos de uso que reutilizam essas definições em cada recorte. Pense na API de Pedidos: ela é definida uma vez em `bounded-context-orders.c4`, participa das relações em `relationships.c4` e aparece no fluxo de criação do pedido. Se a definição mudar, as views continuam usando a mesma peça.

Um trecho do modelo:

```c4
orders_api = service 'API de Pedidos' {
  description 'Cria pedidos e publica o evento de pedido criado.'
  technology 'Kotlin'
  icon tech:kotlin
}
```

No fluxo, a referência `orders.orders_api` aponta para esse serviço dentro do contexto Pedidos:

```c4
orders.checkout.checkout_api -[https]-> orders.orders_api 'Solicita a criação'
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

O build grava o resultado em `dist/`. Há uma instalação e um conjunto de comandos para o repositório inteiro.

## Publicação no GitHub Pages

Em **Settings → Pages → Build and deployment**, selecione **GitHub Actions** como fonte. O workflow `.github/workflows/pages.yml` valida e publica o site a cada push na `main`. Após a primeira execução, acesse [rene-kt.github.io/likec4-template/](https://rene-kt.github.io/likec4-template/). O build usa o caminho do Pages e links com hash para que as views abram diretamente.

## Referências

- [Introdução à DSL do LikeC4](https://likec4.dev/dsl/intro/) — estrutura dos arquivos `.c4`.
- [Tutorial do LikeC4](https://likec4.dev/tutorial/) — primeiro modelo e primeiras views.
- [Modelos](https://likec4.dev/dsl/model/) e [views](https://likec4.dev/dsl/views/) — elementos, relações e recortes.
- [Dynamic views](https://likec4.dev/dsl/views/dynamic/) — jornadas e interações em sequência.
- [CLI do LikeC4](https://likec4.dev/tooling/cli/) — comandos para validar, gerar e visualizar.
- [C4 Model](https://c4model.com/) — a ideia por trás dos níveis de diagramas.
