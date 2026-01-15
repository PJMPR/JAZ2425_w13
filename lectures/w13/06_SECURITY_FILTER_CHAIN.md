# ❤️ SecurityFilterChain – serce konfiguracji bezpieczeństwa

> Jak **deklaratywnie** zdefiniować zasady bezpieczeństwa w Spring Security

---

## ⏱️ 6. SecurityFilterChain – serce konfiguracji bezpieczeństwa

W tym momencie mamy już wszystkie klocki:

* użytkowników w bazie danych,
* `UserDetails` jako adapter,
* `UserDetailsService` jako źródło użytkowników.

Teraz czas na **najważniejszy element konfiguracji** — miejsce, w którym **decydujemy, kto ma dostęp do czego**.

Tym miejscem jest **`SecurityFilterChain`**.

---

## 🧩 6.1. Czym jest SecurityFilterChain

`SecurityFilterChain` to **łańcuch filtrów bezpieczeństwa**, przez który przechodzi **każde żądanie HTTP** trafiające do aplikacji.

Można go traktować jako:

* centralny punkt konfiguracji bezpieczeństwa,
* reguły dostępu do endpointów,
* decyzje dotyczące sposobu logowania.

---

### 🆕 Nowoczesna konfiguracja

Od Spring Security 5.7+:

* ❌ **nie używamy** `WebSecurityConfigurerAdapter`,
* ✅ konfigurujemy bezpieczeństwo przez **bean `SecurityFilterChain`**.

Zalety:

* konfiguracja jest **jawna i czytelna**,
* łatwiejsza do testowania,
* zgodna z nowoczesnym stylem Spring Boot.

---

### 🧾 Deklaratywne definiowanie zasad bezpieczeństwa

Zamiast:

* ręcznie sprawdzać role w kontrolerach,

robimy to:

* **deklaratywnie** w konfiguracji bezpieczeństwa.

➡️ *„Ten endpoint jest publiczny, ten wymaga logowania, a ten roli ADMIN”*.

---

## ⚙️ 6.2. Najważniejsze elementy konfiguracji

Poniżej omówienie najczęściej używanych elementów konfiguracji `SecurityFilterChain`.

---

### 🔐 `authorizeHttpRequests`

Centralne miejsce definiowania **reguł dostępu**.

To tutaj określamy:

* które endpointy są publiczne,
* które wymagają logowania,
* które wymagają konkretnych ról lub uprawnień.

---

### 🎯 `requestMatchers`

Służy do dopasowania żądań:

* po ścieżce (`/api/public/**`),
* po metodzie HTTP (GET, POST, itd.).

Przykłady:

* `/login`
* `/api/users/**`
* `/api/admin/**`

---

### ✅ `permitAll()` vs 🔒 `authenticated()`

* `permitAll()`

  * dostęp dla **każdego** (również niezalogowanego),

* `authenticated()`

  * dostęp tylko dla **zalogowanych użytkowników**.

To najczęstsze rozróżnienie na początkowym etapie.

---

### 👥 Role i uprawnienia

Spring Security wspiera:

* role (`hasRole("USER")`, `hasRole("ADMIN")`),
* uprawnienia (`hasAuthority("READ_REPORTS")`).

> ⚠️ `hasRole("ADMIN")` sprawdza w tle `ROLE_ADMIN`.

---

### 🔑 `formLogin` / 🌐 `httpBasic`

Mechanizmy logowania:

* **`formLogin`**

  * klasyczny formularz HTML,
  * domyślna lub własna strona logowania,

* **`httpBasic`**

  * proste logowanie przez nagłówek HTTP,
  * często używane do testów lub API.

> 💡 W prawdziwych API produkcyjnych częściej spotyka się JWT, ale to osobny temat.

---

## 🧪 6.3. Przykładowe scenariusze

Poniżej typowe przypadki, które niemal zawsze pojawiają się w aplikacjach.

---

### 🌍 Endpoint publiczny

* dostępny bez logowania,
* np. `/login`, `/health`, `/docs`.

➡️ `permitAll()`

---

### 🔐 Endpoint tylko dla zalogowanych

* wymaga poprawnej autentykacji,
* np. `/api/users/me`.

➡️ `authenticated()`

---

### 🛑 Endpoint tylko dla admina

* wymaga konkretnej roli,
* np. `/api/admin/**`.

➡️ `hasRole("ADMIN")`

---
