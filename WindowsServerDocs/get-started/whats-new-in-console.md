---
title: Novidades no Console do Windows no Windows Server 2016
description: Lista os novos recursos importantes do console do Windows Server 2016.
ms.prod: windows-server
ms.technology: server-general
ms.date: 10/04/2016
ms.topic: article
ms.assetid: da9fc582-033b-4973-84e7-0c6024ecfcbc
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: b055c379e1d5ee632e420ffd1362389878d3dfd1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825959"
---
# <a name="whats-new-in-the-windows-console-in-windows-server-2016"></a>Novidades no Console do Windows no Windows Server 2016
>Aplica-se a: Windows Server 2016

O host de console (o código subjacente que dá suporte a todos os aplicativos de modo de caractere, incluindo o prompt de comando do Windows, o prompt do Windows PowerShell e outros) foi atualizado em várias maneiras para adicionar uma variedade de novos recursos.  

## <a name="controlling-the-new-features"></a>Controlar os novos recursos  
A nova funcionalidade está habilitada por padrão, mas você pode ativar e desativar cada um dos novos recursos ou reverter para o host de console anterior por meio da interface de propriedades (principalmente na guia **Opções**) ou com essas chaves do Registro (todas as chaves são valores DWORD em **HKEY_CURRENT_USER\Console**):  

|Chave do Registro|Descrição|  
|----------------|---------------|  
|ForceV2|1 habilita todos os novos recursos de console; 0 desabilita todos os novos recursos. Observação: esse valor não é armazenado em atalhos, apenas na chave do Registro.|  
|LineSelection|1 habilita a seleção de linha; 0 para usar somente o modo de bloqueio|  
|FilterOnPaste|1 habilita o novo comportamento de colagem|  
|LineWrap|1 quebra o texto ao redimensionar as janelas de console|  
|CtrlKeyShortcutsDisabled|0 habilita novos atalhos de tecla; 1 os desabilita|  
|ExtendedEdit Keys|1 habilita o conjunto completo de teclas de seleção de teclado; 0 o desabilita|  
|TrimLeadingZeros|1 corta zeros à esquerda em seleções feitas clicando duas vezes com o mouse; 0 mantém os zeros à esquerda|  
|WindowsAlpha|Define o valor de opacidade entre 30% e 100%. Use 0x4C até 0xFF ou 76 até 255 para especificar o valor|  
|WordDelimiters|Define o caractere usado para ignorar ao selecionar uma palavra inteira por vez no texto com CTRL+SHIFT+SETA (o padrão é o caractere de espaço). Defina esse valor REG_SZ para conter todos os caracteres que você deseja que sejam tratados como delimitadores. Observação: esse valor não é armazenado em atalhos, apenas na chave do Registro.|  

Essas configurações são armazenadas por cada título de janela no Registro em HKCU\Console. As janelas de console abertas por um atalho têm essas configurações armazenadas no atalho; se o atalho for copiado para outro computador, as configurações são movidas com ele para o novo computador. As configurações nos atalhos substituem todas as outras configurações, incluindo padrões e configurações globais. No entanto, se você reverter para o console original com **Usar console herdado** na guia **Opções**, essa configuração será global e será mantida para todas as janelas posteriormente, incluindo após reiniciar o computador.  

Você pode pré-configurar ou criar um script desses parâmetros ao configurar corretamente o registro em um arquivo autônomo ou com o Windows PowerShell.  

Aplicativos de NTVDM de 16 bits sempre revertem para o host de console antigo.  

> [!NOTE]  
> Se você encontrar problemas com as novas configurações do console e não puder resolvê-los com qualquer uma das opções específicas listadas aqui, sempre poderá reverter para o console original definindo ForceV2 como 0 ou com o controle **Usar console herdado** em **Opções**.  

## <a name="console-behavior"></a>Comportamento do console  
Agora você pode redimensionar a janela do console à vontade pegando uma borda com o mouse e arrastando. Barras de rolagem somente aparecem se você definir as dimensões da janela manualmente (usando a guia **Layout** em **Propriedades**) ou se a linha mais longa de texto no buffer for maior do que o tamanho da janela atual.  

A nova janela de console agora oferece suporte a quebra de texto. No entanto, se você usou APIs de console para alterar o texto em um buffer, o console deixará o texto como foi originalmente inserido.  

Janelas do console agora podem ser semitransparentes (para uma transparência mínima de 30%). Você pode ajustar a transparência no menu Propriedades ou com estes comandos de teclado:  

|Para fazer isso:|Use essa combinação de teclas:|  
|---------------|-----------------------------|  
|Aumentar a transparência|CTRL+SHIFT+sinal de mais (+) ou CTRL+SHIFT+mouse rola para cima|  
|Diminuir a transparência|CTRL+SHIFT+sinal de menos (+) ou CTRL+SHIFT+mouse rola para baixo|  
|Alternar o modo tela inteira|ALT+ENTER|  

## <a name="selection"></a>Seleção  
Há muitas opções novas para a seleção de texto e linhas, além de marcar texto e usar o histórico de buffer. O console tenta evitar conflitos com aplicativos que possam estar usando as mesmas teclas.  

**Para desenvolvedores**: se ocorrer um conflito, geralmente você poderá controlar o comportamento de uso da linha de entrada, a entrada processada e os modos de entrada de eco do aplicativo com a API SetConsoleMode(). Se você executar em modo de entrada processada, os atalhos abaixo se aplicarão, mas em outros modos, seu aplicativo deverá lidar com eles. As combinações de teclas não listadas aqui funcionam como nas versões anteriores do console. Você também pode tentar resolver conflitos com diversas configurações na guia **Opções**. Se tudo o mais falhar, você sempre poderá reverter para o console original.  

Agora você pode usar a seleção de clicar e arrastar fora do modo de Edição Rápida e isso poderá selecionar texto em linhas como no Bloco de Notas, em vez de apenas um bloco retangular. As operações de cópia não exigem mais remover quebras de linha. Além da seleção de clicar e arrastar, estas combinações de teclas estão disponíveis:  

**Seleção de texto**  

|Para fazer isso:|Use essa combinação de teclas:|  
|---------------|-----------------------------|  
|Mover o cursor para o caractere à esquerda, estendendo a seleção|SHIFT+SETA PARA ESQUERDA|  
|Mover o cursor para o caractere à direita, estendendo a seleção|SHIFT+SETA PARA DIREITA|  
|Selecionar o texto linha por linha para cima desde o ponto de inserção|SHIFT+SETA PARA CIMA|  
|Estender a seleção de texto uma linha para baixo do ponto de inserção|SHIFT+SETA PARA BAIXO|  
|Se o cursor estiver na linha que está sendo editada, use este comando uma vez para estender a seleção até o último caractere na linha de entrada. Use uma segunda vez para estender a seleção para a margem direita.|SHIFT+END|  
|Se o cursor **não** estiver na linha que está sendo editada, use este comando para selecionar todo o texto do ponto de inserção até a margem direita.|SHIFT+END|  
|Se o cursor estiver na linha que está sendo editada, use este comando uma vez para estender a seleção até o caractere imediatamente após o prompt de comando. Use uma segunda vez para estender a seleção para a margem direita.|SHIFT+HOME|  
|Se o cursor **não** estiver na linha que está sendo editada, use este comando para estender a seleção até a margem esquerda.|SHIFT+HOME|  
|Estender a seleção uma tela para baixo|SHIFT+PAGE DOWN|  
|Estender a seleção uma tela para cima|SHIFT+PAGE UP|  
|Estender a seleção uma palavra à direita (Você pode definir os delimitadores para palavra na chave do Registro WordDelimiters.)|CTRL+SHIFT+SETA PARA DIREITA|  
|Estender a seleção uma palavra à esquerda|CTRL+SHIFT+HOME|  
|Estender a seleção até o início do buffer da tela|CTRL+SHIFT+END|  
|Selecionar todo o texto após o prompt, se o cursor estiver na linha atual e a linha não estiver vazia|CTRL+A|  
|Selecionar todo o buffer, se o cursor **não** estiver na linha atual|CTRL+A|  

**Edição de texto**  

Você pode copiar e colar o texto no console usando os comandos do teclado. CTRL+C agora tem duas funções. Se nenhum texto estiver selecionado ao usá-lo, ele enviará o comando de QUEBRA como de costume. Se o texto estiver selecionado, o primeiro uso copia o texto e limpa a seleção; o segundo uso envia QUEBRA. Aqui estão os comandos de edição:  

|Para fazer isso:|Use essa combinação de teclas:|  
|---------------|-----------------------------|  
|Colar o texto na linha de comando|CTRL+V|  
|Copiar o texto selecionado para a área de transferência|CTRL+INS|  
|Copiar o texto selecionado para a área de transferência; enviar QUEBRA|CTRL+C|  
|Colar o texto na linha de comando|SHIFT+INS|  

**Modo de marca**  

Para entrar no modo de marca a qualquer momento, clique em qualquer lugar na barra de título do console, aponte para **Editar** e selecione **Marca** no menu exibido. Você também pode digitar CTRL+M. Enquanto estiver no modo de marca, use a tecla ALT para identificar o início de uma seleção de quebra automática de linha. (Se **Habilitar seleção de quebra automática de linha** estiver desabilitado, o modo de marca selecionará o texto em um bloco.) No modo de marca, CTRL+SHIFT+SETA seleciona um caractere por vez, e não a palavra inteira como no modo normal. Além das teclas de seleção na seção **Editar texto**, essas combinações estão disponíveis no modo de marca:  

|Para fazer isso:|Use essa combinação de teclas:|  
|---------------|-----------------------------|  
|Entrar no modo de marca para mover o cursor na janela|CTRL+M|  
|Começar a seleção de quebra automática de linha no modo de marca, em conjunto com outras combinações de teclas|ALT|  
|Mover o cursor na direção especificada|Teclas de SETA|  
|Mover o cursor uma página na direção especificada|Teclas PAGE|  
|Mover o cursor para o início do buffer|CTRL+HOME|  
|Mover o cursor para o fim do buffer|CTRL+END|  

**Navegar no histórico**  

|Para fazer isso:|Use essa combinação de teclas:|  
|---------------|-----------------------------|  
|Mover uma linha para cima no histórico de saída|CTRL+SETA PARA CIMA|  
|Mover uma linha para baixo no histórico de saída|CTRL+SETA PARA BAIXO|  
|Mover o visor para o início do buffer (se a linha de comando estiver vazia) ou excluir todos os caracteres à esquerda do cursor (se a linha de comando não estiver vazia)|CTRL+HOME|  
|Mover o visor para a linha de comando (se a linha de comando estiver vazia) ou excluir todos os caracteres à direita do cursor (se a linha de comando não estiver vazia)|CTRL+END|  

**Comandos de teclado adicionais**  

|Para fazer isso:|Use essa combinação de teclas:|  
|---------------|-----------------------------|  
|Abrir a caixa de diálogo Localizar|CTRL+F|  
|Fechar a janela do console|ALT+F4|  
