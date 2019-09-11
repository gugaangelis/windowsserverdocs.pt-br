---
title: Guia de estilo de texto e o design de IU de Windows Admin Center
description: SDK da interface do usuário do centro de administração do Windows e guia de estilo de design
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 229a911039ba88847de42e542f47b344d7a032c2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869602"
---
# <a name="windows-admin-center-ui-text-and-design-style-guide"></a>Guia de estilo de texto e o design de IU de Windows Admin Center

>Aplica-se a: Windows Admin Center

Este tópico descreve a abordagem geral para escrita de texto (UI) da interface de usuário para o Windows Admin Center, bem como alguns convenções específicas e abordagens que estamos aproveitando.

Windows Admin Center e qualquer extensão deve seguir [princípios de voz da Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) para que a experiência seja fácil de usar e amigável. Este guia de estilo baseia-se nestes princípios de voz, bem como o [Guia de estilo de escrita Microsoft](https://docs.microsoft.com/style-guide/welcome/), portanto, certifique-se de fazer check-out de ambos desses recursos para informações sobre coisas como [acessibilidade](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [acrônimos](https://docs.microsoft.com/style-guide/acronyms)e [Escolha do word](https://docs.microsoft.com/style-guide/word-choice/) , como [ Por favor,](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please)e [Desculpe](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## <a name="buttons"></a>Botões

- Botões devem ser uma palavra sempre que possível, especialmente se você planeja localizar sua ferramenta. Dois ou três estão OK, mas tente evitar mais tempo. Se você tiver quatro palavras ou mais, seria melhor usar um controle de link.
- Rótulos de botão devem ser conciso, específicos e auto-explicativos. Em vez de um botão de "Enviar" genérico, use um verbo correspondente à ação de usuário, como "Criar", "Excluir", "Adicionar", "Formatação" etc.
- Se um botão segue uma pergunta, seu rótulo deve corresponder claramente para a pergunta (geralmente "Yes" ou "Não").

## <a name="capitalization"></a>Uso de maiúsculas

Podemos siga o estilo da Microsoft para [capitalização](https://docs.microsoft.com/style-guide/capitalization) - capitalização do estilo sentença de uso para praticamente tudo.

| Elemento de interface do usuário              |Uso de maiúsculas|Comentários|
|-------------------------|--------------|--------|
|Selos (por exemplo, VISUALIZAÇÃO) |Todo em maiúsculas      ||
|Todo o resto          |Estilo de frase|No entanto, há algumas exceções onde podemos desvendar propriedades do objeto do PowerShell que está fora do nosso controle ou de WMI.|

## <a name="colons"></a>Dois-pontos

Use dois-pontos para introduzir listas. Por exemplo:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

Não use dois-pontos no texto da interface do usuário quando um rótulo estiver em uma linha diferente da coisa que ele rotula ou quando houver uma clara distinção entre o rótulo e o que estiver rotulando.

Use dois-pontos no texto da interface do usuário quando um rótulo estiver na mesma linha que o texto que ele rotula e você precisar manter os dois elementos a serem executados juntos.

## <a name="confirmation-messages"></a>Mensagens de confirmação

Caixas de diálogo de confirmação são úteis ao continuar podem ter resultados inesperados, como perda de dados. Eles devem conter informações úteis e verificáveis com um resultado claro, especialmente para eventos que não podem ser revertidos. 

- Certifique-se de que uma confirmação seja necessária. Se não houver nenhuma informação nova a oferecer (por exemplo, "tem certeza?"), uma mensagem de confirmação poderá não ser necessária.  
- Verifique se o cliente deseja prosseguir com a ação.
- Verifique se a instrução principal (título) e o texto explicativo (corpo) não são redundantes.
- No título, defina os resultados possíveis como uma pergunta ou uma instrução sobre o que acontecerá em seguida. Por exemplo, "apagar todos os dados nesta unidade? ou "você está prestes a apagar todos os seus dados".
- Adicione detalhes no corpo. Se houver uma variável, como o nome do item que você está prestes a alterar, inclua-a aqui.
- Inclua uma pergunta simples (no cabeçalho ou no corpo) que marque uma opção clara entre dois botões de ação.
- Para uma escolha complexa, use os botões Sim/Não, que incentivam a leitura cuidadosa. Para uma opção mais simples, use botões específicos para a ação, como excluir tudo ou cancelar.

## <a name="first-run-experiences"></a>Experiências de primeira execução

Na primeira vez que um usuário visita uma página, você tem uma oportunidade para ajudá-los a começar a usar a ferramenta. Isso pode ser:

- Uma sequência de texto em uma página vazia com curtas instruções sobre como começar , por exemplo, "Selecione Adicionar para adicionar um aplicativo."
- Um link para o controle que obtém o usuário iniciado, por exemplo, "Adicione um aplicativo para começar."
- Um pequena e curta animação ou vídeo mostrando o usuário como começar

Aqui estão algumas dicas de nosso guia de estilo do Windows:

### <a name="1-be-helpful"></a>1. Ofereça ajuda

- Evite uma linguagem e estilo de marketing.
- Ao demonstrar ou sugerir algo, verifique se o resultado final está claro; mostrar ao cliente como fazer algo não será eficaz se não souber por que eles estão fazendo isso.
- Não apresente dicas se o cliente não precisar delas.

### <a name="2-show-dont-tell"></a>2. Mostrar, não diga

Mantenha o texto simples possível (pense em animações pequenas ou vídeos).

### <a name="3-dont-overwhelm"></a>3. Não sobrecarregar

- Limitar pop-ups e dicas para 4 por sessão de uso combinados — incluindo notificações do sistema e notificações do shell.
- Verifique se que o tempo de pop-ups é útil.
- Não impeça que o cliente faça algo.
- Certifique-se de que os pop-ups pode ser descartados facilmente.

### <a name="4-keep-it-contextual"></a>4. Mantenha-o contextual

- Momentos de ensino são mais eficazes quando apresentados no momento certo.
- Se você criar tutoriais ou apresentações de slides, mantenha as informações concretas.
- Não use marketing "complicado": concentre-se em dicas e truques específicos.
- Forneça uma maneira para que os clientes retornem ao tutorial mais tarde, se relevante (as pessoas geralmente não retêm informações na primeira vez, mas as instruções de instalação podem ser relevantes apenas uma vez).
- Mensagens vazias são um lugar natural para aprendizado e/ou felicidade: mantenha tudo simples e informativo.

### <a name="5-minimize-painful-setup"></a>5. Minimizar a configuração de difícil

Quando você precisar que o cliente execute outra ação para experimentar o valor completo (entrar em um serviço online etc.), torne o processo o mais simples possível.

- As mensagens devem ser curtas e diretas.
- Evite assustá-los. Se possível, forneça um meio para se conectar a partir de onde estejam.
- Se possível, habilite a opção de execução posterior e, em seguida, lembre-os para executá-la posteriormente.
- Caso retire-os da experiência, forneça uma maneira de voltar de forma rápida e fácil.

## <a name="help-links"></a>Links de ajuda

Aqui estão algumas dicas de nosso guia de estilo do Windows:

### <a name="when-should-we-provide-a-help-link"></a>Quando devemos fornecer um link de ajuda?

Quase nunca. Forneça um link de ajuda somente quando:

- Há uma pergunta óbvia e importante que os clientes provavelmente terão enquanto estão na interface do usuário a resposta para a qual os ajudarão a obter sucesso na tarefa de interface do usuário. 
- Não há espaço suficiente na interface do usuário para fornecer a quantidade de informações necessárias para que os usuários tenham sucesso na tarefa de interface do usuário. 

### <a name="where-should-help-links-appear"></a>Onde os links de ajuda devem ser exibidos? 

- Os links de texto devem parecer próximos do elemento de interface do usuário para o qual a ajuda é direcionada o mais próximo possível. 
- Se você precisar fornecer um link de texto que se aplica a uma tela inteira da interface do usuário, coloque-o na parte inferior esquerda da tela. 
- Se você fornecer um link por meio de um botão de ajuda (?), a dica de ferramenta deverá ser "ajuda".

### <a name="what-url-should-we-use"></a>Qual URL devemos usar?

Nunca vincular diretamente a um endereço da Web — em vez disso, use um serviço de redirecionamento.

Os desenvolvedores da Microsoft devem usar um FWLink, exceto quando é um link de ajuda que os usuários podem precisar digitar manualmente. nesse caso, use um link aka.ms (desde que o destino da URL seja um site que reconheça automaticamente a localidade do navegador, como Docs.microsoft.com)

### <a name="text-guidelines"></a>Diretrizes de texto 

- Use frases completas.
- Não inclua Pontuação final, exceto pontos de interrogação. 
- Você não precisa usar o mesmo texto que o título da tarefa; Use o texto que faz sentido no contexto da interface do usuário, mas verifique se há uma conexão lógica entre os dois. Por exemplo: 
- Link de ajuda: Quais são os riscos de permitir exceções? 
- Título do tópico da ajuda: "Permitindo que um programa se comunique por meio do firewall do Windows"
- Seja o mais específico possível sobre o conteúdo do tópico da ajuda. 
    - Nosso estilo
        - Como o Firewall do Windows ajuda a proteger meu computador?
        - Por que os destaques podem melhorar uma imagem
    - Não é nosso estilo
        - Mais informações sobre o Firewall do Windows
        - Saiba mais sobre o gerenciamento de cores
        - Saiba mais
- Use a frase inteira para o texto do link, não apenas as palavras-chave. 
    - Nosso estilo 
        - [Quais são os riscos de permitir exceções?]()
    - Não é nosso estilo
        - Quais são os [riscos de permitir exceções]()? 

## <a name="error-messages"></a>Mensagens de erro

Aqui estão algumas diretrizes adaptadas do guia de estilo do Windows:

Escrever uma mensagem boa é um equilíbrio entre fornecer uma explicação suficiente, mas não ser muito técnico; entre serem casual e personáveis, mas não irritantes ou ofensivas.

### <a name="general-guidelines"></a>Diretrizes gerais

Use uma mensagem por caso de erro.

#### <a name="headings"></a>Títulos

- Mantenha-o curto e explique concisamente qual é o problema ou **o que fazer**. <br>Algumas superfícies da interface do usuário podem ter títulos que se truncam em vez de serem encapsulados quando forem muito longos, portanto, fique atento para eles.
- Use a solução no título se for uma etapa simples.
- Verifique se o título está relacionado diretamente ao botão, caso o leitor ignore o texto do corpo.
- Evite usar "houve um problema" nos títulos, a menos que você não tenha outra opção. Seja mais específico sobre o problema.
- Evite usar variáveis (como nomes de arquivos, pastas e aplicativos) em títulos. Coloque-os no corpo.

#### <a name="body"></a>Body

- Se o título estiver suficientemente explicando o problema ou a solução, você não precisará do corpo do texto.
- Não repita o título na mensagem com uma palavra ligeiramente diferente.
- Comunique-se de forma clara e concisa o que é a solução.
- Concentre-se primeiro em dar os fatos.
- Não culpa os usuários pelo erro.
- Se houver um código de erro associado ao erro e se você considerar que a inclusão do código de erro pode ajudar o cliente ou o suporte da Microsoft a pesquisar o problema, inclua-o diretamente abaixo do corpo de texto e escreva-o da seguinte maneira:

    Código de erro: ####

    Se o cliente tiver todas as informações necessárias para resolver o erro sem o código, você não precisará incluí-lo.

#### <a name="buttons"></a>Botões

- Escreva o texto do botão para que seja uma resposta específica para a instrução principal. Se isso não for possível, use "Close" para o texto do botão ignorado (em vez de "OK" ou "concluído").
- Se você tiver mais de um botão, torne o botão mais à esquerda a ação que o usuário é incentivado a executar. Torne o botão mais à direita a ação mais conservadora, como "Cancelar".

#### <a name="help-links"></a>Links de ajuda

Considere apenas links de ajuda para mensagens de erro que você não pode tornar específicas e acionáveis.

## <a name="null-state-text"></a>Texto de estado nulo

Aqui está uma ajuda do guia de estilo do Windows.

O estado nulo ocorre quando os dados ou o conteúdo do cliente estão ausentes de um aplicativo ou recurso, quando nenhum resultado é retornado após uma pesquisa, ou quando as informações necessárias estão ausentes em um formulário, como informações de cobrança para uma transação.

### <a name="guidelines"></a>Diretrizes

- Se possível, use situações de estado nulo como uma oportunidade para instruir as pessoas sobre como usar o recurso (por exemplo, como adicionar música, onde encontrar imagens etc.)  
  - Se você tiver um título em sua interface do usuário, explique a ação a ser tomada para "corrigir" o estado nulo (por exemplo, "adicionar algumas músicas") 
  - Divirta-se com o texto. Esse espaço pode ser uma oportunidade para fornecer fascinam, pois provavelmente não será visto várias vezes. 
  - Evite "está aqui aqui". Isso é triste e foi superutilizado. 
  - Evite perguntas como "não conectou sua impressora?" É bom usar uma vez, mas esse formato tende a ser utilizado, e as perguntas colocam um fardo/pressão extra no cliente. Isso também pode parecer decrescente. 
  - A variedade no texto de estado nulo é algo bom. 

### <a name="examples"></a>Exemplos

- "Adicionar alguém como um favorito e você os verá aqui."
- "Obteve qualquer medalha ou clipes de jogos dos quais você está particularmente orgulhoso? Adicione-os à sua demonstração. "
- "Ainda não há ninguém em uma parte. Inicie um! "
- "Quando alguém adicioná-lo como amigo, você os verá aqui."
- "Quando você faz coisas como desbloquear realizações, gravar clipes de jogos e adicionar amigos, você verá tudo aqui."
- "Seus amigos favoritos aparecerão aqui, para que você possa ver quando eles estão online e o que eles estão."

## <a name="punctuation"></a>Pontuação

- Sem pontuação final (pontos e interrogação) para títulos ou frases incompletas. Uma exceção é uma caixa de diálogo de confirmação onde o título faz a pergunta
- Use as instruções do guia de estilo doa Microsoft sobre [pontos](https://docs.microsoft.com/style-guide/punctuation/periods) e [pontos de interrogação](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## <a name="status-messages"></a>Mensagens de status

Mensagens de status consistem em notificações e mensagens de janela pop-up (proposta).

|Tipo de cadeia de caracteres         | Observações                               |
|------------        |-------------------------------------|
|Notificação do sistema               |Frases com a mensagem de pontuação - idealmente com uma variável de objeto para que os usuários podem entender qual o objeto de final se aplica ao caso eles tenham navegado para longe do objeto|
|Título de notificação|Frases w/out pontuação final (Este é um título) - idealmente com uma variável de objeto|
|Detalhes da notificação|Sentenças completos, idealmente com um link para a interface do usuário que exibe o objeto|

Aqui estão algumas recomendações detalhadas para mensagens de notificação:

|Tipo de cadeia de caracteres         | Observações                               |
|------------        |-------------------------------------|
|Iniciado             |Omita quando possível: geralmente pule apenas a mensagem em andamento para minimizar o número de distrações.|
|Em andamento         |Comece com o verbo da ação que você estiver realizando e terminarem com reticências para indicar uma operação em andamento. Veja um exemplo:<br> *Criando o volume "dados do cliente"...*|
|Êxito             |Comece com "Com êxito" e termine com o software que acabamos de fazer. Veja um exemplo:<br> *O volume "dados do cliente" foi criado com êxito.*|
|Falha             |Comece com "Não pôde" e termine com o que o software não conseguiu realizar. Veja um exemplo:<br> *Não foi possível criar o volume "dados do cliente".*|

## <a name="tooltips"></a>Dicas de ferramenta

Boas dicas de ferramenta descrevem brevemente os controles sem rótulo ou fornecem um pouco de informações adicionais para controles rotulados, quando isso é útil. Eles também podem ajudar os clientes a navegar pela interface do usuário oferecendo informações adicionais – e não redundantes, sobre rótulos de controle, ícones, links, etc.

As dicas de ferramenta devem ser usadas com moderação ou não. Eles podem ser uma interrupção para o cliente, portanto, não inclua uma dica de ferramenta que simplesmente repita um rótulo ou declare o óbvio. Ele sempre deve adicionar informações valiosas.

|    Contexto                                 |    Como escrever as dicas de ferramenta    |
|    -----------------------                 |    -------------------------    |
|Quando um controle ou elemento de interface do usuário não está rotulado...|Use uma frase de substantivo simples e descritiva. Por exemplo:<br> Realçando a caneta |
|Quando um elemento de interface do usuário é rotulado, mas sua finalidade precisa de esclarecimento...|<ul><li>Descreva brevemente o que você pode fazer com esse elemento de interface do usuário. </li><li>Use o formulário verbo imperativo. Por exemplo, "localizar texto neste arquivo" (não "localiza texto neste arquivo").</li><li>Não inclua Pontuação final, a menos que haja várias frases completas.</li> </ul>|
|Quando um rótulo de texto é truncado ou provavelmente truncado em alguns idiomas...|<ul><li>Forneça o rótulo não truncado na dica de ferramenta.</li><li>Opcional: Em outra linha, forneça uma descrição de esclarecimento, mas somente se necessário.</li><li>Não forneça uma dica de ferramenta se as informações não truncadas forem fornecidas em outro lugar na página ou no fluxo.</li></ul>|
|Se um atalho de teclado estiver disponível...|<ul><li>Opcional: Forneça o atalho de teclado entre parênteses após o rótulo ou frase descritiva, por exemplo, "Imprimir (Ctrl + P)" ou "localizar texto neste arquivo (Ctrl + F)"</li><li>Não há problema em Adicionar um atalho de teclado útil a uma dica de ferramenta de esclarecimento, mas evite adicionar uma dica de ferramenta somente para mostrar um atalho de teclado. </li></ul>|