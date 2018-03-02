Po vytvoření vlastních kořenových certifikátů, exportujte soubor veřejného klíče .cer kořenového certifikátu (ne privátní klíč). Tento soubor později odešlete do Azure. Následující postup můžete exportovat soubor .cer pro podepsané svým držitelem kořenový certifikát:

1. Chcete-li získat soubor .cer z certifikátu, otevřete **správu uživatelských certifikátů**. Vyhledejte kořenový certifikát podepsaný svým držitelem, obvykle v Certificates - Current User\Personal\Certificates, a klikněte pravým tlačítkem myši. Klikněte na **Všechny úlohy** a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem certifikátu**.

  ![Export](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. V průvodci klikněte na **Další**.

  ![Export certifikátu](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. Vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.

  ![Neexportovat privátní klíč](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER)** a klikněte na **Další**.

  ![Kódování Base-64](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. Pro **soubor pro Export**, **Procházet** do umístění, do které chcete exportovat certifikát. V části **Název souboru** zadejte název souboru. Pak klikněte na **Další**.

  ![Procházet](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Certifikát vyexportujte kliknutím na **Dokončit**.

  ![Dokončit](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Certifikát se úspěšně exportovány.

  ![Úspěch](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Exportovaný certifikát vypadá podobně jako tento:

  ![Exportováno](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Pokud otevřete exportovaný certifikát pomocí poznámkového bloku, zobrazí se něco podobného jako v tomto příkladu. V části modře obsahuje informace, které se nahraje do Azure. Pokud otevřete svůj certifikát programu Poznámkový blok a není podobat tomuto příkladu, obvykle to znamená není ho exportovat pomocí kódování Base-64 formát X.509 (. Formátu CER). Pokud chcete použít v jiném textovém editoru, Pochopte, že některé editory můžou vést k neočekávaným formátování na pozadí. To může způsobit problémy při odeslání text z tento certifikát do Azure.

  ![Otevřít v programu Poznámkový blok](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)