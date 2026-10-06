# POC pessoal de arquitetura com LikeC4

## Objetivo

Transformar este repositório em um template open source para GitHub, pessoal e executável, inspirado na organização LikeC4 observada em `../../c4-sympla`. Ele deve ajudar devs e arquitetos a iniciar e adaptar seus próprios modelos. A POC mostra como um catálogo de bounded contexts, modelos compartilhados e views organizadas por domínio se relacionam, usando apenas componentes fictícios e genéricos.

## Escopo

O repositório terá um único projeto LikeC4 na raiz, com uma única instalação e comandos de desenvolvimento, validação e build. Não haverá Astro, portais de documentação por domínio, configuração `likec4.config.json` por domínio, geração de configurações, múltiplos pacotes nem template de subprojeto. A estrutura legada, os modelos e os nomes de negócio da Sympla não serão copiados. A nomenclatura adotada para agrupar diagramas é `architecture/views/<dominio>/`; o termo usado no projeto de origem não aparecerá no produto final.

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
  views/
    level-1/
      orders-context.c4
    level-2/
      orders-containers.c4
    level-3/
      checkout-components.c4
      place-order-level-3.c4
    level-4/
      checkout-code.c4
package.json
package-lock.json
.gitignore
README.md
```

`architecture/models/` define os elementos compartilhados e as relações estáticas. `architecture/views/level-1/` a `architecture/views/level-4/` organizam os diagramas por nível de detalhe dentro do mesmo projeto LikeC4, sem instalação ou build próprios. Não é necessário criar `architecture/templates/`.

## Modelo de exemplo

O catálogo apresenta Catálogo, Pedidos e Comunicações, com Checkout como subcontexto de Pedidos. Cada contexto e subcontexto tem um arquivo `bounded-context-*.c4` correspondente. Os exemplos incluem um cliente, uma aplicação de compra, APIs com ícones de Python, Kotlin e PHP e cores diferentes por serviço, bancos MySQL e PostgreSQL, uma fila de eventos e um serviço de notificação. A API de Checkout contém componentes e classes fictícias para ilustrar os níveis 3 e 4. Os nomes, textos e relações descrevem uma loja fictícia, sem marca ou infraestrutura particular da Sympla.

`_spec.c4` define somente os tipos, relações e estilos usados pelos exemplos. `_bounded-context-colors.c4` fornece uma cor por contexto. `actors.c4` define o ator humano. `relationships.c4` reúne as relações estáticas entre elementos definidos nos arquivos de contexto. Os identificadores de elementos usam `snake_case`; nomes de arquivos e IDs de views usam `kebab-case`.

## Views

Há uma vista estática por nível: contexto de Pedidos no nível 1, aplicações e banco no nível 2, componentes da API de Checkout no nível 3 e classes fictícias da Aplicação de Checkout no nível 4. `place-order-level-3.c4` apresenta uma jornada de pedido em `dynamic view`, com passos de cliente, checkout, persistência e notificação. Cada referência aponta para um elemento existente no modelo. O fluxo e as classes são ilustrativos, sem afirmar representar um sistema real.

## Experiência de uso

O `README.md` é a porta de entrada do template no GitHub. Ele usa português claro, técnico e conversacional, com perguntas como títulos, exemplos curtos da DSL e uma explicação concreta de como uma definição do modelo é reutilizada por várias views. O tom acompanha o README do `c4-sympla`, sem copiar trechos ou usar seus nomes e imagens. Deve cobrir: objetivo e público, o que é LikeC4 e sua relação com o C4 Model, a árvore de arquivos, a função do catálogo, dos modelos e das views, como o projeto único funciona sem configurações por domínio, os comandos `npm install`, `npm run dev`, `npm run validate` e `npm run build`, e um passo a passo para substituir o exemplo por contextos e jornadas próprios. Inclui links diretos para a documentação oficial de [introdução](https://likec4.dev/dsl/intro/), [tutorial](https://likec4.dev/tutorial/), [modelos](https://likec4.dev/dsl/model/), [views](https://likec4.dev/dsl/views/), [dynamic views](https://likec4.dev/dsl/views/dynamic/), [CLI](https://likec4.dev/tooling/cli/) e [C4 Model](https://c4model.com/). Uma seção curta explica como contribuir no GitHub e aponta a licença já existente.

`package.json` contém apenas `likec4` como dependência de desenvolvimento e scripts para esses comandos. Artefatos como `node_modules/` e `dist/` ficam fora do Git.

## Verificação

Após instalar dependências, `npm run validate` deve aceitar todos os modelos e views, e `npm run build` deve gerar o site estático. Uma inspeção final deve confirmar a ausência de Astro, configurações e pacotes por domínio, modelos específicos da Sympla, referências à antiga nomenclatura de agrupamento e referências LikeC4 quebradas. Os links oficiais do README devem apontar para páginas existentes. Se o LikeC4 exigir configuração explícita para encontrar os arquivos, uma única configuração na raiz poderá ser adicionada; ela não criará outros projetos.
