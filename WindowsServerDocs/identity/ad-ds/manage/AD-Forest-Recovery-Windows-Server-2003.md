---
title: Recuperação de floresta do AD - recuperação do Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: bd15df5360a50e417881d83319344dbdf48f35fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829637"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Recuperação de floresta do AD - recuperação do Windows Server 2003

>Aplica-se a: Windows Server 2003

Este tópico inclui procedimentos de recuperação de floresta para controladores de domínio (DCs) que executam o Windows Server 2003. O processo geral para recuperação de floresta não é diferente com controladores de domínio do Windows Server 2003, mas os procedimentos específicos podem ser diferentes por causa de ferramentas diferentes. Por exemplo, Ntdsutil.exe pode ser usado para fazer backup e restaurar controladores de domínio que executam o Windows Server 2003 os controladores de domínio, enquanto o Backup do Windows Server ou Wbadmin.exe é usado para controladores de domínio que executam o Windows Server 2008 ou posterior.  
  
- [Backup de dados de estado do sistema](#Backing-up-the-System-State-data)  
- [Executar uma restauração não autoritativa](#Performing-a-nonauthoritative restore)  
- [Instalar e configurar o serviço servidor DNS](#Install-and-configure-the-DNS-Server-service)  

## <a name="backing-up-the-system-state-data"></a>Backup de dados de estado do sistema
Use o procedimento a seguir para fazer backup dos dados de estado do sistema, junto com quaisquer outros dados que você selecionou para a operação de backup atual, de um controlador de domínio que executa o Windows Server 2003. Windows Server 2003 inclui a ferramenta Ntbackup, que você pode usar para fazer backup dos dados de estado do sistema.  
  
Associação na **administradores** ou **operadores de Backup**, ou equivalente, é o mínimo necessário para fazer backup de arquivos e pastas.   
  
Se você estiver fazendo backup dos dados de estado do sistema em uma fita e o programa de Backup indica que não há nenhuma mídia disponível, você talvez precise usar o armazenamento removível. Isso adiciona a fita para o pool de mídia livre para que o Backup possa ser usado.  
  
Você só pode fazer backup dos dados de estado do sistema em um computador local. Ele não pode ser fazer backup em um computador remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Para fazer backup dos dados de estado do sistema em um controlador de domínio que executa o Windows Server 2003  
  
1. Clique em **inicie**, aponte para **todos os programas**, aponte para **Acessórios**, aponte para **ferramentas do sistema**e, em seguida, clique em **Backup** .  
2. Sobre o **bem-vindo** , clique em **modo avançado**.  
3. Sobre o **Backup** guia, marque a caixa de seleção para qualquer unidade, pasta ou arquivo que você deseja fazer backup.  
4. Selecione o **estado do sistema** caixa de seleção.  
5. Clique em **iniciar Backup**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Executar uma restauração não autoritativa  

Use o procedimento a seguir para executar uma restauração não autoritativa de um controlador de domínio que executa o Windows Server 2003. Ao realizar uma restauração não autoritativa do Active Directory no Windows Server 2003, você automaticamente executar uma restauração não autoritativa de SYSVOL. Nenhuma etapa adicional é necessária.  
  
> [!NOTE]
> Se você também estiver reinstalando o sistema operacional Windows Server 2003, você pode ou não pode ingressar o computador ao domínio e você pode atribuir qualquer nome para o computador durante a instalação do sistema operacional. Não instale o Active Directory. Após a reinstalação do sistema operacional, vá diretamente para a etapa 4.  
  
Em controladores de domínio do Windows Server 2003 onde você restaurou somente os dados de estado do sistema, você precisará também reinstalar os aplicativos de software que estavam em execução em controladores de domínio antes da recuperação. Restaurar o AD DS no primeiro controlador de domínio no domínio também restaura o registro, porque são parte dos dados de estado do sistema. Tenha isso em mente, se você tiver quaisquer aplicativos em execução nesses controladores de domínio e se ela teve todas as informações armazenadas no registro.  
  
Para salvar o tempo necessário para instalar o software novamente, determine se os aplicativos que precisam ser instalados nos controladores de domínio são compatíveis com a clonagem do controlador de domínio virtual. Esses aplicativos podem ser instalados no DC de origem antes da clonagem para economizar tempo e esforço necessário para instalá-los nos controladores de domínio virtuais clonados.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Para executar uma restauração não autoritativa
  
1. Depois de iniciar o controlador de domínio, pressione F8 para reiniciar o computador no Directory Services Restore DSRM (modo).  
2. Selecione **Directory Services Restore Mode (somente para controladores de domínio Windows)**.  
3. Selecione o sistema operacional que você deseja iniciar no modo de restauração.  
4. Faça logon como um administrador (você só pode usar uma conta do computador local, nenhuma opção de logon de domínio está disponível).  
5. Em um prompt de comando, digite **ntbackup**, e pressione ENTER.  
6. Sobre o **bem-vindo** , clique em **modo avançado**e, em seguida, selecione o **restaurar e gerenciar mídia** guia. (Não selecione **Assistente de restauração**.)  
7. Selecione o arquivo de backup apropriado para restaurar a partir e certifique-se de que o **disco do sistema** e **estado do sistema** caixas de seleção estão marcadas.  
8. Clique em **Iniciar restauração**.  
9. Quando a operação de restauração for concluída, reinicie o computador.  
  
Use o procedimento a seguir para executar uma restauração autoritativa (também conhecido como primária) do SYSVOL em um controlador de domínio que executa o Windows Server 2003. Execute este procedimento somente no primeiro controlador de domínio do Windows Server 2003 ao que está sendo restaurado no domínio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Para executar uma restauração autoritativa do SYSVOL  
  
1. Execute as etapas 1 a 8 no procedimento anterior.  
2. No **Confirmar restauração** caixa de diálogo, clique em **avançado**.  
3. Para executar uma restauração autoritativa do SYSVOL, marque a caixa de seleção **ao restaurar conjuntos de dados duplicados, marcar os dados restaurados como dados primários para todas as réplicas**.  

   > [!NOTE]
   > Marcar os dados restaurados, como os dados primários no Backup são equivalentes à configuração de **BurFlags** entrada para D4 na seguinte subchave do registro:  
   >   
   > **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\\** *GUID*  

4. Quando a operação de restauração for concluída, reinicie o computador.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Instalar e configurar o serviço servidor DNS

Se o controlador de domínio restaurado do backup está executando o Windows Server 2003, você pode instalar o servidor DNS sem conectar o controlador de domínio a qualquer rede.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Para instalar e configurar o serviço servidor DNS  
  
1. Abrir o Assistente de componentes do Windows. Para abrir o Assistente:  

   - Clique em **Iniciar**, clique em **Painel de Controle** e clique em **Adicionar ou Remover Programas**.  
   - Clique em **adicionar ou remover componentes do Windows**.  

2. Na **componentes**, selecione o **serviços de rede** caixa de seleção e, em seguida, clique em **detalhes**.  
3. Na **subcomponentes de serviços de rede**, selecione o **sistema de nome de domínio (DNS)** caixa de seleção, clique em **Okey**e, em seguida, clique em **Avançar**.  
4. Se você for solicitado, na **copiar arquivos**, digite o caminho completo dos arquivos de distribuição e, em seguida, clique em **Okey**.  

   Após a instalação, conclua as seguintes etapas para configurar o servidor DNS.  

5. Clique em **inicie**, aponte para **todos os programas**, aponte para **ferramentas administrativas**e, em seguida, clique em **DNS**.  
6. Crie zonas DNS para os mesmos nomes de domínio DNS que foram hospedadas nos servidores DNS antes do mal funcionamento crítico. Para obter mais informações, consulte Adicionar uma zona de pesquisa direta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configure os dados DNS conforme se encontravam antes do mal funcionamento crítico. Por exemplo:   

   - Configure zonas DNS a ser armazenado no AD DS. Para obter mais informações, consulte alterar o tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configure a zona DNS que está autorizada para registros de recursos de localizador (localizador de controlador de domínio) de controlador de domínio permitir que a atualização dinâmica segura. Para obter mais informações, consulte permitir somente atualizações dinâmicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Certifique-se de que a zona DNS pai contém registros de recurso delegação (nome do servidor (NS) e cola recursos do host (A) registros) para a zona filho que é hospedado neste servidor DNS. Para obter mais informações, consulte Criar uma delegação de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Depois de configurar o DNS, no prompt de comando, digite o seguinte comando e pressione ENTER:  

   **net stop netlogon**

10. Digite o seguinte comando e pressione ENTER:  

   **net start netlogon**

   > [!NOTE]
   > Logon de rede registrará os registros de recursos do localizador do controlador de domínio no DNS para esse controlador de domínio. Se você estiver instalando o serviço servidor DNS em um servidor no domínio filho, esse controlador de domínio não poderão registrar seus registros de imediatamente. Isso ocorre porque ele é isolado atualmente como parte do processo de recuperação e seu servidor DNS primário é o servidor DNS de raiz de floresta. Configure este computador com o mesmo endereço IP conforme ele tinha antes do desastre para evitar falhas de pesquisa de serviço do controlador de domínio.

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
