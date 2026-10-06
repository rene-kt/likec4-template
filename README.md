# LikeC4 Template

Um ponto de partida para quem quer descrever a arquitetura de um sistema com [LikeC4](https://likec4.dev/). Este repositório é um projeto pessoal e open source: você pode explorá-lo, copiar a estrutura e trocar o exemplo por componentes e jornadas do seu próprio sistema.

A arquitetura de exemplo é uma loja fictícia. Ela tem catálogo, pedidos, checkout, contas, pagamentos e comunicações. É pequena de propósito: há peças suficientes para mostrar como o modelo e os diagramas se conectam, sem exigir que você entenda uma plataforma inteira antes de começar.

## O que é o LikeC4 aqui?

O [C4 Model](https://c4model.com/) propõe olhar para um sistema em diferentes níveis de detalhe. O LikeC4 permite descrever os elementos e suas relações em arquivos `.c4` e gerar **views** desse mesmo modelo. Uma view pode mostrar o panorama dos sistemas; outra pode acompanhar uma jornada passo a passo. O LikeC4 deixa você escolher os níveis e recortes que fazem sentido para o seu caso.

Neste template, `architecture/models/` guarda as peças compartilhadas: cliente, sistemas, serviços, componentes, classes ilustrativas, bancos, fila e relações. `architecture/contexts/` reúne os diagramas que escolhem quais dessas peças aparecem em cada recorte. Pense na API de Pedidos: ela é definida uma vez em `bounded-context-orders.c4`, participa das relações em `relationships.c4` e aparece no fluxo de criação do pedido. Se a definição mudar, as views continuam usando a mesma peça.

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

O `bounded-contexts.yaml` é um catálogo para **pessoas**: registra o nome e a responsabilidade de cada contexto. O LikeC4 lê os arquivos `.c4`, não esse YAML. Por isso, quando você alterar o catálogo, atualize também o arquivo `bounded-context-*.c4` correspondente. No exemplo, `payments` tem `purchase` e `processor`; cada um usa `extend payments` em um arquivo como `bounded-context-payments--purchase.c4`. O mesmo padrão aparece em `orders`, com o subcontexto `checkout`.

As relações ficam em `relationships.c4` para que a mesma ligação possa aparecer em diferentes views. Em `architecture/contexts/`, cada pasta reúne os diagramas de um assunto. `orders` acompanha o pedido; `account` acompanha o cadastro; `payments/purchase` acompanha a confirmação do pagamento; e `payments/processor` mostra como o resultado volta ao subcontexto de compra. Essas pastas contêm **views**, enquanto a definição de cada bounded context continua em `architecture/models/bounded-context-*.c4`. Todos os diagramas pertencem ao mesmo projeto LikeC4 e usam os mesmos modelos.

| Pasta | O que mostra |
| --- | --- |
| `level-1/` | O contexto principal, o cliente e os sistemas com que se relaciona. |
| `level-2/` | As aplicações, APIs, filas ou bancos dentro do contexto. |
| `level-3/` | Componentes internos ou um fluxo dinâmico da jornada. |
| `level-4/` | Classes fictícias dentro da Aplicação de Checkout, no exemplo de Pedidos. |

Em `orders`, quatro views estáticas mostram a arquitetura com aproximações sucessivas. `account`, `payments/purchase` e `payments/processor` têm views estáticas nos níveis 1 e 2 e um fluxo dinâmico no nível 3. O fluxo acompanha uma jornada em vez de mostrar a estrutura interna de um único elemento. O [C4 Model](https://c4model.com/diagrams) chama os níveis estáticos de contexto, contêineres, componentes e código; o quarto é opcional e aparece aqui apenas em Pedidos para ensinar a organização. As classes do exemplo não vêm de uma implementação real.

Os ícones e as tecnologias também fazem parte dos exemplos: há serviços em Python, Kotlin e PHP, com cores distintas, e bancos MySQL e PostgreSQL. Troque esses metadados pelas tecnologias do seu sistema quando adaptar o template.

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
4. Crie uma pasta em `architecture/contexts/<contexto>/` para agrupar as views desse assunto. Se o contexto tiver recortes próprios, crie subpastas como `architecture/contexts/payments/purchase/` e `architecture/contexts/payments/processor/`. Coloque `level-1/` até `level-4/` dentro da pasta que descreve a view. Use views estáticas para mostrar a estrutura e uma `dynamic view` para explicar uma jornada.
5. Rode `npm run validate` a cada mudança e abra a interface com `npm run dev` para conferir se o diagrama conta a história que você pretendia.

Depois que seus próprios contextos e views estiverem prontos, remova o exemplo da loja fictícia. Os nomes e as cores são apenas uma sugestão inicial.

Se usar um agente para editar a arquitetura, o [AGENTS.md](AGENTS.md) indica a skill local em `.agents/skills/likec4-dsl/` e os comandos de verificação.

## Onde posso aprender mais?

- [Introdução à DSL do LikeC4](https://likec4.dev/dsl/intro/) — estrutura dos arquivos `.c4`.
- [Tutorial do LikeC4](https://likec4.dev/tutorial/) — primeiro modelo e primeiras views.
- [Modelos](https://likec4.dev/dsl/model/) e [views](https://likec4.dev/dsl/views/) — elementos, relações e recortes.
- [Dynamic views](https://likec4.dev/dsl/views/dynamic/) — jornadas e interações em sequência.
- [CLI do LikeC4](https://likec4.dev/tooling/cli/) — comandos para validar, gerar e visualizar.
- [C4 Model](https://c4model.com/) — a ideia por trás dos níveis de diagramas.

## Quer contribuir?

Encontrou uma explicação confusa ou uma forma mais simples de mostrar a estrutura? Abra uma issue ou envie um pull request no GitHub. Mantenha os exemplos genéricos para que outras pessoas possam adaptar o template com facilidade. O projeto usa a licença descrita em [LICENSE](LICENSE).
