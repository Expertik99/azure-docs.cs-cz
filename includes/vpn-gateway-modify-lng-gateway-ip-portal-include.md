### <a name="gwipnoconnection"></a>Chcete-li upravit IP adresu brány místní sítě – žádné připojení brány

Pomocí tohoto příkladu upravte bránu místní sítě, která nemá připojení brány. Při upravování této hodnoty můžete také zároveň upravit předpony adres.

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.
2. V **IP adresu** pole, je třeba změnit IP adresu.
3. Klikněte na tlačítko **Uložit** uložte nastavení.

### <a name="gwipwithconnection"></a>Chcete-li upravit místní síťová brána brány IP adresa - existující připojení brány

Pokud chcete upravit bránu místní sítě, který má připojení, musíte nejdříve odebrat připojení. Po odebrání připojení můžete upravit IP adresu brány a vytvořit nové připojení. Můžete také zároveň upravit předpony adresy. Způsobí to určitý výpadek připojení VPN. Při upravování IP adresy brány není potřeba odstraňovat bránu VPN. Stačí jenom odebrat připojení.
 
#### <a name="1-remove-the-connection"></a>1. Odeberte připojení.

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **připojení**.
2. Klikněte **...**  na řádek pro připojení klikněte **odstranit**.
3. Klikněte na tlačítko **Uložit** uložte nastavení.

#### <a name="2-modify-the-ip-address"></a>2. Upravte IP adresu.

Můžete také zároveň upravit předpony adresy.

1. V **IP adresu** pole, je třeba změnit IP adresu.
2. Klikněte na tlačítko **Uložit** uložte nastavení.

#### <a name="3-recreate-the-connection"></a>3. Znovu vytvořte připojení.

1. Přejděte k bráně virtuální sítě pro virtuální síť. (Nikoli brány místní sítě.)
2. Na bráně virtuální sítě v **nastavení** klikněte na tlačítko **připojení**.
3. Klikněte **+ přidat** otevřete **přidat připojení** okno.
4. Znovu vytvořte připojení.
5. Klikněte na tlačítko **OK** k vytvoření připojení.