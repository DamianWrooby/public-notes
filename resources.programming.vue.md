---
id: hqiT6U7b5vNg35Irlx4DP
title: Vue
desc: ""
updated: 1642749711559
created: 1632236847239
---

## Instancja Vue

Podpięcie instancji Vue pod element HTML zamienia wnętrze tego elementu w templatkę Vue, w której możemy skorzystać z dyrektyw Vue i innych ficzerów.

## Aktializacja drzewa DOM

Vue po zmianie jakichś danych tworzy nowy virtual DOM i porównuje go ze starym. W prawdzimym drzewie DOM renderowane są tylko te rzeczy, które uległy zmianie. Virtual DOM to javascriptowa, uproszczona wersja drzewa DOM.

## Props

### Prop fallthrough

Kiedy customowemu komponentowi  nadamy propsy:
```html
<base-button type="submit" @click="doSomething">Click me</base-button>
```
To zostaną one automatycznie przypisane do rootowego elementu wewnątrz template'a:

```html
<template>  
  <button>
    <slot></slot>
  </button>
</template>
 
<script>export default {}</script>
```

Czyli w tym przypadku do buttona. Mamy dostęp do tych propsów poprzez _this.$attrs_
### Bindowanie wszystkich propsów

Jeśli jako propsy do komponentu wstawiamy wszystkie właściwości jakiegoś obiektu to zamiast pisać:

```javascript
<template>
  <user-data :firstname="person.firstname" :lastname="person.lastname"></user-data>
</template>
 
<script>
  export default {
    data() {
      return {
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>
```

Możemy zbindować cały obiekt:
```javascript
<template>
  <user-data v-bind="person"></user-data>
</template>
 
<script>
  export default {
    data() {
      return {
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>
```



## Refs

We Vue możemy tak jak w Reactcie przechowywać referencje do elementów drzewa DOM:

```javascript
<input ref="textInput"></input>
```

Można się później do nich dostać z poziomu:
```javascript
this.$refs.textInput;
```
Mamy tu dostęp do całego obiektu tego elementu inputa i wszystkich jego właściwości.
Można podejrzeć poprzez:
```javascript
console.dir(this.$refs.textInput);
```

Można to wykorzystać np. kiedy chcemy przekazać cały value z inputa na kliknięcie, zamiast na zdarzeniu input - przy każdym naciśnięciu klawisza. Może się to przydać przy złożonych formularzach żeby poprawić wydajność.

## Events

**child -> parent**

v-on wychwytuje zdarzenia emitowane przez dzieci poprzez $emit

Dziecko emituje zdarzenie _clicked_:

```javascript
export default {
	methods: {
		onClickButton(event) {
			this.$emit("clicked", "someValue");
		},
	},
};
```

Rodzic otrzymuje przekazany event wraz z payloadem:

```javascript
<div>
    <child @clicked="onClickChild"></child>
</div>
```

```javascript
export default {
	methods: {
		onClickChild(value) {
			console.log(value); // someValue
		},
	},
};
```

Poprzez emitowanie eventów możemy uruchamiać w komponencie rodzica różne metody lub też aktualizować własnego propsa:

```javascript
export default {
	methods: {
		onClickButton(event) {
			this.$emit("update:propName", "newValue");
		},
	},
};
```

Nawet bezpośrendio:

```javascript
<label>
  {{ label }}
  <div
    @:click="$emit('update:propName', $event.target.value)"
  >
</label>
```

#### Przekazywanie obiektu event

Gdy wywołujemy metodę bez argumentów:
```javascript
<input type="text" @input="setName"></input> // w ten sposób niejawnie przekazujemy obiekt event

methods: {
   setName(event) {
      this.name = event.target.value;
   }
}
```

I z argumentami:
```javascript
<input type="text" @input="setName($event, 'Damian')"></input> // w ten sposób przekazujemy już obiekt window jawnie

methods: {
   setName(event, firstName) {
      this.name = event.target.value;
   }
}
```
## Watchers

Use cases:

- Fetching data
- Manipulating the DOM
- Using a browser API, such as local storage or audio playback

Stosujemy, gdy chcemy uruchomić jakiś efekt kiedy zmieni się dany props:

```javascript
export default {
  name: 'ColourChange',
  props: ['colour'],
  watch: {
    colour()
      console.log('The colour has changed!');
    }
  }
```

Watchera możemy podpiąć pod jakąkolwiek dynamiczną zmienną: **computed props**, **props** czy jakieś dane zadeklarowane w **data()**.

**Nie używać do aktualizacji stanu!**

#### Watch is for side effects. If you need to change state you want to use a computed prop instead.

## Computed props

Use cases:

- compose new data from other data
- get nested data
  **let us compose new data from other data**

_**Using computed props to compose new data is one of the most useful patterns to learn when building Vue applications.**_

#### Kiedy używać computed a kiedy methods?

Metod używamy do zmiany istniejących danych, a computed kiedy chcemy zmienić sposób wyświetlania istniejących danych.

Computed props są reaktywne. Jeśli zmieni się jakakolwiek zależność, computed props zareaguje:

```javascript
export default {
	name: "FullName",
	props: ["firstName", "lastName"],
	computed: {
		fullName() {
			return this.firstName + " " + this.lastName;
		},
	},
};
```

Jeśli zmieni się **firstName** to zmieni się również **fullName**.

Getting nested data:

```javascript
computed() {
  importantValue() {
    return this.data.nested.really.deeply.importantValue;
  },
}
```
## Methods vs Computed Props vs Watchers

### Methods

Stosowane do eventów i data binding w przypadku danych, które muszą być cały czas przeliczane i aktualizowane - najbardziej reaktywne. 

```javascript
<div>{{ calculateValue() }}</div>
```

Tak użyte metody są przeliczane przy każdym rerenderze komponentu.

### Computed Props

Stosowane do obliczania danych na podstawie innych zależnych danych. Przeliczane tylko, gdy zmieni się jakaś zmienna użyta wewnątrz.

### Watchers 

Stosowane gdy chcemy uruchomić jakiś kod w reakcji na zmianę jakichś danych.

## Two way data binding

Jeżeli chcemy uzyskać dwustronną komunikację pomiędzy komponentem rodzica a dzieckiem, musimy aktualizować zmienną w komponencie rodzica poprzez event emitowany przez dziecko, a następnie przekazać tę zmienną z powrotem do dziecka jako props:

```javascript
// PARENT
<SampleComponent :inputData="parentData" @update="parentData = $event;" />

// CHILD
<template>
   <div>
      <input type="text" v-model="inputData" @keyup="$emit('update',inputData);" />
   </div>
</template>
<script>
   export default {
      name: "HelloWorld",
      props: {
         inputData: String
      }
   };
</script>
```

Od wersji **2.3.0** jest to uproszczone poprzez modyfikator _**.sync**_:

```javascript
// PARENT
<SampleComponent :inputData.sync="parentData" />

// CHILD
<template>
   <div>
      <input type="text" v-model="inputData" @keyup="$emit('update:inputData', inputData);"/>
   </div>
</template>
<script>
   export default {
      name: "HelloWorld",
      props: {
         inputData: String
      }
   };
</script>
```

Tutaj jednak vue wyrzuca błąd związany ze zmianą propsa w komponencie dziecka.

Możemy skorzystać zatem z **.sync** lub **v-model**:

.sync:

```html
<comp :value.sync="bar"></comp>
```

to skrócone:

```html
<comp :value="bar" @update:value="val => bar = val"></comp>
```

v-model:

```html
<comp v-model="bar"></comp>
```

to skrócone:

```html
<comp :value="bar" @input="val => bar = val"></comp>
```

Dlatego w komponencie dziecka emitujemy odpowiedni event update lub input za pomocą którego aktualizujemy zmienną. Nie możemy aktualizować propsa bezpośrednio z poziomu dziecka.

```html
<span
	@click="$emit('input', bar - 1)"
></span>
```

**v-model na zagnieżdżonych komponentach**
https://zaengle.com/blog/using-v-model-on-nested-vue-components

## Komunikacja pomiędzy komponentami i zarządzanie stanem

Sposoby komunikacji:

1. Props i custom events
2. Props i callbacki (do komponentu dziecka poprzez props przekazujemy callback należący do rodzica, który uruchamiamy z poziomu komponentu dziecka)
3. Event bus - zdarzenia globalne
4. Biblioteka do zarządzania stanem (np. Vuex)

W przypadku gdy chcemy przekazywać dane poprzez propsy i eventy poprzez kilka komponentów tworzymy dwustronną komunikację poprzez **v-model** lub **.sync** i ustawiamy **watchera** na zmienne w **data()**. Aktualizujemy najpierw zmienne w **data()** danego komponentu, następnie watcher zapięty na tę zmienną emituje zdarzenie i przekazuje dane wyżej.

```javascript


watch: {
  template: `
    <ul class="dropdown-menu text-center text-dark shadow saloon-list">
			<li v-for="saloon in saloonsList" :key="saloon.id" class="dropdown-item" @click="inputValue = saloon.id">{{ saloon.name }}</li>
		</ul>`,

  data() {
		return {
			saloonsList: [],
			inputValue: null,
		};
	},

	inputValue: {
			handler(newVal) {
				this.$emit("update:selectedSaloon", newVal);
			},
		},
	},
```
_e-commerce-template-vue-es5_
## Klasy CSS

Dynamiczne klasy możemy uzyskać na dwa sposoby:

Object-syntax (prostsza składnia):

```javascript
<div :class="{dynamicClass: show}">
```

Array-syntax (większa kontrola):

```javascript
<div :class="[show ? 'showClass' : '', isGreen ? 'green' : 'blue']">
```

Statyczne klasy dodajemy standardowo:

```javascript
<div :class="{dynamicClass: show}" class="static-class">
```

Dodanie klasy o dynamicznej wartości i klasy zależnej od flagi.
Klasa **current** zależna od prawdziwości wyrażenia oraz dynamiczna klasa **unit.dayID**

```javascript
:class="[dayOfWeek.includes(unit.dayID.substr(0, 3)) ? 'current' : '', unit.dayID]"
```

## Mixins

Wadą mixinów jest fakt, że kod staje się mniej czytelny, bo nie wiadomo skąd niektóre dane pochodzą.

## Animacje

https://www.youtube.com/watch?v=DGI_aKld0Jg


## Struktura katalogów

Katalog odrębny dla każdego komponentu.

## Rejestracja komponentów

W vue cli lub nuxt rejestrujemy poprzez propercję components a w es5 poprzez Vue.component('users-container', usersContainer); I ten komponent jest dostępny w całej instacji Vue czyli tam gdzie jest zapięta w hmtl.

## v-for po obiekcie

```javascript
<div v-for="([key, value]) in Object.entries(this.user)">
```

Chcemy wylistować klucz i wartość na widoku.

## Dobre praktyki

- v-if na zewnętrznym elemencie przenosić do komponentu rodzica
- unikać js'a w templatkach
- długie warunki z ifów przenosić do computed
- zamiast wysyłać pustego emita, można przekazać funkcję callback jako props
- zamiast ustawiać watchery można skorzystać z gettera i settera computed
- instancję axios utworzyć w osobnym pliku jako singleton