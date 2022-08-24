## Adicionando um painel lateral no Vtex IO

[Documentação da vtex](https://developers.vtex.com/vtex-developer-docs/docs/vtex-store-drawer).

Apenas uma observação que não encontrei na documentação.
Quando for declarar um drawer, a chamada do bloco principal será a referência para renderizar o botão de ação (`drawer-trigger`).

Exemplo da declaração do bloco:
> 💡 Isso Também um exemplo de como fazer um filtro lateral na página de busca/catalogo/categoria
```json
{

    "drawer#filter": {
        "children": [
            "filter-navigator.v3"
        ],
        "blocks": ["drawer-trigger#filter"]
    },

    "drawer-trigger#filter": {
        "children": ["rich-text#filter-trigger"]
    },

}
```

Em qualquer lugar da página on houver a chamada de `drawer#filter` irá renderizar o botão.
> :warning: Chamar o bloco `drawer-trigger#filter` causará confusão total 🤣.