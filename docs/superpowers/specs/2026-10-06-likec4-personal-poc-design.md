# POC pessoal de arquitetura com LikeC4

## Objetivo

Transformar este repositório em um exemplo pessoal e executável da organização LikeC4 observada em `../../c4-sympla`. A POC deve mostrar como um catálogo de bounded contexts, modelos compartilhados e diagramas de um spoke se relacionam, usando apenas componentes fictícios e genéricos.

## Escopo

O repositório terá um único projeto LikeC4 na raiz, com uma única instalação e comandos de desenvolvimento, validação e build. Não haverá Astro, portal de documentação por spoke, configuração `likec4.config.json` por spoke, geração de configurações, múltiplos pacotes nem template de spoke. A estrutura legada, os modelos e os nomes de negócio da Sympla não serão copiados.

Arquivos propostos:

```text
architecture/
  bounded-contexts.yaml
  models/
    _spec.c4
    _bounded-context-colors.c4
    actors.c4
    bounded-context-catalog.c4
    bounded-context-orders.c4
    bounded-context-orders--checkout.c4
    bounded-context-notifications.c4
    relationships.c4
  spokes/
    orders/
      README.md
      diagrams/
        orders-landscape.c4
        place-order-level-3.c4
package.json
package-lock.json
.gitignore
README.md
```

`architecture/models/` define os elementos compartilhados e as relações estáticas. `architecture/spokes/orders/diagrams/` contém apenas views; esse spoke é uma pasta de organização dentro do mesmo projeto LikeC4, sem instalação ou build próprios. Não é necessário criar `architecture/templates/`.

## Modelo de exemplo

O catálogo apresenta Catálogo, Pedidos e Comunicações, com Checkout como subcontexto de Pedidos. Cada contexto e subcontexto tem um arquivo `bounded-context-*.c4` correspondente. Os exemplos incluem um cliente, uma aplicação de compra, APIs, um banco de pedidos, uma fila de eventos e um serviço de notificação. Os nomes, textos e relações descrevem uma loja fictícia, sem marca ou infraestrutura particular da Sympla.

`_spec.c4` define somente os tipos, relações e estilos usados pelos exemplos. `_bounded-context-colors.c4` fornece uma cor por contexto. `actors.c4` define o ator humano. `relationships.c4` reúne as relações estáticas entre elementos definidos nos arquivos de contexto. Os identificadores de elementos usam `snake_case`; nomes de arquivos e IDs de views usam `kebab-case`.

## Views

`orders-landscape.c4` apresenta os contextos, o cliente e a colaboração principal em uma vista estática. `place-order-level-3.c4` apresenta uma única jornada de pedido em `dynamic view`, com passos de cliente, checkout, persistência e notificação. Cada referência da view aponta para um elemento existente no modelo. O fluxo é claramente ilustrativo, sem afirmar representar um sistema real.

## Experiência de uso

O `README.md` explica o propósito da POC, a árvore de arquivos, o papel do catálogo e de cada grupo de arquivos, os comandos `npm install`, `npm run dev`, `npm run validate` e `npm run build`, e como criar um novo contexto ou uma nova view. `package.json` contém apenas `likec4` como dependência de desenvolvimento e scripts para esses comandos. Artefatos como `node_modules/` e `dist/` ficam fora do Git.

## Verificação

Após instalar dependências, `npm run validate` deve aceitar todos os modelos e views, e `npm run build` deve gerar o site estático. Uma inspeção final deve confirmar a ausência de Astro, configurações por spoke, pacotes por spoke, modelos específicos da Sympla e referências LikeC4 quebradas. Se o LikeC4 exigir configuração explícita para encontrar os arquivos, uma única configuração na raiz poderá ser adicionada; ela não criará outros projetos.
