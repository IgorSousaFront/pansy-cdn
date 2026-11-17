# pansy-cdn

Arquivos estáticos da loja Pansy servidos pelo jsDelivr, porque a Nuvemshop
não aceita `.mp4` dentro do tema.

## Como usar

Os arquivos são servidos por:

    https://cdn.jsdelivr.net/gh/USUARIO/pansy-cdn@TAG/video/pansy-hero.mp4

Sempre com **tag** (ou SHA do commit), nunca com `@main` nem sem versão: o
jsDelivr guarda cada versão para sempre, então a URL com tag nunca muda de
conteúdo e nunca dá problema de cache.

## Conteúdo

| Arquivo | Uso | Formato |
|---|---|---|
| `video/pansy-hero.mp4` | fundo do hero no desktop | 1920x1152, 7,76s, sem áudio |
| `video/pansy-hero-mobile.mp4` | fundo do hero no celular | 828x1104, 7,76s, sem áudio |

Os dois são recortes do mesmo trecho do filme de campanha, começando do zero e
terminando num corte, para a volta do loop não dar salto.

## Ao publicar uma versão nova

    git add .
    git commit -m "descrição"
    git tag v2
    git push origin main --tags

Depois troque a tag na URL dentro do admin da loja (seção do hero, campos de
URL do vídeo). Não reaproveite a tag antiga com conteúdo diferente.
