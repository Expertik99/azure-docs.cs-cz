---
title: "Vytvoření služby application gateway s pravidel adres URL na základě cestu směrování - portálu Azure | Microsoft Docs"
description: "Naučte se vytvořit adresu URL na základě cesty pravidla směrování pro aplikační bránu a škálování virtuálních počítačů, nastavit pomocí portálu Azure."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: davidmu
ms.openlocfilehash: 62063c42ab15a071a4500417a5d8adf6bfeac97f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Vytvoření služby application gateway s cesta pravidla založená na směrování pomocí portálu Azure

Na portálu Azure můžete použít ke konfiguraci [pravidla směrování na základě cesty adresy URL](application-gateway-url-route-overview.md) při vytváření [Aplikační brána](application-gateway-introduction.md). V tomto kurzu vytvoříte back-endové fondy používání virtuálních počítačů. Potom můžete vytvořit pravidla směrování pro Ujistěte se, že web přenos dorazí na příslušné servery ve fondech.

V tomto článku se dozvíte, jak:

> [!div class="checklist"]
> * Vytvoření služby Application Gateway
> * Vytváření virtuálních počítačů pro back-end serverů
> * Vytvoření back-endové fondy s back-end serverů
> * Vytvořit naslouchací proces back-end
> * Vytvoření pravidla směrování na základě cesty

![Příklad směrování URL](./media/application-gateway-create-url-route-portal/scenario.png)

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k portálu Azure v [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Virtuální síť je požadován pro komunikaci mezi prostředky, které vytvoříte. V tomto příkladu jsou vytvořeny dvě podsítě: jednu pro aplikační bránu a druhou pro back-end serverů. Můžete vytvořit virtuální síť ve stejnou dobu, kterou vytvoříte službu application gateway.

1. Klikněte na tlačítko **nový** najít v levém horním rohu portálu Azure.
2. Vyberte **sítě** a pak vyberte **Application Gateway** v seznamu doporučený.
3. Pro aplikační bránu, zadejte tyto hodnoty:

    - *myAppGateway* – pro název služby application gateway.
    - *myResourceGroupAG* – pro novou skupinu prostředků.

    ![Vytvořte novou aplikační bránu](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Přijměte výchozí hodnoty u ostatních nastavení a potom klikněte na **OK**.
5. Klikněte na tlačítko **vyberte virtuální síť**, klikněte na tlačítko **vytvořit nový**a potom zadejte tyto hodnoty pro virtuální síť:

    - *myVNet* – pro název virtuální sítě.
    - *10.0.0.0/16* – pro adresní prostor virtuální sítě.
    - *myAGSubnet* – název podsítě.
    - *10.0.0.0/24* – adresního prostoru podsítě.

    ![Vytvoření virtuální sítě](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Klikněte na tlačítko **OK** k vytvoření virtuální sítě a podsítě.
7. Klikněte na tlačítko **zvolte veřejnou IP adresu**, klikněte na tlačítko **vytvořit nový**a potom zadejte název veřejné IP adresy. V tomto příkladu je název veřejné IP adresy *myAGPublicIPAddress*. Přijměte výchozí hodnoty u ostatních nastavení a potom klikněte na **OK**.
8. Přijměte výchozí hodnoty pro konfiguraci naslouchacího procesu nechte zakázáno brány firewall webových aplikací a pak klikněte na tlačítko **OK**.
9. Zkontrolujte nastavení na stránce Souhrn a pak klikněte na tlačítko **OK** vytvoření síťové prostředky a aplikační brány. Ho může trvat několik minut, než aplikační brány, aby lze vytvořit, počkejte na dokončení nasazení přejde k další části úspěšně.

### <a name="add-a-subnet"></a>Přidat podsíť

1. Klikněte na tlačítko **všechny prostředky** v levé nabídce a pak klikněte na tlačítko **myVNet** ze seznamu prostředků.
2. Klikněte na tlačítko **podsítě**a potom klikněte na **podsítě**.

    ![Vytvoření podsítě](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Zadejte *myBackendSubnet* pro název podsítě a pak klikněte na tlačítko **OK**.

## <a name="create-virtual-machines"></a>Vytvoření virtuálních počítačů

V tomto příkladu můžete vytvořit tři virtuální počítače, který se má použít jako back-end serverů pro službu application gateway. Je také nainstalovat službu IIS na virtuálních počítačích, chcete-li ověřit, že aplikační brány byl úspěšně vytvořen.

1. Klikněte na možnost **Nové**.
2. Klikněte na tlačítko **výpočetní** a pak vyberte **Windows Server 2016 Datacenter** v seznamu doporučený.
3. Pro virtuální počítač, zadejte tyto hodnoty:

    - *myVM1* – pro název virtuálního počítače.
    - *azureuser* – pro uživatelské jméno správce.
    - *Azure123456!* pro heslo.
    - Vyberte **použít existující**a potom vyberte *myResourceGroupAG*.

4. Klikněte na **OK**.
5. Vyberte **DS1_V2** pro velikost virtuálního počítače, a klikněte na tlačítko **vyberte**.
6. Ujistěte se, že **myVNet** je vybraná pro virtuální síť a podsíť je **myBackendSubnet**. 
7. Klikněte na tlačítko **zakázané** zakázat Diagnostika spouštění.
8. Klikněte na tlačítko **OK**, zkontrolujte nastavení na stránce Souhrn a pak klikněte na tlačítko **vytvořit**.

### <a name="install-iis"></a>Instalace služby IIS

1. Otevřete prostředí pro interaktivní a ujistěte se, že je nastavena na **prostředí PowerShell**.

    ![Instalace vlastní rozšíření](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Spusťte následující příkaz pro instalaci služby IIS na virtuálním počítači: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Vytvořte dva další virtuální počítače a instalace služby IIS pomocí kroky, které právě dokončila. Zadejte názvy *Můjvp2* a *myVM3* pro názvy a hodnoty VMName v AzureRmVMExtension sady.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Vytvoření back-endové fondy s virtuálními počítači

1. Klikněte na tlačítko **všechny prostředky** a pak klikněte na **myAppGateway**.
2. Klikněte na tlačítko **back-endové fondy**. Výchozí fond byl automaticky vytvořen s aplikační brány. Klikněte na tlačítko **appGatewayBackendPool**.
3. Klikněte na tlačítko **přidat cíl** přidat *myVM1* k appGatewayBackendPool.

    ![Přidat back-end serverů](./media/application-gateway-create-url-route-portal/application-gateway-backend.png)

4. Klikněte na **Uložit**.
5. Klikněte na tlačítko **back-endové fondy** a pak klikněte na **přidat**.
6. Zadejte název *imagesBackendPool* a přidejte *Můjvp2* pomocí **přidat cíl**.
7. Klikněte na **OK**.
8. Klikněte na tlačítko **přidat** znovu a přidat jiného fondu back-end s názvem *videoBackendPool* a přidejte *myVM3* k němu.

## <a name="create-a-backend-listener"></a>Vytvořit naslouchací proces back-end

1. Klikněte na tlačítko **naslouchací procesy** a kliknutím na možnost **základní**.
2. Zadejte *myBackendListener* pro název, *myFrontendPort* pro název front-endový port a potom *8080* jako port pro naslouchací proces.
3. Klikněte na **OK**.

## <a name="create-a-path-based-routing-rule"></a>Vytvoření pravidla směrování na základě cesty

1. Klikněte na tlačítko **pravidla** a pak klikněte na **na základě cesty**.
2. Zadejte *rule2* pro název.
3. Zadejte *bitové kopie* pro název první cesty. Zadejte */images/** pro cestu. Vyberte **imagesBackendPool** pro fond back-end.
4. Zadejte *Video* pro název druhé cesty. Zadejte */video/** pro cestu. Vyberte **videoBackendPool** pro fond back-end.

    ![Vytvoření pravidla, na základě cesty](./media/application-gateway-create-url-route-portal/application-gateway-route-rule.png)

5. Klikněte na **OK**.

## <a name="test-the-application-gateway"></a>Testování služby application gateway

1. Klikněte na tlačítko **všechny prostředky**a potom klikněte na **myAGPublicIPAddress**.

    ![Zaznamenejte veřejná IP adresa brány aplikace](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Zkopírujte veřejnou IP adresu a pak ji vložit do panelu Adresa prohlížeče. Například http://http: / / 40.121.222.19.

    ![Otestovat základní adresu URL v aplikační brány](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. Změňte adresu URL k http://&lt;ip adresu&gt;: 8080/video/test.htm, nahraďte &lt;ip adresu&gt; s IP adresu a měli vidět něco podobného jako v následujícím příkladu:

    ![Testovací adresu URL bitové kopie v aplikační brány](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. Změňte adresu URL k http://&lt;ip adresu&gt;: 8080/video/test.htm, nahraďte &lt;ip adresu&gt; s IP adresu a měli vidět něco podobného jako v následujícím příkladu:

    ![Adresa URL videa testu v aplikační brány](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Další postup

V tomto článku jste se dozvěděli, jak

> [!div class="checklist"]
> * Vytvoření služby Application Gateway
> * Vytváření virtuálních počítačů pro back-end serverů
> * Vytvoření back-endové fondy s back-end serverů
> * Vytvořit naslouchací proces back-end
> * Vytvoření pravidla směrování na základě cesty

Další informace o aplikačních bran a jejich přidružené prostředky, i nadále články s návody.
