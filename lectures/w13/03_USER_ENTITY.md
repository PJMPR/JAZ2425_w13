# 🗄️ Przechowywanie użytkowników w lokalnej bazie danych

> Jak model użytkownika łączy **świat domeny aplikacji** ze **Spring Security**

---

## ⏱️ 3. Przechowywanie użytkowników w lokalnej bazie danych

W prawdziwych aplikacjach użytkownicy **nie są hardkodowani** ani tworzeni automatycznie przy starcie systemu. Muszą być **trwale przechowywani**, najczęściej w relacyjnej bazie danych.

---

## 🧩 3.1. Model użytkownika

Z punktu widzenia bezpieczeństwa użytkownik to **coś więcej niż tylko login i hasło**.

### 👤 Co musi zawierać użytkownik?

#### 🆔 Username

* unikalny identyfikator użytkownika,
* najczęściej login lub email,
* używany podczas procesu logowania.

---

#### 🔑 Password *(hash!)*

* **nigdy** nie przechowujemy hasła w postaci jawnej ❌,
* w bazie zapisujemy **hash hasła**,
* porównywanie odbywa się przez algorytm hashujący.

> 💡 Spring Security wspiera m.in. `BCryptPasswordEncoder`.

---

#### 👥 Role / uprawnienia

* określają **co użytkownik może zrobić**,
* przykłady:

  * role: `USER`, `ADMIN`,
  * uprawnienia: `READ_REPORTS`, `DELETE_USER`.

Role są później wykorzystywane w:

* konfiguracji zabezpieczeń,
* adnotacjach (`@PreAuthorize`).

---

#### 🚦 Status użytkownika

System musi wiedzieć, **czy konto jest aktywne**:

* `enabled` – czy użytkownik może się logować,
* `locked` – czy konto nie jest zablokowane,
* (opcjonalnie) wygaszenie konta lub hasła.

> 💡 To pozwala reagować na nadużycia i polityki bezpieczeństwa.

---

## ⚠️ 3.2. Dlaczego nie wystarczy zwykła encja JPA

Na tym etapie pojawia się częste pytanie:

> ❓ *„Skoro mam encję `User`, to dlaczego Spring Security nie może jej po prostu użyć?”*

### 🧱 Problem

* Spring Security **nie zna** naszej encji JPA,
* nie wie:

  * gdzie jest username,
  * jak sprawdzić hasło,
  * jakie są role,
  * czy konto jest aktywne.

Dla Spring Security nasza encja to tylko **zwykły obiekt domenowy**.

---

### 🌉 Potrzebny adapter

Musimy zbudować **most (adapter)** pomiędzy:

* 🌍 **światem aplikacji**

  * encje JPA,
  * baza danych,
  * logika domenowa,

* 🔐 **światem Spring Security**

  * proces logowania,
  * autoryzacja,
  * kontekst bezpieczeństwa.

Ten adapter tłumaczy:

* *naszego użytkownika* → *użytkownika rozumianego przez Spring Security*.

---

