# 🛡️ Spring Security – ogólny obraz

> Wprowadzenie koncepcyjne do ekosystemu **Spring Security**

---

## ⏱️ 2. Spring Security – ogólny obraz

Spring Security to **standard de facto** w świecie Springa, jeśli chodzi o bezpieczeństwo aplikacji webowych i backendowych. W tej części skupiamy się na **dużym obrazie** – *czym jest*, *co robi* i *czego możemy się spodziewać „out of the box”*.

---

## 🧩 2.1. Czym jest Spring Security

Spring Security to **framework bezpieczeństwa**, który dostarcza gotowe mechanizmy do ochrony aplikacji.

### 🔐 Framework do:

* **autentykacji** – sprawdzania *kim jest użytkownik*,
* **autoryzacji** – decydowania *co użytkownik może zrobić*,
* **ochrony endpointów** – kontrolowania dostępu do URL-i i metod.

Spring Security działa na poziomie **filtrów HTTP** i przechwytuje każde żądanie trafiające do aplikacji.

> 💡 Można o nim myśleć jak o **strażniku stojącym przed kontrolerami**.

---

### 🔗 Integracja z ekosystemem Spring

Spring Security jest głęboko zintegrowany z innymi modułami Springa:

* **Spring MVC**

  * ochrona kontrolerów i endpointów,
  * adnotacje na metodach (`@Secured`, `@PreAuthorize`).

* **Spring Boot**

  * automatyczna konfiguracja (startery),
  * sensowne domyślne ustawienia.

* **Baza danych**

  * użytkownicy i role przechowywane w DB,
  * integracja z `UserDetailsService`.

* **JWT / OAuth2** *(wspomnieć)*

  * obsługa tokenów,
  * integracja z zewnętrznymi providerami (Google, Keycloak, Auth0).

> ⚠️ Na tym etapie **nie wchodzimy w szczegóły JWT ani OAuth2** – to osobne, bardziej zaawansowane tematy.

---

## ⚙️ 2.2. Co Spring Security robi „domyślnie”

Jedną z największych zalet Spring Security jest to, że **działa od razu po dodaniu zależności**.

### 🚀 Automatyczna konfiguracja

Po dodaniu startera:

```
org.springframework.boot:spring-boot-starter-security
```

Spring Boot:

* rejestruje filtry bezpieczeństwa,
* zabezpiecza wszystkie endpointy,
* włącza mechanizm logowania.

➡️ **Zero konfiguracji = aplikacja już jest chroniona**.

---

### 🧑‍💻 Domyślna strona logowania

Jeśli nie zdefiniujemy własnej:

* Spring Security generuje **prostą stronę logowania HTML**,
* dostępna jest pod `/login`,
* obsługuje logowanie przez formularz.

> 💡 Idealna do testów i nauki, **nie do produkcji**.

---

### 👤 Domyślny użytkownik

Domyślnie Spring Security tworzy:

* użytkownika: `user`
* losowe hasło generowane przy starcie aplikacji

Hasło:

* pojawia się **w logach przy uruchamianiu aplikacji**,
* zmienia się przy każdym restarcie.

Przykład logu:

```
Using generated security password: 8f3a9c12-...
```

➡️ Dzięki temu:

* możemy **od razu się zalogować**,
* nie musimy jeszcze mieć bazy danych.

---

