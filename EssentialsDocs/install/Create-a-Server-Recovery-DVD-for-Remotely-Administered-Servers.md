---
title: Criar um DVD de recuperação de servidor para servidores administrados remotamente
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f63a727a28da3b61502c34f1e6194f2c2e9916ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820139"
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Criar um DVD de recuperação de servidor para servidores administrados remotamente

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a><a name="BKMK_HeadlessRecovery"></a>Criar um DVD de recuperação de servidor para servidores administrados remotamente  
 Há dois modelos para redefinição dos padrões de fábrica e recuperação do servidor, e eles diferem com base no hardware que o cliente recebeu.  
  
 **Servidor administrado remotamente**  
  
 Esta opção de recuperação do servidor exige que o processo seja executado a partir de um computador cliente na mesma rede. Como a redefinição de fábrica exige que um imagem específica da hardware seja enviada com o servidor, o parceiro deve criar o DVD de recuperação do servidor.  
  
 **Servidor administrado localmente**  
  
 Este é o modelo clássico em que o servidor é administrado no console do servidor. A mídia de instalação do servidor é usada para executar uma recuperação. Isso exige que o servidor seja enviado com a habilidade de visualizar saída de vídeo, além de incluir um leitor de DVD. O cliente inicializa a partir dessa mídia de instalação do servidor e então escolhe o método de recuperação adequado. Você não precisa criar um DVD de recuperação do servidor para servidores administrados localmente.  
  
> [!NOTE]
>  Para o modelo de servidor administrado localmente, o cliente pode realizar uma redefinição aos padrões de fábrica realizando uma nova instalação. Porém, ele perderá as personalizações do parceiro.  
  
 Existem dois tipos de recuperação do servidor:  
  
 **Redefinição de fábrica**  
  
 Essa recuperação retorna o servidor ao estado original que existia quando o servidor foi enviado da fábrica. Após uma redefinição para os padrões de fábrica, você é solicitado a realizar a configuração inicial do servidor assim como na primeira vez em que o ativou, e todas as configurações e personalizações são perdidas. Isso também é conhecido como dia 0.? Como a redefinição de fábrica exige que um imagem específica da hardware seja enviada com o servidor, o parceiro deve criar o DVD de recuperação do servidor.  
  
 **Restauração bare-metal**  
  
 Essa recuperação presume que você tenha configurado o backup do servidor e que o backup do servidor tenha sido concluído com êxito pelo menos uma vez antes da falha do sistema. A BMR (restauração bare-metal) dá suporte à recuperação do sistema e inicializa as unidades apenas de um backup de servidor anterior.  
  
 Após uma BMR, o servidor é retornado ao estado que existia no momento do backup usado para a restauração. Esse backup normalmente é o mais recente, mas, em alguns casos, pode ser um backup anterior. A recuperação de dados é feita após o sistema ser restaurado usando a assistente de Restauração de Arquivos e Pastas. BMR é o método de recuperação de servidor preferencial porque todas as configurações e definições são mantidas, enquanto uma redefinição aos padrões de fábrica retorna o servidor ao estado do Dia 0.  
  
### <a name="remotely-administered-server-recovery"></a>Recuperação de servidor administrado remotamente  
 Esta seção descreve as personalizações necessárias que o parceiro deve executar e a mídia final que deve ser enviada com cada servidor. Antes de entrar nos detalhes, vamos dar uma olhada na experiência do cliente.  
  
 Nesse cenário, o servidor "¢ s" não está mais funcionando. Isso pode ser causado por um vírus, falha no disco rígido ou alguma outra causa. O cliente insere o DVD de recuperação do servidor em um computador cliente localizado na mesma rede do servidor. O aplicativo de recuperação do servidor guia o cliente por três etapas de recuperação do servidor:  
  
1.  Criar uma unidade flash USB inicializável que é usada para reiniciar o servidor no modo de recuperação. A unidade flash USB deve ter 8 GB ou mais.  
  
2.  Detecta o servidor que agora está no modo de recuperação.  
  
3.  Permite ao cliente escolher a redefinição aos padrões de fábrica ou a restauração bare-metal para retornar o servidor a um estado operacional.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Etapas para a criação de um DVD de recuperação de servidor para vários suporte a vários idiomas  
 Há seis etapas principais para criar um DVD de recuperação do servidor  
  
1.  [Adicional Atualizar WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Coletar imagens de redefinição de fábrica e arquivos XML](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Criar o DVD de recuperação do servidor](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personalizar o assistente](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Criar o arquivo ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Testar o DVD de recuperação](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="step-1-optional-update-winpe"></a><a name="BKMK_Updating"></a>Etapa 1: (opcional) atualizar o WinPE  
 O ADK inclui uma cópia do Windows PE personalizada. Quando essa imagem é inicializada, ela automaticamente inicia o beacon usado pelo aplicativo de recuperação de cliente para se conectar a um servidor no modo de recuperação.  
  
 O Windows PE precisa ser mais personalizado adicionando qualquer driver específico do hardware, como drivers de rede ou de controlador de disco. Depois de inicializar a partir do WinPE, os discos rígidos no sistema precisam ser reconhecíveis e o sistema de rede precisa estar funcionando.  
  
####  <a name="step-2-collect-the-factory-reset-images-and-xml-files"></a><a name="BKMK_Collecting"></a>Etapa 2: coletar as imagens de redefinição de fábrica e os arquivos XML  
 Para redefinir um servidor para os padrões de fábrica, as duas imagens a seguir precisam ser capturadas:  
  
- A imagem da unidade do sistema  
  
- A partição reservada do sistema  
  
  Para capturar essas imagens, a ferramenta GenDiskXML.exe é fornecida. O GenDiskXML.exe também coleta um arquivo chamado disk.xml que é usado pelo processo de recuperação para recriar a configuração do disco.  
  
1.  Após o Sysprep, reinicie o sistema usando qualquer versão de 64 bits do Windows PE.  
  
2.  Para produzir os arquivos .xml e .wim em uma origem externa, execute `GenDiskXML /outputdir:<path>` para produzir os arquivos .xml e .wim em qualquer origem externa. Os arquivos são adicionados ao DVD na próxima etapa.  
  
    > [!NOTE]
    >  O arquivo .wim do sistema será dividido para cumprir a exigência FAT32 de não haver nenhum arquivo maior que 4 GB. Durante o processo, a capacidade exigida do destino usado para capturar os arquivos .wim precisa ser maior que 8 GB para acomodar o processo de divisão.  
  
####  <a name="step-3-create-the-server-recovery-dvd"></a><a name="BKMK_Creating"></a>Etapa 3: criar o DVD de recuperação do servidor  
 Cada servidor enviado da fábrica deve estar acompanhado por um DVD de Recuperação do Servidor. O seu DVD de ferramentas do ADK inclui os arquivos necessários para a criação do DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Para criar o DVD de recuperação do sistema  
  
1.  Crie uma pasta de trabalho para usar como local para armazenar o ISO final.  
  
2.  No CD do parceiro, copie o conteúdo da pasta **Recuperação de Servidor** para a pasta de trabalho que você criou na etapa 1.  
  
3.  Copie o arquivo disk.xml criado ao executar GenDiskXML.exe na pasta **Redefinir**.  
  
4.  Copie os arquivos de imagem criados ao executar **GenDiskXML.exe** na pasta **Reset**. Os arquivos são .wim e .swm, e o número de arquivos pode variar.  
  
5.  Remova GenDiskXML.exe da pasta. Ele é usado apenas para fins de chão de fábrica e não deve ser incluído no DVD enviado ao cliente.  
  
####  <a name="step-4-customize-the-wizard"></a><a name="BKMK_Customizing"></a>Etapa 4: personalizar o assistente  
 O aplicativo de recuperação do servidor deve ser personalizado com uma imagem do dispositivo e um texto que descreva como inicializar o dispositivo específico no modo de recuperação. Devido a essa página do assistente de restauração de arquivos e pastas ser específica do hardware, as etapas para inicializar o servidor no modo de recuperação irão variar.  
  
> [!NOTE]
>  Os nomes de arquivo listados devem corresponder exatamente.  
  
1. A página do assistente tem um link para ajuda adicional. Se esse arquivo .chm existir, ele substituirá o FWLink para a Ajuda na Web. O arquivo de Ajuda está localizado em:  
  
    <\>raiz do DVD \\$OEM $ \Customization\\< nome da cultura\>\RestartHelp.chm  
  
2. O arquivo contém o texto que o cliente vê na página do assistente. O texto deve explicar como inicializar o servidor no modo de recuperação. O controle é rolável, o que impõe um limite prático à quantidade de texto que pode ser adicionada.  
  
    O seguinte arquivo é usado para substituir a imagem de amostra no assistente e é principalmente sobre a identidade visual. Deve ser um arquivo .png. O tamanho do arquivo precisa ser de 256 pixels x 256 pixels ou será cortado quando for exibido no assistente.  
  
    <\>raiz do DVD \\$OEM $ \Customization\\< nome da cultura\>\RestartInstructions.rtf  
  
3. <\>raiz do DVD \\$OEM $ \Customization\\< nome da cultura\>\ServerImage.png  
  
   Quando você estiver convertendo seu DVD de recuperação do servidor para suporte a vários idiomas, certifique-se de fazer o seguinte:  
  
4. Você deve sempre ter a pasta en-us. Se o aplicativo de recuperação do servidor não encontrar os arquivos específicos da cultura que correspondam ao computador cliente no qual está em execução, ele retorna para en-us.  
  
5. Em cada pasta de cultura que você criar, adicione os três arquivos de personalização (.png, .chm e .rtf).  
  
6. Copie ambas as pastas de cultura dos pacotes de idiomas\\< CultureName\>recuperação \Server para a raiz do DVD de recuperação do servidor. Por exemplo: As pastas ES e ES-ES serão copiadas da raiz do DVD para dar suporte a espanhol.  
  
7. Finalize o arquivo ISO.  
  
   Nomes de cultura compatíveis incluem:  

|-|-|  
|-CS-CZ<br /><br /> -de-DE<br /><br /> -en-US<br /><br /> -es-ES<br /><br /> -fr-FR<br /><br /> -hu-HU<br /><br /> -it-IT<br /><br /> -ja-JP<br /><br /> -ko-KR<br /><br /> -NL-NL |-PL-PL<br /><br /> -pt-BR<br /><br /> -pt-PT<br /><br /> -RU-RU<br /><br /> -VA-SE<br /><br /> -TR-TR<br /><br /> -ZH-CN<br /><br /> -ZH-HK<br /><br /> -zh-TW
  
####  <a name="step-5-create-the-iso-file"></a><a name="BKMK_CreatingISO"></a>Etapa 5: criar o arquivo ISO  
 A pasta que foi criada e todos o seu conteúdo podem ser gravados em um DVD. Esse é o DVD que será fornecido aos clientes com o novo servidor.  
  
####  <a name="step-6-test-the-recovery-dvd"></a><a name="BKMK_Testing"></a>Etapa 6: testar o DVD de recuperação  
 Após concluir a instalação do servidor, configure o backup do servidor, execute um backup do servidor e então teste do DVD de recuperação.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Para configurar e executar um backup do servidor  
  
1.  Abra o Dashboard, clique na guia **Dispositivos** e clique em **Configurar Backup para o servidor** no painel **Tarefas**.  
  
2.  Siga as instruções do assistente para configurar um backup de servidor de backup. Será necessário um disco rígido externo para o backup.  
  
3.  Clique em **Iniciar um backup para o servidor** no painel **Tarefas** e siga as instruções do assistente.  
  
4.  Quando o backup for concluído, verifique se foi bem-sucedido.  
  
###### <a name="to-restore-a-server"></a>Para restaurar um servidor  
  
1.  Insira o DVD de recuperação que você criou em um computador cliente conectado à mesma rede do servidor através de um hub ou comutador.  
  
2.  Clique duas vezes em **setup.exe**. O Assistente de Recuperação de Servidor o conduz pelo mesmo processo que o cliente irá seguir.  
  
3.  Clique em **Restaurar o servidor de um backup** e siga as instruções do assistente.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)