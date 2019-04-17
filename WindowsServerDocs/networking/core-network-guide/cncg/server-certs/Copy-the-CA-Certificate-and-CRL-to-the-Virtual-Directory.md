---
title: Copie o certificado da CA e CRL para o diretório Virtual
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e37bfce7f8cf33fd7fcb5e6227d783c28bd29d35
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copie o certificado da CA e CRL para o diretório Virtual

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para copiar o certificado da CA raiz lista de certificados revogados e Enterprise da sua autoridade de certificação para um diretório virtual em seu servidor Web e para garantir que o AD CS é configurado corretamente. Antes de executar os comandos a seguir, certifique-se de que você substitua nomes de diretório e o servidor com aqueles que são apropriados para a implantação.  
  
Para executar este procedimento, você deve ser um membro do **Admins. do domínio **.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Para copiar a lista de certificados revogados do CA1 para WEB1  
  
1.  Na CA1, execute o Windows PowerShell como administrador e, em seguida, publique o CRL com o seguinte comando:  
  
    - Tipo `certutil -crl`, e pressione ENTER.  
  
    - Para copiar o certificado da CA para o compartilhamento de arquivos em seu servidor Web, digite `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`, e pressione ENTER.  
    - Para copiar as listas de revogação de certificados para o compartilhamento de arquivos em seu servidor Web, digite `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, e pressione ENTER.  
  
2. Para reiniciar o AD CS, digite `Restart-Service certsvc`, e pressione ENTER.  
  
2.  Para verificar se seus locais de extensão CDP e AIA estão configurados corretamente, digite `pkiview.msc`, e pressione ENTER. O pkiview Enterprise PKI MMC é aberta.  
  
3.  Clique no nome da CA. Por exemplo, se o nome da CA é corp-CA1-CA, clique em **corp-CA1-CA **. No painel de detalhes, verifique se o **Status** valor para o **certificado da CA**, **AIA local #1**, e **CDP local #1** são todos **Okey **.  
  
A ilustração a seguir mostra o painel de resultados pkiview com o status de Okey para todos os itens.  
  
! [adcs_pkiviewmedia/adcs_pkiview.png)  
  
> [!IMPORTANT]  
> Se **Status** para qualquer item não está **Okey**, faça o seguinte:  
> -   Abra o compartilhamento em seu servidor Web para verificar o certificado e arquivos de lista de revogação de certificado foram copiados com êxito para o compartilhamento. Se eles não foram copiados com êxito para o compartilhamento, modificar seus comandos de cópia com a origem de arquivo correta e compartilhar destino e execute os comandos novamente.  
> -   Verifique se você inseriu os locais corretos para CDP e AIA na guia extensões CA Certifique-se de que não há nenhum espaço extra ou outros caracteres nos locais que você forneceu.  
> -   Verifique se que você copiou o certificado CA e CRL para o local correto no seu servidor Web, e se o local corresponde a localização que você forneceu para os locais CDP e AIA na autoridade de certificação.  
> -   Verifique se você definiu corretamente as permissões para a pasta virtual onde o certificado da CA e CRL são armazenados.  
  


