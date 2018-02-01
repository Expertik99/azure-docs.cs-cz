## <a name="create-a-service-principal"></a>Vytvoření instančního objektu

Pokud chcete vytvořit objekt služby přístup k registru systému kontejneru, můžete použít následující skript. Aktualizace `ACR_NAME` proměnné s názvem vaší registru kontejneru a volitelně `--role` hodnotu [az ad sp vytvořit pro rbac] [ az-ad-sp-create-for-rbac] příkaz udělit oprávnění jiný. Musíte mít [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nainstalovat pomocí tohoto skriptu.

Po spuštění skriptu si všimněte instanční objekt **ID** a **heslo**. Jakmile máte svoje přihlašovací údaje, můžete nakonfigurovat aplikace a služby k ověření vaší registru kontejneru jako objekt služby.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Použít stávající instanční objekt

Pokud chcete udělit přístup k registru do existující služby hlavní, je nutné přiřadit ke službě hlavní novou roli. Stejně jako u vytváření hlavní novou službu, můžete udělit pro vyžádání obsahu, zápis a čtení a přístup vlastníka.

Tento skript používá [vytvořit přiřazení role az] [ az-role-assignment-create] příkaz udělit *vyžádání* oprávnění k hlavní název služby zadáte v `SERVICE_PRINCIPAL_ID` proměnné. Upravit `--role` hodnotu, pokud chcete udělit různé úrovně přístupu.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

Můžete najít obě tyto skripty na Githubu, stejně jako verze pro prostředí PowerShell:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
