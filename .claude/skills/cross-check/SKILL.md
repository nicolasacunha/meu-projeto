---
name: cross-check
description: "Use para verificar a pasta de contexto de um projeto (CLAUDE.md + contexto/*.md) contra si mesma e contra uma entrega. Três modos — auditar o contexto (contradições, lacunas, trechos genéricos ou inventados), rodar 'a prova' (pergunta genérica → a resposta sai específica ou genérica?), e checar uma entrega (texto/proposta/copy/plano batendo com o contexto e as decisões fechadas). Dispara com: 'cross check', 'crosscheck', 'audita o contexto', 'roda a prova', 'confere se isso bate com o contexto', 'isso respeita as decisões?'."
---

# cross-check

Verificação cruzada da pasta de contexto de um projeto. Nasceu do fluxo "Contexto: o seu negócio em arquivos que a IA lê": o contexto define a qualidade de tudo que vem depois, então ele precisa ser lido, auditado e mantido — nunca aprovado no escuro.

## Princípio que rege tudo

A IA inventa quando falta informação — esse é o comportamento padrão, não um defeito raro. Se um arquivo de contexto afirma uma dor, um cliente ou um número que não é verdade, **toda entrega feita depois herda essa mentira**. O trabalho do cross-check é separar contexto de ficção. Na dúvida, marcar como suspeito e perguntar — nunca preencher por conta própria.

## Antes de qualquer modo: localizar o contexto

1. Encontrar a raiz do contexto no projeto atual:
   - `CLAUDE.md` na raiz.
   - Pasta `contexto/` (aceitar também `context/`, `.context/`).
2. Ler **todos** os arquivos inteiros: `CLAUDE.md` e cada `contexto/*.md`. Não amostrar, não pular — a auditoria só vale se o arquivo foi lido do começo ao fim.
3. Se não houver pasta de contexto, dizer isso e parar — não há o que cruzar.

## Modo 1 — Auditar o contexto

Objetivo: achar o que está errado, faltando ou genérico **antes** de o contexto ser usado.

Rodar quatro passagens sobre os arquivos lidos:

1. **Contradições entre arquivos.** Cruzar as afirmações de cada arquivo com as dos outros. Ex.: `negocio.md` diz que fecha vendas sozinho, `decisoes.md` diz que contratou closer → contradição. Listar par a par (arquivo A × arquivo B) com a linha de cada lado.
2. **Lacunas.** Marcadores explícitos (`[a definir]`, `[validar]`, `TODO`, `???`, `<...>`) e lacunas implícitas (um arquivo referencia algo — produto, preço, decisão — que nenhum arquivo descreve). O `CLAUDE.md` lista arquivos que não existem? Um arquivo existe mas não está listado no `CLAUDE.md`?
3. **Genérico.** Aplicar o teste da aula frase a frase: *"isso soa como esta empresa específica, ou como qualquer empresa?"* Sinalizar frases que serviriam para qualquer negócio ("melhore o relacionamento com clientes", "invista em marketing", "soluções inovadoras"). Contexto bom nomeia produto, cliente e restrição.
4. **Possível invenção.** Números, datas, nomes de clientes, métricas e afirmações factuais que aparecem sem origem. Não apagar nem corrigir sozinho — listar e pedir confirmação do dono.

**Saída:** uma tabela por passagem, com `arquivo:linha`, o trecho, e a ação sugerida. Fechar com um placar (`X contradições · Y lacunas · Z genéricos · W a confirmar`) e a pergunta: quer que eu corrija os itens que dá pra corrigir sem inventar?

Nunca preencher lacuna nem "consertar" invenção por conta própria. Corrigir só o que é derivável do próprio contexto (ex.: alinhar o `CLAUDE.md` à lista real de arquivos) e sempre reportar o que mudou.

## Modo 2 — Rodar a prova

Objetivo: medir se o contexto está funcionando, do jeito que a aula faz no Passo 4.

1. Escolher (ou pedir) uma pergunta genérica de negócio. Default: *"Me sugere três formas de aumentar a receita da minha empresa este trimestre."*
2. Responder a pergunta **usando exclusivamente** os arquivos de contexto lidos — nada de conhecimento externo sobre o setor.
3. Avaliar a própria resposta contra três critérios, em tabela "genérico vs. com contexto":
   - **Nomeia o produto?** (o nome real, não "o serviço")
   - **Nomeia o cliente / o gargalo?** (o ICP real, não "os clientes")
   - **Respeita a restrição?** (o que o negócio decidiu não fazer)
4. Veredito:
   - Passou → a resposta serve só para esta empresa. Dizer isso.
   - Genérico → **isso é o mapa do que falta no contexto.** Apontar qual arquivo deveria conter a informação ausente e sugerir acrescentá-la lá.

A prova nunca "aprova" sozinha uma mudança no contexto — ela aponta a lacuna; quem escreve o arquivo é o dono.

## Modo 3 — Checar uma entrega

Objetivo: conferir se um artefato (proposta, LP, copy, e-mail, plano, roteiro) bate com o contexto e não viola decisão fechada.

1. Receber o texto da entrega (o usuário cola ou aponta o arquivo).
2. Cruzar contra os arquivos de contexto em três eixos:
   - **Fidelidade factual.** A entrega afirma algo (produto, preço, público, número) que contradiz o contexto? Marcar cada divergência com `entrega ↔ arquivo:linha`.
   - **Decisões.** Ler `decisoes.md` (se existir) e checar se a entrega reabre ou viola alguma decisão marcada como fechada. Ex.: propõe um closer quando a decisão é só SDR.
   - **Voz.** Se existir `voz.md`, checar tom: a entrega soa como a empresa fala, ou caiu no genérico/corporativês que o `voz.md` proíbe?
3. **Saída:** lista de divergências em ordem de gravidade (viola decisão > contradiz fato > destoa da voz), cada uma com a citação dos dois lados e a correção sugerida. Se estiver tudo alinhado, dizer isso explicitamente — sem inventar problema para parecer útil.

## Escolha de modo

- Pedido menciona "audita", "confere o contexto", "tem furo?" → Modo 1.
- Pedido menciona "roda a prova", "isso ainda sai genérico?" → Modo 2.
- Pedido traz um texto/arquivo para conferir, ou "isso bate com o contexto?" → Modo 3.
- Ambíguo → perguntar qual dos três, em uma linha. Não rodar os três sem necessidade.

## Regras invioláveis

- Ler o arquivo inteiro antes de julgá-lo. Auditoria por amostragem é pior que nenhuma.
- Nunca inventar para preencher lacuna. Marcar e perguntar.
- Corrigir apenas o derivável; reportar toda mudança.
- Responder no idioma do projeto (PT-BR por padrão aqui).
