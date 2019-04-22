---
title: Configurar hosts para migração ao vivo sem Clustering de Failover
description: Fornece instruções para configurar a migração ao vivo em um ambiente não clusterizado
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 49e36d7fae5eec07772fd6f82c5a5d69838351d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821987"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configurar hosts para migração ao vivo sem Clustering de Failover

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Este artigo mostra como configurar os hosts que não estão em cluster para que você pode fazer migrações ao vivo entre eles. Use estas instruções se você não configurou a migração ao vivo quando você instalou o Hyper-V, ou se você quiser alterar as configurações. Para configurar hosts clusterizados, use ferramentas de Clustering de Failover.  
  
## <a name="requirements-for-setting-up-live-migration"></a>Requisitos para configurar a migração ao vivo  
  
Para configurar hosts não clusterizados para migração ao vivo, você precisará de:   
  
-  Uma conta de usuário com permissão para executar as várias etapas. A associação no grupo local de administradores do Hyper-V ou o grupo de administradores nos computadores de origem e de destino atende a esse requisito, a menos que você está configurando a delegação restrita. A associação no grupo de administradores de domínio é necessária para configurar a delegação restrita.  
  
- A função Hyper-V no Windows Server 2016 ou Windows Server 2012 R2 instalado nos servidores de origem e destino. Você pode fazer uma migração dinâmica entre hosts que executam o Windows Server 2016 e Windows Server 2012 R2 se a máquina virtual é pelo menos a versão 5. <br>Para instruções de atualização de versão, consulte [máquinas virtuais de atualização de versão no Hyper-V no Windows 10 ou Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obter instruções de instalação, consulte [instalar a função Hyper-V no Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
  
- Computadores de origem e de destino que pertencem ao mesmo domínio do Active Directory, ou que pertencem a domínios que confiam uns nos outros.    
- As ferramentas de gerenciamento do Hyper-V instaladas em um computador que executa o Windows Server 2016 ou Windows 10, a menos que as ferramentas são instaladas no servidor de origem ou destino e você vai executar as ferramentas do servidor.  
  
## <a name="consider-options-for-authentication-and-networking"></a>Considere as opções de autenticação e de rede  
  
Considere como você deseja configurar o seguinte:  
  
-  **Autenticação**: Protocolo que será usado para autenticar o tráfego da migração ao vivo entre os servidores de origem e de destino? A escolha determina se será preciso fazer logon no servidor de origem antes de iniciar uma migração ao vivo:   
   - Kerberos permite que você evite ter de fazer logon no servidor, mas requer a delegação restrita para ser configurado. Consulte abaixo para obter instruções.  
   - CredSSP permite que você evite configurar a delegação restrita, mas exige que você entrar no servidor de origem. Você pode fazer isso por meio de uma sessão de console local, uma sessão de área de trabalho remota ou uma sessão remota do Windows PowerShell.  
  
      CredSPP requer entrar para situações que podem não ser óbvias. Por exemplo, se você entrar no testserver01 para mover uma máquina virtual para TestServer02 e depois quiser retornar a máquina virtual para TestServer01, você precisará entrar no TestServer02 antes de tentar retornar a máquina virtual para TestServer01. Se você não fizer isso, a tentativa de autenticação falhar, ocorre um erro, e a seguinte mensagem é exibida:  
    
      "Falha na operação de migração de máquina Virtual na origem da migração.  
      Falha ao estabelecer uma conexão com host *nome do computador*: Nenhuma credencial estiver disponível no pacote de segurança 0x8009030E."
  
-   **Desempenho**: Faz sentido configurar opções de desempenho? Essas opções podem reduzir o uso da CPU e rede, bem como tornar as migrações ao vivo. Considere seus requisitos e sua infraestrutura e testar configurações diferentes para ajudá-lo a decidir. As opções são descritas no final da etapa 2.  
  
-  **Preferência de rede**: Você permitirá o tráfego da migração ao vivo através de qualquer rede disponível ou isolará o tráfego em redes específicas? Como prática recomendada de segurança, convém isolar o tráfego em redes confiáveis privadas porque o tráfego da migração ao vivo não é criptografado quando enviado pela rede. O isolamento de rede pode ser obtido por uma rede fisicamente isolada ou por outra tecnologia de rede confiável, como as VLANs.  
  
## <a name="BKMK_Step1"></a>Etapa 1: Configurar a delegação restrita (opcional)  
Se você tiver optado por usar o Kerberos para autenticar o tráfego da migração ao vivo, configure a delegação restrita usando uma conta que seja membro do grupo Administradores de domínio.  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Use o snap-in Usuários e computadores para configurar a delegação restrita  
  
1.  Abra o snap-in Usuários e Computadores do Active Directory. (No Gerenciador do servidor, selecione o servidor se ele não estiver selecionado, clique em **ferramentas** >> **Active Directory Users and Computers**).  
  
2.  No painel de navegação do **Active Directory Users and Computers**, selecione o domínio e clique duas vezes o **computadores** pasta.  
  
3.  Dos **computadores** pasta, a conta de computador do servidor de origem com o botão direito e, em seguida, clique em **propriedades**.  
  
4.  Partir **propriedades**, clique no **delegação** guia.  
  
5.  Na guia delegação, selecione **confiar no computador para delegação apenas a serviços especificados** e, em seguida, selecione **usar qualquer protocolo de autenticação**.  
  
6.  Clique em **Adicionar**.  
  
7.  Partir **adicionar serviços**, clique em **usuários ou computadores**.  
  
8.  Partir **selecionar usuários ou computadores**, digite o nome do servidor de destino. Clique em **verificar nomes** verificá-lo e, em seguida, clique em **Okey**.  
  
9. Partir **adicionar serviços**, na lista de serviços disponíveis, faça o seguinte e, em seguida, clique em **Okey**:  
  
    -   Para mover o armazenamento de máquina virtual, selecione **cifs**. Isso é necessário se você quiser mover o armazenamento, juntamente com a máquina virtual, bem como se você deseja mover apenas o armazenamento da máquina virtual. Se o servidor estiver configurado para usar o armazenamento SMB para Hyper-V, isso já deve estar selecionado.  
  
    -   Para mover máquinas virtuais, selecione **Serviço de Migração de Sistema Virtual da Microsoft**.  
  
10. Na guia **Delegação** da caixa de diálogo Propriedades, verifique se os serviços selecionados na etapa anterior estão listados como os serviços aos quais o computador de destino poderá apresentar credenciais delegadas. Clique em **OK**.  
  
11. Na pasta **Computers** , selecione a conta de computador do servidor de destino e repita o processo. Na caixa de diálogo **Selecionar Usuários ou Computadores**, especifique o nome do servidor de destino.  
  
As alterações de configuração entrem em vigor depois que ambos os procedimentos a seguir ocorrem:  
  
  -  As alterações são replicadas para os controladores de domínio que os servidores que executam o Hyper-V estão conectados.  
  -  O controlador de domínio emite um novo tíquete Kerberos.  
  
## <a name="BKMK_Step2"></a>Etapa 2: Configurar os computadores de origem e de destino para migração ao vivo  
Essa etapa inclui escolhendo opções de autenticação e de rede. Como prática recomendada de segurança, recomendamos que você selecionar redes específicas a ser usado para tráfego da migração ao vivo, como discutido acima. Esta etapa mostra como escolher a opção de desempenho.   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Use o Gerenciador do Hyper-V para configurar os computadores de origem e destino para migração ao vivo  
  
1.  Abra o Gerenciador Hyper-V. (No Gerenciador do servidor, clique em **ferramentas** >>**Gerenciador Hyper-V**.)  
  
2.  No painel de navegação, selecione um dos servidores. (Se não estiver listado, clique com botão direito **Gerenciador do Hyper-V**, clique em **conectar ao servidor**, digite o nome do servidor e clique em **Okey**. Repita para adicionar mais servidores).  
  
3.  No **ação** painel, clique em **configurações do Hyper-V** >>**migrações ao vivo**.  
  
4.  No painel **Migrações ao Vivo** , marque **Habilitar migrações ao vivo de entrada e saída**.  
  
5.  Sob **migrações dinâmicas simultâneas**, especifique um número diferente se você não quiser usar o padrão de 2.  
  
6.  Em **Migrações ao vivo de entrada**, se desejar usar conexões de redes específicas para aceitar o tráfego da migração ao vivo, clique em **Adicionar** para digitar as informações de endereços IP. Caso contrário, clique em **Usar qualquer rede disponível para migração ao vivo**. Clique em **OK**.  
  
7.  Para escolher opções de Kerberos e o desempenho, expanda **migrações ao vivo** e, em seguida, selecione **recursos avançados**.  
  
    - Se você tiver configurado a delegação restrita, sob **protocolo de autenticação**, selecione **Kerberos**.  
    - Sob **opções de desempenho**, examine os detalhes e escolha uma opção diferente, se for apropriado para seu ambiente.   
  
8. Clique em **OK**.  
  
9. Selecione outro servidor no Gerenciador do Hyper-V e repita as etapas.  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Use o Windows PowerShell para configurar os computadores de origem e destino para migração ao vivo  
  
Três cmdlets estão disponíveis para configurar a migração ao vivo em hosts não clusterizados: [Enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [Set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx), e [Set-VMHost](https://technet.microsoft.com/library/hh848524.aspx). Este exemplo usa três e faz o seguinte:   
  - Configura a migração ao vivo no host local  
  - Permite que o tráfego da migração apenas em uma rede específica  
  - Escolhe Kerberos como protocolo de autenticação   
  
Cada linha representa um comando separado.  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
Set-VMHost também permite que você escolha uma opção de desempenho (e muitas outras configurações de host). Por exemplo, para escolher o SMB, mas deixe o protocolo de autenticação definido como o padrão do CredSSP, digite:  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
Esta tabela descreve como funcionam as opções de desempenho.  
  
|Opção|Descrição|  
|----------|---------------|  
    |TCP/IP|Copia a memória da máquina virtual para o servidor de destino ao longo de uma conexão TCP/IP.|  
    |Compactação|Compacta o conteúdo da memória da máquina virtual antes de copiá-lo para o servidor de destino ao longo de uma conexão TCP/IP. **Observação:** Esse é o **padrão** configuração.|  
    |SMB|Copia a memória da máquina virtual para o servidor de destino ao longo de uma conexão SMB 3.0.<br /><br />-O SMB Direct é usado quando os adaptadores de rede nos servidores de origem e destino têm recursos direto memória RDMA (acesso remoto) habilitado.<br />-O SMB Multichannel detecta automaticamente e usa várias conexões quando uma configuração adequada de SMB Multichannel é identificada.<br /><br />Para obter mais informações, consulte [Melhorar o desempenho de um servidor de arquivos com o SMB Direct](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|  
      
 ## <a name="next-steps"></a>Próximas etapas
 
 Depois de configurar os hosts, você está pronto para fazer uma migração ao vivo. Para obter instruções, consulte [usar migração ao vivo sem Clustering de Failover para mover uma máquina virtual](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md). 
    


