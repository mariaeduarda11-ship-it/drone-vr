# 🛰️ Drone em Realidade Virtual

Visualizador de **nuvem de pontos de drone** que roda direto no navegador e funciona em **óculos de realidade virtual** (Meta Quest e outros com WebXR). Feito para apresentações em escolas: os alunos veem o bairro como uma **maquete em cima de uma mesa** ou entram nele em **tamanho real**, e comparam a altura de casas e árvores.

## Dados deste projeto

- **Origem:** levantamento com drone processado no Agisoft Metashape (271 fotos, voo a 237 m, resolução de 5,36 cm/pixel, área de 1,29 km²).
- **Arquivo:** `dados/nuvem.las` foi gerado a partir do `MDT_REAL.tif` (pixel de 0,41 m, SIRGAS 2000 / UTM 23S). São cerca de **730 mil pontos** que cobrem ~460 × 770 m, com altitudes de **61 a 90 m**.
- **Cores:** as cores mostram a **altitude** (azul = mais baixo, vermelho = mais alto) com sombreamento do relevo. Como o ortomosaico (1 GB) não foi usado, o arquivo não tem as cores reais das fotos.
- **Ajustes:** a página abre com a maquete **girando** e o relevo **exagerado 3×**. Os dois podem ser mudados no painel ou no `CONFIG` do `index.html`.

## O que ele faz

- Abre arquivos **.LAS**, **.PLY**, **.XYZ / .TXT / .CSV** (exportados do Metashape, Pix4D, WebODM, DroneDeploy, CloudCompare…)
- **Cores reais** (RGB das fotos) ou **cores por altura** (azul = chão, vermelho = mais alto), com legenda em metros
- **Medição de altura**: clique (ou aperte o gatilho no VR) num telhado ou numa árvore e aparece a altura em metros, com uma linha até o chão
- **Modo maquete** (o bairro inteiro em 1,6 m sobre uma mesa) e **modo tamanho real** (escala 1:1)
- Se não houver arquivo, carrega um **bairro de exemplo** com casas, uma escola, uma praça e árvores

## Publicar no GitHub Pages (uns 5 minutos)

1. Crie um repositório no GitHub (ex.: `drone-vr`), marcado como **Public**.
2. Envie o `index.html` e a pasta `dados/` com a sua nuvem com o nome **`nuvem.las`**.
3. No repositório, vá em **Settings → Pages → Branch: `main` / `(root)` → Save**.
4. Em 1 ou 2 minutos o site fica em: `https://SEU-USUARIO.github.io/drone-vr/`

Para usar outro nome de arquivo, troque `arquivoPadrao` no começo do script do `index.html`, ou passe na URL:
`https://SEU-USUARIO.github.io/drone-vr/?arquivo=dados/meu_bairro.ply`

> ⚠️ **Limite de tamanho**: pelo site do GitHub dá para enviar arquivos de até **25 MB**. Pelo Git ou pelo GitHub Desktop, até **100 MB**. Veja abaixo como reduzir a nuvem.
>
> 💡 **Plano B**: o botão **"Abrir nuvem de pontos"** carrega um arquivo do próprio computador ou dos óculos, sem precisar subir nada no GitHub.

## Preparar a nuvem no CloudCompare (gratuito)

1. Abra a nuvem densa exportada do software do drone.
2. **Reduza a quantidade de pontos**: `Edit → Subsample → Space`, com distância de **0,15 a 0,30 m**. O objetivo é chegar a uns **0,5 a 1,5 milhão de pontos**.
   - LAS com cor ocupa uns 26 a 34 bytes por ponto: **700 mil pontos ≈ 20 a 24 MB** (cabe no envio pelo site).
3. Se quiser, recorte só a área de interesse: `Edit → Segment` (tesoura).
4. **Salve como LAS** (`File → Save → LAS cloud`). Dê preferência ao **LAS em vez de PLY**: o LAS guarda as coordenadas UTM com precisão total.
   - O formato **.LAZ (comprimido) não é aceito**. Salve como .LAS mesmo.

## Equipamentos

| Aparelho | Como funciona |
|---|---|
| **Meta Quest 2/3/Pro** | Abra o link no navegador do Quest e toque em **ENTER VR**. Funciona muito bem. |
| **Computador + projetor** | Mouse: arrastar gira, roda dá zoom, botão direito move. Clique para medir. |
| **Celular Android** | Navegação com o dedo; o modo VR depende do aparelho. |
| **iPhone** | Navegação com o dedo (o Safari não tem modo VR imersivo). |

O site precisa estar em **https** para o VR funcionar. O GitHub Pages já é https.

## Controles no VR

- **Analógico esquerdo**: andar
- **Analógico direito**: ← → gira o corpo, ↑ ↓ sobe e desce (dá para "voar" por cima do bairro)
- **Gatilho**: medir a altura do ponto apontado
- **Botão lateral (grip)**: alterna entre **maquete** e **tamanho real**

## Ideias para a aula

- Peça aos alunos que **estimem** a altura da escola e de uma árvore, e depois **meçam** no VR.
- Troque para **cores por altura**: as árvores e os prédios se destacam do chão na hora.
- Em **tamanho real**, "fique em pé" na rua e olhe para cima, depois voe até a altura em que o drone voou.
- Discuta: como o drone sabe a altura? (fotogrametria: várias fotos sobrepostas → triangulação → pontos 3D)

## Observação técnica

A altura é medida em relação ao "chão" da nuvem (o nível de 2% dos pontos mais baixos). Em terrenos muito inclinados, a medida de quem está na parte alta fica maior do que a altura real do objeto. Para medidas exatas, use uma nuvem normalizada (altura = MDS − MDT).

---

### Prompt para recriar ou adaptar com uma IA

> Crie um visualizador web de nuvem de pontos de drone em um único `index.html` usando three.js (via CDN/importmap) com WebXR, para publicar no GitHub Pages. Requisitos: abrir arquivos LAS (sem compressão, formatos de ponto 0–10, com RGB 16 bits), PLY e XYZ/CSV, com redução automática para no máximo ~1,5 milhão de pontos; converter Z-up para Y-up e centralizar; colorir por RGB ou por altura acima do chão (percentil 2%) com legenda; dois modos: maquete sobre uma mesa virtual e escala real 1:1; medir a altura de um ponto clicado (mouse ou gatilho do controle VR) com linha até o chão e rótulo em metros; locomoção com os analógicos no VR; arrastar e soltar arquivos; carregar `dados/nuvem.las` automaticamente e, se não existir, gerar um bairro de exemplo com casas, árvores e uma escola. Interface em português, voltada para alunos de escola pública.
