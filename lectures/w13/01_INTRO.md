# 🔐 Wprowadzenie – po co w ogóle autoryzować użytkowników?

> Materiał wprowadzający do wykładu o **Spring Security**

---

## ⏱️ 1. Wprowadzenie

Autoryzacja i autentykacja to fundamenty bezpieczeństwa każdej aplikacji webowej. Niezależnie od tego, czy budujemy prostą aplikację CRUD, czy rozbudowany system korporacyjny – **kontrola dostępu** jest kluczowa.


## 🧩 1.1. Problem biznesowy i techniczny

### 🛡️ Czym jest bezpieczeństwo aplikacji webowej?

Bezpieczeństwo aplikacji webowej to zbiór mechanizmów i praktyk, które mają na celu:

* ochronę **danych użytkowników** 📄,
* zabezpieczenie **zasobów systemu** 🗄️,
* zapewnienie, że użytkownicy mogą wykonywać **tylko dozwolone akcje** ✅.

W praktyce oznacza to m.in.:

* weryfikację tożsamości użytkownika,
* kontrolę dostępu do endpointów,
* ochronę sesji i tokenów.

---

### ❌ Co się dzieje, gdy nie mamy żadnej autentykacji?

Brak autentykacji oznacza, że:

* **każdy jest anonimowy** 👤,
* system nie wie, *kto* wykonuje daną operację,
* nie ma możliwości rozróżnienia użytkowników.

Efekt?

* brak odpowiedzialności,
* brak kontroli,
* ogromne ryzyko nadużyć.

---

### 🚨 Przykłady realnych zagrożeń

#### 🔍 Dostęp do cudzych danych

* odczyt profili innych użytkowników,
* wyciek danych osobowych (RODO ❗),
* utrata zaufania klientów.

#### ✏️ Modyfikacja zasobów

* edycja lub usuwanie danych bez uprawnień,
* manipulacja zamówieniami, płatnościami,
* sabotaż systemu.

#### 🤖 Brute force / 🕵️ Session hijacking

* masowe próby logowania,
* przejęcie sesji użytkownika,
* podszywanie się pod inną osobę.

> 💡 **Wniosek:** brak zabezpieczeń to zaproszenie do ataku.

---

## 🔑 1.2. Autentykacja vs Autoryzacja

Te pojęcia są często mylone, ale oznaczają **coś zupełnie innego**.

---

### 👤 Autentykacja (Authentication) – *kim jesteś?*

Autentykacja to proces **potwierdzania tożsamości użytkownika**.

Przykłady:

* login + hasło 🔑,
* token JWT 🎟️,
* OAuth2 / logowanie przez Google 🔐.

➡️ Wynik autentykacji: **wiemy, kim jest użytkownik**.

---

### 🧾 Autoryzacja (Authorization) – *co możesz zrobić?*

Autoryzacja odpowiada na pytanie, **do jakich zasobów ma dostęp użytkownik**.

Przykłady:

* role (`USER`, `ADMIN`) 👥,
* uprawnienia (`READ_REPORTS`, `DELETE_USER`) 🧩,
* polityki dostępu do endpointów.

➡️ Wynik autoryzacji: **wiemy, na co użytkownik ma pozwolenie**.

---
