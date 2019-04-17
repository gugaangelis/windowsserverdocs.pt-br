---
title: "Instalar o Windows Server Essentials no modo1 de migração"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 808a4b1e120fa559d603b34ad006b18de6b94378
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Instalar o Windows Server Essentials no modo1 de migração

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode ter apenas um servidor na rede que esteja executando o Windows Server Essentials e servidor deve ser um controlador de domínio para a rede.  
  
 Quando você instala o Windows Server Essentials no modo de migração, o Assistente de instalação executa as seguintes tarefas:  
  
1.  Instala e configura o software de servidor Windows Server Essentials no servidor de destino.  
  
2.  Para atualizar o esquema de domínio para a versão mais recente.  
  
3.  Ingressar em cada servidor de destino no domínio existente. O servidor de origem e o servidor de destino podem estar membros do domínio mesmo após a conclusão do processo de migração. Após a migração, você deve remover o servidor de origem da rede em 21 dias.  
  
    > [!WARNING]
    >  Uma mensagem de erro é adicionada ao log de eventos de cada dia durante o período de carência de 21 dias até que você remova o servidor de origem da sua rede. O texto de lê a mensagem, "a verificação da função FSMO detectou uma condição em seu ambiente que está fora de conformidade com a política de licenciamento. O servidor de gerenciamento deve manter o controlador de domínio primário e o domínio nomenclatura funções mestre do Active Directory. Mova as funções do Active Directory para o servidor de gerenciamento agora. Esse servidor será ser desligado automaticamente se o problema não for corrigido em 21 dias desde o momento em que essa condição foi detectada primeiro." Após o período de carência de 21 dias, o servidor de origem será desligado.  
  
4.  Transfere as funções mestre (também chamado de operações de mestres únicas flexíveis ou FSMO) de operações do servidor de origem para o servidor de destino. Funções mestre de operações são tarefas especializado de controlador de domínio, que são usadas quando os métodos de transferência de dados e de atualização padrão são inadequados. Quando o servidor de destino se torna um controlador de domínio, ele deve conter as operações funções mestre.  
  
5.  Configura o servidor de destino como um servidor de catálogo global. O servidor de catálogo global é um controlador de domínio que gerencia um repositório de dados distribuídas. Ele contém uma representação pesquisável, parcial de todos os objetos em cada domínio na floresta do Active Directory.  
  
6.  Configura o servidor de destino como o servidor de licenciamento de site.  
  
##  <a name="BKMK_Install"></a>Instalar o Windows Server Essentials no servidor de destino  
 Para instalar e configurar o Windows Server Essentials no servidor de destino no modo de migração, execute o seguinte procedimento.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Para instalar o Windows Server Essentials no servidor de destino  
  
1.  Ative o servidor de destino e insira DVD1 do Windows Server Essentials na unidade de DVD. Se você vir uma mensagem perguntando se você quer inicializar a partir de um CD ou DVD, pressione qualquer tecla para fazê-lo.  
  
    > [!NOTE]
    >  Se o servidor de destino dá suporte à inicialização a partir de uma unidade flash USB, você pode usar o **Windows 7 USB/DVD Download Tool** para criar uma unidade Flash USB inicializável do arquivo ISO do Windows Server Essentials. Usar uma unidade flash USB pode significativamente acelerar o processo de instalação porque unidades flash leem dados muito mais rápidos do que discos de DVD-ROM. Depois de criar uma unidade flash USB inicializável, você pode adicionar um arquivo de resposta para a unidade flash. Você pode [baixar o Windows 7 USB/DVD Download Tool](https://go.microsoft.com/fwlink/p/?LinkId=248282) gratuito no site do Microsoft Store.  
  
    > [!NOTE]
    >  Se o servidor de destino não for inicializado do DVD, reinicie o computador e verificar a configuração do BIOS para garantir que **DVD-ROM** é listada primeiro na sequência de inicialização. Para obter mais informações sobre como alterar a sequência de inicialização de configuração do BIOS, consulte a documentação do fabricante do hardware.  
  
2.  Clique em **nova instalação**.  
  
3.  Se você tiver um disco rígido interno que não é exibido na lista, clique em **carregar Drivers** e instalar o driver necessário antes de continuar.  
  
4.  Marque a caixa de seleção que verifica todos os arquivos e pastas em seu disco rígido principal serão excluídas e clique em **instalar**.  
  
5.  Sobre o **modo de instalação do servidor de escolher** página, clique em **migração do servidor**e forneça as informações necessárias de migração.  
  
6.  Quando o **seu servidor será migrado com êxito** mensagem for exibida, clique em **fechar**.  
  
 Após a conclusão da instalação, você está automaticamente conectado com a conta de usuário administrador e a senha que você forneceu no arquivo de resposta da migração.  
  
> [!NOTE]
>  Para desbloquear a área de trabalho enquanto está instalando o Windows Server Essentials, use a conta de administrador interno e deixar a senha em branco.  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a>Verificar a integridade do controlador de domínio  
 Antes de prosseguir com a migração, você deve garantir que o controlador de domínio e a rede do Windows Server Essentials são íntegros.  
  
 A tabela a seguir lista as ferramentas que você pode usar para diagnosticar problemas em seu servidor de destino e a rede e do domínio:  
  
|Ferramenta|Descrição|  
|----------|-----------------|  
|Netdiag|Ajuda a isolar problemas de conectividade e rede. Para obter mais informações e baixar, consulte [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analisa o estado dos controladores de domínio em uma floresta ou empresa e relata problemas para ajudá-lo na solução de problemas. Para obter mais informações e baixar, consulte [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Ajuda a diagnosticar problemas de replicação entre controladores de domínio. Essa ferramenta requer parâmetros de linha de comando para executar. Para obter mais informações e baixar, consulte [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 Você deve corrigir todos os problemas que essas ferramentas relatarem antes de prosseguir com a migração.  
  
> [!NOTE]
>  Se você pretende migrar email para outro servidor do Exchange no local, consulte [integrar um servidor do Exchange local diante com o Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) para obter informações sobre como configurar o servidor do Exchange no local.
