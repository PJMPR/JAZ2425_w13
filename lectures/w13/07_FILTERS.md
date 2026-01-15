# 🧩 Filtry w Spring Security – co naprawdę dzieje się pod maską

> Zrozumienie **mechanizmu filtrów** to klucz do debugowania i pracy z zaawansowanym bezpieczeństwem

---

## ⏱️ 7. Filtry w Spring Security – co naprawdę dzieje się pod maską

Do tej pory konfigurowaliśmy Spring Security w sposób **deklaratywny**:

* role,
* endpointy,
* reguły dostępu.

Teraz cofamy się o jeden poziom niżej i odpowiadamy na pytanie:

> ❓ *Co Spring Security faktycznie robi z każdym requestem HTTP?*

Odpowiedź: **przepuszcza go przez łańcuch filtrów**.

---

## 🧩 7.1. Czym są filtry

Filtry w Spring Security to komponenty, które:

* przechwytują **każde żądanie HTTP**,
* wykonują logikę bezpieczeństwa,
* decydują, czy request może przejść dalej.

---

### ⚠️ Filtr ≠ Interceptor ≠ Controller

To bardzo ważne rozróżnienie:

* **Filtr**

  * działa na poziomie serwletów,
  * uruchamia się **zanim** request trafi do Spring MVC,

* **Interceptor**

  * działa już w Spring MVC,
  * jest wywoływany **przed/po kontrolerze**,

* **Controller**

  * zawiera logikę biznesową aplikacji.

➡️ Filtry są **najwcześniejszym punktem**, w którym Spring Security może zareagować.

---

### 🚦 Filtry działają przed logiką aplikacji

To oznacza, że:

* request może zostać **zatrzymany**, zanim dotknie kontrolera,
* kontroler może **nigdy się nie wykonać**,
* decyzje o 401 / 403 zapadają właśnie tutaj.

---

## 🔗 7.2. Łańcuch filtrów (Filter Chain)

Spring Security nie używa jednego filtra, lecz **całego łańcucha filtrów**.

### 🔄 Jak to działa?

* każde żądanie przechodzi **po kolei przez filtry**,
* każdy filtr:

  * może coś sprawdzić,
  * może zmodyfikować kontekst bezpieczeństwa,
  * może przerwać dalsze przetwarzanie.

---

### 🧱 Przykłady odpowiedzialności filtrów

#### 🔐 Sprawdzanie autentykacji

* czy użytkownik jest zalogowany,
* czy istnieje sesja lub kontekst bezpieczeństwa,
* czy dostarczono poprawne dane logowania.

---

#### 🧠 Obsługa sesji

* tworzenie i odczyt sesji użytkownika,
* przypisanie użytkownika do requestu,
* zarządzanie wygasaniem sesji.

---

#### 🚨 Obsługa wyjątków bezpieczeństwa

* brak autentykacji → **401 Unauthorized**,
* brak uprawnień → **403 Forbidden**,
* przekierowanie na login lub zwrot odpowiedzi błędu.

---

> 💡 Każdy z tych kroków to osobny filtr w łańcuchu.

---

## 🧠 7.3. Dlaczego to ważne

Zrozumienie filtrów daje **realną przewagę** podczas pracy ze Spring Security.

---

### 🔍 Zrozumienie problemów

Dzięki temu wiemy:

* **dlaczego request jest blokowany**,
* w którym momencie zapada decyzja o odrzuceniu,
* czy problem dotyczy:

  * braku logowania,
  * braku roli,
  * złej konfiguracji filtrów.

---

### 🚫 Skąd biorą się błędy 401 / 403

* **401 Unauthorized**

  * użytkownik **nie jest autentykowany**,

* **403 Forbidden**

  * użytkownik jest zalogowany,
  * ale **nie ma wymaganych uprawnień**.

➡️ Oba błędy powstają **na poziomie filtrów**, nie w kontrolerach.

---

### 🧱 Podstawa do zaawansowanych tematów

Zrozumienie filter chain to fundament pod:

* **JWT** (filtr sprawdzający token),
* **custom filters** (np. logowanie requestów, audyt),
* integrację z zewnętrznymi systemami bezpieczeństwa.

Bez tej wiedzy Spring Security wydaje się „czarną skrzynką”.

---

