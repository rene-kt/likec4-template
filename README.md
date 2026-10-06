# LikeC4 Template

Um ponto de partida para quem quer descrever a arquitetura de um sistema com [LikeC4](https://likec4.dev/). Este repositório é um projeto pessoal e open source: você pode explorá-lo, copiar a estrutura e trocar o exemplo por componentes e jornadas do seu próprio sistema.

A arquitetura de exemplo é uma loja fictícia. Ela tem catálogo, pedidos, checkout e comunicações. É pequena de propósito: há peças suficientes para mostrar como o modelo e os diagramas se conectam, sem exigir que você entenda uma plataforma inteira antes de começar.

## O que é o LikeC4 aqui?

O [C4 Model](https://c4model.com/) propõe olhar para um sistema em diferentes níveis de detalhe. O LikeC4 permite descrever os elementos e suas relações em arquivos `.c4` e gerar **views** desse mesmo modelo. Uma view pode mostrar o panorama dos sistemas; outra pode acompanhar uma jornada passo a passo. O LikeC4 deixa você escolher os níveis e recortes que fazem sentido para o seu caso.

Neste template, `architecture/models/` guarda as peças compartilhadas: cliente, sistemas, serviços, banco, fila e relações. `architecture/views/` escolhe quais dessas peças aparecem em cada diagrama. Pense na API de Pedidos: ela é definida uma vez em `bounded-context-orders.c4`, participa das relações em `relationships.c4` e aparece no fluxo de criação do pedido. Se a definição mudar, as views continuam usando a mesma peça.

Um trecho do modelo:

```c4
orders_api = service 'API de Pedidos' {
  description 'Cria pedidos e publica o evento de pedido criado.'
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
└── views/
    └── orders/
        ├── orders-landscape.c4           # visão geral
        └── place-order-level-3.c4        # jornada detalhada
```

O `bounded-contexts.yaml` é um catálogo para **pessoas**: registra o nome e a responsabilidade de cada contexto. O LikeC4 lê os arquivos `.c4`, não esse YAML. Por isso, quando você alterar o catálogo, atualize também o arquivo `bounded-context-*.c4` correspondente. No exemplo, `orders` tem o subcontexto `checkout`, definido em `bounded-context-orders--checkout.c4` com `extend orders`.

As relações ficam em `relationships.c4` para que a mesma ligação possa aparecer em diferentes views. Em `architecture/views/orders/`, a visão geral mostra cliente e contextos; a `dynamic view` mostra a sequência ilustrativa de uma compra até o processamento do evento de pedido. As duas pertencem ao mesmo projeto LikeC4 e usam os mesmos modelos.

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

O build grava o resultado em `dist/`. Há uma instalação e um conjunto de comandos para o repositório inteiro.

## Como adapto o template ao meu sistema?

1. Comece pelo `architecture/bounded-contexts.yaml`: escreva os contextos e a responsabilidade de cada um.
2. Crie um `architecture/models/bounded-context-<id>.c4` para cada contexto. Se houver um subcontexto, siga o exemplo `bounded-context-orders--checkout.c4` e use `extend` no contexto pai.
3. Defina atores em `actors.c4` e ligações entre componentes em `relationships.c4`.
4. Crie arquivos em `architecture/views/<dominio>/` para mostrar os recortes que interessam à sua equipe. Uma view estática serve para o panorama; uma `dynamic view` ajuda a explicar uma jornada.
5. Rode `npm run validate` a cada mudança e abra a interface com `npm run dev` para conferir se o diagrama conta a história que você pretendia.

Depois que seus próprios contextos e views estiverem prontos, remova o exemplo da loja fictícia. Os nomes e as cores são apenas uma sugestão inicial.

## Onde posso aprender mais?

- [Introdução à DSL do LikeC4](https://likec4.dev/dsl/intro/) — estrutura dos arquivos `.c4`.
- [Tutorial do LikeC4](https://likec4.dev/tutorial/) — primeiro modelo e primeiras views.
- [Modelos](https://likec4.dev/dsl/model/) e [views](https://likec4.dev/dsl/views/) — elementos, relações e recortes.
- [Dynamic views](https://likec4.dev/dsl/views/dynamic/) — jornadas e interações em sequência.
- [CLI do LikeC4](https://likec4.dev/tooling/cli/) — comandos para validar, gerar e visualizar.
- [C4 Model](https://c4model.com/) — a ideia por trás dos níveis de diagramas.

## Quer contribuir?

Encontrou uma explicação confusa ou uma forma mais simples de mostrar a estrutura? Abra uma issue ou envie um pull request no GitHub. Mantenha os exemplos genéricos para que outras pessoas possam adaptar o template com facilidade. O projeto usa a licença descrita em [LICENSE](LICENSE).
