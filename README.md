# pansy-cdn

Arquivos estáticos da loja Pansy servidos pelo jsDelivr, porque a Nuvemshop
não aceita `.mp4` dentro do tema (limite de 2048 KB por arquivo).

## Como usar

    https://cdn.jsdelivr.net/gh/igorsousafront/pansy-cdn@TAG/video/pansy-hero.mp4

Sempre com **tag** (ou SHA do commit), nunca com `@main` nem sem versão: o
jsDelivr guarda cada versão para sempre, então a URL com tag nunca muda de
conteúdo e nunca dá problema de cache. Limite do jsDelivr: 20 MB por arquivo.

## Conteúdo

| Arquivo | Uso | Formato | Peso |
|---|---|---|---|
| `video/pansy-hero.mp4` | fundo do hero no desktop | 1920x1152, 12,64s, sem áudio | 8,8 MB |
| `video/pansy-hero-mobile.mp4` | fundo do hero no celular | 828x1104, 12,64s, sem áudio | 4,3 MB |

Os dois são recortes do mesmo trecho do filme de campanha, de 0 a 12,6376s —
que é um corte do filme, para a volta do loop não dar salto.

## Receita de encode

Fonte: `pansy-ecommerce/assets/video/pansy-novembro-26-v2.mp4`, o proxy H264 de
**2880x2160** a 28 Mbps. Não usar o proxy de 1440x1080: tem 4,5 Mbps e dá
resultado pior.

    # desktop (recorte 5:3)
    ffmpeg -i MASTER -t 12.6376 \
      -vf "crop=2880:1728:0:216,scale=1920:1152:flags=lanczos" \
      -c:v libx264 -profile:v high -preset slow -tune film -crf 29 \
      -pix_fmt yuv420p -g 48 -movflags +faststart -an video/pansy-hero.mp4

    # celular (recorte 3:4)
    ffmpeg -i MASTER -t 12.6376 \
      -vf "crop=1620:2160:630:0,scale=828:1104:flags=lanczos" \
      -c:v libx264 -profile:v high -preset slow -tune film -crf 28 \
      -pix_fmt yuv420p -g 48 -movflags +faststart -an video/pansy-hero-mobile.mp4

O filme tem muito grão, que é caro de comprimir: sem denoise, CRF 21 dá 37 MB
e CRF 25 dá 20 MB. O CRF 29 mantém o grão limpo (sem o empastamento da v1) em
8,8 MB. **Não usar `hqdn3d`**: tira o grão e deixa a imagem plastificada.

## Ao publicar uma versão nova

    git add .
    git commit -m "descrição"
    git tag v3
    git push origin main --tags

Depois troque a tag na URL dentro do admin da loja (seção do hero, campos de
URL do vídeo). Não reaproveite uma tag antiga com conteúdo diferente.

A pasta `input/` está no `.gitignore`: masters não entram no repositório.
