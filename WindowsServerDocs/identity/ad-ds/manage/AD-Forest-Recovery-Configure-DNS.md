---
title: "Recuperação de floresta do AD - serviço de configuração de servidor DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f24570965fd8b3f3e050779c42758865cbee2728
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Recuperação de floresta do AD - Configurando o serviço de servidor DNS 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
Se a função de servidor DNS não estiver instalada no controlador de domínio que restaurar a partir de backup, você deve instalar e configurar o servidor DNS.  
  

## <a name="install-and-configure-the-dns-server-service"></a>Instalar e configurar o serviço de servidor DNS  
Conclua essa etapa para cada controlador de domínio restaurado que não está em execução como um servidor DNS após a restauração for concluída.  
  
> [!NOTE]
>  Se o controlador de domínio que você restaurado a partir do backup estiver executando o Windows Server 2008 R2, o controlador de domínio deverá ser conectado a uma rede isolada para instalar o servidor DNS. Conecte-se cada um dos servidores DNS restaurados a uma rede isolada, mutuamente compartilhada. Executar repadmin /replsum para verificar que a replicação está funcionando entre os servidores DNS restaurados. Depois de verificar replicação, você pode se conectar as controladores de domínio restaurados para a rede de produção se a função de servidor DNS já estiver instalada, você pode aplicar um hotfix que torna possível para um servidor DNS iniciar enquanto o servidor não estiver conectado a qualquer rede. Você deve corrigi-lo para a imagem de instalação do sistema operacional durante seus processos de compilação automatizada. Para obter mais informações sobre o hotfix, consulte [975654 do artigo](https://go.microsoft.com/fwlink/?LinkId=184691) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=184691). 

Conclua as etapas de instalação e configuração abaixo.
  
### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Para instalar e o serviço de servidor DNS usando o Gerenciador do servidor  
  
1.  Abra o Gerenciador do servidor e clique em **adicionar funções e recursos**.  
2.  No Assistente para adicionar funções, se o **Before You Begin** página será exibida, clique em **próxima**.  
3.  Sobre o **tipo de instalação** tela select **baseada em função ou recurso com base em instalação** e clique em **próxima**.
4.  Sobre o **seleção de servidor** tela Selecione o servidor e clique em **próxima**.
5.  Sobre o **funções de servidor** tela select **servidor DNS**, se solicitado click **adicionar recursos** e clique em **próxima**.
6.  Sobre o **recursos** tela click **próxima**.
7.  Leia as informações sobre o **servidor DNS** página e, em seguida, clique em **próxima**.
![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8.  Sobre o **confirmação** página, verifique se a função de servidor DNS será instalado e clique em **instalar**.  
  
     
### <a name="to-configure-the-dns-server-service"></a>Para configurar o serviço de servidor DNS 
1.  Abra o Gerenciador do servidor, clique em **ferramentas** e clique em **DNS**.
![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)    
2.  Crie zonas DNS para os mesmos nomes de domínio DNS que eram hospedados nos servidores DNS antes do mau funcionamento crítico. Para obter mais informações, consulte Adicionar uma zona de pesquisa direta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
3.  Configure os dados DNS que existia antes do mau funcionamento crítico. Por exemplo:  
  
    -   Configure zonas DNS a serem armazenadas no AD DS. Para obter mais informações, consulte alterar o tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configure a zona DNS autoritativo para registros de recurso de localizador DC de controlador de domínio permitir atualizações dinâmicas seguras. Para obter mais informações, consulte permitir somente atualizações dinâmicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
4. Certifique-se de que a zona DNS pai contém registros de recurso delegação (nome do servidor (NS) e cola recursos do host (A) registros) para a zona filho que está hospedado no servidor DNS. Para obter mais informações, consulte Criar uma delegação de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
5. Depois de configurar DNS, você pode agilizar o registro dos registros de logon de rede.  
  
    > [!NOTE]
    >  As atualizações dinâmicas só funcionam quando um servidor de catálogo global estiver disponível.  
  
     No prompt de comando, digite o seguinte comando e pressione ENTER:  
  
     **Net stop netlogon**  
  
6. Digite o seguinte comando e pressione ENTER:  
  
     **Net start netlogon**  

![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
