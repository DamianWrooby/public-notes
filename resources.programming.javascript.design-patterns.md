---
id: 0m9aH0YQzgtzHqdOhkMea
title: Design Patterns
desc: ""
updated: 1638904441871
created: 1638904422355
---

## Singleton

```javascript
class UsersService {

  #users = []

  private constructor() {}

  private static instance = null

  static getInstance() {
    if (!UsersService.instance) {
      UsersService.instance = new UsersService()
    }
    return UsersService.instance
  }

  get users() {
      return this.#users
  }

  fetch() {
    this.#users = [
      { id: 1, name: 'Krystian', age: 25 },
      { id: 2, name: 'Paweł', age: 30 },
      { id: 3, name: 'Mateusz', age: 28 }
    ]
  }

  remove(user) {
    const { id } = user
    this.#users = this.#users.filter(user => user.id !== id)
  }

}

const usersService = UsersService.getInstance()

usersService.fetch()

usersService.remove({ id: 1, name: 'Krystian', age: 25 })


const usersServiceAnotherInstnace = UsersService.getInstance()

usersService === usersServiceAnotherInstnace ? console.log('Ta sama instancja') : console.log('Coś całkiem innego')
```
