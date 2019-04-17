---
title: Guia de estilo de texto e o design de IU de Windows Admin Center
description: Interface do usuário do Windows Admin Center texto e design guia de estilo SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab5bee55975b803a77db0b6cdb179b76590e1d83
ms.sourcegitcommit: 56cccb09a35b2d3eef9056e2406c3a1762d28682
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4739976"
---
# Guia de estilo de texto e o design de IU de Windows Admin Center

>Aplicável a: Windows Admin Center

Este tópico descreve a abordagem geral para escrita de texto (UI) da interface de usuário para o Windows Admin Center, bem como alguns convenções específicas e abordagens que estamos aproveitando.

Windows Admin Center e qualquer extensão deve seguir [princípios de voz da Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) para que a experiência seja fácil de usar e amigável. Este guia de estilo baseia-se nestes princípios de voz, bem como o [Guia de estilo de escrita Microsoft](https://docs.microsoft.com/style-guide/welcome/), portanto, certifique-se de fazer check-out de ambos desses recursos para informações sobre coisas como [acessibilidade](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [acrônimos](https://docs.microsoft.com/style-guide/acronyms)e [Escolha do word](https://docs.microsoft.com/style-guide/word-choice/) , como [ Por favor,](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please)e [Desculpe](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## Botões

- Botões devem ser uma palavra sempre que possível, especialmente se você planeja localizar sua ferramenta. Dois ou três são Okey, mas evite mais. Se você tiver quatro palavras ou mais tempo, seria melhor usar um controle de link.
- Rótulos de botão devem ser conciso, específicos e auto-explicativos. Em vez de um botão de "Enviar" genérico, use um verbo correspondente à ação de usuário, como "Criar", "Excluir", "Adicionar", "Formatação" etc.
- Se um botão segue uma pergunta, seu rótulo deve corresponder claramente para a pergunta (geralmente "Yes" ou "Não").

## Uso de maiúsculas

Podemos siga o estilo da Microsoft para [capitalização](https://docs.microsoft.com/style-guide/capitalization) - capitalização do estilo sentença de uso para praticamente tudo.

| Elemento de interface do usuário              |Uso de maiúsculas|Comentários|
|-------------------------|--------------|--------|
|Selos (por exemplo, VISUALIZAÇÃO) |Todo em maiúsculas      ||
|Todo o resto          |Estilo de frase|No entanto, há algumas exceções onde podemos desvendar propriedades do objeto do PowerShell que está fora do nosso controle ou de WMI.|

## Dois-pontos

Use vírgulas para apresentar listas. Por exemplo:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

Não use vírgulas no texto de interface do usuário quando um rótulo fica em um diferente de linha da coisa que ele rótulos ou quando há uma distinção clara entre o rótulo e o que ele está rotulando.

Use dois-pontos no texto de interface do usuário quando um rótulo está na mesma linha como o texto ele rótulos e você precisa impedir que os dois elementos em execução juntos.

## Mensagens de confirmação

Caixas de diálogo de confirmação são úteis quando continuando pode ter resultados inesperados, como perda de dados. Eles devem conter informações úteis destacadas com um resultado clara, especialmente para eventos que não pode ser revertida. 

- Certifique-se de que uma confirmação é necessária. Se não houver nenhuma informação nova oferecer (por exemplo, "tem certeza?"), uma mensagem de confirmação não poderão ser necessária.  
- Verifique se que o cliente quer continuar com a ação.
- Verifique se a instrução principal (título) e texto de explicação (corpo) não são redundantes.
- No título, defina os possíveis resultados como uma pergunta ou uma instrução sobre o que acontecerá em seguida. Por exemplo, "apagar todos os dados nessa unidade? ou "Você está prestes a apagar todos os seus dados".
- Adicione detalhes no corpo. Se houver uma variável, como o nome do item que estiver sobre alteração, incluí-lo aqui.
- Inclua uma pergunta simples (seja no cabeçalho ou no corpo) que quadros uma opção clara entre os dois botões de ação.
- Para uma opção complexa, use o Sim/não botões, que incentivar leitura cuidadosa. Para uma opção mais simples, use botões que são específicas para a ação, como excluir todos ou Cancelar.

## Experiências de primeira execução

Na primeira vez que um usuário visita uma página, você tem uma oportunidade para ajudá-los a começar a usar a ferramenta. Isso pode ser:

- Uma sequência de texto em uma página vazia com curtas instruções sobre como começar , por exemplo, "Selecione Adicionar para adicionar um aplicativo."
- Um link para o controle que obtém o usuário iniciado, por exemplo, "Adicione um aplicativo para começar."
- Um pequena e curta animação ou vídeo mostrando o usuário como começar

Aqui estão algumas dicas de nosso guia de estilo do Windows:

### 1. Seja solicito

- Evite uma linguagem e estilo de marketing.
- Quando você demonstrar ou sugerir algo, verifique se o resultado final está claro; apenas mostrar para o cliente como fazer algo não é uma maneira eficaz caso não saibam o que estão fazendo.
- Não apresente dicas se o cliente não precisar delas.

### 2. Mostre, não fale

Mantenha o texto simples possível (pense em animações pequenas ou vídeos).

### 3. Não sobrecarregue

- Limitar pop-ups e dicas para 4 por sessão de uso combinados — incluindo notificações do sistema e notificações do shell.
- Verifique se que o tempo de pop-ups é útil.
- Não impede que o cliente fazendo algo.
- Certifique-se de que os pop-ups pode ser descartados facilmente.

### 4. Mantenha tudo contextual

- Momentos de ensino são mais eficazes quando apresentados no momento certo.
- Se você criar tutoriais ou apresentações de slides, mantenha as informações concretas.
- Não use marketing "complicado": concentre-se em dicas e truques específicos.
- Forneça uma maneira dos clientes retornarem ao tutorial depois, caso seja relevante (pessoas geralmente não retêm as informações na primeira vez, mas as instruções de configuração podem ser mais relevantes somente uma vez).
- Mensagens vazias são um lugar natural para aprendizado e/ou felicidade: mantenha tudo simples e informativo.

### 5. Minimizar instalações desagradáveis

Quando você precisar que o cliente execute outra ação para experimentar o valor completo (entrar em um serviço online etc.), torne o processo o mais simples possível.

- As mensagens devem ser curtas e diretas.
- Evite assustá-los. Se possível, forneça um meio para se conectar a partir de onde estejam.
- Se possível, habilite a opção de execução posterior e, em seguida, lembre-os para executá-la posteriormente.
- Caso retire-os da experiência, forneça uma maneira de voltar de forma rápida e fácil.

## Links da Ajuda

Aqui estão algumas dicas de nosso guia de estilo do Windows:

### Quando deve fornecemos um link de ajuda?

Quase nunca. Fornecer um link de Ajuda somente quando:

- Não há uma pergunta óbvia e importante que os clientes têm probabilidade de ter enquanto estiverem na interface do usuário a resposta ao qual o ajudará a ter sido bem-sucedida na tarefa de interface do usuário. 
- Não há espaço suficiente na interface do usuário para fornecer a quantidade de informações necessárias para que os usuários bem-sucedidas na tarefa de interface do usuário. 

### Onde deve ajudar links aparecem? 

- Links de texto deve aparecer mais próximo o elemento de interface do usuário que a Ajuda seja direcionada para possível. 
- Se você deve fornecer um link de texto que se aplica a uma tela de interface do usuário inteira, coloque-o na parte inferior esquerda da tela. 
- Se você fornecer um link por meio de um botão de Ajuda (?), a dica de ferramenta deve ser "Ajuda".

### Qual URL podemos usar?

Nunca se vincular diretamente a um endereço da web — em vez disso, use um serviço de redirecionamento.

Os desenvolvedores da Microsoft devem usar um FWLink exceto quando for um link de Ajuda que os usuários podem precisar digitar manualmente, que usam caso um link aka.MS/Project-Rome (desde que o destino da URL é um site que reconhece automaticamente a localidade do navegador, como Docs.microsoft.com)

### Diretrizes de texto 

- Use sentenças completos.
- Não inclua pontuação, exceto para pontos de interrogação final. 
- Você não precisa usar o mesmo texto como o título da tarefa; usar o texto que faz sentido no contexto da interface do usuário, mas certifique-se de que há uma conexão lógica entre os dois. Por exemplo: 
- Link de Ajuda: quais são os riscos de permitir exceções? 
- Título do tópico de Ajuda: "Permitir que um programa se comunique através do Firewall do Windows"
- Seja o mais específico possível sobre o conteúdo do tópico da Ajuda. 
    - Nosso estilo
        - Como o Firewall do Windows ajuda a proteger meu computador?
        - Por que destaques podem melhorar a uma imagem
    - Não nosso estilo
        - Para obter mais informações sobre o Firewall do Windows
        - Saiba mais sobre o gerenciamento de cores
        - Saiba mais
- Use a frase inteira para o texto do link, não apenas as palavras-chave. 
    - Nosso estilo 
        - [Quais são os riscos de permitir exceções?]()
    - Não nosso estilo
        - Quais são os [riscos de permitir exceções]()? 

## Mensagens de erro

Aqui estão algumas diretrizes adaptado do guia de estilo do Windows:

Escrever uma boa mensagem é um equilíbrio entre fornecendo suficiente explicação, mas não estão sendo excessivamente técnico; entre sendo casual e forma agradável, mas não irritante ou ofensivas.

### Diretrizes gerais

Use uma mensagem por caso de erro.

#### Títulos

- Seja sucinto e explicar forma concisa, qual é o problema ou **o ideal é que fazer**. <br>Alguns superfícies de IU podem ter títulos que truncar em vez de encapsulamento quando eles são muito longos, portanto, preste atenção para eles.
- Use a solução no título se é uma etapa simples.
- Certifique-se de que o título se relaciona diretamente para o botão no caso do leitor ignora o corpo de texto.
- Evite usar "Houve um problema" nos cabeçalhos, a menos que você não tenha nenhuma outra opção. Ser mais específico sobre o problema.
- Evite o uso de variáveis (como nomes de arquivo, pasta e aplicativo) nos cabeçalhos. Colocá-los no corpo.

#### Body

- Se o título suficientemente explica o problema ou a solução, você não precisa de corpo de texto.
- Não repita o título da mensagem com palavras ligeiramente diferentes.
- Informar claramente e forma concisa que a solução é.
- Focalizar dando fatos pela primeira vez.
- Não culpe usuários para o erro.
- Se houver um código de erro associado ao erro e se você acha que incluindo o código de erro pode ajudar o cliente ou o suporte da Microsoft para pesquisar o problema, incluí-lo diretamente abaixo do texto do corpo e gravá-lo da seguinte maneira:

    Código de erro: # # #

    Se o cliente tiver todas as informações necessárias para resolver o erro sem o código, você não precisará incluí-lo.

#### Botões

- Escreva texto do botão para que seja uma resposta específica à instrução principal. Se isso não for possível, use "Fechar" para o texto do botão de descarte (em vez de "Okey" ou "Feito").
- Se você tiver mais de um botão, torne o botão mais à esquerda a ação que o usuário é incentivado a fazer. Tornar o botão mais à direita a ação mais conservador, como "Cancelar".

#### Links da Ajuda

Considere somente links de ajuda para mensagens de erro que você não pode fazer específicos e acionáveis.

## Texto de estado nulo

Veja alguns Ajuda do guia de estilo do Windows.

Estado nulo ocorre quando o conteúdo ou dados do cliente está ausente de um aplicativo ou recurso, quando nenhum resultado for retornado após uma pesquisa ou quando for necessário faltam informações de um formulário, como informações de uma transação de cobrança.

### Diretrizes

 - Se possível, use situações de estado nulo como uma oportunidade para ensinar as pessoas sobre como usar o recurso (por exemplo, como adicionar música, onde a localizar as imagens, etc.)  
- Se você tiver um título da interface do usuário, explique a ação para tirar o estado nulo (por exemplo, "adicionar alguns música") "corrigir" 
- Divirta-se com o texto. Nesse espaço pode ser uma oportunidade de fornecer satisfação, já que ele provavelmente não aparecerão várias vezes. 
- Evite "É solitárias aqui". Isso é o último e tem sido usada em excesso. 
- Evite perguntas como "Ainda não conectado sua impressora?" Okey para usar uma vez, mas esse formato tende a obter usada em excesso e perguntas pressionam extra sobrecarga/cliente. Ele também pode se sentir condescending. 
- Variedade de texto de estado nulo é bom. 

### Exemplos

- "Adicionar alguém como favorita, e você verá-los aqui."
- "Você tem qualquer conquistas ou clipes de jogos que você está especialmente orgulhosos do? Adicioná-los para seu showcase."
- Do "ninguém em um terceiro ainda. Inicie um!"
- "Quando alguém o adiciona como um amigo, você poderá vê-los aqui."
- "Quando você colocar como desbloquear conquistas, gravar clipes de jogos e adicionar amigos, você verá tudo aqui."
- "Seus amigos Favoritos aparecerá aqui, para que você possa ver quando eles estiverem online e o que elas até."

## Pontuação

- Sem pontuação final (pontos e interrogação) para títulos ou frases incompletas. Uma exceção é uma caixa de diálogo de confirmação onde o título faz a pergunta
- Use as instruções do guia de estilo doa Microsoft sobre [pontos](https://docs.microsoft.com/style-guide/punctuation/periods) e [pontos de interrogação](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## Mensagens de status

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
|Em andamento         |Comece com o verbo da ação que você estiver realizando e terminarem com reticências para indicar uma operação em andamento. Aqui está um exemplo:<br> *Criando o volume de "Dados do cliente"...*|
|Sucesso             |Comece com "Com êxito" e termine com o software que acabamos de fazer. Veja um exemplo:<br> *Criado com êxito o volume "Dados de cliente".*|
|Falha             |Comece com "Não pôde" e termine com o que o software não conseguiu realizar. Veja um exemplo:<br> *Não foi possível criar o volume "Dados de cliente".*|

## Dicas de ferramenta

Dicas de ferramentas boa brevemente descrevem os controles sem rótulo ou fornecem um pouco de informações adicionais para controles rotulados, quando isso é útil. Eles também podem ajudar os clientes navegar a interface do usuário, oferecendo adicionais — não redundantes — informações sobre rótulos de controle, ícones, links, etc.

Dicas de ferramenta devem ser usadas com moderação ou não. Elas podem ser uma interrupção para o cliente, portanto, não inclua uma dica de ferramenta que simplesmente se repete um rótulo ou informando o óbvio. Ele deve sempre adicionar informações importantes.

|    Contexto                                 |    Como gravar as dicas de ferramentas    |
|    -----------------------                 |    -------------------------    |
|Quando um controle ou elemento de interface do usuário está sem rótulo...|Use uma frase nominal simples e descritivas. Por exemplo:<br> Realce de caneta |
|Quando um elemento de interface do usuário é identificado, mas sua finalidade precisa esclarecimento...|<ul><li>Descreva brevemente que você pode fazer com esse elemento de interface do usuário. </li><li>Use o formulário de verbo imperativo. Por exemplo, o "localizar texto nesse arquivo" (não "localiza o texto nesse arquivo").</li><li>Não inclua pontuação final, a menos que haja várias sentenças completas.</li> </ul>|
|Quando um rótulo de texto estiver truncado ou provavelmente truncar em alguns idiomas...|<ul><li>Fornece o rótulo untruncated na dica de ferramenta.</li><li>Opcional: Em outra linha, forneça uma descrição esclarecer, mas somente se necessário.</li><li>Não fornece uma dica de ferramenta se as informações untruncated é fornecida em outro lugar na página ou fluxo.</li></ul>|
|Se um atalho de teclado está disponível...|<ul><li>Opcional: Fornecer o atalho de teclado entre parênteses após o rótulo ou frase descritiva, por exemplo, "Imprimir (Ctrl + P)" ou "Localizar texto nesse arquivo (Ctrl + F)"</li><li>Ele é Okey para adicionar um atalho de teclado úteis para uma dica de ferramenta esclarecer, mas evite adicionar uma dica de ferramenta somente para mostrar um atalho de teclado. </li></ul>|