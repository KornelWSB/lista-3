import subprocess
import json

def send_request(url):
    result = subprocess.run(['curl', '-s', url], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    if result.returncode != 0:
        return None, result.stderr.decode('utf-8')
    return result.stdout.decode('utf-8'), None

def check_response(response, keys):
    try:
        data = json.loads(response)
    except json.JSONDecodeError:
        return False

    if isinstance(data, list):
        data = data[0] if data else {}

    return all(key in data for key in keys)

def run_test(url, keys):
    response, error = send_request(url)
    if error:
        print(f"Test for {url}: FAILED - Error in sending request: {error}")
        return False

    if check_response(response, keys):
        print(f"Test for {url}: PASSED")
        return True
    else:
        print(f"Test for {url}: FAILED - Required keys missing in response")
        return False

def main():
    tests = [
        {"url": "https://jsonplaceholder.typicode.com/posts", "keys": ["userId", "id", "title"]},
        {"url": "https://jsonplaceholder.typicode.com/comments", "keys": ["postId", "id", "email"]},
        {"url": "https://jsonplaceholder.typicode.com/users", "keys": ["id", "name", "email"]}
    ]

    all_passed = True
    for test in tests:
        if not run_test(test["url"], test["keys"]):
            all_passed = False

    if all_passed:
        print("All tests PASSED")
    else:
        print("Some tests FAILED")

if __name__ == "__main__":
    main()
-----------------------

# API Test Script

## Opis

Ten skrypt automatyzuje testowanie endpointów publicznego API JSONPlaceholder przy użyciu narzędzia `curl`. Skrypt wysyła żądania GET do trzech endpointów i sprawdza, czy odpowiedzi zawierają określone kluczowe elementy JSON.

## Endpointy testowane

1. `/posts` - Sprawdza obecność kluczy: `userId`, `id`, `title`
2. `/comments` - Sprawdza obecność kluczy: `postId`, `id`, `email`
3. `/users` - Sprawdza obecność kluczy: `id`, `name`, `email`

## Wymagania

- Python 3
- curl

## Uruchomienie

1. Upewnij się, że masz zainstalowany Python 3 oraz curl.
2. Skopiuj plik `api_test.py` do swojego lokalnego środowiska.
3. Uruchom skrypt:
   ```sh
   python api_test.py

   ------------------

   
