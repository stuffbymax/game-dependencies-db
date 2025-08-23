
## 🎮 Game Compatibility Helper JSON

This JSON file serves as a **reference database for fixing compatibility issues** with popular PC games, particularly when launching older titles or resolving common runtime errors. It provides detailed metadata for each game including:

### ✅ What It Contains

For each game entry, the JSON includes:

* **Game Name & Cover Image**
  A visual and name identifier for easy reference.

* **DirectX Version**
  Indicates the required DirectX version (e.g., DirectX 9.0c, 11, or 12).

* **Visual C++ Redistributables (`vcredist`)**
  Lists the Microsoft VC++ versions needed (e.g., 2008, 2015–2022).

* **.NET Framework Version**
  Specifies the required version of the .NET Framework.

* **Critical DLL Dependencies**
  Lists DLL files that the game depends on. These are often the root cause of startup errors.

* **Download Links**
  Direct links to official Microsoft sources for:

  * DirectX Redistributables
  * VC++ Redistributables
  * .NET Framework installers

* **Common Fixes**
  A list of practical steps users can follow to resolve compatibility problems (e.g., running as administrator, using compatibility mode, disabling overlays).

### 🧩 Purpose

This JSON is designed to help users:

* Troubleshoot and fix game launch issues.
* Quickly locate and download missing dependencies.
* Set up classic or modern games on Windows systems more reliably.

### 🛠 Example Use Cases

* Building a **troubleshooting assistant tool** or launcher that reads this JSON and checks for missing components.
* Creating a **mod manager or patch installer** that ensures all runtimes are correctly installed.
* Publishing a **website or app** that recommends fixes based on known game compatibility problems.

---

# Fetch JSON from GitHub in Different Languages

---

## JavaScript

### Node.js (with `fetch`)

```js
import fetch from 'node-fetch'; // in Node.js < 18, otherwise fetch is global

async function fetchGames() {
  const url = 'https://raw.githubusercontent.com/stuffbymax/game-dependencies-db/refs/heads/main/games.json';
  const response = await fetch(url);
  if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
  const data = await response.json();
  console.log(data);
}

fetchGames().catch(console.error);
```

### Browser

```js
async function fetchGames() {
  const url = 'https://raw.githubusercontent.com/stuffbymax/game-dependencies-db/refs/heads/main/games.json';
  const response = await fetch(url);
  if (!response.ok) {
    console.error('Network response was not ok', response.statusText);
    return;
  }
  const data = await response.json();
  console.log(data);
}
fetchGames();
```

---

## Python

```python
import requests

def fetch_games():
    url = 'https://raw.githubusercontent.com/stuffbymax/game-dependencies-db/refs/heads/main/games.json'
    resp = requests.get(url)
    resp.raise_for_status()
    data = resp.json()
    print(data)

if __name__ == "__main__":
    fetch_games()
```

---

## C (using libcurl + cJSON)

```c
#include <stdio.h>
#include <stdlib.h>
#include <curl/curl.h>
#include "cJSON.h"

struct MemoryBlock {
    char *data;
    size_t size;
};

static size_t write_callback(void *contents, size_t size, size_t nmemb, void *userp) {
    size_t realsize = size * nmemb;
    struct MemoryBlock *mem = (struct MemoryBlock *) userp;

    char *new_mem = realloc(mem->data, mem->size + realsize + 1);
    if (!new_mem) return 0;

    mem->data = new_mem;
    memcpy(&(mem->data[mem->size]), contents, realsize);
    mem->size += realsize;
    mem->data[mem->size] = 0;

    return realsize;
}

int main(void) {
    CURL *curl = curl_easy_init();
    struct MemoryBlock chunk = { .data = NULL, .size = 0 };
    const char *url = "https://raw.githubusercontent.com/stuffbymax/game-dependencies-db/refs/heads/main/games.json";

    if (!curl) {
        fprintf(stderr, "Failed to init curl\n");
        return EXIT_FAILURE;
    }

    curl_easy_setopt(curl, CURLOPT_URL, url);
    curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, write_callback);
    curl_easy_setopt(curl, CURLOPT_WRITEDATA, (void *)&chunk);
    CURLcode res = curl_easy_perform(curl);

    if (res != CURLE_OK) {
        fprintf(stderr, "curl failed: %s\n", curl_easy_strerror(res));
        curl_easy_cleanup(curl);
        return EXIT_FAILURE;
    }

    curl_easy_cleanup(curl);

    cJSON *json = cJSON_Parse(chunk.data);
    if (!json) {
        fprintf(stderr, "JSON parse error: %s\n", cJSON_GetErrorPtr());
        free(chunk.data);
        return EXIT_FAILURE;
    }

    char *printed = cJSON_Print(json);
    printf("%s\n", printed);

    free(printed);
    cJSON_Delete(json);
    free(chunk.data);

    return EXIT_SUCCESS;
}
```

⚠️ Requires linking against `libcurl` and `cJSON`.

---

## C# (using `HttpClient` + `System.Text.Json`)

```csharp
using System;
using System.Net.Http;
using System.Text.Json;
using System.Threading.Tasks;

public class Game {
    public string name { get; set; }
    public string image { get; set; }
    public string directx { get; set; }
    public string[] vcredist { get; set; }
    public string dotnet { get; set; }
    public string[] dlls { get; set; }
    public Downloads downloads { get; set; }
    public string[] fixes { get; set; }
    public string[] manuals { get; set; }
}

public class Downloads {
    public string directx { get; set; }
    public System.Collections.Generic.Dictionary<string, string> vcredist { get; set; }
    public string dotnet { get; set; }
}

public class Program {
    public static async Task Main(string[] args) {
        var url = "https://raw.githubusercontent.com/stuffbymax/game-dependencies-db/refs/heads/main/games.json";
        using var client = new HttpClient();
        var json = await client.GetStringAsync(url);
        var options = new JsonSerializerOptions { PropertyNameCaseInsensitive = true };
        var games = JsonSerializer.Deserialize<Game[]>(json, options);

        foreach (var game in games) {
            Console.WriteLine($"Name: {game.name}, DirectX: {game.directx}");
        }
    }
}
```

---

## Summary

| Language       | Tooling Used                      | Notes                            |
| -------------- | --------------------------------- | -------------------------------- |
| **JavaScript** | `fetch` (browser/Node.js)         | Modern and built-in              |
| **Python**     | `requests` + `json()`             | Simple and widely used           |
| **C**          | `libcurl` + `cJSON`               | Low-level, needs extra libraries |
| **C#**         | `HttpClient` + `System.Text.Json` | Async, clean, strongly typed     |

---

