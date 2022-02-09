---
id: ZAG9myETBVD9zuEHoPlzp
title: Asynchroniczność
desc: ''
updated: 1637749632332
created: 1631174609398
---

Jeżeli mamy pojedynczy strzał do API możemy skorzystać z **async&await**. Jeżeli jednak tych zapytań jest kilka następujących po sobie to oczekiwanie na rozwiązanie każdego awaita będzie czasochłonne. Żeby to poprawić możemy skorzystać z Promise.all() ale musimy pamiętać, że w przypadku, gdy jedna promisa się wywali, nie dostaniemy nic:

```javascript
async function getRicks() {
	try {
		const ricksPromise = axios.get(endpoint);
		const mortiesPromise = axios.get(endpoint);

		const [ricks, morties] = Promise.all([ricksPromise, mortiesPromise]);

		console.log(ricks, morties);
	} catch(err) {
		// error tutaj może wystąpić w 2 przypadkach - kiedy mamy problem z siecią i kiedy w naszej promisie nie został obsłużony reject
		console.log(err);
	}
}
```

[Promise](resources.programming.javascript.asynchronicznosc.promise.md)

[Async & Await](resources.programming.javascript.asynchronicznosc.async&await.md)

## Przykłady:

[Promise chaining](https://codesandbox.io/s/asynchronus-js-promise-chaining-6zwsu?file=/src/index.js)

[Async & Await](https://codesandbox.io/s/asynchronus-js-async-await-gim05?file=/src/index.js)

[Generatory](https://codesandbox.io/s/asynchronus-js-generator-b684p?file=/src/index.js)
