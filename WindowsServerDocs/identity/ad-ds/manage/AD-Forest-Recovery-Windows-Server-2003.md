---
title: Recuperação de floresta do AD-recuperação do Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 43a2034cb707d4333abdce5f5b2b09d6c4b5a33a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390063"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Recuperação de floresta do AD-recuperação do Windows Server 2003

>Aplica-se a: Windows Server 2003

Este tópico inclui procedimentos de recuperação de floresta para controladores de domínio (DCs) que executam o Windows Server 2003. O processo geral de recuperação de floresta não é diferente dos DCs do Windows Server 2003, mas os procedimentos específicos podem ser diferentes devido a ferramentas diferentes. Por exemplo, o Ntdsutil. exe pode ser usado para fazer backup e restaurar DCs que executam DCs do Windows Server 2003, enquanto Backup do Windows Server ou Wbadmin. exe é usado para DCs que executam o Windows Server 2008 ou posterior.  
  
- [Fazendo backup dos dados de estado do sistema](#backing-up-the-system-state-data)  
- [Executando uma restauração não autoritativa](#performing-a-nonauthoritative-restore)  
- [Instalar e configurar o serviço do servidor DNS](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>Fazendo backup dos dados de estado do sistema
Use o procedimento a seguir para fazer backup dos dados de estado do sistema, juntamente com quaisquer outros dados que você selecionou para a operação de backup atual, de um controlador de domínio que executa o Windows Server 2003. O Windows Server 2003 inclui a ferramenta Ntbackup, que pode ser usada para fazer backup dos dados de estado do sistema.  
  
A associação em **Administradores** ou **operadores de backup**, ou equivalente, é o mínimo necessário para fazer backup de arquivos e pastas.   
  
Se você estiver fazendo backup dos dados de estado do sistema em uma fita e o programa de backup indicar que não há mídia não utilizada disponível, talvez seja necessário usar o armazenamento removível. Isso adiciona a fita ao pool de mídia livre para que o backup possa usá-la.  
  
Você só pode fazer backup dos dados de estado do sistema em um computador local. Não é possível fazer o backup em um computador remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Para fazer backup dos dados de estado do sistema em um controlador de domínio que executa o Windows Server 2003  
  
1. Clique em **Iniciar**, aponte **para todos os programas**, **acessórios**, ferramentas do **sistema**e clique em **backup**.  
2. Na página de **boas-vindas** , clique em **modo avançado**.  
3. Na guia **backup** , marque a caixa de seleção de qualquer unidade, pasta ou arquivo do qual você deseja fazer backup.  
4. Marque a caixa de seleção **estado do sistema** .  
5. Clique em **Iniciar backup**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Executando uma restauração não autoritativa  

Use o procedimento a seguir para executar uma restauração não autoritativa de um controlador de domínio que executa o Windows Server 2003. Ao executar uma restauração não autoritativa em Active Directory no Windows Server 2003, você executa automaticamente uma restauração não autoritativa do SYSVOL. Nenhuma etapa adicional é necessária.  
  
> [!NOTE]
> Se você também estiver reinstalando o sistema operacional Windows Server 2003, você poderá ou não ingressar o computador no domínio e poderá fornecer qualquer nome ao computador durante a instalação do sistema operacional. Não instale Active Directory. Depois de reinstalar o sistema operacional, vá diretamente para a etapa 4.  
  
Nos controladores de domínio do Windows Server 2003 em que você restaurou somente os dados de estado do sistema, também é necessário reinstalar os aplicativos de software que estavam em execução nos DCs antes da recuperação. Restaurar AD DS no primeiro DC no domínio também restaura o registro porque ambos fazem parte dos dados de estado do sistema. Tenha isso em mente se você tivesse algum aplicativo em execução nesses controladores de dados e se eles tivessem informações armazenadas no registro.  
  
Para poupar tempo necessário para reinstalar o software, determine se os aplicativos que precisam ser instalados nos controladores de domínio são compatíveis com a clonagem de DC virtual. Esses aplicativos podem ser instalados no DC de origem antes da clonagem a fim de economizar tempo e esforço necessários para instalá-los nos DCs virtuais clonados.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Para executar uma restauração não autoritativa
  
1. Depois de iniciar o controlador de domínio, pressione F8 para reiniciar o computador em Modo de Restauração dos Serviços de Diretório (DSRM).  
2. Selecione **modo de restauração dos serviços de diretório (somente controladores de domínio do Windows)** .  
3. Selecione o sistema operacional que você deseja iniciar no modo de restauração.  
4. Faça logon como administrador (você só pode usar uma conta de computador local, nenhuma opção de logon de domínio está disponível).  
5. Em um prompt de comando, digite **Ntbackup**e pressione Enter.  
6. Na página de **boas-vindas** , clique em **modo avançado**e selecione a guia **restaurar e gerenciar mídia** . (não selecione **Assistente de restauração**.)  
7. Selecione o arquivo de backup apropriado para restaurar e verifique se as caixas de seleção **disco do sistema** e **estado do sistema** estão marcadas.  
8. Clique em **Iniciar restauração**.  
9. Quando a operação de restauração for concluída, reinicie o computador.  
  
Use o procedimento a seguir para executar uma restauração autoritativa (também conhecida como primária) do SYSVOL em um controlador de domínio que executa o Windows Server 2003. Execute este procedimento somente no primeiro DC do Windows Server 2003 que é restaurado no domínio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Para executar uma restauração autoritativa do SYSVOL  
  
1. Execute as etapas 1 a 8 no procedimento anterior.  
2. Na caixa de diálogo **confirmar restauração** , clique em **avançado**.  
3. Para executar uma restauração autoritativa do SYSVOL, marque a caixa de seleção **ao restaurar conjuntos de dados replicados, marque os dados restaurados como os dados primários para todas as réplicas**.  

   > [!NOTE]
   > Marcar os dados restaurados como os dados primários no backup é equivalente a definir a entrada **BurFlags** como D4 na seguinte subchave do registro:  
   >   
   > **HKEY_LOCAL_MACHINE conjuntos de réplicas do \system\currentcontrolset\services\ntfrs\parameters\cumulative\\** *GUID*  

4. Quando a operação de restauração for concluída, reinicie o computador.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Instalar e configurar o serviço do servidor DNS

Se o DC que você restaurou do backup estiver executando o Windows Server 2003, você poderá instalar o servidor DNS sem conectar o controlador de domínio a qualquer rede.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Para instalar e configurar o serviço do servidor DNS  
  
1. Abra o assistente de componentes do Windows. Para abrir o assistente:  

   - Clique em **Iniciar**, clique em **Painel de Controle** e clique em **Adicionar ou Remover Programas**.  
   - Clique em **Adicionar ou remover componentes do Windows**.  

2. Em **componentes**, marque a caixa de seleção **serviços de rede** e clique em **detalhes**.  
3. Em **Subcomponentes de serviços de rede**, marque a caixa de seleção **DNS (sistema de nomes de domínio)** , clique em **OK**e em **Avançar**.  
4. Se for solicitado, em **copiar arquivos de**, digite o caminho completo dos arquivos de distribuição e clique em **OK**.  

   Após a instalação, conclua as etapas a seguir para configurar o servidor DNS.  

5. Clique em **Iniciar**, aponte para **todos os programas**, aponte para **Ferramentas administrativas**e clique em **DNS**.  
6. Crie zonas DNS para os mesmos nomes de domínio DNS que foram hospedados nos servidores DNS antes do mau funcionamento crítico. Para obter mais informações, consulte Adicionar uma zona de pesquisa direta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configure os dados DNS como existiam antes do mau funcionamento crítico. Por exemplo:  

   - Configure as zonas DNS a serem armazenadas no AD DS. Para obter mais informações, consulte alterar o tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configure a zona DNS que é autoritativa para registros de recurso do localizador de controlador de domínio (localizador de DC) para permitir atualização dinâmica segura. Para obter mais informações, consulte permitir somente atualizações dinâmicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Verifique se a zona DNS pai contém registros de recurso de delegação (servidor de nomes (NS) e registros de recurso de host de União (A)) para a zona filho hospedada nesse servidor DNS. Para obter mais informações, consulte criar uma delegação de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Depois de configurar o DNS, no prompt de comando, digite o seguinte comando e pressione ENTER:  

   **net stop Netlogon**

10. Digite o seguinte comando e pressione ENTER:  

    **net start Netlogon**

    > [!NOTE]
    > O logon de rede registrará os registros de recurso do localizador de DC no DNS para esse controlador de domínio. Se você estiver instalando o serviço do servidor DNS em um servidor no domínio filho, esse DC não poderá registrar seus registros imediatamente. Isso ocorre porque ele está isolado no momento como parte do processo de recuperação, e seu servidor DNS primário é o servidor DNS raiz da floresta. Configure este computador com o mesmo endereço IP que tinha antes do desastre para evitar falhas de pesquisa do serviço DC.

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
