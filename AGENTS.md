# Instruções para agentes

Este repositório é um template que demonstra uma organização escalável de um projeto LikeC4: modelos e relações canônicas em `architecture/models/`; visões e casos de uso em `architecture/contexts/`.

Antes de alterar modelos, relações, views ou comandos da ferramenta, leia `.agents/skills/likec4-dsl/SKILL.md` e a referência da skill correspondente ao assunto. Consulte também o `README.md` para entender o exemplo e `architecture/bounded-contexts.yaml` para identificar a responsabilidade de cada contexto.

## Onde editar

- `architecture/models/`: especificação, atores, definições dos contextos e relações compartilhadas.
- `architecture/models/bounded-context-<id>.c4`: sistema e componentes que pertencem ao contexto do catálogo YAML.
- `architecture/models/bounded-context-<pai>--<filho>.c4`: subcontexto definido com `extend <pai>`, como `payments.purchase` e `payments.processor`.
- `architecture/models/relationships.c4`: relações estáticas entre elementos.
- `architecture/contexts/<contexto>/level-1/` a `level-4/`: views de um contexto como um todo.
- `architecture/contexts/<contexto>/<subcontexto>/level-1/` a `level-4/`: views de um recorte próprio, como `payments/purchase` e `payments/processor`. Crie apenas os níveis necessários.

Use IDs de elementos em `snake_case`, IDs de views em `kebab-case` e caminhos completos (`contexto.elemento`) nas referências entre arquivos. Atualize o catálogo YAML, os modelos, as relações, as views e o README quando uma mudança afetar mais de uma dessas partes. Mantenha exemplos genéricos, sem nomes de sistemas reais.

Há apenas um `package.json` na raiz. As pastas em `architecture/contexts/` organizam fontes `.c4` do mesmo projeto; não crie um pacote ou uma configuração LikeC4 para cada pasta. O projeto atual valida sem arquivo de configuração LikeC4. Preserve esse funcionamento ao acrescentar contextos e views.

## Verificação

Depois de editar qualquer arquivo `.c4`, execute na raiz:

```bash
npm run validate
npm run build
```

Confira os diagramas gerados quando mudar predicados de uma view. A validação sintática sozinha não garante que a view mostra os elementos esperados.
