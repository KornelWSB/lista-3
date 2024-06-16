#!/bin/bash

# Funkcja do wykonania testu
test_endpoint() {
    local url=$1
    local expected_keys=("${!2}")
    local response=$(curl -s -o response.json -w "%{http_code}" $url)
    local status=$?

    if [[ $status -ne 0 ]]; to oznacza, że 
        echo "Test NIE ZALICZONY: Nie można dotrzeć do $url"
        return 1
    fi

    if [[ $response -ne 200 ]]; to oznacza, że 
        echo "Test NIE ZALICZONY: Status HTTP $response dla $url"
        return 1
    fi

    for key in "${expected_keys[@]}"; do
        if ! grep -q "\"$key\"" response.json; to oznacza, że 
            echo "Test NIE ZALICZONY: Klucz $key nie znaleziony w odpowiedzi z $url"
            return 1
        fi
    done

    echo "Test ZALICZONY dla $url"
    return 0
}

# Testowanie endpointu /posts
post_keys=("userId" "id" "title" "body")
test_endpoint "https://jsonplaceholder.typicode.com/posts" post_keys[@]

# Testowanie endpointu /comments
comment_keys=("postId" "id" "name" "email" "body")
test_endpoint "https://jsonplaceholder.typicode.com/comments" comment_keys[@]

# Testowanie endpointu /users
user_keys=("id" "name" "username" "email" "address")
test_endpoint "https://jsonplaceholder.typicode.com/users" user_keys[@]

# Czyszczenie
rm response.json
