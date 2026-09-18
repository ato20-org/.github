# ATO20

**A sua IDE para RPG de mesa.**

Feito para mesas presenciais ou em LAN: desenvolva suas histórias e controle suas cenas. O
mestre monta a próxima cena no notebook enquanto a mesa continua vendo a atual na TV, e cada
jogador acompanha pelo próprio celular.

Aplicativo de desktop. Sem servidor, sem conta, sem login.

**Open source e gratuito** — minha contribuição para a comunidade de RPG.

## Três telas, uma máquina

| Tela | Onde roda | O que é |
| --- | --- | --- |
| **Mestre** | no aplicativo | A tela do mestre: monta as cenas, arrasta as imagens, esconde regiões e decide o que entra no ar |
| **Espectador** | navegador, na TV da mesa | Só o palco, sem controle nenhum. Fica na TV atrás do mestre, e mostra a cena que está no ar |
| **Jogador** | navegador, no celular | O celular de cada jogador: a ficha dele, o caderno de notas e os dados que ele joga na mesa |

A cena **em edição** e a cena **no ar** são separadas — é isso que permite preparar a
próxima enquanto a mesa segue na atual.

Quem serve o Espectador e o Jogador é um daemon HTTP que roda dentro do próprio
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

Uma extensão é uma **pasta com `manifest.json` dentro**. Instalar é copiar essa pasta para
a máquina — é o mesmo formato que se publica aqui no GitHub, então quem clona o repositório
já tem exatamente o que o diálogo de importar pede.

São duas naturezas, e a separação não é arrumação: **tema** é CSS que a cascata aplica, e o
pior que ele faz é deixar a interface feia; **funcionalidade** é um módulo ESM que roda com
o alcance da janela, e instalar uma é confiar em quem a escreveu, do mesmo jeito que se
confia numa extensão do VSCode.

Não há loja, não há revisão e não há sandbox. O aplicativo mostra autor e repositório antes
de habilitar — o resto é essa confiança.

## Licença

[MIT](../LICENSE). Use, modifique e distribua.
