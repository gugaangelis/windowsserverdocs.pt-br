---
title: "Recuperação de floresta do AD - recuperação do Windows Server 2003"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: daebbc65b9864554fde839c3865f507ab787e6b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Recuperação de floresta do AD - recuperação do Windows Server 2003

>Aplica-se a: Windows Server 2003

Este tópico inclui procedimentos de recuperação de floresta para controladores de domínio () que executam o Windows Server 2003. O processo geral para a recuperação de floresta não é diferente com controladores de domínio do Windows Server 2003, mas procedimentos específicos podem variar por causa de diferentes ferramentas. Por exemplo, Ntdsutil.exe pode ser usado para fazer backup e restaurar controladores de domínio que executam o Windows Server 2003 DCs, enquanto o Backup do Windows Server ou Wbadmin.exe é usado para controladores de domínio que executam o Windows Server 2008 ou posterior.  
  
-   [Backup dos dados de estado do sistema](#Backing-up-the-System-State-data)  
  
-   [Executar uma restauração não autorizada](#Performing-a-nonauthoritative restore)  
  
-   [Instalar e configurar o serviço de servidor DNS](#Install-and-configure-the-DNS-Server-service)  
  
  
## <a name="backing-up-the-system-state-data"></a>Backup dos dados de estado do sistema  
 Use o procedimento a seguir para fazer backup dos dados de estado do sistema, juntamente com quaisquer outros dados que você selecionou para a operação de backup atual, de um controlador de domínio que executa o Windows Server 2003. Windows Server 2003 inclui a ferramenta Ntbackup, que você pode usar para fazer backup dos dados de estado do sistema.  
  
 A associação ao grupo **administradores** ou **operadores de Backup**, ou equivalente, é o requisito mínimo para fazer backup de arquivos e pastas.   
  
 Se você estiver fazendo backup dos dados de estado do sistema em uma fita e o programa Backup indica que nenhuma mídia não utilizada está disponível, você terá que usar o armazenamento removível. Isso adiciona a fita para o pool de mídia livre para que o Backup pode usá-lo.  
  
 Você só pode fazer backup dos dados de estado do sistema em um computador local. Você não faça backup em um computador remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Para fazer backup dos dados de estado do sistema em um controlador de domínio que executa o Windows Server 2003  
  
1.  Clique em **iniciar**, aponte para **todos os programas**, aponte para **Acessórios**, aponte para **ferramentas do sistema**e clique em **Backup**.  
  
2.  Sobre o **boas-vindas** página, clique em **modo avançado**.  
  
3.  Sobre o **Backup** guia, marque a caixa de seleção para qualquer unidade, pasta ou arquivo que você quer fazer backup.  
  
4.  Selecione o **estado do sistema** caixa de seleção.  
  
5.  Clique em **iniciar o Backup**.  
  
  
## <a name="performing-a-nonauthoritative-restore"></a>Executar uma restauração não autorizada  
 Use o procedimento a seguir para executar uma restauração não autorizada de um controlador de domínio que executa o Windows Server 2003. Ao executar uma restauração não autorizada do Active Directory no Windows Server 2003, você automaticamente executar uma restauração não autorizada de SYSVOL. Não há etapas adicionais são necessárias.  
  
> [!NOTE]
>  Se você também estiver reinstalando o sistema operacional Windows Server 2003, você pode ou não pode ingressar o computador ao domínio e você pode dar qualquer nome para o computador durante a instalação do sistema operacional. Não instale o Active Directory. Após a reinstalação do sistema operacional, vá diretamente para a etapa 4.  
  
 Em controladores de domínio do Windows Server 2003 onde você restaurou somente os dados de estado do sistema, você precisa também reinstalar os aplicativos de software que estavam sendo executados em controladores de domínio antes da recuperação. Restaurar o AD DS no primeiro controlador de domínio no domínio também restaura o registro, pois ambas fazem parte dos dados de estado do sistema. Tenha isso em mente se você tinha aplicativos em execução nesses controladores de domínio e tinham que as informações armazenadas no registro.  
  
 Para economizar tempo necessário para reinstalar o software, determine se os aplicativos que precisam ser instaladas em controladores de domínio são compatíveis com virtual DC clonagem. Esses aplicativos podem ser instalados na fonte de DC antes de clonagem para economizar tempo e esforço necessário para instalá-los em controladores de domínio virtuais clonados.  
  
#### <a name="to-perform-a-nonauthoritative-restore"></a>Para executar uma restauração sem autorização  
  
1.  Depois que você inicia o controlador de domínio, pressione F8 para reiniciar o computador no modo de restauração dos serviços de diretório (DSRM).  
  
2.  Selecione **modo de restauração de serviços de diretório (somente controladores de domínio Windows)**.  
  
3.  Selecione o sistema operacional que você deseja iniciar no modo de restauração.  
  
4.  Faça logon como um administrador (você só pode usar uma conta de computador local, nenhuma opção de logon de domínio estiver disponível).  
  
5.  Em um prompt de comando, digite **ntbackup**, e pressione ENTER.  
  
6.  Sobre o **boas-vindas** página, clique em **modo avançado**e, em seguida, selecione o **restaurar e gerenciar mídia** guia (não selecione **Assistente para restauração do**.)  
  
7.  Selecione o arquivo de backup apropriado para restaurar a partir e garantir que o **disco do sistema** e **estado do sistema** caixas de seleção são selecionadas.  
  
8.  Clique em **Iniciar restauração**.  
  
9. Quando a operação de restauração estiver concluída, reinicie o computador.  
  
 Use o procedimento a seguir para executar uma restauração (também conhecido como primária) autorizada SYSVOL em um controlador de domínio que executa o Windows Server 2003. Execute este procedimento somente no primeiro Windows Server 2003 DC que é restaurado no domínio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Para executar uma restauração autorizada do SYSVOL  
  
1.  Execute as etapas 1 a 8 no procedimento anterior.  
  
2.  No **Confirmar restauração** caixa de diálogo, clique em **avançado**.  
  
3.  Para executar uma restauração autorizada do SYSVOL, marque a caixa de seleção **ao restaurar conjuntos de dados duplicados, marcar os dados restaurados como dados primários para todas as réplicas**.  
  
    > [!NOTE]
    >  Marcar os dados restaurados como dados primários no Backup equivale à configuração o **BurFlags** entrada para D4 sob a subchave de registro:  
    >   
    >  **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative réplica Sets\\***GUID*  
  
4.  Quando a operação de restauração estiver concluída, reinicie o computador.  
  
 
## <a name="install-and-configure-the-dns-server-service"></a>Instalar e configurar o serviço de servidor DNS  
 Se o controlador de domínio que você restaurado a partir do backup estiver executando o Windows Server 2003, você pode instalar o servidor DNS sem precisar se conectar o controlador de domínio a qualquer rede.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Para instalar e configurar o serviço de servidor DNS  
  
1.  Abrir o Assistente de componentes do Windows. Para abrir o Assistente:  
  
    -   Clique em **iniciar**, clique em **painel de controle**e clique em **adicionar ou remover programas**.  
  
    -   Clique em **Adicionar/remover componentes do Windows**.  
  
2.  Em **componentes**, selecione o **serviços de rede** caixa de seleção e, em seguida, clique em **detalhes**.  
  
3.  Em **subcomponentes de serviços de rede**, selecione o **sistema de nome de domínio (DNS)** caixa de seleção, clique em **Okey**e clique em **próxima**.  
  
4.  Se você for solicitado, em **copiar os arquivos de**, digite o caminho completo dos arquivos de distribuição e clique em **Okey**.  
  
     Após a instalação, conclua as seguintes etapas para configurar o servidor DNS.  
  
5.  Clique em **iniciar**, aponte para **todos os programas**, aponte para **ferramentas administrativas**e clique em **DNS**.  
  
6.  Crie zonas DNS para os mesmos nomes de domínio DNS que eram hospedados nos servidores DNS antes do mau funcionamento crítico. Para obter mais informações, consulte Adicionar uma zona de pesquisa direta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
  
7.  Configure os dados DNS que existia antes do mau funcionamento crítico. Por exemplo:  
  
    -   Configure zonas DNS a serem armazenadas no AD DS. Para obter mais informações, consulte alterar o tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configure a zona DNS autoritativo para registros de recurso de localizador DC de controlador de domínio permitir atualizações dinâmicas seguras. Para obter mais informações, consulte permitir somente atualizações dinâmicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
8.  Certifique-se de que a zona DNS pai contém registros de recurso delegação (nome do servidor (NS) e cola recursos do host (A) registros) para a zona filho que está hospedado no servidor DNS. Para obter mais informações, consulte Criar uma delegação de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
  
9. Depois de configurar DNS, no prompt de comando, digite o seguinte comando e pressione ENTER:  
  
     **Net stop netlogon**  
  
10. Digite o seguinte comando e pressione ENTER:  
  
     **Net start netlogon**  
  
    > [!NOTE]
    >  Logon de rede registrará os registros de recurso localizador de DC no DNS para esse controlador de domínio. Se você estiver instalando o serviço de servidor DNS em um servidor no domínio filho, esse controlador de domínio não será capaz de registrar seus registros imediatamente. Isso é porque ele é isolado atualmente como parte do processo de recuperação e seu servidor DNS primário é o servidor DNS raiz da floresta. Configure o computador com o mesmo endereço IP que tinha antes do erro para evitar falhas de pesquisa do serviço de DC.

## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
