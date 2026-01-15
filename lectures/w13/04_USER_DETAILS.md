# 🌉 UserDetails – most między bazą danych a Spring Security

> Kluczowy element integracji **encji użytkownika** z mechanizmami Spring Security

---

## ⏱️ 4. UserDetails – most między bazą danych a Spring Security

W tym momencie łączymy wszystko, o czym mówiliśmy wcześniej:

* użytkownika w bazie danych,
* proces logowania,
* decyzje autoryzacyjne Spring Security.

Centralnym elementem tego połączenia jest **`UserDetails`**.

---

## 🧩 4.1. Czym jest UserDetails

`UserDetails` to **interfejs Spring Security**, który opisuje użytkownika **z punktu widzenia bezpieczeństwa**.

Spring Security **nie pracuje bezpośrednio na naszych encjach** – oczekuje obiektu, który:

* ma jasno zdefiniowane dane logowania,
* dostarcza role/uprawnienia,
* informuje o stanie konta.

---

### 📋 Interfejs opisujący

#### 🔑 Dane logowania

* `getUsername()` – identyfikator użytkownika,
* `getPassword()` – **hash hasła**.

#### 👥 Role – `GrantedAuthority`

* reprezentują uprawnienia użytkownika,
* najczęściej mapowane z ról (`ROLE_USER`, `ROLE_ADMIN`),
* wykorzystywane przy autoryzacji.

#### 🚦 Status konta

Spring Security musi wiedzieć, czy konto:

* jest aktywne,
* nie jest zablokowane,
* nie wygasło.

➡️ Wszystkie te informacje dostarcza `UserDetails`.

---

### ❓ Dlaczego Spring Security go wymaga

Spring Security:

* musi działać **niezależnie od struktury naszej domeny**,
* potrzebuje **jednolitego kontraktu** dla użytkownika,
* obsługuje różne źródła użytkowników (DB, LDAP, OAuth2).

`UserDetails` jest właśnie tym kontraktem.

> 💡 Dzięki temu mechanizmy bezpieczeństwa są **spójne i rozszerzalne**.

---

## 🛠️ 4.2. Implementacja UserDetails

Najczęściej implementujemy `UserDetails` jako **adapter** wokół encji JPA.

Schemat:

```
Encja JPA (User)
        ↓
UserDetails (UserPrincipal)
```

---

### 🔍 Kluczowe metody

#### `getUsername()`

* zwraca login lub email,
* musi być zgodna z tym, czego używamy do logowania.

---

#### `getPassword()`

* zwraca **hash hasła** z bazy danych,
* Spring Security porównuje go z hasłem podanym przez użytkownika.

> ❗ Nigdy nie zwracamy hasła w postaci jawnej.

---

#### `getAuthorities()`

* zwraca kolekcję `GrantedAuthority`,
* mapuje role/uprawnienia użytkownika,
* przykład: `ROLE_USER`, `ROLE_ADMIN`.

To **kluczowa metoda dla autoryzacji**.

---

#### Metody statusu konta

* `isEnabled()` – czy konto jest aktywne,
* `isAccountNonLocked()` – czy konto nie jest zablokowane,
* (opcjonalnie) `isAccountNonExpired()`, `isCredentialsNonExpired()`.

> 💡 Pozwalają wdrażać polityki bezpieczeństwa bez dodatkowego kodu.

---

### 🔁 Mapowanie encji użytkownika → UserDetails

Typowe mapowanie:

* pola encji → metody `UserDetails`,
* role z bazy → `GrantedAuthority`,
* status konta → metody `isEnabled()` itd.

`UserDetails` **nie musi być encją JPA** – to warstwa pośrednia.

---

## ⚠️ 4.3. Najczęstsze błędy

### ❌ Przechowywanie plaintext password

* ogromne zagrożenie bezpieczeństwa,
* brak zgodności z Spring Security,
* naruszenie podstawowych zasad bezpieczeństwa.

➡️ **Zawsze hashujemy hasła**.

---

### ❌ Brak ról

* użytkownik może się zalogować,
* ale **nie ma dostępu do żadnych endpointów**,
* trudne do debugowania dla początkujących.

➡️ Zawsze upewnij się, że `getAuthorities()` coś zwraca.

---

### ❌ Mylenie `ROLE_USER` vs `USER`

Spring Security:

* **role muszą mieć prefiks `ROLE_`**,
* `hasRole("USER")` → sprawdza `ROLE_USER`.

Częsty błąd:

* zapisanie w bazie `USER` zamiast `ROLE_USER`.

---

