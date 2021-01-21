---
ms.openlocfilehash: 7a5acca4169b1f763e178e0a05b01548765ff45d
ms.sourcegitcommit: 4d260b655cb52b990dda79c239a9721f2e964625
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570861"
---
Запросы REST выполняются по протоколу HTTP с помощью тех же HTTP-команд, которые веб-браузеры используют для извлечения страниц и отправки данных на серверы. В этом упражнении вы создадите класс, который использует команду GET для получения данных репозитория .NET от веб-API GitHub.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `Constants`. Удалите в **Constants.cs** весь код шаблона и замените его приведенным ниже кодом:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string GitHubReposEndpoint = "https://api.github.com/orgs/dotnet/repos";
        }
    }
    ```

    В этом коде определяется одна константа — конечная точка, к которой будут выполняться веб-запросы.

1. В **обозревателе решений** в проекте **WebServicesTutorial** добавьте новый класс с именем `Repository`. Затем в файле **Repository.cs** удалите весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class Repository
        {
            [JsonProperty("name")]
            public string Name { get; set; }

            [JsonProperty("description")]
            public string Description { get; set; }

            [JsonProperty("html_url")]
            public Uri GitHubHomeUrl { get; set; }

            [JsonProperty("homepage")]
            public Uri Homepage { get; set; }

            [JsonProperty("watchers")]
            public int Watchers { get; set; }
        }
    }
    ```

    В этом коде определяется класс `Repository`, который используется для моделирования данных JSON, полученных от веб-службы. Каждое свойство снабжено атрибутом `JsonProperty`, который содержит имя поля JSON. Newtonsoft.Json будет использовать это сопоставление имен полей JSON со свойствами CLR при десериализации данных JSON в объекты модели.

    > [!NOTE]
    > Приведенные выше определения класса были упрощены и не полностью моделируют данные JSON, полученные от веб-службы.

1. В **обозревателе решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `RestService`. Затем удалите в **RestService.cs** весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<List<Repository>> GetRepositoriesAsync(string uri)
            {
                List<Repository> repositories = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        repositories = JsonConvert.DeserializeObject<List<Repository>>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return repositories;
            }
        }
    }
    ```

    В этом коде определятся один метод `GetRepositoriesAsync`, который получает данные репозитория .NET от веб-API GitHub. Этот метод использует метод `HttpClient.GetAsync` для отправки запроса GET к веб-API, указанному аргументом `uri`. Веб-API отправляет ответ, который хранится в объекте `HttpResponseMessage`. Ответ включает код состояния HTTP, который указывает, успешно ли выполнен HTTP-запрос. Если запрос выполнен успешно, веб-API возвращает код состояния HTTP 200 (ОК) и ответ JSON, который находится в свойстве `HttpResponseMessage.Content`. Эти данные JSON считываются в `string` с помощью метода `HttpContent.ReadAsStringAsync`, а затем десериализируются в объект `List<Repository>` с помощью метода `JsonConvert.DeserializeObject`. Этот метод использует сопоставления между именами полей JSON и свойствами CLR, которые определены в классе `Repository` для выполнения десериализации.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **панели решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `Constants`. Удалите в **Constants.cs** весь код шаблона и замените его приведенным ниже кодом:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string GitHubReposEndpoint = "https://api.github.com/orgs/dotnet/repos";
        }
    }
    ```

    В этом коде определяется одна константа — конечная точка, к которой будут выполняться веб-запросы.

1. На **панели решений** в проекте **WebServicesTutorial** добавьте новый класс с именем `Repository`. Затем в файле **Repository.cs** удалите весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class Repository
        {
            [JsonProperty("name")]
            public string Name { get; set; }

            [JsonProperty("description")]
            public string Description { get; set; }

            [JsonProperty("html_url")]
            public Uri GitHubHomeUrl { get; set; }

            [JsonProperty("homepage")]
            public Uri Homepage { get; set; }

            [JsonProperty("watchers")]
            public int Watchers { get; set; }
        }
    }
    ```

    В этом коде определяется класс `Repository`, который используется для моделирования данных JSON, полученных от веб-службы. Каждое свойство снабжено атрибутом `JsonProperty`, который содержит имя поля JSON. Newtonsoft.Json будет использовать это сопоставление имен полей JSON со свойствами CLR при десериализации данных JSON в объекты модели.

    > [!NOTE]
    > Приведенные выше определения класса были упрощены и не полностью моделируют данные JSON, полученные от веб-службы.

1. На **панели решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `RestService`. Затем удалите в **RestService.cs** весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<List<Repository>> GetRepositoriesAsync(string uri)
            {
                List<Repository> repositories = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        repositories = JsonConvert.DeserializeObject<List<Repository>>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return repositories;
            }
        }
    }
    ```

    В этом коде определятся один метод `GetRepositoriesAsync`, который получает данные репозитория .NET от веб-API GitHub. Этот метод использует метод `HttpClient.GetAsync` для отправки запроса GET к веб-API, указанному аргументом `uri`. Веб-API отправляет ответ, который хранится в объекте `HttpResponseMessage`. Ответ включает код состояния HTTP, который указывает, успешно ли выполнен HTTP-запрос. Если запрос выполнен успешно, веб-API возвращает код состояния HTTP 200 (ОК) и ответ JSON, который находится в свойстве `HttpResponseMessage.Content`. Эти данные JSON считываются в `string` с помощью метода `HttpContent.ReadAsStringAsync`, а затем десериализируются в объект `List<Repository>` с помощью метода `JsonConvert.DeserializeObject`. Этот метод использует сопоставления между именами полей JSON и свойствами CLR, которые определены в классе `Repository` для выполнения десериализации.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.
