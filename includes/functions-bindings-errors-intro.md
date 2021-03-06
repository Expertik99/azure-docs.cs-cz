Služba Azure Functions [aktivačními událostmi a vazbami](..\articles\azure-functions\functions-triggers-bindings.md) komunikovat s různými službami Azure. Při integraci s těmito službami, pravděpodobně vyvolá chyby, které pocházejí z rozhraní API podpůrné služby Azure. Může také dojít k chybám při pokusu o komunikaci s dalšími službami v kódu funkce pomocí REST nebo klientských knihoven. Pokud chcete nedošlo ke ztrátě dat a zajistit správné chování funkcí, je důležité pro zpracování chyb, buď z zdroje.

Následující aktivační události mají podporu předdefinován opakovaný pokus:

* [Azure Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Queue storage](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (fronty nebo tématu)](../articles/azure-functions/functions-bindings-service-bus.md)

Ve výchozím nastavení jsou tyto triggery opakována až pět x. Po páté opakování tyto triggery zapsat zprávu do speciální [nezpracovatelných fronty](..\articles\azure-functions\functions-bindings-storage-queue.md#trigger---poison-messages). 

Pro jiné funkce triggery neexistuje žádný předdefinován opakovaný pokus při výskytu chyby během provádění funkce. Aby se zabránilo ztrátě informací o aktivační události musí dojít k chybě ve funkci, doporučujeme použít try-catch – bloky v kódu funkce zachytit všechny chyby. Pokud dojde k chybě, zapisovat informace předaného do funkce má aktivační procedura do fronty zvláštní zpráva "nezpracovatelných". Tento přístup je stejný jako ten používané [aktivační událost objektů Blob storage](..\articles\azure-functions\functions-bindings-storage-blob.md#trigger---poison-blobs). 

Tímto způsobem můžete zachytit aktivační události, které může být ztraceno v důsledku chyby a opakujte později použití jiné funkce pro zpracování zpráv z fronty nezpracovatelných pomocí uložené informace.  
