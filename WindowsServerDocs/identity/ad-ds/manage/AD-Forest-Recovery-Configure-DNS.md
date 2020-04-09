---
title: Recuperação de floresta do AD-configurar o serviço do servidor DNS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 144a45f2a835d9cca60b5be5aac7569809c45b7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824169"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Recuperação de floresta do AD-Configurando o serviço do servidor DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Se a função de servidor DNS não estiver instalada no controlador de domínio que você restaura do backup, você deverá instalar e configurar o servidor DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Instalar e configurar o serviço do servidor DNS

Conclua esta etapa para cada controlador de domínio restaurado que não está sendo executado como um servidor DNS após a conclusão da restauração. 

> [!NOTE]
> Se o DC que você restaurou do backup estiver executando o Windows Server 2008 R2, você deverá conectar o controlador de domínio a uma rede isolada para instalar o servidor DNS. Em seguida, conecte cada um dos servidores DNS restaurados a uma rede isolada e compartilhada mutuamente. Execute repadmin/replsum para verificar se a replicação está funcionando entre os servidores DNS restaurados. Depois de verificar a replicação, você poderá conectar os DCs restaurados à rede de produção se a função de servidor DNS já estiver instalada, você poderá aplicar um hotfix que possibilita a inicialização de um servidor DNS enquanto o servidor não estiver conectado a nenhuma rede. Você deve corrigir o hotfix na imagem de instalação do sistema operacional durante os processos de compilação automatizados. Para obter mais informações sobre o hotfix, consulte o [artigo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=184691). 

Conclua as etapas de instalação e configuração abaixo.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Para instalar o e o serviço do servidor DNS usando Gerenciador do Servidor  

1. Abra Gerenciador do Servidor e clique em **adicionar funções e recursos**. 
2. No Assistente para Adicionar Funções, se for exibida a página **Antes de Começar**, clique em **Avançar**. 
3. Na tela **tipo de instalação** , selecione instalação baseada em **função ou recurso** e clique em **Avançar**.
4. Na tela de **seleção do servidor** , selecione o servidor e clique em **Avançar**.
5. Na tela **funções do servidor** , selecione **servidor DNS**, se solicitado, clique em **Adicionar recursos** e clique em **Avançar**.
6. Na tela **recursos** , clique em **Avançar**.
7. Leia as informações na página do **servidor DNS** e clique em **Avançar**.
   ![servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. Na página **confirmação** , verifique se a função servidor DNS será instalada e clique em **instalar**. 

### <a name="to-configure-the-dns-server-service"></a>Para configurar o serviço do servidor DNS

1. Abra Gerenciador do Servidor, clique em **ferramentas** e clique em **DNS**.
   ![servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Crie zonas DNS para os mesmos nomes de domínio DNS que foram hospedados nos servidores DNS antes do mau funcionamento crítico. Para obter mais informações, consulte Adicionar uma zona de pesquisa direta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configure os dados DNS como existiam antes do mau funcionamento crítico. Por exemplo:  

   - Configure as zonas DNS a serem armazenadas no AD DS. Para obter mais informações, consulte alterar o tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configure a zona DNS que é autoritativa para registros de recurso do localizador de controlador de domínio (localizador de DC) para permitir atualização dinâmica segura. Para obter mais informações, consulte permitir somente atualizações dinâmicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Verifique se a zona DNS pai contém registros de recurso de delegação (servidor de nomes (NS) e registros de recurso de host de União (A)) para a zona filho hospedada nesse servidor DNS. Para obter mais informações, consulte criar uma delegação de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Depois de configurar o DNS, você pode acelerar o registro dos registros NETLOGON.

   > [!NOTE]
   > As atualizações dinâmicas seguras funcionam apenas quando um servidor de catálogo global está disponível. 

   No prompt de comando, digite o seguinte comando e pressione ENTER:  

   **net stop Netlogon**  

6. Digite o seguinte comando e pressione ENTER:  

   **net start Netlogon**  

   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
