---
id: a46e6425-7334-4715-a982-49432ebe348a
title: Forms
desc: ''
updated: 1622730138489
created: 1619468415050
---
# Formularze

Button wysyłający formularz musi mieścić się wewnątrz formularza i zawierać atrybut ```type='submit'```

Do funkcji obsługującej wysyłkę formularza dodajemy:

```javascript
const submitHandler = (event) => {
    event.preventDefault();
}
```

By uniknąć domyślnej akci wysłania requesta i przeładowania strony.

Dobre praktyki:
https://www.reddit.com/r/webdev/comments/nm6wcl/18_cards_of_how_to_design_web_forms/