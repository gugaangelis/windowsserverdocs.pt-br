---
title: Configurar hosts para migração ao vivo sem clustering de failover
description: Fornece instruções para configurar a migração dinâmica em um ambiente não clusterizado
manager: dongill
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: kbdazure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 7bcd4e625f340ba7358a8ce9bdd860581c390e96
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948015"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configurar hosts para migração ao vivo sem clustering de failover

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Este artigo mostra como configurar hosts que não são clusterizados para que você possa fazer migrações ao vivo entre eles. Use estas instruções se você não tiver configurado a migração dinâmica quando instalou o Hyper-V ou se quiser alterar as configurações. Para configurar hosts clusterizados, use ferramentas para clustering de failover.

## <a name="requirements-for-setting-up-live-migration"></a>Requisitos para configurar a migração dinâmica

Para configurar hosts não clusterizados para migração dinâmica, você precisará de:

-  Uma conta de usuário com permissão para executar as várias etapas. A associação no grupo Administradores locais do Hyper-V ou no grupo Administradores nos computadores de origem e de destino atende a esse requisito, a menos que você esteja configurando a delegação restrita. A associação no grupo Administradores de domínio é necessária para configurar a delegação restrita.

- A função Hyper-V no Windows Server 2016 ou no Windows Server 2012 R2 instalada nos servidores de origem e de destino. Você pode fazer uma migração ao vivo entre hosts que executam o Windows Server 2016 e o Windows Server 2012 R2 se a máquina virtual for, no mínimo, a versão 5. <br>Para obter instruções de atualização de versão, consulte [atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obter instruções de instalação, consulte [instalar a função Hyper-V no Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).

- Computadores de origem e de destino que pertencem ao mesmo domínio de Active Directory ou pertencem a domínios que confiam um no outro.
- As ferramentas de gerenciamento do Hyper-V instaladas em um computador que executa o Windows Server 2016 ou o Windows 10, a menos que as ferramentas estejam instaladas no servidor de origem ou de destino e você execute as ferramentas do servidor.

## <a name="consider-options-for-authentication-and-networking"></a>Considere as opções de autenticação e rede

Considere como você deseja configurar o seguinte:

-  **Autenticação**: qual protocolo será usado para autenticar o tráfego de migração ao vivo entre os servidores de origem e de destino? A escolha determina se você precisará entrar no servidor de origem antes de iniciar uma migração ao vivo:
   - O Kerberos permite que você evite ter que entrar no servidor, mas requer que a delegação restrita seja configurada. Consulte abaixo para obter instruções.
   - O CredSSP permite evitar a configuração da delegação restrita, mas exige que você entre no servidor de origem. Você pode fazer isso por meio de uma sessão de console local, uma sessão de Área de Trabalho Remota ou uma sessão remota do Windows PowerShell.

      O CredSPP requer a entrada em busca de situações que podem não ser óbvias. Por exemplo, se você entrar no TestServer01 para mover uma máquina virtual para TestServer02 e, em seguida, desejar mover a máquina virtual de volta para o TestServer01, você precisará entrar no TestServer02 antes de tentar mover a máquina virtual de volta para TestServer01. Se você não fizer isso, a tentativa de autenticação falhará, ocorrerá um erro e a seguinte mensagem será exibida:

      "Falha na operação de migração da máquina virtual na origem da migração.
      Falha ao estabelecer uma conexão com o *nome do computador*host: nenhuma credencial está disponível no pacote de segurança 0x8009030E. "

-   **Desempenho**: faz sentido configurar as opções de desempenho? Essas opções podem reduzir o uso da rede e da CPU, bem como fazer com que as migrações ao vivo se tornem mais rápidas. Considere seus requisitos e sua infraestrutura e teste diferentes configurações para ajudá-lo a decidir. As opções são descritas no final da etapa 2.

-  **Preferência de rede**: permitirá o tráfego de migração ao vivo por meio de qualquer rede disponível ou isolará o tráfego para redes específicas? Como prática recomendada de segurança, convém isolar o tráfego em redes confiáveis privadas porque o tráfego da migração ao vivo não é criptografado quando enviado pela rede. O isolamento de rede pode ser obtido por uma rede fisicamente isolada ou por outra tecnologia de rede confiável, como as VLANs.

## <a name="step-1-configure-constrained-delegation-optional"></a><a name="BKMK_Step1"></a>Etapa 1: configurar a delegação restrita (opcional)
Se você decidiu usar o Kerberos para autenticar o tráfego de migração dinâmica, configure a delegação restrita usando uma conta que seja membro do grupo Administradores de domínio.

### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Usar o snap-in usuários e computadores para configurar a delegação restrita

1.  Abra o snap-in Usuários e Computadores do Active Directory. (Em Gerenciador do Servidor, selecione o servidor se ele não estiver selecionado, clique em **ferramentas**  >>  **Active Directory usuários e computadores**).

2.  No painel de navegação em **Active Directory usuários e computadores**, selecione o domínio e clique duas vezes na pasta **computadores** .

3.  Na pasta **computadores** , clique com o botão direito do mouse na conta de computador do servidor de origem e clique em **Propriedades**.

4.  Em **Propriedades**, clique na guia **delegação** .

5.  Na guia Delegação, selecione **confiar neste computador para delegação somente aos serviços especificados** e, em seguida, selecione **usar qualquer protocolo de autenticação**.

6.  Clique em **Adicionar**.

7.  Em **Adicionar serviços**, clique em **usuários ou computadores**.

8.  Em **Selecionar usuários ou computadores**, digite o nome do servidor de destino. Clique em **verificar nomes** para verificá-lo e clique em **OK**.

9. Em **Adicionar serviços**, na lista de serviços disponíveis, faça o seguinte e, em seguida, clique em **OK**:

    -   Para mover o armazenamento de máquina virtual, selecione **cifs**. Isso será necessário se você quiser mover o armazenamento junto com a máquina virtual, bem como se deseja mover apenas o armazenamento de uma máquina virtual. Se o servidor estiver configurado para usar o armazenamento SMB para Hyper-V, isso já deve estar selecionado.

    -   Para mover máquinas virtuais, selecione **Serviço de Migração de Sistema Virtual da Microsoft**.

10. Na guia **Delegação** da caixa de diálogo Propriedades, verifique se os serviços selecionados na etapa anterior estão listados como os serviços aos quais o computador de destino poderá apresentar credenciais delegadas. Clique em **OK**.

11. Na pasta **Computers**, selecione a conta de computador do servidor de destino e repita o processo. Na caixa de diálogo **Selecionar Usuários ou Computadores**, especifique o nome do servidor de destino.

As alterações de configuração entram em vigor após as seguintes ocorrências:

  -  As alterações são replicadas para os controladores de domínio nos quais os servidores que executam o Hyper-V estão conectados.
  -  O controlador de domínio emite um novo tíquete Kerberos.

## <a name="step-2-set-up-the-source-and-destination-computers-for-live-migration"></a><a name="BKMK_Step2"></a>Etapa 2: configurar os computadores de origem e de destino para migração dinâmica
Esta etapa inclui a escolha de opções para autenticação e rede. Como prática recomendada de segurança, recomendamos que você selecione redes específicas a serem usadas para o tráfego de migração ao vivo, conforme discutido acima. Esta etapa também mostra como escolher a opção de desempenho.

### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Usar o Gerenciador do Hyper-V para configurar os computadores de origem e de destino para migração dinâmica

1.  Abra o Gerenciador do Hyper-V. (Em Gerenciador do Servidor, clique em **ferramentas**  >> **Gerenciador do Hyper-V**.)

2.  No painel de navegação, selecione um dos servidores. (Se não estiver listado, clique com o botão direito do mouse em **Gerenciador do Hyper-V**, clique em **conectar ao servidor**, digite o nome do servidor e clique em **OK**. Repita para adicionar mais servidores.)

3.  No painel **ação** , clique em **configurações do Hyper-V**  >> **migrações ao vivo**.

4.  No painel **Migrações ao Vivo**, marque **Habilitar migrações ao vivo de entrada e saída**.

5.  Em **migrações dinâmicas simultâneas**, especifique um número diferente se você não quiser usar o padrão de 2.

6.  Em **Migrações ao vivo de entrada**, se desejar usar conexões de redes específicas para aceitar o tráfego da migração ao vivo, clique em **Adicionar** para digitar as informações de endereços IP. Caso contrário, clique em **Usar qualquer rede disponível para migração ao vivo**. Clique em **OK**.

7.  Para escolher as opções de desempenho e Kerberos, expanda **migrações ao vivo** e selecione **recursos avançados**.

    - Se você tiver configurado a delegação restrita, em **protocolo de autenticação**, selecione **Kerberos**.
    - Em **Opções de desempenho**, examine os detalhes e escolha uma opção diferente, se for apropriado para o seu ambiente.

8. Clique em **OK**.

9. Selecione o outro servidor no Gerenciador do Hyper-V e repita as etapas.

### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Usar o Windows PowerShell para configurar os computadores de origem e de destino para a migração ao vivo

Três cmdlets estão disponíveis para configurar a migração ao vivo em hosts não clusterizados: [Enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx)e [set-VMHost](https://technet.microsoft.com/library/hh848524.aspx). Este exemplo usa todos os três e faz o seguinte:
  - Configura a migração dinâmica no host local
  - Permite o tráfego de migração de entrada somente em uma rede específica
  - Escolhe o Kerberos como o protocolo de autenticação

Cada linha representa um comando separado.

```PowerShell
PS C:\> Enable-VMMigration

PS C:\> Set-VMMigrationNetwork 192.168.10.1

PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos
```

Set-VMHost também permite que você escolha uma opção de desempenho (e muitas outras configurações de host). Por exemplo, para escolher o SMB, mas deixar o protocolo de autenticação definido como o padrão de CredSSP, digite:

```PowerShell
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMB
```

Esta tabela descreve como funcionam as opções de desempenho.

|Opção|Descrição|
|----------|---------------|
    |TCP/IP|Copia a memória da máquina virtual para o servidor de destino em uma conexão TCP/IP.|
    |Compactação|Compacta o conteúdo de memória da máquina virtual antes de copiá-la para o servidor de destino em uma conexão TCP/IP. **Observação:** Essa é a configuração **padrão** .|
    |SMB|Copia a memória da máquina virtual para o servidor de destino em uma conexão SMB 3,0.<p>-O SMB Direct é usado quando os adaptadores de rede nos servidores de origem e de destino têm recursos RDMA (acesso remoto direto à memória) habilitados.<br />-O SMB Multichannel detecta e usa várias conexões automaticamente quando uma configuração de vários canais SMB adequada é identificada.<p>Para obter mais informações, consulte [Melhorar o desempenho de um servidor de arquivos com o SMB Direct](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|

 ## <a name="next-steps"></a>Próximas etapas

 Depois de configurar os hosts, você estará pronto para fazer uma migração ao vivo. Para obter instruções, consulte [usar migração dinâmica sem clustering de failover para mover uma máquina virtual](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md).
