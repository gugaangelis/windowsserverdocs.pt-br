---
title: Solucionar problemas de clientes ADBA
description: Percorre um processo de solução de problemas para um problema de ativação do Windows
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: d56b2d002b0403971dc50fab639f77bddf1f8809
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959878"
---
# <a name="example-troubleshooting-active-directory-based-activation-adba-clients-that-do-not-activate"></a>Exemplo: Solucionar problemas de clientes ADBA (ativação baseada no Active Directory) que não ativam

> [!NOTE]
> Este artigo foi publicado originalmente como um blog do TechNet em 26 de março de 2018.

Olá, todo mundo! Meu nome é Mike Kammer e sou Platforms PFE com a Microsoft há mais de dois anos agora. Recentemente ajudei um cliente na implantação do Windows Server 2016 em seu ambiente. Aproveitamos essa oportunidade para migrar também sua metodologia de ativação de um Servidor KMS para a [Ativação baseada no Active Directory](/previous-versions/windows/hh852637(v=win.10)).

Como procedimento adequado para fazer todas as alterações, iniciamos nossa migração no ambiente de teste do cliente. Iniciamos nossa implantação seguindo as instruções nesta excelente postagem no blog de Charity Shelbourne, [Ativação baseada no Active Directory vs. Serviço de Gerenciamento de Chaves](https://techcommunity.microsoft.com/t5/Core-Infrastructure-and-Security/Active-Directory-Based-Activation-vs-Key-Management-Services/ba-p/256016). Os controladores de domínio em nosso ambiente de teste estavam executando o Windows Server 2012 R2; por isso, não precisamos preparar nossa floresta. Instalamos a função em um Controlador de Domínio do Windows Server 2012 R2 e escolhemos a ativação baseada no Active Directory como nosso método de ativação de volume. Instalamos nossa chave do KMS e a nomeamos "Ativação do AD do KMS (** LAB)". Nós seguimos a postagem no blog passo a passo.

Começamos criando quatro máquinas virtuais, dois Windows 2016 Standard e dois Windows 2016 Datacenter. Neste ponto, tudo foi ótimo e todos estavam felizes. Criamos um servidor físico que executa o Windows 2016 Standard e o computador foi ativado corretamente. É aí que nossa história acaba.

Haha! Brincadeira! Nada é tão fácil assim. Na verdade, a instalação e a configuração foram muito fáceis, portanto, essa parte era simples e direta. Voltei ao escritório na segunda-feira e todas as máquinas virtuais que eu tinha criado na semana anterior mostravam que elas não foram ativadas. Ei! Não está certo! Voltei para o computador físico e estava certo. Fui até o cliente para discutir o que tinha acontecido. É claro que a primeira pergunta foi “O que mudou durante o fim de semana?” E, como sempre, a resposta foi “nada”. Daquela vez, nada tinha mudado mesmo e nós tivemos que descobrir o que estava acontecendo.

Fui até um dos meus servidores problemáticos, abri um prompt de comando e conferi minha saída do comando **slmgr /ao-list**. A opção **/ao-list** exibe todos os objetos de ativação no Active Directory.

![Imagem que mostra o comando slmgr](./media/032618_1700_Troubleshoo1.png)

![Imagem que mostra os resultados do comando slmgr](./media/032618_1700_Troubleshoo2.png)

Os resultados mostram que nós temos dois Objetos de Ativação: um para o Server 2012 R2 e nossa Ativação do AD do KSM (** LAB) recém-criada que é nossa licença do Windows Server 2016. Isso confirma que nosso Active Directory foi configurado corretamente para ativar Clientes KMS do Windows

Sabendo que o comando **slmgr** é meu amigo para a ativação da licença, continuei com opções diferentes. Tentei a opção **/dlv**, que exibirá informações detalhadas sobre a licença. Isso me pareceu bem. Eu estava executando a versão Standard do Windows Server 2016; há uma ID de ativação, uma ID de instalação, uma URL de validação e até mesmo uma chave do produto (Product Key) parcial.

![Imagem que mostra os resultados de slmgr /dlv](./media/ActivationTroubleshoot2b.jpg)

Alguém percebeu o que eu deixei passar neste momento? Voltaremos a isso após minhas outras etapas de solução de problemas, mas é suficiente dizer que a resposta está na captura de tela.

Meu pensamento agora é que, por algum motivo, a chave está corrompida; portanto, eu uso a opção **/upk**, que desinstala a chave atual. Embora tenha sido eficaz na remoção da chave, geralmente não é a melhor maneira de fazer isso. Caso o servidor seja reinicializado antes de obter uma nova chave, ele pode deixar o servidor em um estado inadequado. Descobri que usar a opção **/ipk** (o que eu faço posteriormente em minha solução de problemas) substitui a chave existente e é um caminho muito mais seguro a seguir. Aprenda com minhas etapas incorretas!

![Imagem que mostra o comando slmgr /upk e seu resultado](./media/032618_1700_Troubleshoo3.png)

Executei a opção **/dlv** outra vez para ver as informações detalhadas sobre a licença. Isso não me forneceu nenhuma informação útil, apenas um erro de chave do produto (Product Key) não encontrada. Porque, é claro, não há nenhuma chave, já que eu acabei de desinstalá-la!

![Imagem que mostra o comando slmgr /dlv e seu resultado](./media/032618_1700_Troubleshoo4.png)

Imaginei dificilmente daria certo, mas eu tentei a opção **/ato**, que deve ativar o Windows com relação aos servidores KMS conhecidos (ou o Active Directory, como pode ser o caso). Novamente, apenas um erro de produto não encontrado.

![Imagem que mostra o comando slmgr /ato e seu resultado](./media/032618_1700_Troubleshoo5.png)

Meu próximo pensamento foi que, às vezes, interromper e iniciar um serviço pode ser a solução, então eu tentei isso em seguida. Precisei interromper e iniciar o Serviço de Plataforma de Proteção de Software da Microsoft (serviço SPPSvc). Em um prompt de comando administrativo, usei os comandos **net stop** e **net start**. Observei que, em um primeiro momento, o serviço não estava sendo executando. Então pensei que deveria ser isso!

![Imagem que mostra os resultados do comando net stop e net start](./media/032618_1700_Troubleshoo6.png)

Mas não. Após iniciar o serviço e tentar ativar o Windows novamente, ainda recebi o erro de produto não encontrado.

Depois, examinei o Log de Eventos do Aplicativo em um dos servidores com problemas. Encontrei um erro relacionado à Ativação da Licença, ID de Evento 8198, cujo código é 0x8007007B.

![Imagem que mostra os detalhes da ID de evento 8198](./media/032618_1700_Troubleshoo7.png)

Ao pesquisar esse código, encontrei um artigo que informava que meu código de erro queria dizer que a sintaxe de rótulo de volume, o nome do arquivo ou o nome do diretório estava incorreto. Lendo os métodos descritos no artigo, não parecia que nenhum deles se enquadrava na minha situação. Quando executei o comando **nslookup -type=all _vlmcs._tcp**, encontrei o servidor KMS existente (ainda muitos computadores do Windows 7 e Server 2008 no ambiente, portanto era preciso mantê-lo por perto) e os cinco controladores de domínio também. Isso indicava que não era um problema de DNS e que meus problemas estavam em outro lugar.

![Imagem que mostra os resultados do comando nslookup](./media/032618_1700_Troubleshoo8.png)

Então soube que não havia nada de errado com o DNS. O Active Directory estava devidamente configurado como uma fonte de ativação do KMS. Meu servidor físico foi ativado corretamente. Isso poderia ser um problema apenas com VMs? Como uma observação interessante neste ponto, meu cliente informa que alguém em um departamento diferente decidiu criar mais de uma dúzia de máquinas virtuais do Windows Server 2016 também. Agora eu suponho que tenho mais dezenas de servidores com os quais lidar que serão ativados. Mas não! Esses servidores foram ativados sem problemas.

Voltei para meu comando **slmgr** para descobrir como fazer esses monstros serem ativados. Desta vez, vou usar a opção **/ipk**, que me permitirá instalar uma chave do produto (Product Key). Acessei [este site](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612867(v=ws.11)) para obter as chaves adequadas para minha versão Standard do Windows Server 2016. Alguns dos meus servidores são Datacenter, mas eu precisei corrigir este primeiro.

![Imagem que mostra a lista de chaves de instalação do cliente KMS](./media/032618_1700_Troubleshoo9.png)

Usei a opção **/ipk** para instalar uma chave do produto (Product Key), escolhendo a chave do Windows Server 2016 Standard.

![Imagem que mostra o comando slmgr /ipk e seu resultado](./media/032618_1700_Troubleshoo10.png)

Daqui em diante, capturei apenas os resultados de minhas experiências com o Datacenter, mas eles eram os mesmos. Usei a opção **/ato** para forçar a ativação. Recebemos a incrível mensagem que o produto foi ativado com sucesso!

![Imagem que mostra o comando slmgr /ato e seu resultado](./media/032618_1700_Troubleshoo11.png)

Usando a opção **/dlv** novamente, pudemos ver que agora fomos ativados pelo Active Directory.

![Imagem que mostra o comando slmgr /dlv e seu resultado](./media/032618_1700_Troubleshoo12.png)

E agora, o que tinha dado errado? Por que eu preciso remover a chave instalada e adicionar essas chaves genéricas para ativar essas máquinas adequadamente? Por que a outra dúzia de computadores foi ativada sem problemas? Como eu disse anteriormente, deixei passar alguma chave nas etapas iniciais de exame do problema. Fiquei totalmente confuso, então entrei em contato com a Charity da postagem inicial no blog para ver se ela poderia me ajudar. Ela viu o problema imediatamente e me ajudou a entender o que eu tinha deixado passar no início.

Quando executei a primeira opção **/dlv**, a chave estava na descrição. A descrição era Sistema Operacional Windows®, Canal COMERCIAL. Eu tinha examinado isso e achei que Canal COMERCIAL queria dizer que ela tinha sido adquirida e era uma chave válida.

![Imagem que mostra os resultados de slmgr /dlv, com as informações de canal realçadas](./media/032618_1700_Troubleshoo13.png)

Quando examinamos a saída da opção **/dlv** de um servidor devidamente ativado, observe que a descrição agora consta canal VOLUME_KMSCLIENT. Isso nos informa que há, de fato, uma licença de volume.

![Imagem que mostra os resultados de slmgr /dlv, com as informações de canal realçadas](./media/032618_1700_Troubleshoo14.png)

Então, o que canal COMERCIAL quer dizer? Bom, isso significa que a mídia que foi usada para instalar o sistema operacional era um ISO do MSDN. Voltei ao meu cliente e perguntei se, por acaso, havia uma segunda ISO do Windows Server 2016 flutuando ao redor da rede. Acontece que sim, havia outro ISO na rede e ele tinha sido usado para criar a outra dúzia de computadores. Eles compararam os dois ISOs e certamente o que foi dado a mim para criar os servidores virtuais era, na verdade, um ISO do MSDN. Eles removeram esse ISO do MSDN de sua rede e agora todos os nossos servidores existentes estão ativados. Não é preciso se preocupar mais com a falha na ativação de builds futuros.

Espero que isso tenha sido útil e que possa poupar seu tempo no futuro!

Mike
