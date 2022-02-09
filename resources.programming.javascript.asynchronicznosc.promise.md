---
id: k8rFOawocYhUjr13t9CaF
title: Promise
desc: ""
updated: 1643047201214
created: 1631188494857
---

## Promise constructor

```javascript
function loadImageAsync(url) {
	return new Promise((resolve, reject) => {
		const img = new Image();
		img.addEventListener("load", (event) => resolve(img));
		img.addEventListener("error", (event) =>
			reject(new Error(`Failed to load ${url}`))
		);
		img.src = url;
	});
}
```

Konstruktor przyjmuje dwie funkcje: **resolve()** i **reject()**, które zostają wywołane w przypadku pozytywnego i negatywnego rozwiązania promisy. Możemy do nich przekazać co ma zostać zwrócone w obu przypadkach. To co przekażemy do resolve() możemy później wykorzystać w then np. jako **(res) => console.log(res)**

Promise może mieć 3 stany:

- pending,
- fulfilled,
- rejected

Do określenia akcji, które mają zostać wykonane, gdy promisa przejdzie w stan _settled_ służą metody **promise.then()**, **promise.catch()** i **promise.finally()**

Funkcja then ma dwa callbacki - fulfilled i rejected, pierwszy rozpatruje pozytywny, drugi negatywny wynik promisy

**response trzeba zrzucić do funkcji json**

```javascript
userPromise = fetch(url);

userPromise
	.then(
		(response) => {
			response.json();
		},
		(reason) => {
			console.log(reason);
		}
	)
	.then(console.log(res));
```

JS wprowadził jednak nową składnię - w Promisie można użyć też then i catch oraz finally (jak try i catch)
Tak więc zamiast dwóch callbacków w then można użyć catch

## Event loop

```javascript
const usersPromise = fetch("https://jsonplaceholder.typicode.com/users");
usersPromise
	.then(
		(response) => response.json(),
		(reason) => reason
	)
	.then((res) => console.log(res));

console.log("Finished");
console.log("Finished");
console.log("Finished");
console.log("Finished");
setTimeout(() => {
	console.log("Finished Timeout");
}, 500);
```

Co zwróci konsola?
To zależy od tego w jakim czasie Promisa zostanie rozwiązana.
Po rozwiązaniu od razu zostanie wrzucona do callback queue, więc zależnie od tego w jakim czasie trafił tam setTimeout, tak zostanie wykonana. W przypadku rejectowanej Promisy, wykona się ona od razu i trafi przed rezultat wykonania setTimeout.

W tym przypadku

1. Finished x4
2. Wynik Promisa
3. Finished Timeout

**setTimeout()** nie jest funkcją asynchroniczną

## Promise chaining

Przy łączeniu promisów nieistotne, który się zrejectuje, promisa po prostu uruchomi catch'a:

```javascript
const promiseFirst = new Promise((resolve, reject) => {
    // resolve(5)
    reject('Fail 1')
})

const promiseSecond = new Promise((resolve, reject) => {
    resolve(10)
    // reject('Fail 2')
})

const promiseTest = new Promise((resolve, reject) => {
    resolve(15)
    // reject('Fail 3')
})

promiseFirst
    .then((res) => res)
    .then((res) => promiseSecond.then((resSecond) => ({ res, resSecond }))
    .then((res) => promiseTest.then((resTest) => ({ ...res, resTest })))
    .then((res) => console.log(res))
    .catch(console.log)
    .finally(() => console.log('Loader off'))
```

Jeżeli chcesz stworzyć samo-rozwiązującą się promisę z prostą wartością użyj **Promise.resolve()**:

```javascript
function job() {
	if (test) {
		return aNewPromise();
	} else {
		return Promise.resolve(42); // return an anto-resolved promise with `42` in data.
	}
}
```

**Promise chaining wykorzystujemy w przypadku, gdy do kolejnego strzału do API potrzebujemy danych z poprzedniego.**

## Promise.all()

Jeżeli api jest niestabilne to trzeba uważać na Promise.all() bo gdy jedna promisa się wywali to reszta się nie wykona.

Z metody **Promise.all()** otrzymujemy tablicę promis, które następnie musimy obsłużyć poprzez **.then** lub **async/await**, w przeciwnym przypadku nie będziemy mieć dostępu do zwróconych danych:

```javascript
const resources = [
	"producers",
	"saloons",
	`options?type=single&category=${category}`,
];

const requests = resources.map((resource) =>
	fetch(`http://localhost:3000/${resource}`)
);

Promise.all(requests)
	.then((res) => res.map((el) => el.json()))
	.then((res) => {
		res[0].then((producers) => {
			producers.forEach((producer, index) => {
				producersTemplate += `
						<div class="form-check">
							<input class="form-check-input" type="checkbox" value="" id="customCheck${index}" />
							<label class="form-check-label" for="customCheck${index}"> ${producer.name} </label>
						</div>
					`;
			});

			producersContainer.insertAdjacentHTML("beforeend", producersTemplate);
		});
		res[1].then((saloons) => {
			saloons.forEach((saloon) => {
				saloonsTemplate += `
						<li class="dropdown-item">${saloon.name}</li>
					`;
			});

			saloonsContainer.insertAdjacentHTML("beforeend", saloonsTemplate);
		});
		res[2].then((options) => {
			options.forEach((option, index) => {
				optionsTemplate += `
					<div class="form-check">
						<input class="form-check-input" type="checkbox" value="" id="customCheck${index}" />
						<label class="form-check-label" for="customCheck${index}"> ${option.label} </label>
					</div>
					`;
			});

			optionsContainer.insertAdjacentHTML("beforeend", optionsTemplate);
		});
	})
	.catch((err) => {
		console.log(err);
	});
```

### Promise.all() - then vs async/await

```javascript
async fetchProductAmount(categories) {
	const requests = categories.map((category) => fetch(`${config.apiUrl}/products?category=${category.id}`));

	try {
		const values = await Promise.all(requests);
		console.log(values);
		values.map(async (value) => {
		const products = await value.json();
		categories.find((el) => el.id === products[0].category).products = products.length;
		});
	} catch (err) {
		console.warn(err);
	}

	// TO JEST TO SAMO CO PONIŻEJ

	Promise.all(requests)
		.then((res) => res.map((el) => el.json()))
		.then((res) => {
			for (const el of res) {
				el.then((products) => {
				categories.find((el) => el.id === products[0].category).products = products.length;
						});
					}
		})
		.catch((err) => {
			console.warn(err);
		});
	},
```

## Promise.any()

Zwraca pierwszą zakończoną pozytywnie promisę. Jeśli wszystkie się wysypią zwraca aggregate error.

## Promise.race()

Zwraca wynik pierwszej zwróconej promisy, niezależnie czy jest resolved czy rejected.
Bierze odpowiedź pierwszej rozwiązanej promisy.

Jeśli mamy do wysłania dużo requestów to warto skorzystać z generatora, bo wtedy możemy pobierać dane nawet gdy któryś request się wysypał.

!! return użyty w catch generatorze go zatrzymuje i zamyka

```javascript
import axios from "axios";

const fetchUsers = () =>
	axios.get("https://jsonplaceholder.typicode.com/users/");

const fetchUserById = (id) =>
	axios.get(`https://jsonplaceholder.typicode.com/users/${id}`);

const fetchPostsByUserId = (id) =>
	axios.get(`https://jsonplaceholder.typicode.com/posts?userId=${id}`);

async function* setupUsers(users) {
	for (const user of users) {
		try {
			const [summary, posts] = await Promise.all([
				fetchUserById(user.id),
				fetchPostsByUserId(user.id),
			]);
			yield { summary: summary.data, user, posts: posts.data };
		} catch (e) {
			console.log("elo");
		}
	}
}

const init = async () => {
	try {
		const { data: users } = await fetchUsers();

		const iterator = setupUsers(users);

		for await (const user of iterator) {
			console.log(user);
		}
	} catch (e) {
		console.log(e);
	}
};

init();
```
