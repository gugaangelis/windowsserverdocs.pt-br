---
title: Recuperação de floresta do AD - pools RID gerando
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: c8f91226e10ea6681933d5a5dc00b92f5ab2179c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862977"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Recuperação de floresta do AD - gerar o valor de pools RID disponíveis 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para aumentar o valor da ID relativo (RID) que o mestre de operações do RID alocará depois que esse controlador de domínio é restaurado de pools. Ao aumentar o valor dos pools RID disponíveis, você pode garantir que nenhum controlador de domínio aloca um RID para uma entidade de segurança que foi criada após o backup que foi usado para restaurar o domínio. 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Sobre Pools de RID do Active Directory e rIDAvailablePool

Cada domínio possui um objeto **CN = RID Manager$, CN = System, DC**=<*domain_name*>. Esse objeto tem um atributo chamado **rIDAvailablePool**. Esse valor de atributo mantém o espaço RID global para um domínio inteiro. O valor é um inteiro grande com partes superior e inferior. A parte superior define o número de entidades de segurança que podem ser alocados para cada domínio (0x3FFFFFFF ou pouco mais de 1 bilhão). A parte inferior é o número de RIDs foram alocados no domínio. 
  
> [!NOTE]
> No Windows Server 2016 e 2012, o número de entidades de segurança que podem ser alocados é aumentado para pouco mais de 2 bilhões. Para obter mais informações, consulte [a emissão de RID Gerenciando](https://technet.microsoft.com/library/jj574229.aspx). 
  
- Valor de exemplo: 4611686014132422708  
- Parte inferior: 2100 (início do próximo pool RID a ser alocado)  
- Parte superior: 1073741823 (número total de RIDs que podem ser criados em um domínio)  
  
Quando você aumenta o valor do inteiro grande, você pode aumentar o valor da parte de baixo. Por exemplo, se você adicionar 100.000 como o valor de exemplo de 4611686014132422708 uma soma de 4611686014132522708, a nova parte baixa é 102100. Isso indica que o pool RID próxima que será alocado pelo mestre de RID começa com 102100 em vez de 2100. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Para aumentar o valor de disponíveis pools RID usando adsiedit e a Calculadora

1. Abra o Gerenciador do servidor, clique em **ferramentas** e clique em **ADSI Edit**.
2. Clique com botão direito, selecione **se conectar ao** e conecte-se fazer o contexto de nomenclatura padrão e clique em **Okey**.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Navegue até o seguinte caminho de nome distinto: **CN = RID Manager$, CN = System, DC =<domain name>**.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. Clique com botão direito e selecione as propriedades de CN = RID Manager$. 
4. Selecione o atributo **rIDAvailablePool**, clique em **editar**e, em seguida, copie o valor de inteiro grande na área de transferência.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. Iniciar a Calculadora e para o **modo de exibição** menu, selecione **modo Científico**. 
6. Adicione 100.000 como o valor atual.
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. Usando ctrl-c, ou o **cópia** comando da **editar** menu, copie o valor para a área de transferência. 
8. Na caixa de diálogo Editar do adsiedit, cole esse novo valor. 
   ![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Clique em **Okey** na caixa de diálogo, e **Apply** na folha de propriedades para atualizar o **rIDAvailablePool** atributo. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Para aumentar o valor de pools RID disponíveis, usando o LDP  
  
1. No prompt de comando, digite o seguinte comando e pressione ENTER:  
   **ldp**  
2. Clique em **Conexão**, clique em **Connect**, digite o nome do Gerenciador de RID e, em seguida, clique em **Okey**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. Clique em **Conexão**, clique em **associar**, selecione **ligar com credenciais** e digite suas credenciais administrativas e, em seguida, clique em **Okey**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. Clique em **modo de exibição**, clique em **árvore** e, em seguida, digite o seguinte caminho de nome distinto:  CN = RID Manager$, CN = System, DC =*nome de domínio*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. Clique em **navegue**e, em seguida, clique em **modificar**. 
6. Adicionar 100.000 ao atual **rIDAvailablePool** valor e, em seguida, digite a soma em **valores**. 
7. Na **Dn**, digite `cn=RID Manager$,cn=System,dc=` *< nome de domínio\>*. 
8. Na **Editar atributo de entrada**, tipo `rIDAvailablePool`. 
9. Selecione **substituir** como a operação e clique **Enter**.
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Clique em **executar** para executar a operação. Clique em **Fechar**.
11. Para validar a alteração, clique em **modo de exibição**, clique em **árvore**e, em seguida, digite o seguinte caminho de nome distinto:   CN = RID Manager$, CN = System, DC =*nome de domínio*.   Verifique as **rIDAvailablePool** atributo. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
