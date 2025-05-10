# Plan działania: Qdrant z Caddy w środowisku lokalnym i produkcyjnym

## 1. Struktura projektu

```
qdrant/
├── .env                        # Zmienne środowiskowe
├── .gitignore                  # Ignorowanie plików (w tym .env)
├── docker-compose.local.yml    # Konfiguracja dla środowiska lokalnego
├── docker-compose.prod.yml     # Konfiguracja dla środowiska produkcyjnego
├── config/
│   ├── Caddyfile.local         # Konfiguracja Caddy dla środowiska lokalnego
│   └── Caddyfile.prod          # Konfiguracja Caddy dla środowiska produkcyjnego
├── scripts/
│   ├── setup-domain.sh         # Skrypt do konfiguracji domeny na produkcji
│   ├── test-api.sh             # Skrypt do testowania API
│   ├── backup.sh               # Skrypt do tworzenia backupu
│   └── restore.sh              # Skrypt do przywracania danych z backupu
├── backup/                     # Katalog na pliki backupu
└── docs/
    ├── INSTRUCTIONS.md         # Instrukcje użytkowania
    ├── Plan.md                 # Istniejący plan projektu
    └── Tutorial korzystania z API Qdrant.md  # Istniejący tutorial
```

## 2. Implementacja plików konfiguracyjnych

### 2.1. Plik `.env`
- Utworzenie pliku z następującymi zmiennymi:
  - `ENV=local` (lub `ENV=prod` na produkcji)
  - `QDRANT_API_KEY=tajny_klucz_123`
  - `ADMIN_USER=admin`
  - `ADMIN_PASSWORD=haslo123`
  - `ADMIN_PASSWORD_HASH=xxx` (wygenerowany przez Caddy)
  - `DOMAIN=vector.example.com` (tylko dla produkcji)

### 2.2. Pliki Docker Compose

#### `docker-compose.local.yml`:
- Konfiguracja dla Qdrant i Caddy w środowisku lokalnym
- Mapowanie portów tylko dla localhost
- Wolumeny dla danych Qdrant z prefixem `local_`

#### `docker-compose.prod.yml`:
- Konfiguracja dla Qdrant i Caddy w środowisku produkcyjnym
- Mapowanie portów 80 i 443 dla SSL
- Wolumeny dla danych Qdrant z prefixem `prod_`
- Dodatkowe wolumeny dla Caddy (/data i /config)

### 2.3. Pliki Caddyfile

#### `Caddyfile.local`:
- Konfiguracja dla localhost:8080
- Basic Auth z hashem z .env
- Walidacja API Key
- Reverse proxy do Qdrant

#### `Caddyfile.prod`:
- Konfiguracja dla domeny z .env
- Automatyczne SSL
- Basic Auth z hashem z .env
- Walidacja API Key
- Reverse proxy do Qdrant

## 3. Skrypty

### 3.1. `setup-domain.sh`
- Skrypt do konfiguracji domeny na produkcji
- Parametry: nazwa domeny
- Aktualizacja pliku .env
- Generowanie odpowiedniego Caddyfile
- Informacja o krokach manualnych (np. konfiguracja DNS)

### 3.2. `test-api.sh`
- Skrypt do testowania API Qdrant
- Przykłady użycia curl z uwzględnieniem Basic Auth i API Key
- Testy dla podstawowych operacji (tworzenie kolekcji, dodawanie punktów, wyszukiwanie)

### 3.3. `backup.sh`
- Skrypt do tworzenia backupu danych Qdrant
- Zapisywanie backupu w katalogu backup/ z timestampem
- Opcje dla środowiska lokalnego i produkcyjnego

### 3.4. `restore.sh`
- Skrypt do przywracania danych z backupu
- Parametr: ścieżka do pliku backupu
- Opcje dla środowiska lokalnego i produkcyjnego

## 4. Dokumentacja

### 4.1. Aktualizacja `INSTRUCTIONS.md`
- Instrukcje instalacji i konfiguracji
- Przykłady użycia API z curl
- Opis różnic między środowiskiem lokalnym i produkcyjnym
- Instrukcje backupu i przywracania danych

## 5. Implementacja

### 5.1. Etap 1: Podstawowa konfiguracja
- [x] Utworzenie struktury katalogów
- [x] Utworzenie pliku .env
- [x] Utworzenie pliku .gitignore
- [x] Implementacja docker-compose.local.yml
- [x] Implementacja docker-compose.prod.yml
- [x] Implementacja Caddyfile.local
- [x] Implementacja Caddyfile.prod
- [x] Testowanie lokalnej konfiguracji

### 5.2. Etap 2: Skrypty
- [ ] Implementacja skryptu setup-domain.sh
- [x] Implementacja skryptu test-api.sh
- [ ] Implementacja skryptu backup.sh
- [ ] Implementacja skryptu restore.sh
- [x] Nadanie uprawnień wykonywania dla skryptów (chmod +x)
- [x] Testowanie skryptów w środowisku lokalnym

### 5.3. Etap 3: Dokumentacja
- [ ] Utworzenie INSTRUCTIONS.md
- [x] Dodanie przykładów użycia API
- [ ] Dokumentacja procesu instalacji
- [ ] Dokumentacja różnic między środowiskami
- [ ] Dokumentacja procesu backupu i przywracania
- [x] Utworzenie dokumentacji rozwiązywania problemów (Troubleshooting.md)

### 5.4. Etap 4: Testowanie produkcyjne
- [ ] Konfiguracja środowiska testowego dla produkcji
- [ ] Testowanie konfiguracji produkcyjnej
- [ ] Weryfikacja działania SSL
- [ ] Weryfikacja procesu backupu
- [ ] Weryfikacja procesu przywracania danych
- [ ] Finalne testy API

## 6. Dodatkowe uwagi

- Używać `docker compose` (wersja 2) zamiast `docker-compose`
- Dodać .env do .gitignore
- Zapewnić odpowiednie uprawnienia dla skryptów (chmod +x)
- Przygotować instrukcje dla użytkowników końcowych
