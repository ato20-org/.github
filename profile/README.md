# ATO20

**Ferramenta para organizar e exibir cenas de RPG de mesa.**

Feita para jogo presencial: o mestre monta a próxima cena no notebook enquanto a mesa
continua vendo a atual na TV, e cada jogador acompanha pelo próprio celular.

Aplicativo de desktop. Sem servidor, sem conta, sem login.

## Três telas, uma máquina

| Tela | Onde roda | O que é |
| --- | --- | --- |
| **Operador** | no aplicativo | A tela do mestre: monta cenas, arrasta imagens, esconde regiões, decide o que entra no ar |
| **Assistir** | navegador | Só o palco, sem controle. Vai na TV atrás do mestre |
| **Plateia** | navegador | O celular de cada jogador, com a ficha e os anexos dele |

A cena **em edição** e a cena **no ar** são separadas — é isso que permite preparar a
próxima enquanto a mesa segue na atual.

Quem serve as duas telas de espectador é um daemon HTTP que roda dentro do próprio
aplicativo, na porta 20200 da rede local. Ninguém instala nada além do mestre: os jogadores
lêem um QR e abrem uma aba.

## Uma campanha é uma pasta

O modelo é o do Obsidian. Você aponta o aplicativo para uma pasta, e ela é a campanha:

```
minha-campanha/
  config.json          nome, código da mesa, versão do formato
  ordem.json           a ordem das cenas, qual está aberta, qual está no ar
  cenas/*.json         itens, áreas escondidas, câmera
  assets/              os mapas, os retratos, as trilhas
  jogadores/{id}/      o que cada jogador anexou
  .ato20/              miniaturas e estado da sessão (derivado, descartável)
```

Isso existe por causa de um custo que travou a versão anterior. Ela guardava mapas e
trilhas no Storage do Supabase, e uma campanha grande enche o plano gratuito: a saída seria
pagar servidor por usuário ou empilhar compressão para caber. No disco de quem opera esse
custo não existe, e o teto passa a ser o HD.

O formato é texto onde dá: `git diff` numa cena mostra o token que andou, e um `config.json`
aberto no editor diz o que a campanha é. A campanha inteira cabe num `.ato20.zip`, e o zip
abre em qualquer outra máquina com a mesa continuando a valer.

## Extensões

Uma extensão é uma **pasta com `manifesto.json` dentro**. Instalar é copiar essa pasta para
a máquina — é o mesmo formato que se publica aqui no GitHub, então quem clona o repositório
já tem exatamente o que o diálogo de importar pede.

São duas naturezas, e a separação não é arrumação: **tema** é CSS que a cascata aplica, e o
pior que ele faz é deixar a interface feia; **funcionalidade** é um módulo ESM que roda com
o alcance da janela, e instalar uma é confiar em quem a escreveu, do mesmo jeito que se
confia numa extensão do VSCode.

Não há loja, não há revisão e não há sandbox. O aplicativo mostra autor e repositório antes
de habilitar — o resto é essa confiança.

## Os repositórios

| Repositório | O que é |
| --- | --- |
| [`ato20`](https://github.com/ato20-org/ato20) | O aplicativo. Next.js + Tauri 2, com daemon `axum` e `rusqlite` no mesmo processo |
| [`ato20-exemplos`](https://github.com/ato20-org/ato20-exemplos) | Extensões de exemplo: dois temas, dois plugins de funcionalidade |
| [`next.ato20`](https://github.com/ato20-org/next.ato20) | A landing page |

## Rodar

Requer Node 20+, pnpm e Rust estável. No Linux, as dependências do Tauri: `webkit2gtk-4.1`,
`gtk3`, `libayatana-appindicator`, `librsvg`.

```bash
pnpm install
pnpm tauri dev
```

`pnpm dev` sozinho serve as telas em `localhost:3000`, mas o Operador aparece dizendo "abra
pelo aplicativo": uma aba de navegador não alcança o disco.

## Estado

Projeto pessoal, em migração de uma versão web anterior. O Operador está completo; Assistir,
Plateia, fichas de jogador e exportar/importar funcionam na rede local. Ainda não há release
publicada — por enquanto se roda a partir do código.

MIT.
