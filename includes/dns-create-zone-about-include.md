K hostování záznamů DNS v určité doméně se používá zóna DNS. Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.

Například doména contoso.com může obsahovat několik záznamů DNS, třeba mail.contoso.com (pro poštovní server) a www.contoso.com (pro web).

Při vytváření zóny DNS v DNS Azure:

* Název zóny musí být v rámci skupiny prostředků jedinečný a zóna ještě nesmí existovat. Jinak se operace nezdaří.
* V jiné skupině prostředků nebo v rámci jiného předplatného Azure se dá znovu použít stejný název zóny.
* Pokud má několik zón stejný název, každé instanci se přiřadí jiná adresa názvového serveru. U registrátora názvu domény je možné nakonfigurovat pouze jednu sadu adres.

> [!NOTE]
> Pro vytvoření zóny DNS s názvem domény v DNS Azure nemusíte tento název domény vlastnit. Doménu ale musíte vlastnit, pokud chcete nakonfigurovat názvové servery DNS Azure jako správné názvové servery pro daný název domény u registrátora názvu domény.
> 
> Další informace najdete v tématu [Delegování domény do DNS Azure](../articles/dns/dns-domain-delegation.md).