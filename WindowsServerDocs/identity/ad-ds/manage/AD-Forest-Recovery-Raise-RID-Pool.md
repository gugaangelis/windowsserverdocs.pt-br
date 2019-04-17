---
title: "Recuperação de floresta do AD - pools acionamento marca d'"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adfs
ms.openlocfilehash: e6b5dc8b9c0b701fe2cd1b0c88f7edc22802c393
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Recuperação de floresta do AD - aumentando o valor de pools RID disponíveis 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Use o procedimento a seguir para aumentar o valor do ID relativo (RID) pools o mestre de operações RID alocará depois DC é restaurado. Acionando o valor dos pools de RID disponíveis, você pode garantir que nenhum DC aloca um RID para uma entidade de segurança que foi criada após o backup que foi usado para restaurar o domínio.  
 
## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Sobre Pools de LIVRAR do Active Directory e rIDAvailablePool
 Cada domínio tem um objeto **CN = $RID Manager, CN = System, DC**=<*domain_name*>. Esse objeto tem um atributo chamado **rIDAvailablePool**. Esse valor de atributo mantém o espaço RID global para um domínio inteiro. O valor é um inteiro grande com partes superiores e inferiores. A parte superior define o número de objetos de segurança que pode ser alocado para cada domínio (0x3FFFFFFF ou apenas mais de 1 bilhão). A parte inferior é o número de RIDs que tenham sido alocados no domínio.  
  
> [!NOTE]
>  No Windows Server 2016 e 2012, o número de objetos de segurança que pode ser alocado é aumentado para aproximadamente 2 bilhões. Para obter mais informações, consulte [emissão Gerenciando d'](https://technet.microsoft.com/library/jj574229.aspx).  
  
-   Valor de exemplo: 4611686014132422708  
  
-   Parte inferior: 2100 (início do próximo pool RID para ser alocado)  
  
-   Parte superior: 1073741823 (número total de RIDs que podem ser criados em um domínio)  
  
 Quando você aumenta o valor do número inteiro grande, você pode aumentar o valor da parte de baixo. Por exemplo, se você adicionar 100.000 como o valor de exemplo de 4611686014132422708 para uma soma das 4611686014132522708, a parte de baixo novo é 102100. Isso indica que o próximo pool RID que será alocado pelo mestre RID começará com 102100 em vez de 2100.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator--"></a>Para aumentar o valor de pools RID disponíveis usando adsiedit e a Calculadora '  
1.  Abra o Gerenciador do servidor, clique em **ferramentas** e clique em **ADSI Edit**.    
2.  Clique com botão direito, selecione **se conectar a** e conecte-se fazer o contexto de nomenclatura padrão e clique em **Okey**.
![ADSI Edit](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Navegue até o seguinte caminho de nome diferenciado: **CN = $RID Manager, CN = System, DC =<domain name>**.
![ADSI Edit](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3.  Clique com botão direito e e selecione as propriedades de CN = RID Manager$.  
4.  Selecione o atributo **rIDAvailablePool**, clique em **editar**e, em seguida, copie o valor inteiro longo na área de transferência.
![ADSI Edit](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5.  Iniciar a Calculadora e o **exibição** menu, selecione **modo Científico**.  6.  Adicione 100.000 como o valor atual.  
![ADSI Edit](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7.  Usando ctrl-c, ou o **cópia** comando o **editar** menu, copie o valor para a área de transferência.  
8.  Na caixa de diálogo Editar de adsiedit, cole este novo valor. 
![ADSI Edit](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Clique em **Okey** na caixa de diálogo, e **aplicar** na folha de propriedades para atualizar o **rIDAvailablePool** atributo.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Para aumentar o valor de pools RID disponíveis usando LDP  
  
1.  No prompt de comando, digite o seguinte comando e pressione ENTER:  
  
     **Ldp**  
  
2.  Clique em **Conexão**, clique em **conectar**, digite o nome do Gerenciador de RID e, em seguida, clique em **Okey**.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3.  Clique em **Conexão**, clique em **associar**, selecione **ligar com credenciais** e digitar suas credenciais administrativas e clique em **Okey**.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4.  Clique em **exibição**, clique em **árvore** e, em seguida, digite o seguinte caminho de nome diferenciado: CN = RID Manager$, CN = System, DC =*nome de domínio*  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5.  Clique em **procurar**e clique em **modificar**.  
6.  Adicionar 100.000 a atual **rIDAvailablePool** valor e, em seguida, digite a soma em **valores **.  
7.  Em **Dn**, tipo `cn=RID Manager$,cn=System,dc=`*< domínio name\ >*.  
8.  Em **atributo de entrada editar**, tipo `rIDAvailablePool`.  
9. Selecione **substituir** como a operação e depois clique em **Enter **. </br>
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Clique em **executar** para executar a operação.  Clique em **fechar**.
11. Para validar a alteração, clique em **exibição**, clique em **árvore**e, em seguida, digite o seguinte caminho de nome diferenciado: CN = $RID Manager, CN = System, DC =*nome de domínio *.    Verifique o **rIDAvailablePool** atributo.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
 
