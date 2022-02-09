---
id: 230bf379-fac9-4de9-9494-9d6dd73d7f1e
title: Memo
desc: ''
updated: 1627928508328
created: 1620678656696
---

Kiedy używać HOC memo?

- Pure Functional Component - kiedy mamy komponent funkcyjny i przy tych samych propsach zawsze będzie renderował to samo.
- Kiedy renderuje się często - w przeciwnym wypadku może to być overkill
- Komponent zazwyczaj rerenderuje się z tymi samymi propsami 
- Komponent jest średni lub duży
- Stosować tylko na komponentach, których rodzice mogą ulec zmianie w związku z czymś co nie ma wpływu na ten komponent. W przeciwnym wypadku mamy niepotrzebny kod  w komponencie, który jest zawsze aktualizowany - komponent i tak się rerenderuje, bo zmieniają się np. propsy.