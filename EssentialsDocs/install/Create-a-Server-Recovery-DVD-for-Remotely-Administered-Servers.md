---
title: "Criar uma DVD de recuperação do servidor para servidores administrados remotamente"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7bfe1686ac84962cdb4ab1cde8d6ca5226cb9d44
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Criar uma DVD de recuperação do servidor para servidores administrados remotamente

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a>Criar um DVD de recuperação de servidor para servidores administrados remotamente  
 Há dois modelos para a recuperação de restauração e o servidor de fábrica, e eles variam de acordo com o hardware que o cliente recebeu.  
  
 **Servidor administrado remotamente**  
  
 Essa opção de recuperação de servidor requer que o processo é executado em um computador cliente na mesma rede. Como a restauração de fábrica requer que uma imagem específica do hardware ser fornecidos com o servidor, o parceiro deve criar DVD de recuperação do servidor.  
  
 **Servidor administrado localmente**  
  
 Esse é o modelo clássico onde o servidor é administrado no console do servidor. A mídia de instalação do servidor é usada para executar uma recuperação. Isso requer que o servidor é fornecido com a capacidade de exibir a saída de vídeo além de incluir um leitor de DVD. O cliente seja inicializado pela mídia de instalação desse servidor e, em seguida, escolha o método de recuperação apropriadas. Você não precisa criar um DVD de recuperação de servidor para servidores que são administradas localmente.  
  
> [!NOTE]
>  Para o modelo de servidor administrado localmente, o cliente pode realizar uma fábrica de redefinir realizando uma nova instalação. No entanto, eles poderão perder o parceiro de personalizações.  
  
 Existem dois tipos de recuperação do servidor.  
  
 **Restauração de fábrica**  
  
 Essa recuperação retorna o servidor ao estado original que existia quando o servidor foi enviado de fábrica. Após uma restauração de fábrica, você será solicitado a executar a configuração inicial do servidor, assim como você estava na primeira vez que o ligou e todas as configurações e personalizações são perdidas. Isso também é conhecido como dia 0.? Como a restauração de fábrica requer que uma imagem específica do hardware ser fornecidos com o servidor, o parceiro deve criar DVD de recuperação do servidor.  
  
 **Restauração bare-metal**  
  
 Essa recuperação pressupõe que você configurou um backup do servidor e o backup do servidor concluída com êxito pelo menos uma vez antes da falha do servidor. Restauração bare-metal (BMR) dá suporte a recuperação das unidades de sistema e de inicialização somente de um backup do servidor anterior.  
  
 Após uma BMR, o servidor é retornado para o estado que existia no momento do backup que é usado para a restauração. Isso normalmente é o backup mais recente, mas em alguns casos, pode ser um backup anterior. Recuperação de dados é feita depois que o sistema for restaurado usando o Assistente de pastas e restaurar arquivos. BMR é o método de recuperação de servidor preferencial porque todas as configurações e configuração são retornados, enquanto uma redefinição de fábrica retorna o servidor para um estado de dia 0.  
  
### <a name="remotely-administered-server-recovery"></a>Administrado remotamente a recuperação de servidor  
 Esta seção descreve as personalizações necessárias que o parceiro deve executar e a mídia final deve ser fornecido com cada servidor. Antes de investigar os detalhes, vamos examine a experiência do cliente.  
  
 Nesse cenário, o cliente "¢ s servidor não está funcionando. Isso pode ser causado por um vírus, uma falha do disco rígido ou alguma outra causa. O cliente insere a DVD de recuperação do servidor em um computador cliente que está localizado na mesma rede que o servidor. O aplicativo de recuperação do servidor-los percorre com três etapas para recuperar seus servidores:  
  
1.  Cria uma unidade flash inicializável USB que é usada para reiniciar o servidor no modo de recuperação. A unidade flash USB deve ter 8 GB ou maior.  
  
2.  Detecta o servidor que agora está no modo de recuperação.  
  
3.  Permite que o cliente escolher uma redefinição de fábrica ou uma restauração bare-metal e, em seguida, retorna seu servidor para um estado de funcionamento.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Suportam a etapas de criação de uma DVD de recuperação de servidor para vários idiomas  
 Há seis principais etapas para criar um DVD de recuperação do servidor  
  
1.  [(Opcional) Atualizar o WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Coletar os arquivos XML e imagens de restauração de fábrica](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Criar DVD de recuperação do servidor](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personalizar o Assistente](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Criar o arquivo ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Testar a DVD de recuperação](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a>Etapa 1: (Opcional) atualizar WinPE  
 O ADK inclui uma cópia do Windows PE personalizada. Quando essa imagem é inicializada, ele inicia automaticamente o aviso que é usado pelo aplicativo de recuperação do cliente para se conectar a um servidor no modo de recuperação.  
  
 Windows PE precisa mais ser personalizados com a adição de todos os drivers de hardware específico, como rede ou drivers de controlador de disco. Após a inicialização do WinPE, os discos rígidos no sistema precisa ser reconhecível e a rede deve estar funcionando.  
  
####  <a name="BKMK_Collecting"></a>Etapa 2: Coletar as imagens de restauração de fábrica e arquivos XML  
 Para redefinir um servidor de padrões de fábrica, as duas imagens a seguir precisam ser capturada:  
  
-   A imagem de disco do sistema  
  
-   A partição do sistema reservado  
  
 Para capturar essas imagens, a ferramenta GenDiskXML.exe é fornecida. GenDiskXML.exe também coleta um arquivo chamado disk.xml que é usado pelo processo de recuperação para recriar a configuração de disco.  
  
1.  Após o Sysprep, reinicialize o sistema usando qualquer versão de 64 bits do Windows PE.  
  
2.  Para exibir os arquivos. XML e. wim a uma fonte externa, execute `GenDiskXML /outputdir:<path>`para gerar os arquivos. XML e. wim para qualquer fonte externa. Os arquivos são adicionados ao DVD na próxima etapa.  
  
    > [!NOTE]
    >  O arquivo. wim do sistema será dividido para atender à exigência de FAT32 de nenhum arquivo mais de 4 GB. Durante o processo, a capacidade necessária para o destino é usado para capturar os arquivos. wim deve ser maior que 8 GB para acomodar o processo de divisão.  
  
####  <a name="BKMK_Creating"></a>Etapa 3: Criar DVD de recuperação do servidor  
 Cada servidor saem da fábrica deve ser acompanhado por uma DVD de recuperação do servidor. O DVD de ferramentas do ADK inclui os arquivos necessários para criar o DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Para criar DVD de recuperação do servidor  
  
1.  Crie uma pasta de trabalho para usar como o local para armazenar o ISO final.  
  
2.  No CD do parceiro, copie o conteúdo do **recuperação de servidor** para a pasta de trabalho que você criou na etapa 1.  
  
3.  Copie o arquivo disk.xml que foi criado quando você executou GenDiskXML.exe para o **redefinir** pasta.  
  
4.  Copie os arquivos de imagem que foram criados quando você executou **GenDiskXML.exe** para o **redefinir** pasta. Os arquivos são os arquivos. wim e. swm e o número de arquivos pode variar.  
  
5.  Remova GenDiskXML.exe da pasta. Ele é usado apenas para fins de fábrica, e não devem ser incluído no DVD do que é enviado ao cliente.  
  
####  <a name="BKMK_Customizing"></a>Etapa 4: Personalizar o Assistente  
 O aplicativo de recuperação do servidor deve ser personalizado com uma imagem de um dispositivo e o texto que descreve como iniciar o dispositivo específico no modo de recuperação. Esta página do Assistente de pastas de restaurar arquivos e é específico do hardware, as etapas para iniciar o servidor no modo de recuperação variam.  
  
> [!NOTE]
>  Os nomes de arquivo que estão listados devem corresponder exatamente.  
  
1.  A página Assistente tem um link para obter ajuda adicional. Se esse arquivo. chm existir, ela substituirá o FWLink para ajuda na web. O arquivo de Ajuda está localizado em:  
  
     < DVD Root\ > \\$OEM$\Customization\\ < cultura name\ > \RestartHelp.chm  
  
2.  Esse arquivo contém o texto que o cliente vê na página do assistente. O texto deve explicar como inicializar o servidor no modo de recuperação. O controle é rolável, que coloca um limite prático na quantidade de texto que pode ser adicionado.  
  
     O arquivo a seguir é usado para substituir a imagem de exemplo no assistente, e é principalmente sobre identidade visual. Ele deve ser um arquivo. PNG. O tamanho do arquivo deve ser 256 pixels de pixels x 256 ou ela será cortada quando ele é exibido no assistente.  
  
     < DVD Root\ > \\$OEM$\Customization\\ < cultura name\ > \RestartInstructions.rtf  
  
3.  < DVD Root\ > \\$OEM$\Customization\\ < cultura name\ > \ServerImage.png  
  
 Quando você estiver convertendo sua recuperação de servidor DVD para dar suporte a vários idiomas, certifique-se de que você faça o seguinte:  
  
1.  Você sempre deve ter a en-us pasta. Se o aplicativo de recuperação de servidor não encontra os arquivos específicos de cultura que correspondem a ele está sendo executado no computador cliente, ele utilizará en-us.  
  
2.  Em cada pasta de cultura que você criar, adicione os arquivos de personalização três (.png,. chm e. rtf).  
  
3.  Copie ambas as pastas de cultura do idioma Packs\\ < CultureName\ > \Server recuperação na raiz do DVD de recuperação do servidor. Por exemplo: as pastas ES e ES-ES seriam copiadas para a raiz do DVD para dar suporte a espanhol.  
  
4.  Finalize o arquivo ISO.  
  
 Nomes de cultura com suporte incluem:  

|-|-|  
|-Cs-CZ<br /><br /> -De-DE<br /><br /> -En-US<br /><br /> -Es-ES<br /><br /> -Fr-FR<br /><br /> -Hu-HU<br /><br /> -It-IT<br /><br /> -Ja-JP<br /><br /> -Ko-KR<br /><br /> -Nl-NL |-pl-PL<br /><br /> -Pt-BR<br /><br /> -Pt-PT<br /><br /> -Ru-RU<br /><br /> -Sv-SE<br /><br /> -Tr-TR<br /><br /> -Zh-CN<br /><br /> -Zh-HK<br /><br /> -Zh-TW
  
####  <a name="BKMK_CreatingISO"></a>Etapa 5: Criar o arquivo ISO  
 A pasta que foi criada e todo o conteúdo pode ser gravado em um DVD. Este é o DVD que será fornecido aos clientes com o novo servidor.  
  
####  <a name="BKMK_Testing"></a>Etapa 6: Testar a DVD de recuperação  
 Depois de concluir a instalação do servidor, configurar o backup do servidor, execute um backup do servidor e depois testar a DVD de recuperação.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Para configurar e executar um backup do servidor  
  
1.  Abra o painel, clique no **dispositivos** guia e, em seguida, clique em **configurar o backup do servidor** no **tarefas** painel.  
  
2.  Siga as instruções no Assistente para configurar um backup do servidor de backup. Você precisa de um disco rígido externo para o backup.  
  
3.  Clique em **iniciar um backup do servidor** no **tarefas** painel e siga as instruções no assistente.  
  
4.  Quando o backup é concluído, verificar se ele foi bem-sucedida.  
  
###### <a name="to-restore-a-server"></a>Para restaurar um servidor  
  
1.  Insira a recuperação DVD que você criou em um computador cliente que está conectado à mesma rede que o servidor por meio de um hub ou um switch.  
  
2.  Clique duas vezes em **setup.exe**. O Assistente de recuperação de servidor solicita que você pelo mesmo processo de que o cliente seguirá.  
  
3.  Clique em **restaurar o servidor de um backup**e siga as instruções no assistente.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)