---
title: Instalar o Windows Server Essentials no modo1 de migração
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847687"
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Instalar o Windows Server Essentials no modo1 de migração

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode ter apenas um servidor em sua rede que está executando o Windows Server Essentials, e ele deve ser um controlador de domínio para a rede.  
  
 Quando você instala o Windows Server Essentials no modo de migração, o Assistente de instalação executa as seguintes tarefas:  
  
1.  Instala e configura o software de servidor do Windows Server Essentials no servidor de destino.  
  
2.  Atualiza o esquema de domínio para a versão mais recente.  
  
3.  Adiciona o servidor de destino ao domínio existente. O servidor de origem e o servidor de destino podem ambos ser membros do mesmo domínio até que o processo de migração seja concluído. Após a conclusão da migração, você deve remover o servidor de origem da rede em 21 dias.  
  
    > [!WARNING]
    >  Uma mensagem de erro é adicionada ao log de eventos por dia durante o período de cortesia 21 dias até que você remova o servidor de origem da rede. O texto com a mensagem “A Verificação de Função FSMO detectou uma condição em seu ambiente que não está em conformidade com a política de licenciamento. O servidor de gerenciamento deve conter o controlador de domínio primário e funções do Active Directory do mestre de nomeação de domínios. Mova as funções do Active Directory para o servidor de gerenciamento agora. Este servidor será desligado automaticamente se o problema não for corrigido dentro de 21 dias a partir do momento em que a condição foi detectada pela primeira vez." Após o período de cortesia de 21 dias, o servidor de origem será desativado.  
  
4.  Transfere as funções do mestre de operações (também chamadas de operações de mestre único flexíveis, ou FSMO) do servidor de origem para o servidor de destino. Funções de mestre de operações são tarefas específicas de controladores de domínio usadas quando métodos de atualização e transferência de dados padrão são inadequados. Quando o Servidor de Destino se torna um controlador de domínio, ele deve hospedar as funções mestre das operações.  
  
5.  Configura o servidor de destino como um servidor de catálogo global. O servidor de catálogo global é um controlador de domínio que gerencia um repositório de dados distribuído. Contém uma representação parcial e pesquisável de cada objeto em cada domínio na floresta do Active Directory.  
  
6.  Configura o servidor de destino como o servidor de licenciamento de site.  
  
##  <a name="BKMK_Install"></a> Instalar o Windows Server Essentials no servidor de destino  
 Para instalar e configurar o Windows Server Essentials no servidor de destino no modo de migração, execute o procedimento a seguir.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Para instalar o Windows Server Essentials no servidor de destino  
  
1.  Ative o servidor de destino e insira o DVD1 do Windows Server Essentials na unidade de DVD. Se você visualizar uma mensagem perguntando se deseja inicializar a partir de um CD ou DVD, pressione qualquer tecla.  
  
    > [!NOTE]
    >  Se o servidor de destino der suporte à inicialização a partir de uma unidade flash USB, você pode usar o **ferramenta de Download de USB/DVD do Windows 7** para criar uma unidade Flash USB inicializável do arquivo ISO do Windows Server Essentials. O uso uma unidade flash USB pode acelerar consideravelmente o processo de instalação, pois as unidades flash leem dados muito mais rapidamente do que as unidades de DVD-ROM. Depois de criar uma unidade flash USB inicializável, você pode adicionar um arquivo de resposta para a unidade flash. Você pode [baixar a ferramenta de Download de USB/DVD do Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=248282) gratuitamente no site da Microsoft Store.  
  
    > [!NOTE]
    >  Se o servidor de destino não inicializar do DVD, reinicie o computador e verifique a configuração do BIOS para garantir que o **DVD-ROM** seja listado primeiro na sequência de inicialização. Para obter mais informações sobre como alterar a sequência de inicialização de configuração do BIOS, consulte a documentação do fabricante do hardware.  
  
2.  Clique em **Nova Instalação**.  
  
3.  Se você tiver um disco rígido interno que não é exibido na lista, clique em **Carregar Drivers** e instale o driver necessário antes de continuar.  
  
4.  Marque a caixa de seleção que verifica que todos os arquivos e pastas no disco rígido primário serão excluídas e clique em **Instalar**.  
  
5.  Na página **Escolher modo de instalação do servidor** página, clique em **Migração do servidor** e, em seguida, forneça as informações de migração necessárias.  
  
6.  Quando a mensagem **Seu servidor foi migrado com sucesso** for exibida, clique em **Fechar**.  
  
 Após a conclusão da instalação, você é automaticamente conectado com a conta de usuário administrador e a senha fornecidas no arquivo de resposta da migração.  
  
> [!NOTE]
>  Para desbloquear a área de trabalho durante a instalação do Windows Server Essentials, use a conta de administrador interno e deixe a senha em branco.  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a> Verificar a integridade do controlador de domínio  
 Antes de prosseguir com a migração, você deve garantir que o controlador de domínio e a rede do Windows Server Essentials estejam íntegros.  
  
 A tabela a seguir lista as ferramentas que você pode usar para diagnosticar problemas em sua rede e o servidor de destino e no domínio:  
  
|Ferramenta|Descrição|  
|----------|-----------------|  
|Netdiag|Ajuda a isolar problemas de rede e conectividade. Para obter mais informações e para baixar, consulte o [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analisa o estado dos controladores de domínio em uma floresta ou empresa e informa os problemas para ajudá-lo a solucioná-los. Para obter mais informações e para baixar, consulte o [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Ajuda no diagnóstico de problemas de replicação entre controladores de domínio. A execução dessa ferramenta requer parâmetros de linha de comando. Para obter mais informações e para baixar, consulte o [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 Você deve corrigir todos os problemas apresentados pelas ferramentas antes de continuar a migração.  
  
> [!NOTE]
>  Se você planeja migrar emails para outro servidor do Exchange no local, consulte [integrar um servidor do Exchange local com o Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) para obter informações sobre como configurar seu servidor do Exchange no local.
