---
id: 1Z39CjcFvjBH8ijGF3bOL
title: Forms
desc: ''
updated: 1632132104675
created: 1632132104675
---
## Validacja HTML5

Walidacja uruchamiana jest podczas zdarzenia _submit_ na formularzu:

```javascript
form.addEventListener("submit", (event) => checkForm(event, questions));
```
Inputy ostylować możemy za pomocą pseudoklas **:valid** i **:invalid**. 

Inputy mają jednak te klasy przypisane od razu po załadowaniu formularza dlatego musimy dodać klasę do formularza po kliknęciu przycisku wysyłki:

```javascript
checkBtn.addEventListener("click", () => {
		form.classList.add("form-sent");
	});
```

Dla inputów typu **text** możemy dodać pattern:
```html
<input
	type="text"
	name="name"
	value=""
	id="name"
	placeholder="Wpisz swoje imię"
	pattern="[A-ZŻŚŹĘĆŁ]{1}[a-zążśźęćńół]{1,20}"
	required
/>
```

W przypadku inputów radio wystarczy, że jeden ma atrybut **required** jednak dobrą praktyką jest, by dodawać każdemu.
